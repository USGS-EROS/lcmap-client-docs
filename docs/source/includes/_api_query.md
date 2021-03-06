# API: Data Queries

The following subsections provide documentation for querying the Earth data that is stored in the LCMAP system.

LCMAP data are processed to a level that enables direct use in quantitative applications including:

* Exploratory data analysis
* Numerical modeling
* Geospatial, multispectral, and multi-temporal manipulation

These are made available for purposes of data reduction, analysis, and interpretation. Characteristics of these data include:

* Scaled or standardized physical units and data types
* Consistent spatial coverage and cartographic projection
* Common data formats and accompanying metadata of sufficient detail on data provenance, geographic extent, scaling coefficients, and processing history

These are provided in order to enable independent reconstruction of the dataset.

Tiles may be retrieved in different formats: as JSON, GeoTIFF, and NetCDF. This is controlled by setting the HTTP request Accept header to:

* application/vnd.usgs.lcmap.v1.0+json
* application/vnd.usgs.lcmap.v1.0+geotiff
* application/vnd.usgs.lcmap.v1.0+netcdf

## Landsat

```python
from lcmap.client import Client
client = Client()

ubid = "LANDSAT_8/OLI_TIRS/sr_band1"
x, y = -1850865, 2956785
t1, t2 = '2013-01-01', '2015-01-01'
spec, tiles = client.data.tiles(ubid, x, y, t1, t2)
```

```shell
LCMAP_ACCEPT_HDR="Accept: application/vnd.usgs.lcmap.v0.5+json"
POINT='-1850865,2956785'
BAND='LANDSAT_8/OLI_TIRS/sr_band1'
TIME='2013-01-01/2015-01-01'
TILES=$(curl -s \
  -H "$LCMAP_ACCEPT_HDR" \
  -H "$LCMAP_TOKEN_HDR" \
  "http://localhost:1077/api/data/tiles?band=$BAND&point=$POINT&time=$TIME")
```

```vb
TBD
```

```clojure
TBD
```

```ruby
TBD
```

```r
TBD
```

The API provides a list of uniformly shaped tiles for each spectral band of Landsat 5, 7, 8 and a tile specification.

Client libraries use the tile specification to transform response data into runtime environment specific data structures. For example, the Python client converts tiles into Numpy arrays.

To retrieve tiles, specify a point in projection coordinate system, the univeral band ID (ubid), and an ISO-8601 date range. The structure of a UBID is &lt;mission&gt;/&lt;sensor&gt;/&lt;band-short-name&gt; (e.g. "LANDSAT_5/TM/sr_band1"). These are the available missions, sensors, and band short names:

| Mission          | Sensor    | Band Short Name       |
|------------------|-----------|-----------------------|
| LANDSAT_5        | TM        | toa_band[1..7], toa_qa, sr_band[1..7], sr_cloud_qa, sr_cloud_shadow_qa, sr_snow_qa, sr_adjacent_cloud_qa, cfmask |
| LANDSAT_7        | ETM       | toa_band[1..7], toa_qa, sr_band[1..7], sr_cloud_qa, sr_cloud_shadow_qa, sr_snow_qa, sr_adjacent_cloud_qa, cfmask |
| LANDSAT_8        | OLI_TIRS  | toa_band[1..7], toa_qa, sr_band[1..7], sr_cloud_qa, sr_cloud_shadow_qa, sr_snow_qa, sr_adjacent_cloud_qa, cfmask |

## Tile Specifications

```python
client.data.specs("LANDSAT_8/OLI_TIRS/sr_band1")
```

```shell
BAND='LANDSAT_8/OLI_TIRS/sr_band1'
SPECS=$(curl -s \
  -H "$LCMAP_ACCEPT_HDR" \
  -H "$LCMAP_TOKEN_HDR" \
  "http://localhost:1077/api/data/specs?band=$BAND")
```

```vb
TBD
```

```clojure
TBD
```

```ruby
TBD
```

```r
TBD
```

## Scene Metadata

```python
client.data.scene("LC80460272013104LGN01")
```

```shell
ID="LC80460272013104LGN01"
SCENE=$(curl -s \
  -H "$LCMAP_ACCEPT_HDR" \
  -H "$LCMAP_TOKEN_HDR" \
  "http://localhost:1077/api/data/scenes/$ID")
```

```vb
TBD
```

```clojure
TBD
```

```ruby
TBD
```

```r
TBD
```

## NDVI Model Results

```python
x, y = -1909498+(128*30), 2978512
t1, t2 = "2013-01-01", "2014-01-01"
_, ndvi = client.data.tiles("LCMAP/SEE/NDVI", x, y, t1, t2)
```

```shell
POINT="-1909498,2978512"
TIME="2013-01-01/2014-01-01"
BAND="LCMAP/SEE/NDVI"

TILES=$(curl -s \
  -H "$LCMAP_ACCEPT_HDR" \
  -H "$LCMAP_TOKEN_HDR" \
  "http://localhost:1077/api/data/tiles?band=$BAND&point=$POINT&time=$TIME")

echo $TILES
```

```vb
TBD
```

```clojure
TBD
```

```ruby
TBD
```

```r
TBD
```

### Future Improvements

* Provide a list of available bands of data and support queries by mission, sensor, and spectral band name.
* Provide scene metadata as a separate resource.
* Provide data from a variety of spatial areas (CONUS, Hawaii, and others) in area-specific projection coordinate systems.
* Provide paginated results.

## CCDC

TBD
