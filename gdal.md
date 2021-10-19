

## Virtual file system 
### Introduction
GDAL can access files located on “standard” file systems, i.e. in the / hierarchy on Unix-like systems or in C:, D:, etc… drives on Windows. But most GDAL raster and vector drivers use a GDAL-specific abstraction to access files. This makes it possible to access less standard types of files, such as in-memory files, compressed files (.zip, .gz, .tar, .tar.gz archives), encrypted files, files stored on network (either publicly accessible, or in private buckets of commercial cloud storage services), etc.

Each special file system has a prefix, and the general syntax to name a file is /vsiPREFIX/…

Example:

```bash
gdalinfo /vsizip/my.zip/my.tif
```

Chaining
It is possible to chain multiple file system handlers.

### ogrinfo a shapefile in a zip file on the internet:

```bash
ogrinfo -ro -al -so /vsizip//vsicurl/https://raw.githubusercontent.com/OSGeo/gdal/master/autotest/ogr/shp/data/poly.zip
```


### ogrinfo a shapefile in a zip file on an ftp:

```bash
ogrinfo -ro -al -so /vsizip//vsicurl/ftp://user:password@example.com/foldername/file.zip/example.shp
```



**Drivers supporting virtual file systems**

Virtual file systems can only be used with GDAL or OGR drivers supporting the “large file API”, which is now the vast majority of file based drivers. The full list of these formats can be obtained by looking at the driver marked with ‘v’ when running either gdalinfo --formats or ogrinfo --formats.

Notable exceptions are the netCDF, HDF4 and HDF5 drivers.

### /vsizip/ (.zip archives)
/vsizip/ is a file handler that allows reading ZIP archives on-the-fly without decompressing them beforehand.

To point to a file inside a zip file, the filename must be of the form /vsizip/path/to/the/file.zip/path/inside/the/zip/file, where path/to/the/file.zip is relative or absolute and path/inside/the/zip/file is the relative path to the file inside the archive.

To use the .zip as a directory, you can use /vsizip/path/to/the/file.zip or /vsizip/path/to/the/file.zip/subdir. Directory listing is available with VSIReadDir(). A VSIStatL() (“/vsizip/…”) call will return the uncompressed size of the file. Directories inside the ZIP file can be distinguished from regular files with the VSI_ISDIR(stat.st_mode) macro as for regular file systems. Getting directory listing and file statistics are fast operations.

Note: in the particular case where the .zip file contains a single file located at its root, just mentioning /vsizip/path/to/the/file.zip will work.

Examples:

```bash
/vsizip/my.zip/my.tif  (relative path to the .zip)
/vsizip//home/even/my.zip/subdir/my.tif  (absolute path to the .zip)
/vsizip/c:\users\even\my.zip\subdir\my.tif
```

.kmz, .ods and .xlsx extensions are also detected as valid extensions for zip-compatible archives.

Starting with GDAL 2.2, an alternate syntax is available so as to enable chaining and not being dependent on .zip extension, e.g.: `/vsizip/{/path/to/the/archive}/path/inside/the/zip/file`. Note that `/path/to/the/archive` may also itself use this alternate syntax.

Write capabilities are also available. They allow creating a new zip file and adding new files to an already existing (or just created) zip file.

**Creation of a new zip file:**
```bash
fmain = VSIFOpenL("/vsizip/my.zip", "wb");
subfile = VSIFOpenL("/vsizip/my.zip/subfile", "wb");
VSIFWriteL("Hello World", 1, strlen("Hello world"), subfile);
VSIFCloseL(subfile);
VSIFCloseL(fmain);
```
Addition of a new file to an existing zip:
```bash
newfile = VSIFOpenL("/vsizip/my.zip/newfile", "wb");
VSIFWriteL("Hello World", 1, strlen("Hello world"), newfile);
VSIFCloseL(newfile);
```
Starting with GDAL 2.4, the GDAL_NUM_THREADS configuration option can be set to an integer or ALL_CPUS to enable multi-threaded compression of a single file. This is similar to the pigz utility in independent mode.

Read and write operations cannot be interleaved. The new zip must be closed before being re-opened in read mode.

### /vsigzip/ (gzipped file)
/vsigzip/ is a file handler that allows on-the-fly reading of GZip (.gz) files without decompressing them in advance.

To view a gzipped file as uncompressed by GDAL, you must use the /vsigzip/path/to/the/file.gz syntax, where path/to/the/file.gz is relative or absolute.

Examples:

```bash
/vsigzip/my.gz # (relative path to the .gz)
/vsigzip//home/even/my.gz # (absolute path to the .gz)
/vsigzip/c:\users\even\my.gz
```

`VSIStatL()` will return the uncompressed file size, but this is potentially a slow operation on large files, since it requires uncompressing the whole file. Seeking to the end of the file, or at random locations, is similarly slow. To speed up that process, “snapshots” are internally created in memory so as to be able being able to seek to part of the files already decompressed in a faster way. This mechanism of snapshots also apply to /vsizip/ files.

When the file is located in a writable location, a file with extension .gz.properties is created with an indication of the uncompressed file size (the creation of that file can be disabled by setting the `CPL_VSIL_GZIP_WRITE_PROPERTIES` configuration option to NO).

Write capabilities are also available, but read and write operations cannot be interleaved.

Starting with GDAL 2.4, the GDAL_NUM_THREADS configuration option can be set to an integer or ALL_CPUS to enable multi-threaded compression of a single file. This is similar to the pigz utility in independent mode. 

##### /vsitar/ (.tar, .tgz archives)
/vsitar/ is a file handler that allows on-the-fly reading in regular uncompressed .tar or compressed .tgz or .tar.gz archives, without decompressing them in advance.

To point to a file inside a .tar, .tgz .tar.gz file, the filename must be of the form /vsitar/path/to/the/file.tar/path/inside/the/tar/file, where path/to/the/file.tar is relative or absolute and path/inside/the/tar/file is the relative path to the file inside the archive.

To use the .tar as a directory, you can use /vsizip/path/to/the/file.tar or `/vsitar/path/to/the/file.tar/subdir`. Directory listing is available with VSIReadDir(). A VSIStatL() (“/vsitar/…”) call will return the uncompressed size of the file. Directories inside the TAR file can be distinguished from regular files with the VSI_ISDIR(stat.st_mode) macro as for regular file systems. Getting directory listing and file statistics are fast operations.

Note: in the particular case where the .tar file contains a single file located at its root, just mentioning /vsitar/path/to/the/file.tar will work.

Examples:

```bash
/vsitar/my.tar/my.tif # (relative path to the .tar)
/vsitar//home/even/my.tar/subdir/my.tif # (absolute path to the .tar)
/vsitar/c:\users\even\my.tar\subdir\my.tif
```



GDAL Rasterize

For this example, I used a shape file as a mask.  I wanted to keep all the pixels with values (i.e. not "No Data") inside the shapes, and remove all those outside the shapes. Once that was done, I set the no data value to 0 and only kept the pixels w/values over 88.

```bash
gdal_rasterize -i -burn 0 mask.shp input.tif
gdal_translate -a_nodata 0 input.tif output.tif
gdal_sieve.py -st 88 -8 output.tif
```

Ratified Raster Brick

```bash
require(rgdal)
library(sf)
# The input file geodatabase
fgdb <- "land.gdb"

# Read the feature class
fc <- st_read(dsn=fgdb,layer="all_land")
forest <- read.csv("IDs_to_keep.csv")
world.map <- fc[fc$gridcode %in% forest$gridcode,]

library(raster)
ext <- extent(world.map)
gridsize <- 30
r <- raster(ext, res=gridsize)

selected_fields <- c("Own", "Cover", "Secondary")

library(fasterize)
test <- lapply(selected_fields, function(x) {
   myraster <- fasterize(world.map, r, field=x)
   names(myraster) <- x
   if(is.factor(world.map[,x][[1]])) {
      field <- x
      myraster <- ratify(myraster)
      rat <- levels(myraster)[[1]]
      rat[[field]] <- levels(world.map[,x][[1]])[rat$ID]
   levels(myraster) <- rat}
return(myraster)})
```

## MrSID Format

The tool mrsiddecode is part of the MrSid SDK from Extensis and can be downloaded from here: https://www.extensis.com/support/developers


Use the binary called mrsiddecode to convert *.sid to geotiff
```bash
mrsiddecode -i input.sid -o output.tif -of tifg -wf
```


Compress and Change the -a_srs parameter to your EPSG
```bash
gdal_translate -a_srs EPSG:25832 -co COMPRESS=JPEG -co PHOTOMETRIC=YCBCR -co TILED=YES output.tif outputcomp.tif
```
