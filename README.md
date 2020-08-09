# PRISM
Precipitation-elevation Regression on Independent Slope Models 

####################################################################################
!!!!!! First of all TopoToolbox must downloaded and added to MATLAB file path !!!!!!
####################################################################################

This function is provided for spatial interpolation of precipitation with monthly and annual time scales based on PRISM approach with distance, elevation, topographic FACET and coastal proximity weights taken into account. 
The Exe_PRISM_M_St and Exe_PRISM_Yr_St scripts yield model error based on a few statistical indices and leave-one-out-cross-validation method, on monthly and annual time steps, respectively. Inputs including stations time series and DEMs are also provided along with the codes. Notice the format of station time series and coordinates. The parameters can be calibrated with the help of any optimization algorithm.
The Exe_PRISM_M_All and Exe_PRISM_Yr_All are able to estimate preciptation in each cell of input DEM on monthly and annual time steps, respectively. Each month or year estimated precipitaion can be accessed by a matrix of values and can be then converted to a tif file.  
