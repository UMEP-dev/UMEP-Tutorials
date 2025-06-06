.. _IntroToSOLWEIG:

Thermal Comfort - Introduction to SOLWEIG, URock and SpatialTC
=========================================

.. note:: Click `here <IntroductionToSOLWEIGNYC>` to find the same tutorial as below but for data from New York City, US.

Introduction
------------

In this tutorial you will use a model **SOlar and LongWave Environmental
Irradiance Geometry model (SOLWEIG)** to estimate the mean radiant
temperature (T\ :sub:`mrt`).

SOLWEIG is a model that simulates spatial variations of 3D radiation
fluxes and the T\ :sub:`mrt` in complex urban settings. It is also able
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

UMEP is a python plugin used in conjunction with
`QGIS <http://www.qgis.org>`__. To install the software and the UMEP
plugin see the `getting
started <http://umep-docs.readthedocs.io/en/latest/Getting_Started.html>`__
section in the UMEP manual.

As UMEP is under constant development, some documentation may be missing
and/or there may be instability. Please report any issues or suggestions
to our `repository <https://github.com/UMEP-dev/UMEP>`__.

Data for this exercise
~~~~~~~~~~~~~~~~~~~~~~

The UMEP tutorial datasets can be downloaded from our here repository
`here <https://github.com/Urban-Meteorology-Reading/Urban-Meteorology-Reading.github.io/tree/master/other%20files/Goteborg_SWEREF99_1200.zip>`__.

-  Download, extract and add the raster layers (DSM, CDSM, DEM and land
   cover) from the **DataForTutorials folder** into a new QGIS session (see
   below).

   -  Create a new project
   -  Examine the geodata by adding the layers (*dsm_wijkpark*,
      *cdsm_wijkpark*, *dem_wijkpark* and *lc_wijkpark*) to your project (***Layer
      > Add Layer > Add Raster Layer** or drag and drop).

-  Coordinate system of the grids is Amersfoort / RD New (EPSG:28992). If you
   look at the lower right hand side you can see the CRS used in the
   current QGIS project.
-  Have a look at `DailyShading` on how you can visualize DSM and CDSM at the same time.
-  Examine the different datasets before you move on.
-  To add a legend to the **land cover** raster you can load
   **landcoverstyle.qml** found in the test dataset. Right click on the
   land cover (*Properties -> Style (lower left) -> Load Style*).

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
to prepare your input data. It is also possible use the plugin for a single point in time. 

Required meteorological data to calculate T\ :sub:`mrt` is: 

#. Air temperature (°C)
#. Relative humidity (%)
#. Incoming shortwave radiation (W m\ :sup:`2`)

The model performance will increase if diffuse and direct beam solar radiation is 
available but the mdoel can also calculate these variables. 

**If Point Of Interest (POIs) is used, wind speed (m/s) is also required** (see below).


How to Run SOLWEIG from the UMEP-plugin
---------------------------------------

#. Open SOLWEIG from *UMEP -> Processor -> Outdoor Thermal Comfort ->
   Mean radiant temperature (SOLWEIG)*.

   -  You will make use of a test dataset from observations for Gothenburg, Sweden.

    .. figure:: /images/SOLWEIG_Interface.png
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
	  
    .. figure:: /images/SOLWEIG_Svf_solweig.png
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

#. Another preprocessing plugin is needed to create the building wall
   heights and aspect. Open UMEP from the processing toolbox again and then 
   *Pre-Processor -> Urban geometry: Wall height and aspect* and use the settings as shown below. QGIS scales loaded rasters by a *cumulative count out* approach 
   (98%). As the height and aspect layers are filled with zeros where no wall are present it might appear as if there is no walls identified. Rescale your 
   results to see the walls identified (*Layer Properties > Symbology*).
   
    .. figure:: /images/SOLWEIG_wallgeight_solweig.png
       :alt:  None
       :width: 505px
       :align: center
       
       Settings for the Wall height and aspect plugin.

#. Re-open the SOLWEIG plugin and use the settings shown below. You will
   use the GUI to set one point in time (i.e. a summer hour in
   Gothenburg, Sweden) hence, no input meteorological file is needed for
   now. No information on vegetation or ground cover is added for this
   first try. Click **Run**. 
   
    .. figure:: /images/SOLWEIG_Tmrt1_solweig.png
       :alt:  None
       :width: 100%
       :align: center
       
       The settings for your first SOLWEIG run (click on figure for larger image).
      
#. Examine the output (Average T\ :sub:`mrt` (°C). What is the main
   driver to the spatial variations in T\ :sub:`mrt`?
#. Add 3D vegetation information by ticking *Use vegetation scheme
   (Lindberg, Grimmond 2011)* and add **CDSM_Krbig** as the *Vegetation
   Canopy DSM*. As no TDSM exists we estimate it by using 25% of the
   canopy height. Leave the tranmissivity as 3%. Tick *Save generated
   Trunk Zone DSM* (a tif file, **TDSM.tif**, will be generated in the
   specified output folder and used in a later section: **Climate
   sensitive planning**). Also tick *Save generated building grid* as
   this will be needed later in this tutorial. Leave the other settings
   as before (Step 4) except for changing your output directory,
   otherwise results from your first run will be overwritten. Run the
   model again and compare the result with your first run.
#. Add your last spatial dataset, the **land cover** grid by ticking
   *Use land cover scheme (Lindberg et al. 2016)*. Run and compare the
   result again with the previous runs.

Urban Wind Field - Introduction to URock
========================================

Introduction
------------

.. note:: The tools in this tutorial are found in **UMEP for Processing**. The tool can be found from the menu (*Plugins>Manage and Install Plugins*).

In this tutorial you will make use the model **URock** to estimate wind fields in an urban setting using a semi-empirical wind model based on Röckle (1990).

URock can be used to calculate the 3D wind field of an urban area using information about the wind (at least speed and direction at a given height) and geographical data describing the area of interest (building and vegetation footprint and height). Two main stages are used: wind field initialization and wind field balance. For a detailed description of the model see, `Bernard et al. (2023) <https://egusphere.copernicus.org/preprints/2023/egusphere-2023-354/>`__.

The model requires **meteorological** forcing data (wind speed and direction) and geometry information for buildings and trees. 

Steps
~~~~~

#. Produce relevant input data needed to run the model using URock Prepare.
#. Run the model
#. Examine the model output using URock Analyzer

Initial Practical steps
-----------------------

**UMEP for Processing** is a Python plugin used in conjunction with
`QGIS <http://www.qgis.org>`__. To install the software and the UMEP
plugin (if not already installed), see the `getting started <http://umep-docs.readthedocs.io/en/latest/Getting_Started.html>`__
section in the UMEP manual.

As **UMEP for Processing** is under constant development, some documentation may be missing
and/or there may be instability. Please report any issues or suggestions
to our `repository <https://github.com/UMEP-dev/UMEP>`__.

Data for this exercise
~~~~~~~~~~~~~~~~~~~~~~

The UMEP tutorial datasets can be downloaded from our here repository
`here <https://github.com/Urban-Meteorology-Reading/Urban-Meteorology-Reading.github.io/blob/master/other%20files/Annedal_EPSG3006.zip>`__.

-  Download, extract and add the raster layers (DSM, CDSM, DEM), the building polygon layer and the line profile layer into a new QGIS session. Coordinate system of the grids is Sweref99 TM (EPSG:3006). 

To run **URock**, you need a building vector dataset including building height attributes and/or a vegetation vector layer including height and some additional optional info such as attenuation factor (see below). Here, you will make use of raster DSM, DEM and CDSM to generate information for URock.

URock Prepare
-------------
#. Open **URock Prepare** from the **Pre-Processing** section in **UMEP for Processing** found in the **Processing Toolbox**. 
#. Use the settings shown below except for the output where you maybe need to specify a specific location on your computer where you have read and write access.

    .. figure:: /images/urockprepare.jpg
       :alt:  None
       :width: 100%
       :align: center
       
       Dialog for the settings in URock (part1)

   If you have a dataset with points including tree location and attributes with heights and/or ratio information, this can also be used to generate vegetation data. Now click **Run** and two new files that are ready to use in URock will be created. The current version of URock does not include ground topography (hopefully available in upcoming versions). The DEM is used to derive building heights comparing the DSM and the DEM.

URock
-----

#. Open the URock interface (*UMEP > Processing > Urban Wind Field: URock*). Here you can make a lot of settings (divided into two figures). In your first run only buildings will be included and affecting the wind pattern:

    .. figure:: /images/urock1.jpg
       :alt:  None
       :width: 100%
       :align: center
       
       Dialog for the settings in URock (part 1)
       
    .. figure:: /images/urock2.jpg
       :alt:  None
       :width: 100%
       :align: center
       
       Dialog for the settings in URock (part1)


#. When all the settings are made, click **Run**.

The computation will take some time depending on your computer standard. During the computation, you can follow the steps in the log-window in the URock-interface. A large part of the computation time is related to creation of all the different zones around buildings and vegetation. If you want an even more detailed picture of the process, open the Python Console in QGIS. However, this will somehow slow down the computational process. When the computation is finished, the tool will load the raster windspeed and the vector points at 1.5 meter above ground level.


Tutorial finished.

Thermal Comfort - Spatial Thermal Comfort
=========================================

Introduction
------------

In this last step of the tutorial you will use the SpatialTC tool to produce maps of thermal comfort indices using outputs from the two previous steps (SOLWEIG and URock). 

The two previous modeling steps provided us with Tmrt (SOLWEIG) and wind fields (URock). These outputs are combined in the **SpatialTC**-tool to generate raster maps on thermal indices such as PET, UTCI and COMFA.


Produce map of Physiological Equivalent Temperature (PET) with SpatialTC
---------------------------------

You need to specify two rasters: one of the mean radiant temperature that has been produced by SOLWEIG and one with the pedestrian wind speed produced by URock.

  - Load the *Tmrt_1983_173_1600D.tif* into your QGIS project. This file can be found in your outout folder form the previous SOLWEG-run. Do not change the file name as the info in the name will be used to identify the meteorological information that is needed to calcualte PET.

  - Last you need to select the thermal comfort index to map (PET for this tutorial). The Advanced parameters describing the person to consider for the comfort index can also be defined but the default values are kept for this tutorial. Then click **Run**. 

    .. figure:: /images/spatialtc.png
       :alt:  None
       :width: 100%
       :align: center
       
       Settings for the Spatial TC tool.
    
When the computation is finished, you should have a map as shown below.

    .. figure:: /images/spatialtc_result.jpg
       :alt:  None
       :width: 100%
       :align: center
       
       Spatial variations of PET produced with the Spatial TC tool.

Try to produce output maps of Universal Thermal Climate Index (UTCI) and COMfort FormulA (COMFA).

Tutorial finished.

