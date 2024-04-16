# HPC - Traitement de données multidimensionnelles 

## Traitement des données extraites de Sentinel
Supposons que l'on ait besoin d'extraire des données depuis un satellite, et que l'on veuille les recupérer et les traiter afin de les intégrer dans de futures plateformes. Le format 
NetCDF (network Common Data Form) est un format de fichier qui permet de stocker des données scientifiques multidimensionnelles (variables), telles que la température, l'humidité, la pression, la vitesse et la direction du vent.

### Exploration, extraction et traitement des données de satellite
* D'abord se connecter sur [www.peps.cnes.fr](https://peps.cnes.fr/rocket/#/home)
* Effectuer une recherche basée sur les données géographiques
* Identifier et télécharger le données sources correspondantes
* Appliquer le code Python afin d'extraire et de plotter le DEM
  
![image](https://github.com/DiopBabacarEdu/TaoueY-HPC/assets/20286290/a34cfe6e-576f-489c-8dec-0051d8ae3c70)

### Étapes de traitement et de traçage
Afin d'extraire ces données, nous pouvons nous reférer à ce code ci-dessous :
```python
#########################################################
#	Plotting Envoronmental Data From Sentinel
# - HUM = 'humidity'
# - TOZON = 'total_ozone'
# - TCWV = 'total_columnar_water_vapour'
# - SEA_PRESSURE = 'sea_level_pressure'
#########################################################

import numpy as np
from netCDF4 import Dataset  # http://code.google.com/p/netcdf4-python/
import matplotlib.pyplot as plt
from mpl_toolkits.basemap import Basemap

HUM = 'humidity'
TOZON = 'total_ozone'
TCWV = 'total_columnar_water_vapour'
SEA_PRESSURE = 'sea_level_pressure'

def plot_parameter(coord_file,param_file,_param):
	nc_c = coord_file  # Filename 1
	nc_m = param_file            # Filename 2
	nc_coord = Dataset(nc_c, 'r')    # Dataset for coordinates
	nc_param = Dataset(nc_m, 'r')    # Dataset for meteo data

	lats = nc_coord.variables['latitude'][:]  # extract/copy the data
	lons = nc_coord.variables['longitude'][:]
	param_to_plot = nc_param.variables[_param][:]
	print(lats)
	
	
	map = Basemap(llcrnrlon=-18.,llcrnrlat=11.9,urcrnrlon=-10.,urcrnrlat=17.,resolution='i', projection='tmerc', lat_0 = 14.666020, lon_0 = -14.787668)
	#map = Basemap(llcrnrlon=-18.,llcrnrlat=11.9,urcrnrlon=-10.,urcrnrlat=17.,resolution='i', projection='tmerc', lat_0 = 14.666020, lon_0 = -14.787668)
	#map=Basemap(projection='mill',lat_ts=10,llcrnrlon=lons.min(),urcrnrlon=lons.max(),llcrnrlat=lats.min(),urcrnrlat=lats.max(),resolution='c')
	map.drawcoastlines()
	map.drawstates()
	map.drawcountries()
	map.drawlsmask(land_color='Linen', ocean_color='aqua')

	#lon, lat = np.meshgrid(lons, lats)
	x, y = map(lons, lats)

	param_ = map.contourf(x,y,param_to_plot[:])
	cb = map.colorbar(param_,"bottom", size="5%", pad="2%")
	plt.title(nc_param.variables[_param].long_name)
	cb.set_label("%s (%s)" % (nc_param.variables[_param].long_name,nc_param.variables[_param].units))
	plt.show()

#Uncomment to plot each of the given parameters
plot_parameter('tie_geo_coordinates.nc','tie_meteo.nc',TOZON)
#plot_parameter('tie_geo_coordinates.nc','tie_meteo.nc',HUM)
#plot_parameter('tie_geo_coordinates.nc','tie_meteo.nc',TCWV)
#plot_parameter('tie_geo_coordinates.nc','tie_meteo.nc',SEA_PRESSURE)
```

### Répartition de la couche d'ozone au-dessus la surface terrestre
![image](https://github.com/DiopBabacarEdu/TaoueY-HPC/assets/20286290/9e0d0df9-b84d-4d64-8591-74ffd7ed6a32)

### Répartition du niveau d'humidité relative 
![image](https://github.com/DiopBabacarEdu/TaoueY-HPC/assets/20286290/65eacf9f-fc5b-4f11-9bb6-6f17598f0d80)

### Répartition du niveau de vapeur au dessus de la surface terrestre
![image](https://github.com/DiopBabacarEdu/TaoueY-HPC/assets/20286290/d76a4cc0-0a75-46b8-a0ff-aa42a7a8e9bd)


