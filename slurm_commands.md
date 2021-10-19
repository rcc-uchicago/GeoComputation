
<a href="https://rcc.uchicago.edu/docs/using-midway/index.html" target="_blank">Midway User Guide</a>

<a href="https://mdw3.rcc.uchicago.edu/user-guide/" target="_blank">Midway3 User Guide</a> 
## **SLURM General commands**

Get documentation on a command:

`man <command>`



Try the following commands:

`man sbatch`
`man squeue`
`man scancel`


* * *




## **Submitting jobs**

The following example script specifies a partition, time limit, memory allocation and number of cores. All your scripts should specify values for these four parameters. You can also set additional parameters as shown, such as jobname and output file. For This script performs a simple task — it generates of file of random numbers and then sorts it. A detailed explanation the script is available [here](https://rcc.uchicago.edu/docs/running-jobs/


#Submitting_batch_jobs_using_the_sbatch_command).

```bash
#!/bin/sh

#SBATCH --job-name=single-node-cpu-example
#SBATCH --account=pi-[group]
#SBATCH --partition=caslake
#SBATCH --ntasks-per-node=1  # number of tasks
#SBATCH --cpus-per-task=1    # number of threads per task

# LOAD MODULES
module load python

# DO COMPUTE WORK
export OMP_NUM_THREADS=$SLURM_CPUS_PER_TASK
python hello.py
```

Now you can submit your job with the command:

`sbatch myscript.sh`



If you want to test your job and find out when your job is estimated to run use (note this does not actually submit the job):



`sbatch --test-only myscript.sh`

* * *

## 

## **Information on jobs**

List all current jobs for a user:
`squeue -u <username>`

List all running jobs for a user:
`squeue -u <username> -t RUNNING`

List all pending jobs for a user:
`squeue -u <username> -t PENDING`

List priority order of jobs for the current user (you) in a given partition:
`showq-slurm -o -u -q <partition>`

List all current jobs in the shared partition for a user:
`squeue -u <username> -p shared`

List detailed information for a job (useful for troubleshooting):
`scontrol show jobid -dd <jobid>`

List status info for a currently running job:
`sstat --format=AveCPU,AvePages,AveRSS,AveVMSize,JobID -j <jobid> --allsteps`

Once your job has completed, you can get additional information that was not available during the run. This includes run time, memory used, etc.
To get statistics on completed jobs by jobID:
`sacct -j <jobid> --format=JobID,JobName,MaxRSS,Elapsed`

To view the same information for all jobs of a user:
`sacct -u <username> --format=JobID,JobName,MaxRSS,Elapsed`




* * *


## 

## **Controlling jobs**

To cancel one job:
`scancel <jobid>`

To cancel all the jobs for a user:
`scancel -u <username>`

To cancel all the pending jobs for a user:
`scancel -t PENDING -u <username>`

To cancel one or more jobs by name:
`scancel --name myJobName`

To hold a particular job from being scheduled:
`scontrol hold <jobid>`

To release a particular job to be scheduled:
`scontrol release <jobid>`

To requeue (cancel and rerun) a particular job:
`scontrol requeue <jobid>`




* * *




## **Job arrays and useful commands**

As shown in the commands above, its easy to refer to one job by its Job ID, or to all your jobs via your username. What if you want to refer to a subset of your jobs? The answer is to submit your job set as a job array. Then you can use the job array ID to refer to the set when running SLURM commands. See the following excellent resources for further information:

[SLURM job arrays](http://slurm.schedmd.com/job_array.html)

To cancel an indexed job in a job array:
`scancel <jobid>_<index>`e.g.
`scancel 1234_4`



To find the original submit time for your job array
`sacct -j 32532756 -o submit -X --noheader | uniq`




* * *




## **Advanced (but useful!) commands**

The following commands work for individual jobs and for job arrays, and allow easy manipulation of large numbers of jobs. You can combine these commands with the parameters shown above to provide great flexibility and precision in job control. (Note that all of these commands are entered on one line)

Suspend all running jobs for a user (takes into account job arrays):
`squeue -ho %A -t R | xargs -n 1 scontrol suspend`

Resume all suspended jobs for a user:
`squeue -o "%.18A %.18t" -u <username> | awk '{if ($2 =="S"){print $1}}' | xargs -n 1 scontrol resume`

After resuming, check if any are still suspended:
`squeue -ho %A -u $USER -t S | wc -l`

The following is useful if your group has its own queue and you want to quickly see utilization.
`lsload |grep 'Hostname\|<partition>'`


