# Processing Satellite Data using NetCDF Python library
## Basics

This git provides some scripts to preprocess satelite Copernicus 
data extracted from the Sentinel Hub (https://peps.cnes.fr/rocket/).
The initial format of the data ius in NetCDF, a common format for climate and geo related data.
Data contains several parameters mapped into a grid with several layers.

## Parameters
- DEM (Digital Elevation Map)
- Climate parameters (Temperature, Humidity, Ozone, etc)
- Technical parameters (Orbit, colors, optical informations, etc.)

## Possible applications
- Radio planning in wide areas
- Daily mapping of climate parameters 
- Sensor coverage estimation
- Combining with other data sources for learning and prediction
- Simulation of complex systems (flooding, dynamic evolution, cellular automata, etc.)

## Steps for processing and plotting

- Connect to www.peps.cnes.fr 
- Make a research based on location
- Download the relevant products 
- Apply this python code to extract data and plot
