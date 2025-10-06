.. _IntroToSOLWEIGICUC12:

Thermal Comfort - Introduction to SOLWEIG, URock and SpatialTC
=========================================

Introduction
------------
This tutorial is divided into three parts. In part 1 you will use the 
SOLWEIG model to calculate mean radiant temperature. In part 2 you will use
the URock model to estimate wind fields and in part 3 you will combine your 
results from part 1 and part 2 into the Physiological Equivalent Temperature
index.

Objectives
----------

To introduce SOLWEIG, URock and SpatialTC and how to run the models within `UMEP (Urban
Multi-scale Environmental Predictor) <http://umep-docs.readthedocs.io>`__. 

Help with Abbreviations can be found `here <http://umep-docs.readthedocs.io/en/latest/Abbreviations.html>`__.

Steps
~~~~~

#. Different kinds of input data, that are needed to
   run the models, will be generated.
#. How to run the models
#. How to examine the model output
#. Add additional information (vegetation and ground cover) to improve
   the model outcome and to examine the effect of climate sensitive
   design

Initial Practical steps
-----------------------

UMEP is a Python plugin used in conjunction with
`QGIS <http://www.qgis.org>`__. To install the software and the UMEP
plugin see the `getting
started <http://umep-docs.readthedocs.io/en/latest/Getting_Started.html>`__
section in the UMEP manual.

As UMEP is under constant development, some documentation may be missing
and/or there may be instability. Please report any issues or suggestions
to our `repository <https://github.com/UMEP-dev/UMEP>`__.

Data for this exercise
~~~~~~~~~~~~~~~~~~~~~~

The dataset for this tutorial can be downloaded from our repository
`here <https://github.com/UMEP-dev/UMEP-Tutorials/blob/master/docs/source/data/ICUC12_TC.zip>`__.

-  Download, extract and add the raster layers (DSM, CDSM, DEM and land
   cover) from the **input_dataset folder** into a new QGIS session (see
   below).

   -  Create a new project
   -  Examine the geodata by adding the layers (*dsm_rotterdam*,
      *cdsm_rotterdam*, *dem_rotterdam* and *lc_rotterdam*) to your project (***Layer
      > Add Layer > Add Raster Layer** or drag and drop).

-  Coordinate system of the grids is Amersfoort / RD New (EPSG:28992). If you
   look at the lower right hand side you can see the CRS used in the
   current QGIS project.
-  Have a look at `DailyShading` on how you can visualize DSM and CDSM at the same time.
-  Examine the different datasets before you move on.
-  To add a legend to the **land cover** raster you can load
   **landcoverstyle.qml** found in the test dataset. Right click on the
   land cover (*Properties -> Style (lower left) -> Load Style*).

Part 1: SOlar and LongWave Environmental Irradiance Geometry Model (SOLWEIG)
-----------------------------------------------------------------------------

In this part of the tutorial you will use the **SOlar and LongWave Environmental
Irradiance Geometry model (SOLWEIG)** model to estimate the mean radiant
temperature (T\ :sub:`mrt`).

SOLWEIG is a model that simulates spatial variations of 3D radiation
fluxes and T\ :sub:`mrt` in complex urban settings. It is also able
to model spatial variations of shadow patterns. T\ :sub:`mrt` is one of
the key meteorological variables governing human energy balance and the
thermal comfort of people. It is derived from summing all the radiative
(shortwave and longwave) fluxes (both direct and reflected) to which the
human body is exposed. In SOLWEIG, T\ :sub:`mrt` is derived by modelling
shortwave and longwave radiation fluxes in six directions (upward,
downward and from the four cardinal points) and angular factors.

The model requires **meteorological** forcing data (global shortwave
radiation (K\ :sub:`down`), air temperature (T\ :sub:`a`), relative humidity (RH)),
urban geometry (DSMs), and geographic information (latitude, longitude
and elevation). To determine T\ :sub:`mrt`, continuous maps of sky view
factors are required. Both vegetation and ground cover information can
be added to increase the accuracy of the model output. Below, 
a schematic flowchart of SOLWEIG in shown. The `full
manual <http://umep-docs.readthedocs.io/en/latest/OtherManuals/SOLWEIG.html>`__ provides more
detail.

.. figure:: /images/SOLWEIG_flowchart.png
   :alt:  Overview of SOLWEIG
   :width: 100%

   Overview of SOLWEIG

SOLWEIG Model Inputs
--------------------

Details of the model inputs and outputs are provided in the `SOLWEIG
manual <http://umep-docs.readthedocs.io/en/latest/OtherManuals/SOLWEIG.html>`__. As this tutorial is
concerned with a **simple application** only the most critical
parameters are used. Many other parameters can be modified to more
appropriate values, if applicable. The table below provides an overview
of the parameters that can be modified in the Simple application of
SOLWEIG.

Data use and type abbreviations:
R: required, O: Optional, N : not needed, 
S: Spatial, M: Meteorological, 

.. list-table:: Input data and parameters
   :widths: 30 30 5 5 30

   * - **Data**
     - **Definition**
     - **Use**
     - **Type**
     - **Description**
   * - Ground and building digital surface model (DSM)
     - High resolution surface model of ground and building heights
     - R
     - S
     - Given in metres above sea level (m asl)
   * - Digital elevation model (DEM) 
     - High resolution surface model of the ground 
     - R\* 
     - S 
     - R\* if land cover is absent to identify buildings. Given in m asl. Must be same resolution as the DSM.
   * - Digital canopy surface model (CDSM) 
     - High resolution surface model of 3D vegetation 
     - O 
     - S
     - Given in metres above ground level (m agl). Must be same resolution as the DSM.
   * - Digital trunk zone surface model (TDSM) 
     - High resolution surface model of trunk zone heights (underneath tree canopy) 
     - O 
     - S 
     - Given in m agl. Must be same resolution as the DSM.
   * - Land (ground) cover information (LC) 
     - High resolution surface model of ground cover 
     - O 
     - S 
     - Must be same resolution as the DSM. Five different ground covers are currently available (building, paved, grass, bare soil and water)
   * - UMEP formatted meteorological data 
     - Meteorological data from one nearby observation station, preferably at 1-2 m above ground. 
     - R 
     - M 
     - Any time resolution can be given.
   * - Latitude (°) 
     - Solar related calculations 
     - R 
     - O
     - Obtained from the ground and building DSM coordinate system
   * - Longitude (°) 
     - Solar related calculations 
     - R
     - O
     - Obtained from the ground and building DSM coordinate system
   * - `UTC (h) <https://en.wikipedia.org/wiki/Coordinated_Universal_Time>`__
     - Time zone 
     - R
     - O 
     - Influences solar related calculations. Set in the interface of the model.
   * - Human exposure parameters 
     - Absorption of radiation and posture 
     - R 
     - O 
     - Set in the interface of the model.
   * - Environmental parameters
     - e.g. albedos and emissivites of surrounding urban fabrics 
     - R 
     - O 
     - Set in the interface of the model.
	 

Meterological input data should be in UMEP format. You can use the
`Meterological Preprocessor <http://umep-docs.readthedocs.io/en/latest/pre-processor/Meteorological%20Data%20MetPreprocessor.html>`__
to prepare your input data. It is also possible to use the plugin for a single point in time. 

Required meteorological data to calculate T\ :sub:`mrt` are: 

#. Air temperature (°C)
#. Relative humidity (%)
#. Incoming global shortwave radiation (W m\ :sup:`2`)

The model performance will increase if diffuse and direct beam solar radiation is 
available but the model can also calculate these variables. 


How to Run SOLWEIG from the UMEP-plugin
---------------------------------------

#. Open SOLWEIG in the Processing Toolbox from *UMEP -> Processor -> Outdoor Thermal Comfort: 
   SOLWEIG v2025a*.

   -  You will make use of a test dataset from observations for an area in the central parts of Gothenburg, Sweden.

    .. figure:: /images/ICUC12/SOLWEIG_GUI.PNG
       :alt:  None
       :width: 100%
       :align: center

       Dialog for the SOLWEIG model (click on figure for larger image)

#. To be able to run the model, some additional spatial datasets needs to
   be created.

   -  Close the SOLWEIG plugin and open UMEP from the processing toolbox then 
      *Pre-Processor -> Urban geometry: Sky View Factor*.
   -  To run SOLWEIG various sky view factor (SVF) maps for both
      vegetation and buildings must be created (see `Lindberg and
      Grimmond
      (2011) <http://link.springer.com/article/10.1007/s00704-010-0382-8>`__
      for details).
   -  You can create all SVFs needed (vegetation and buildings) at the
      same time. Use the settings as shown below. Use an appropriate
      output folder for your computer. 
	  
    .. figure:: /images/ThermalComfort/SVF.PNG
       :alt:  None
       :width: 487px
       :align: center
       
       Settings for the SkyViewFactorCalculator.
      
   -  When the calculation is done, a map will appear in the map canvas.
      This is the 'total' SVF i.e., including both buildings and
      vegetation. Examine the dataset.
   -  Where are the highest and lowest values found?
   -  If you look in your output folder you will find a zip-file containing all the
      necessary SVF maps needed to run the SOLWEIG-model.

#. Another pre-processing plugin is needed to create the building wall
   heights and aspect. Open UMEP from the processing toolbox again and then 
   *Pre-Processor -> Urban geometry: Wall height and aspect* and use the settings as shown below. QGIS scales loaded rasters by a *cumulative count out* approach 
   (98%). As the height and aspect layers are filled with zeros where no wall are present it might appear as if there is no walls identified. Rescale your 
   results to see the walls identified (*Layer Properties > Symbology*).
   
    .. figure:: /images/ThermalComfort/wallHeightAspect.PNG
       :alt:  None
       :width: 505px
       :align: center
       
       Settings for the Wall height and aspect plugin.

#. Re-open the SOLWEIG plugin and use the settings shown below. 
   You will use vegetation (cdsm_rotterdam.tif) and ground cover (lc_rotterdam.tif). 
   As no TDSM exists we estimate it by using 25% of the canopy height. Leave the tranmissivity as 3%.
   You will use meteorological forcing data from `KNMI (Royal Netherlands Meteorological Institute) <https://daggegevens.knmi.nl/klimatologie/uurgegevens>`__.
   This data is in UTC 0. The solar radiation is global and therefore we have to tick "Estimate diffuse
   and direct shortwave radiation from global radiation". Remember to tick "Save necessary raster(s) for the TreePlanter and Spatial TC tools". 
   Specify an output folder that you can easily find. Click **Run**. 
   
    .. figure:: /images/ThermalComfort/SOLWEIG1.PNG
       :alt:  None
       :width: 100%
       :align: center
       
       The settings for your first SOLWEIG run (part 1) (click on figure for larger image).

    .. figure:: /images/ThermalComfort/SOLWEIG2.PNG
       :alt:  None
       :width: 100%
       :align: center
       
       The settings for your first SOLWEIG run (part 2) (click on figure for larger image).

#. Add the Tmrt_average.tif from your output folder and examine it (Average T\ :sub:`mrt` (°C). What is the main
   driver to the spatial variations in T\ :sub:`mrt`?
#. Now add the Tmrt_2025_172_1200D.tif from the output folder. This file will be used later in the tutorial.

Part 2: Urban Wind Field - Introduction to URock
------------------------------------------------

In this part you will make use the model **URock** to estimate wind fields in an urban setting using a semi-empirical wind model based on Röckle (1990).

URock can be used to calculate the 3D wind field of an urban area using information about the wind (at least speed and direction at a given height) and geographical data describing the area of interest (building and vegetation footprint and height). Two main stages are used: wind field initialization and wind field balance. For a detailed description of the model see, `Bernard et al. (2023) <https://egusphere.copernicus.org/preprints/2023/egusphere-2023-354/>`__.

The model requires **meteorological** forcing data (wind speed and direction) and geometry information for buildings and trees. 

Steps
~~~~~

#. Produce relevant input data needed to run the model using URock Prepare.
#. Run the model
#. Examine the model output using URock Analyzer

Data for this exercise
~~~~~~~~~~~~~~~~~~~~~~

We will use the DSM, CDSM and DEM that we used to force SOLWEIG. We, however, have to add another file; **BuildingsRotterdam.gpkg** that should also be in your input dataset.

To run **URock**, you need a building vector dataset including building height attributes and/or a vegetation vector layer including height and some additional optional info such as attenuation factor (see below). Here, you will make use of raster DSM, DEM and CDSM to generate information for URock.

URock Prepare
-------------
#. Open **URock Prepare** from the **Pre-Processing** section in **UMEP for Processing** found in the **Processing Toolbox**. 
#. Use the settings shown below except for the output where you maybe need to specify a specific location on your computer where you have read and write access.

    .. figure:: /images/ThermalComfort/URock_prepare.PNG
       :alt:  None
       :width: 100%
       :align: center
       
       Dialog for the settings in URock prepare

   If you have a dataset with points including tree location and attributes with heights and/or ratio information, this can also be used to generate vegetation data. Now click **Run** and two new files that are ready to use in URock will be created. The current version of URock does not include ground topography (hopefully available in upcoming versions). The DEM is used to derive building heights comparing the DSM and the DEM.

URock
-----
#. Open the URock interface (*UMEP > Processing > Urban Wind Field: URock*). Here you can make a lot of settings (divided into two figures). 
   We will use a wind speed of 4 m/s with a wind direction set to 110 degrees. To increase the speed of the calculations we will use 4 meter horizontal and vertical resolutions.
   When all the settings are made, click **Run**.

    .. figure:: /images/ThermalComfort/URock1.PNG
       :alt:  None
       :width: 100%
       :align: center
       
       Dialog for the settings in URock (part 1)
       
    .. figure:: /images/ThermalComfort/URock2.PNG
       :alt:  None
       :width: 100%
       :align: center
       
       Dialog for the settings in URock (part 2)


The computation will take some time depending on your computer standard. During the computation, you can follow the steps in the log-window in the URock-interface. A large part of the computation time is related to creation of all the different zones around buildings and vegetation. If you want an even more detailed picture of the process, open the Python Console in QGIS. However, this will somehow slow down the computational process. When the computation is finished, the tool will load the raster windspeed and the vector points at 1.5 meter above ground level.


Part 3: Thermal Comfort - Spatial Thermal Comfort
-------------------------------------------------

In this last step of the tutorial you will use the SpatialTC tool to produce maps of thermal comfort indices using outputs from the two previous steps (SOLWEIG and URock). 

The two previous modeling steps provided us with Tmrt (SOLWEIG) and wind fields (URock). These outputs are combined in the **SpatialTC**-tool to generate raster maps on thermal indices such as PET, UTCI and COMFA.


Produce map of Universal Thermal Climate Index (UTCI) with SpatialTC
---------------------------------

You need to specify two rasters: one of the mean radiant temperature that has been produced by SOLWEIG (**Tmrt_2025_172_1200D.tif**) and one with the pedestrian wind speed produced by URock (**urock_outputWS.tif**).

  - Load the *Tmrt_2025_172_1200D.tif* into your QGIS project if you have not done this already. This file can be found in your outout folder form the previous SOLWEG-run. Do not change the file name or its location as the info in the name will be used to identify the meteorological information that is needed to calcualte PET.

  - Last you need to select the thermal comfort index to map (UTCI for this tutorial). The Advanced parameters describing the person to consider for the comfort index (PET or COMFA) can also be defined but the default values are kept for this tutorial. Then click **Run**. 

    .. figure:: /images/ThermalComfort/SpatialTC.PNG
       :alt:  None
       :width: 100%
       :align: center
       
       Settings for the Spatial TC tool.
    
When the computation is finished, you should have a map as shown below.

    .. figure:: /images/ICUC12/UTCI.png
       :alt:  None
       :width: 100%
       :align: center
       
       Spatial variations of UTCI produced with the Spatial TC tool.

Try to produce output maps of Physiological Equivalent Temperature (PET) and COMfort FormulA (COMFA).

Tutorial finished.

