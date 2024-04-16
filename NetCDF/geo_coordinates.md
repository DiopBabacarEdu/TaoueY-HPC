# HPC - Extraction de données sous forme matricielle
## Traitement des données extraites de Sentinel
Supposons que l'on ait besoin d'extraire des données depuis un satellite, et que l'on veuille les recupérer et les traiter sous forme matricielle 
afin de les intégrer dans de futures plateformes. L'une des structures de données les plus puissantes permettant de représenter des données sont les matrices.   

### Les matrices comme structures de données
* Les matrices offrent une structure de données efficace pour stocker et manipuler des données tabulaires, facilitant les opérations vectorielles et 
l'implémentation d'algorithmes d'algèbre linéaire.
* Elles sont essentielles dans de nombreux domaines scientifiques et techniques, y compris la physique, 
l'ingénierie, les statistiques et l'apprentissage automatique, ainsi que dans le traitement d'images où elles représentent des images sous forme de pixels.
* En outre, les matrices sont utilisées pour formuler et résoudre des problèmes d'optimisation et de recherche opérationnelle, et elles jouent un rôle crucial 
dans le stockage de données dans les bases de données relationnelles.


### Exploration, extraction et traitement des données
* D'abord se connecter sur [www.peps.cnes.fr](https://peps.cnes.fr/rocket/#/home)
* Effectuer une recherche basée sur les données géographiques
* Identifier et télécharger le données sources correspondantes
* Appliquer le code Python afin d'extraire et de plotter le DEM

### Étapes de traitement et de traçage
Afin d'extraire ces données, nous pouvons nous reférer à ce code ci-dessous :
```python
##########################################
#@Plotting DEM Data From Sentinel		
#@require Python Compiler, 3.1.
#@authors : B.Diop
#@organisation : CINERI
#@Website: www.cineri.sn 
#@contact : babacar.diop@cineri.sn
#@Github: DiopBabacarEdu
#@This code has been proposed as tutorial part of the CINERI users
#########################################

from netCDF4 import Dataset
import numpy as np
import matplotlib.pyplot as plt
from mpl_toolkits.basemap import Basemap

#######################################
#	Global variables
#
File_Of_Coord='tie_geo_coordinates.nc'
File_Of_Meteo='tie_meteo.nc'
HUM = 'humidity'
TOZON = 'total_ozone'
TCWV = 'total_columnar_water_vapour'
SEA_PRESSURE = 'sea_level_pressure'
#######################################

def extract_coords(coord_file):
        nc_c = coord_file                # Filename 'tie_geo_coordinates.nc'
        nc_coord = Dataset(nc_c, 'r')    # Dataset for coordinates
        lats = nc_coord.variables['latitude'][:] 
        lons = nc_coord.variables['longitude'][:]
        return lats, lons

def extract_param(param_file,_param):
        nc_m = param_file                # Filename 'tie_geo_meteo.nc' 
        nc_param = Dataset(nc_m, 'r')    # Dataset for meteo data
        param_to_plot = nc_param.variables[_param][:]
        return param_to_plot


root_grp = Dataset('satelite_sn.nc', 'w', format='NETCDF4')
root_grp.description = 'Satelite data for Senegal \n From https://peps.cnes.fr'

# dimensions
root_grp.createDimension('tie_row',4091)
root_grp.createDimension('tie_column',77)

# variables
latitudes = root_grp.createVariable('latitude', 'f4', ('tie_row', 'tie_column',))
longitudes = root_grp.createVariable('longitude', 'f4', ('tie_row', 'tie_column',))
total_ozone = root_grp.createVariable('total_ozone', 'f4', ('tie_row', 'tie_column',))
humidity = root_grp.createVariable('humidity', 'f4', ('tie_row', 'tie_column',))
total_columnar_water_vapour = root_grp.createVariable('total_columnar_water_vapour', 'f4', ('tie_row', 'tie_column',))
sea_level_pressure = root_grp.createVariable('sea_level_pressure', 'f4', ('tie_row', 'tie_column',))

# data
latitudes[:],longitudes[:] = extract_coords(File_Of_Coord)
total_ozone[:] = extract_param(File_Of_Meteo,TOZON)
humidity[:] = extract_param(File_Of_Meteo,HUM)
total_columnar_water_vapour[:] = extract_param(File_Of_Meteo,TCWV)
sea_level_pressure[:] = extract_param(File_Of_Meteo,SEA_PRESSURE)

# group
# my_grp = root_grp.createGroup('my_group')

root_grp.close()
