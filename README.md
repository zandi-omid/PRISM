# PRISM
Precipitation-elevation Regression on Independent Slope Models 

This function is provided for spatial interpolation of precipitation with monthly and annual time scales based on PRISM approach with distance, elevation, topographic FACET and coastal proximity weights taken into account. The codes are written in MATLAB computer programming language.
The Exe_PRISM_M_St and Exe_PRISM_Yr_St scripts yield model error based on a few statistical indices and leave-one-out-cross-validation method, on monthly and annual time steps, respectively. Inputs including stations time series and DEMs are also provided along with the codes. Notice the format of station time series and coordinates.  The parameters can be calibrated with the help of any optimization algorithm.
The Exe_PRISM_M_All and Exe_PRISM_Yr_All are able to estimate preciptation in each cell of the input DEM on monthly and annual time steps, respectively. In other words the main output of the model which is the precipitation spatial dataset is generated by these two scripts. Each month or year estimated precipitaion can be accessed by a matrix of values and can be then converted to a tif file. The Exe_PRISM_M_st and Exe_PRISM_Yr_st function perform Leave one out cross validation to evaluate the accuracy of the developed model.
% This functions interpolate precipitation using DEM and station data. The
% algorithm is based on PRISM model. All of the weights are enabled except
% vertical layer and topographic positions. Cluster weights can be enabled
% manually by uncommenting appropriate lines of code and multiplying
% cluster weighting to the final weights.
% it works with each station annual precipitation and
% interpolate precipitaion in stations cell only to evaluate the accuracy
% of model.
% %%%%%%%%%%%%%%%%%%% Adding Stations Information %%%%%%%%%%%%%%%%%%%%%%%
% #### InputStInformation : A text file containing information in below
% order: ($ shows column)
% $1 --> code
% $2 --> X degree (Longitude)
% $3 --> Y degree (Latitude)
% $4 --> Year
% $5 --> Water Year
% $6 --> Day
% $7 --> Month
% $8 --> Precipitation
% It must be entered as a string. .e.g 'input_fullcode_st_coord.txt'
% %%%%%%%%%%%%%%%% Adding Refrence DEMs and Grided data %%%%%%%%%%%%%%%%%
% #### LargeRefDem : Reference DEM which is aggregated from 90m to 900m by
% GaussianAggregatorNew function. This DEM must be much larger than
% watershed. It should be entered as a string and tif format. .e.g
% 'LargeDem.tif'

% #### WithExtentDem : With extent DEM which is attained by clipping large
% reference DEM with Extract by Mask tool in ArcGIS and watershed
% shapefile. This DEM contains within and out of watershed pixels. It
% should be entered as a string and tif format. .e.g 'WithExtentDem.tif'

% #### WithinDem : Within DEM which is attained by clipping WithExtentDem
% with Extract by Mask tool in ArcGIS and watershed shapefile. This Dem
% only contains within watershed pixels and out of watershed pixels are
% zero. Note that the row and column numbers of both WithExtent and Within
% DEMs must be equal.

% #### PreparedFacetGrid : In case of entering the value of
% FacetGridGenerator variable as 1 this grid is used. This variable must be
% entered as a string .e.g. 'Facet19_5_90_5_2.tif' and note that the format
% must be tif.

% #### PreparedIndex3DGrid : In case of entering the value of
% Index3DGridGenerator variable as 1 this grid is used. This variable must
% be entered as a string .e.g. '3DIndex_75_250_20.tif' and note that the
% format must be tif.

% %%%%%%%%%%%%%%%%%%%%%%%% Setting Parameters %%%%%%%%%%%%%%%%%%%%%%%%%

% #### StartYear : Starting year of simulation. The observed precipitation
% of stations from this year up to the last available year is chosen for
% simulation.

% #### RadiusS : Radius of first level of smoothing by applying a gaussian
% low-pass filter. The S at the end of this parameter addresses the first
% level of smoothing. .e.g. 19 means 9 cells ( (19 - 1) / 2 ) around each
% target cell. The resolution is assumed to be roughly 800m  which means
% 9*800 = 7.2km smoothing radius.

% #### SigmaS : Standard deviation of gaussian low-pass filter for first
% level of smoothing. The resulting DEM after applying the gaussian
% low-pass filter is called PRECIPITATION DEM.

% #### FacetGridGenerator : Asking whether to generate FACET grid or to use
% previously prepared FACET grids. 1 means yes generate and 0 means no and
% use prepared FACET grids.

% #### Index3DGridGenerator : Asking whether to generate 3DIndex grid or to
% use previously prepared 3DIndex grids. 1 means yes generate and 0 means
% no and use prepared FACET grids.

% #### WavelengthSS : Smoothig PRECIPITATION DEM again with 4 DEM smoothing
% levels range from DEM resolution to 36km. The filtering wavelengths
% windows are : 1*1 (DEM resolution) , 30*30 (15*800~12km) , 60*60 (~24km)
% , 90*90(~36km). The SS at the end of this parameter addresses the second
% level of smoothing.

% #### SigmaSS : Standard deviation of gaussian low-pass filter for second
% level of smoothing.

% #### TopographicFacetIteration : Number of times The 16 rules for
% generating FACET grid from the aspect is applied.

% #### SearchRadiiRegression : Searching radius to find related stations
% for estimating precipitation of each cell.

% #### MinSt : Minimum number of stations required for performing
% regression.

% #### SlopeMin : Minimum allowable regression slope.

% #### SlopeMax : Maximum allowable regression slope.

% #### ShowingRegressionDetailsAndGraphs : Asking whether to show each
% iteration regression results and graphs. 1 means yes show and 0 means no
% don't show.

% #### DeltaFThreshold : The minimum absolute orientation difference
% between target cell and related station. Maximum possible difference is 4
% compass points.

% #### FacetWeightingExponent : Facet weighting exponent.

% #### SearchRadiiMinETHGrid : Search radius to find the minimum elevation
% in generating effective terrain height grid. .e.g 22 km considering each
% cell 800m this parameter must be entered 28 (28*800m ~ 22km).

% #### AveragingRadiiETHGrid1 : Spatially averaging to produce base dem.
% .e.g. 11 km considering each cell 800m results in 14 cells but it must be
% multiplied by 2.

% #### AveragingRadiiETHGrid2 : Again averaging to reach a smoothed grid.
% .e.g. 11 km considering each cell 800m results in 14 cells but it must be
% multiplied by 2.

% #### H2 : 2D threshold in calculating 3D index.

% #### H3 : 3D threshold in calculating 3D index.

% #### SearchRadii3D : Searching radius in calculating the areal 3d index.

% #### DistanceWeightingExponet : Distance Weighting Exponet.

% #### MinRadiiInflueceDistWeighting : Minimum radius of influence used in
% distance weighting function. It is recomended to be equal to half of
% RadiusS.

% #### Fd : Importance factor for distance weighting.

% #### ElevWeightingExponent : Elevation Weighting Exponent.

% #### MinElevDiff : Minimum elevation difference in calculating elevation
% weighting. It must be entered in meter.

% #### MaxElevDiff : Minimum elevation difference in calculating elevation
% weighting. It must be entered in meter.

% #### Fz : Importance factor for elevation weighting.

% #### Tup : Terrain penalty for upslope in calculating coastal proximity.

% #### Tdown : Terrain penalty for downslope in calculating coastal
% proximity.

% #### PathPenaltyEachPixel : Path length penalty in calculating coastal
% proximity.

% #### MaxProximityDiff : Maximum coastal proximity index difference
% between target cell and related station above which station weight is
% equall to zero.

% #### MinProximityDiff : Minimum coastal proximity index difference
% between target cell and related station below which weight is equall to
% one.

% #### CoastalProximityWeightingExponent : Coastal Proximity Weighting
% Exponent.

% #### PercentRadiiInfluence : Horizontal separation distance used in
% clustering. It must be entered in km. Note that it is used unless cluster
% weighting is enabled. It can be enabled manually by uncommenting
% appropriate lines of code and multiplying cluster weighting to the final
% weights.

% #### ElevPrecision : The elevation precision in km used in clustering. It
% must be entered in meter. Note that it is used unless cluster weighting
% is enabled.  It can be enabled manually by uncommenting appropriate lines
% of code and multiplying cluster weighting to the final weights.

####################################################################################

!!!!!! First of all TopoToolbox must downloaded and added to MATLAB file path !!!!!!
It can be downloaded from https://github.com/wschwanghart/topotoolbox.git.
####################################################################################
