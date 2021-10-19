## QGIS 

### Calling algorithms from the Python console and Bash
```python
from qgis import processing
```
To find the right name for your algorithm, use the processingRegistry. Type the following line in console:
```python
for alg in QgsApplication.processingRegistry().algorithms():
        print(alg.id(), "->", alg.displayName())
```
Or inside Linux type the following line:
```bash
qgis_process list
```
You will get the following output

```python
QGIS (native c++)
	native:addautoincrementalfield	Add autoincremental field
	native:addfieldtoattributestable	Add field to attributes table
	native:adduniquevalueindexfield	Add unique value index field
	native:addxyfields	Add X/Y fields to layer
	native:affinetransform	Affine transform
	native:aggregate	Aggregate
	native:angletonearest	Align points to features
	native:antimeridiansplit	Geodesic line split at antimeridian
	native:arrayoffsetlines	Array of offset (parallel) lines
	native:arraytranslatedfeatures	Array of translated features
	native:aspect	Aspect
	native:assignprojection	Assign projection
	native:atlaslayouttoimage	Export atlas layout as image
	native:atlaslayouttopdf	Export atlas layout as PDF
	native:bookmarkstolayer	Convert spatial bookmarks to layer
	native:boundary	Boundary
	native:boundingboxes	Bounding boxes
	native:buffer	Buffer
	native:bufferbym	Variable width buffer (by M value)
	native:calculatevectoroverlaps	Overlap analysis
	native:cellstatistics	Cell statistics
	native:centroids	Centroids
	native:clip	Clip
	native:collect	Collect geometries
	native:combinestyles	Combine style databases
	native:converttocurves	Convert to curved geometries
	native:convexhull	Convex hull
	native:countpointsinpolygon	Count points in polygon
	native:createattributeindex	Create attribute index
	native:createconstantrasterlayer	Create constant raster layer
	native:creategrid	Create grid
	native:createpointslayerfromtable	Create points layer from table
	native:createrandombinomialrasterlayer	Create random raster layer (binomial distribution)
	native:createrandomexponentialrasterlayer	Create random raster layer (exponential distribution)
	native:createrandomgammarasterlayer	Create random raster layer (gamma distribution)
	native:createrandomgeometricrasterlayer	Create random raster layer (geometric distribution)
	native:createrandomnegativebinomialrasterlayer	Create random raster layer (negative binomial distribution)
	native:createrandomnormalrasterlayer	Create random raster layer (normal distribution)
	native:createrandompoissonrasterlayer	Create random raster layer (poisson distribution)
	native:createrandomuniformrasterlayer	Create random raster layer (uniform distribution)
	native:createspatialindex	Create spatial index
	native:dbscanclustering	DBSCAN clustering
	native:deleteduplicategeometries	Delete duplicate geometries
	native:deleteholes	Delete holes
	native:densifygeometries	Densify by count
	native:densifygeometriesgivenaninterval	Densify by interval
	native:detectvectorchanges	Detect dataset changes
	native:difference	Difference
	native:dissolve	Dissolve
	native:dropgeometries	Drop geometries
	native:dropmzvalues	Drop M/Z values
	native:equaltofrequency	Equal to frequency
	native:explodehstorefield	Explode HStore Field
	native:explodelines	Explode lines
	native:extendlines	Extend lines
	native:extenttolayer	Create layer from extent
	native:extractbinary	Extract binary field
	native:extractbyattribute	Extract by attribute
	native:extractbyexpression	Extract by expression
	native:extractbyextent	Extract/clip by extent
	native:extractbylocation	Extract by location
	native:extractmvalues	Extract M values
	native:extractspecificvertices	Extract specific vertices
	native:extractvertices	Extract vertices
	native:extractzvalues	Extract Z values
	native:fieldcalculator	Field calculator
	native:filedownloader	Download file
	native:fillnodata	Fill NoData cells
	native:filterverticesbym	Filter vertices by M value
	native:filterverticesbyz	Filter vertices by Z value
	native:fixgeometries	Fix geometries
	native:flattenrelationships	Flatten relationship
	native:forcerhr	Force right-hand-rule
	native:fuzzifyrastergaussianmembership	Fuzzify raster (gaussian membership)
	native:fuzzifyrasterlargemembership	Fuzzify raster (large membership)
	native:fuzzifyrasterlinearmembership	Fuzzify raster (linear membership)
	native:fuzzifyrasternearmembership	Fuzzify raster (near membership)
	native:fuzzifyrasterpowermembership	Fuzzify raster (power membership)
	native:fuzzifyrastersmallmembership	Fuzzify raster (small membership)
	native:generatepointspixelcentroidsinsidepolygons	Generate points (pixel centroids) inside polygons
	native:geometrybyexpression	Geometry by expression
	native:greaterthanfrequency	Greater than frequency
	native:highestpositioninrasterstack	Highest position in raster stack
	native:hillshade	Hillshade
	native:hublines	Join by lines (hub lines)
	native:importphotos	Import geotagged photos
	native:interpolatepoint	Interpolate point on line
	native:intersection	Intersection
	native:joinattributesbylocation	Join attributes by location
	native:joinattributestable	Join attributes by field value
	native:joinbynearest	Join attributes by nearest
	native:kmeansclustering	K-means clustering
	native:layertobookmarks	Convert layer to spatial bookmarks
	native:lessthanfrequency	Less than frequency
	native:linedensity	Line density
	native:lineintersections	Line intersections
	native:linesubstring	Line substring
	native:lowestpositioninrasterstack	Lowest position in raster stack
	native:meancoordinates	Mean coordinate(s)
	native:mergelines	Merge lines
	native:mergevectorlayers	Merge vector layers
	native:minimumenclosingcircle	Minimum enclosing circles
	native:multiparttosingleparts	Multipart to singleparts
	native:multiringconstantbuffer	Multi-ring buffer (constant distance)
	native:nearestneighbouranalysis	Nearest neighbour analysis
	native:offsetline	Offset lines
	native:orderbyexpression	Order by expression
	native:orientedminimumboundingbox	Oriented minimum bounding box
	native:orthogonalize	Orthogonalize
	native:package	Package layers
	native:pixelstopoints	Raster pixels to points
	native:pixelstopolygons	Raster pixels to polygons
	native:pointonsurface	Point on surface
	native:pointsalonglines	Points along geometry
	native:pointtolayer	Create layer from point
	native:poleofinaccessibility	Pole of inaccessibility
	native:polygonfromlayerextent	Extract layer extent
	native:polygonize	Polygonize
	native:polygonstolines	Polygons to lines
	native:postgisexecutesql	PostgreSQL execute SQL
	native:printlayoutmapextenttolayer	Print layout map extent to layer
	native:printlayouttoimage	Export print layout as image
	native:printlayouttopdf	Export print layout as PDF
	native:projectpointcartesian	Project points (Cartesian)
	native:promotetomulti	Promote to multipart
	native:randomextract	Random extract
	native:randompointsinextent	Random points in extent
	native:randompointsinpolygons	Random points in polygons
	native:randompointsonlines	Random points on lines
	native:rasterbooleanand	Raster boolean AND
	native:rasterize	Convert map to raster
	native:rasterlayerstatistics	Raster layer statistics
	native:rasterlayeruniquevaluesreport	Raster layer unique values report
	native:rasterlayerzonalstats	Raster layer zonal statistics
	native:rasterlogicalor	Raster boolean OR
	native:rastersampling	Sample raster values
	native:rastersurfacevolume	Raster surface volume
	native:reclassifybylayer	Reclassify by layer
	native:reclassifybytable	Reclassify by table
	native:rectanglesovalsdiamonds	Rectangles, ovals, diamonds
	native:refactorfields	Refactor fields
	native:removeduplicatesbyattribute	Delete duplicates by attribute
	native:removeduplicatevertices	Remove duplicate vertices
	native:removenullgeometries	Remove null geometries
	native:renametablefield	Rename field
	native:repairshapefile	Repair Shapefile
	native:reprojectlayer	Reproject layer
	native:rescaleraster	Rescale raster
	native:reverselinedirection	Reverse line direction
	native:rotatefeatures	Rotate
	native:roundrastervalues	Round raster
	native:ruggednessindex	Ruggedness index
	native:savefeatures	Save vector features to file
	native:saveselectedfeatures	Extract selected features
	native:segmentizebymaxangle	Segmentize by maximum angle
	native:segmentizebymaxdistance	Segmentize by maximum distance
	native:serviceareafromlayer	Service area (from layer)
	native:serviceareafrompoint	Service area (from point)
	native:setlayerencoding	Set layer encoding
	native:setlayerstyle	Set layer style
	native:setmfromraster	Set M value from raster
	native:setmvalue	Set M value
	native:setzfromraster	Drape (set Z value from raster)
	native:setzvalue	Set Z value
	native:shortestpathlayertopoint	Shortest path (layer to point)
	native:shortestpathpointtolayer	Shortest path (point to layer)
	native:shortestpathpointtopoint	Shortest path (point to point)
	native:shpencodinginfo	Extract Shapefile encoding
	native:simplifygeometries	Simplify
	native:singlesidedbuffer	Single sided buffer
	native:slope	Slope
	native:smoothgeometry	Smooth
	native:snapgeometries	Snap geometries to layer
	native:snappointstogrid	Snap points to grid
	native:spatialiteexecutesql	SpatiaLite execute SQL
	native:spatialiteexecutesqlregistered	SpatiaLite execute SQL (registered DB)
	native:splitfeaturesbycharacter	Split features by character
	native:splitlinesbylength	Split lines by maximum length
	native:splitvectorlayer	Split vector layer
	native:splitwithlines	Split with lines
	native:stylefromproject	Create style database from project
	native:subdivide	Subdivide
	native:sumlinelengths	Sum line lengths
	native:swapxy	Swap X and Y coordinates
	native:symmetricaldifference	Symmetrical difference
	native:taperedbuffer	Tapered buffers
	native:tinmeshcreation	TIN Mesh Creation
	native:transect	Transect
	native:translategeometry	Translate
	native:truncatetable	Truncate table
	native:union	Union
	native:wedgebuffers	Create wedge buffers
	native:writevectortiles_mbtiles	Write Vector Tiles (MBTiles)
	native:writevectortiles_xyz	Write Vector Tiles (XYZ)
	native:zonalhistogram	Zonal histogram
	native:zonalstatisticsfb	Zonal statistics
```
