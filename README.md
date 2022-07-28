# ***eamenaR*** <img src="https://raw.githubusercontent.com/eamena-oxford/eamena-arches-dev/main/www/eamenaR_logo.png" width='100px' align="right"/>
> R package for front-end statistical analysis of the EAMENA database

The ***eamenaR*** allows to analyse GeoJSON files exported by [EAMEANA searches](https://github.com/eamena-oxford/eamena-arches-dev/tree/main/data/geojson#readme), or to connect directly to the Postgres database of EAMENA

# Install and load package

Install the R package

```
devtools::install_github("eamena-oxford/eamenaR")
```

And load the package

```
library(eamenaR)
```

# Main functions

## Prepare your data

### GeoJSON file

Create a search in EAMENA, in the export menu, copy the GeoJSON URL, paste it into your web browser and create a GeoJSON file (see this [document](https://github.com/eamena-oxford/eamena-arches-dev/tree/main/data/geojson#readme)).

### Geometries

Find the grid identifier of a heritage place by comparing its geometry to a GeoJSON of grid squares (gs). By default, the Grid Square file is **grid_squares.geojson** ([rendered](https://github.com/eamena-oxford/eamenaR/blob/main/inst/extdata/grid_squares.geojson) | [raw](https://raw.githubusercontent.com/eamena-oxford/eamenaR/main/inst/extdata/grid_squares.geojson))


```
library(dplyr)

grid.id <- geom_within_gs(resource.wkt = "POINT(0.9 35.8)")
grid.id
```
Will return `"E00N35-44"`

## Spatial

Distribution map for the default GeoJSON file **caravanserail.geojson** Heritage Places ([rendered](https://github.com/eamena-oxford/eamena-arches-dev/blob/main/data/geojson/caravanserail.geojson) | [raw](https://raw.githubusercontent.com/eamena-oxford/eamena-arches-dev/main/data/geojson/caravanserail.geojson))

```
geojson_map(map.name = "caravanserail", export.plot = T)
```

<p align="center">
  <img alt="img-name" src="https://raw.githubusercontent.com/eamena-oxford/eamenaR/main/results/caravanserail.png" width="500">
</p>

Maps can also be calculated on the values of GeoJSON fields, by adding the [field names](https://github.com/eamena-oxford/eamenaR/blob/main/results/caravanserail_list_fields) in the function options.

```
geojson_map(map.name = "caravanserail", 
            field.names = c("Disturbance.Cause.Type.", "Damage.Extent.Type"),
            export.plot = T)
```

It will create as many cards as there are different values, here is an example:

<p align="center">
  <img alt="img-name" src="https://raw.githubusercontent.com/eamena-oxford/eamenaR/main/results/caravanserail_Disturbance.Cause.Type._Lack_of_Maintenance_Management_Legal_Measures_and_Activities.png" width="300">
  <img alt="img-name" src="https://raw.githubusercontent.com/eamena-oxford/eamenaR/main/results/caravanserail_Damage.Extent.Type_1-10perc.png" width="300">
</p>

Retrieve the matches between these IDs and the EAMENA IDs by running:

```
geojson_stat(stat.name = "caravanserail", stat = "list_ids", export.stat = T)
```

This will give the data frame [caravanserail_list_ids.tsv] (https://github.com/eamena-oxford/eamenaR/blob/main/results/caravanserail_list_ids.tsv). If you want the IDs in a list (e.g. for a figure caption), run :

```
geojson_stat(stat.name = "caravanserail", stat = "list_ids", export.stat = F)
```

Will give:

```
1: EAMENA-0192223, 2: EAMENA-0192598, 3: EAMENA-0192599, [...], 153: EAMENA-0194775, 154: EAMENA-0194776, 155: EAMENA-0194777, 156: EAMENA-0194778
```

## Cultural Periods
### Plot cultural period from a GeoJSON file

Create a hash dictonnary named `d` to store all data

```
library(hash)

d <- hash()
```

Store all periods and sub-periods represented in the GeoJSON in the `d` dictonnary, and plot them by EAMENA ID

```
d <- list_cultural_periods(db = "geojson", 
                           d = d)
plot_cultural_periods(d = d, field = "periods", plot.type = "by.eamenaid", export.plot = T)
plot_cultural_periods(d = d, field = "subperiods", plot.type = "by.eamenaid", export.plot = T)
```
<p align="center">
  <img alt="img-name" src="https://raw.githubusercontent.com/eamena-oxford/eamenaR/main/results/cultural_periods_byeamenaid.png" width="500">
<br><br>
and superiods
<br><br>
  <img alt="img-name" src="https://raw.githubusercontent.com/eamena-oxford/eamenaR/main/results/cultural_subperiods_byeamenaid.png" width="500">
</p>

Here, the `plot_cultural_periods()` function will export two PNG charts for the default **caravanserail.geojson** file. These charts are saved by default in the `results/` folder. 
Periods and subperiods represented in a GeoJSON file can also be summed in a histogram

```
plot_cultural_periods(d = d, field = "subperiods", plot.type = "histogram", export.plot = T)
```
<p align="center">
  <img alt="img-name" src="https://raw.githubusercontent.com/eamena-oxford/eamenaR/main/results/cultural_subperiods_histog.png" width="500">
</p>

