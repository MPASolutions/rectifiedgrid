RectifiedGrid
=============

RectifiedGrid is a Python package which, combining into a single class
several pythonpackages (e.g. Numpy, SciPy, shapely, rasterio, fiona,
geopandas, owslib, matplotlib-basemaps), simplifies geospatial
grid-based analyses. Numpy is a notable package for scientific
computing with a support for large, multi-dimensional
arrays and matrices: RectifiedGrid extends the numpy MaskedArray class
by adding geospatial functionalities (i.e. projection awareness,
boundingboxes, raster algebra). RectifiedGrid employs rasterio and
fiona under the hood for raster and vector I/O and owslibto access
data through OGC interoperable services.

RectifiedGrid has been initialy developed to support Integrated
Coastal Management and Maritime Spatial Planning analyses.

Installation
============
When using Rectifiedgrid, you need to make sure that Geopandas is installed with rtree support.
Refer to following link for more informations: 
* http://geopandas.org/install.html#installing-with-pip
* http://toblerity.org/rtree/install.html

Usage
=====

### Reading and plot GeoTIFF 

```python
import rectifiedgrid as rg
grid = rg.read_raster('test/data/adriatic_ionian.tiff', masked=True)
grid.plotmap()
```

![Alt text](/docs/images/adriatic_ionian_grid.png?raw=true "Adriatic Ionian Grid")

### Plotting options

RectifiedGrid wraps Matplotlib Basemap Toolkit functions.

```python
grid.plotmap(rivers=True, countries=True,
             grid=True, coast=True)
```

![Alt text](/docs/images/plot_options.png?raw=true "Plotting options")


### Map algebra: Ndvi calculation

```python
import rectifiedgrid as rg

b4 = rg.read_raster('test/data/b04.tiff', masked=True)
b8 = rg.read_raster('test/data/b08.tiff', masked=True)

ndvi = (b8 - b4)/(b8 + b4)
ndvi.plotmap(cmap=cmap_ndvi, legend=True, vmin=-1, vmax=1)
```

![Alt text](/docs/images/ndvi.png?raw=true "Ndvi example")


### Wrapping array-wise functions: distance from coast

RectifiedGrid implements a function wrapper (wrap_func) to apply
array-wise functions.

In this example we use the distance_transform_bf (from
scipy.ndimage,morphology) to calculate the distance from the coast for
the Adriatic-Inonian region.

```python
from scipy.ndimage.morphology import distance_transform_bf
distances = grid.wrap_func(distance_transform_bf)

# plotting
plt.figure(figsize=[10, 8])
distances.plotmap(rivers=True, countries=True,
             grid=True, coast=True, legend=True)

```

![Alt text](/docs/images/distances.png?raw=true "Distances example")

How to Cite
===========
Please, when you use rectifiedgrid cite as:

Menegon S, Sarretta A, Barbanti A, Gissi E, Venier C. (2016) Open
source tools to support Integrated Coastal Management and Maritime
Spatial Planning. PeerJ Preprints 4:e2245v2. doi: [10.5334/jors.106]
(https://doi.org/10.7287/peerj.preprints.2245v2)
