# ***eamenaR*** a R package for front-end statistical analysis <br> of the EAMENA database <img src="https://raw.githubusercontent.com/eamena-oxford/eamena-arches-dev/main/www/eamenaR_logo.png" width='100px' align="right"/>

The ***eamenaR*** allows to analyse GeoJSON files exported from [EAMEANA searches](https://github.com/eamena-oxford/eamena-arches-dev/tree/main/data/geojson#readme), or connect directly the Postgres DB of EAMENA

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

### Geometries

Return the Grid ID of an Heritage Place by comparing their geometry to a GeoJSON of Grid Squares (gs)

```
library(dplyr)

grid.id <- geom_within_gs(resource.wkt = "POINT(0.9 35.8)")
grid.id
```
Will return `"E00N35-44"`

## Spatial

Distribution map of the default GeoJSON file

```
geojson_map(map.name = "caravanserail", export.plot = T)
```

<p align="center">
  <img alt="img-name" src="https://raw.githubusercontent.com/eamena-oxford/eamenaR/main/results/caravanserail.png" width="500">
</p>

Maps can also be computed on the GeoJSON fields' values, adding the [fields' names](https://github.com/eamena-oxford/eamenaR/blob/main/results/caravanserail_list_fields) in the function options. 

```
geojson_map_temp(map.name = "caravanserail", 
                 field.names = c("Disturbance.Cause.Type.", "Damage.Extent.Type"),
                 export.plot = T)
```

It will create as many maps as different values, here a sample:

<p align="center">
  <img alt="img-name" src="https://raw.githubusercontent.com/eamena-oxford/eamenaR/main/results/caravanserail_Disturbance.Cause.Type._Lack_of_Maintenance_Management_Legal_Measures_and_Activities.png" width="300">
  <img alt="img-name" src="https://raw.githubusercontent.com/eamena-oxford/eamenaR/main/results/caravanserail_Damage.Extent.Type_1-10perc.png" width="300">
</p>

To retrieve the correspondances between these IDs and the EAMENA IDs, running:

```
geojson_stat_temp(stat.name = "caravanserail", stat = "list_ids", export.stat = T)
```

Will give the dataframe [caravanserail_list_ids.tsv](https://github.com/eamena-oxford/eamenaR/blob/main/results/caravanserail_list_ids.tsv). If you want the IDs in a list, run:

```
geojson_stat_temp(stat.name = "caravanserail", stat = "list_ids", export.stat = F)
```

Will give:

```
1: EAMENA-0192223, 2: EAMENA-0192598, 3: EAMENA-0192599, 4: EAMENA-0192600, 5: EAMENA-0192601, 6: EAMENA-0192602, 7: EAMENA-0192603, 8: EAMENA-0192604, 9: EAMENA-0192605, 10: EAMENA-0192606, 11: EAMENA-0192608, 12: EAMENA-0192609, 13: EAMENA-0192610, 14: EAMENA-0192611, 15: EAMENA-0192612, 16: EAMENA-0192613, 17: EAMENA-0192614, [...], 144: EAMENA-0194766, 145: EAMENA-0194767, 146: EAMENA-0194768, 147: EAMENA-0194769, 148: EAMENA-0194770, 149: EAMENA-0194771, 150: EAMENA-0194772, 151: EAMENA-0194773, 152: EAMENA-0194774, 153: EAMENA-0194775, 154: EAMENA-0194776, 155: EAMENA-0194777, 156: EAMENA-0194778)
```

## Cultural Periods
### Plot cultural period from a GeoJSON file

Create a hash dictonnary named `d` to store all data

```
library(hash)

d <- hash()
```

Store all periods and subperiods represented in the GeoJSON to the `d` dictonnary, and plot them by EAMENA ID

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

Here, the `plot_cultural_periods()` function will export two PNG diagrams for the [caravanserail.geojson](https://github.com/eamena-oxford/eamena-arches-dev/blob/main/data/geojson/caravanserail.geojson) file (by default). These graphics are created by default in the `results/` folder. 
Periods and subperiods represented in a GeoJSON file can also be summed in a histogram

```
plot_cultural_periods(d = d, field = "subperiods", plot.type = "histogram", export.plot = T)
```
<p align="center">
  <img alt="img-name" src="https://raw.githubusercontent.com/eamena-oxford/eamenaR/main/results/cultural_subperiods_histog.png" width="500">
</p>

