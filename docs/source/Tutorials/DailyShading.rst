.. _DailyShading:

Introducing UMEP - Daily shading
================================

Introduction
------------

In this exercise you will be introduced to QGIS and UMEP and some of its capabilities. 
You will apply a shadow casting model in `Gothenburg,
Sweden <https://en.wikipedia.org/wiki/Gothenburg>`__ to
investigate daliy shading patterns for pedestrians and how solar access varies with seasons and through vegetation.

Initial Practical steps
-----------------------

- If **QGIS** and **UMEP** is not on your computer you will `need to install <http://umep-docs.readthedocs.io/en/latest/Getting_Started.html>`__.

- When installed, **UMEP** can be found in the menu bar in QGIS.


Data for this Tutorial
~~~~~~~~~~~~~~~~~~~~~~

.. figure:: /images/SEBE_Gothenburg.png
   :alt:  None
   :width: 356px
   :align: center

   Central Gothenburg study area (red square).
   
Geodata covers a small area in central **Gothenburg, Sweden**. Data are projected in SWEREF99 1200 (EPSG:3007) which is in the Swedish national coordinate system.

:download:`Datasets for Gothenburg, Sweden </data/Goteborg_SWEREF99_1200.zip>`

You will make use of two raster datasets:

.. list-table:: Input data and parameters
   :widths: 25 25 50

   * - **Name**
     - **Definition**
     - **Description**
   * - DSM_KRbig.tif
     - Ground and building DSM
     - Raster dataset: derived from a 3D vector roof structure dataset and a digital elevation model (DEM)
   * - CDSM_KRbig.asc
     - Vegetation canopy DSM
     - Raster dataset: drived from a LiDAR dataset

Add data to your QGIS project
-----------------------------

- Start by downloading the data from the link above. The data are zipped - unzip the data first.
- First, add **DSM_KRbig.tif** to your project. Use *Layer > Add Layer > Add Raster Layer* to open the raster file.
- Examine the DSM. Your project is now using *EPSG:3007* as coordinate system (CRS). This can be seen at the lower right hand side in your QGIS window.
- Now add **CDSM_KRbig.asc** to your project. Data can also be added by drag and drop between the *Browser*-panel and the *Layers*-panel to the left.

Now you should see two layer in your *Layers*-panel. You should also see that **CDSM_KRbig** has a question mark (?) next to it. This is because no CRS is connected to this raster data. You will need to indicate the CRS that is associated with these data. 

- Right-click on **CDSM_KRbig** and go to *Set CRS -> Set Layer CRS...*. Choose EPSG:3007. 

When loading a dataset into QGIS, a default apperance is used. The CDSM which includes higher vegetation (trees and bushes) has the default value of zero (black) where no vegetation is present. Lets now change the apperance of the CDSM:

- Right-click on **CDSM_KRbig** and choose *Properties...*.
- Go to the *Symbology*-tab and choose **Singleband pseudocolor** as *Rendertype*. Choose **Greens** as *Color ramp*. Click **Apply**.
- Go to the *Transparency*-tab and add an *Additional no data value* of **0**. Click **OK**.

Now you see the DSM where the CDSM has the value zero.

.. figure:: /images/CDSMoverDSM.jpg
   :alt:  None
   :width: 100%
   :align: center

   The CDSM and DSM used in this exercise.


Calculate daily shadow patterns
-------------------------------

- Open the **Daily Shadow Pattern** tool located at *UMEP -> Processor -> Solar Energy -> Daily Shadow Pattern* in the menu bar. 

This tool can do what it says, namely calculate gound shadow patterns based on a DSM and a CDSM.

- Create a directory e.g. on your Desktop called **DailyShading**. Also create a sub-directory to **DailyShading** called **June21_buildings**. 
- Use this directory to save the result.
- Set-up the tool as shown in the figure below. This will generate ground shadows from buildings every 30 minute on June 21, 2020. Click **Run**.

  .. figure:: /images/Dailyshading21juneBuildings.jpg
     :alt:  None
     :width: 75%
     :align: center

     Settings for calculating ground shadows from buildings with 30 minute interval on June 21, 2020.

When calculations are finished, a new layer has appeared in your QGIS project (**shadow_fraction_on_20200621**). This layer shows the fraction (0 to 1) of sunshine for all pixels in the raster. This layer is presented with a transparancy (*Global Opacity*) of 50%. You can change that under the *Transparency*-tab for the layer.

- Change the *Global Opacity* to 100% for easier comparison later on.
- Rename **shadow_fraction_on_20200621** to **shadow_fraction_on_20200621_buildings** by right-click on the layer in the *Layers*-panel and choose *Rename Layer*.
- Go to the directory that you used as output folder. Here you find each individual shadow map for every 30 minute on June 21.
- Now re-run with the same settings but include the CDSM as shown below. Create a new sub-directory called **June21_buildingsvegetation** and use this as your output location.

  .. figure:: /images/Dailyshading21juneBuildingsVegetation.jpg
     :alt:  None
     :width: 75%
     :align: center

     Settings for calculating ground shadows from buildings and vegetation with 30 minute interval on June 21, 2020.

- Rename your new **shadow_fraction_on_20200621**-layer to **shadow_fraction_on_20200621_buildingsvegetation**.
- Change the *Global Opacity* to **100%**.

Now you can compare the two created layers by tick them on oand off in the *Layers*-panel. You can clearly see the shadows created underneath the trees.

- Finally calulate shadows for December 21, 2020 using both buildings and vegetation. Now set *Transparacy of light through vegeation* to **50%** (defoliated trees) and save the results in a sub-directory called **Dec21_buildingsvegetation**.

- Examine the difference in shadows between the Winter and Summer solstice in Sweden Gothenburg.


Some short remarks on what you have done so far: 

-  Critical is the **building and ground** `DSM <http://umep-docs.readthedocs.io/en/latest/Abbreviations.html>`__ for the calculations of shadows.
-  Optionally **vegetation** (trees and bushes) can be included as they can shadow buildings, walls and roofs reducing the potential solar energy production
-  Two vegetation DSMs are required when the *Use vegetation DSMs* is ticked:
   
  + One to describe the top of the vegetation (Vegetation Canopy DSM).
  + One to describe the bottom, underneath the canopies (Vegetation Trunk Zone DSM). As Trunk Zone DSMs are very rare, an option to create this from the canopy DSM is available. 
  + You can set the amount of light (shortwave radiation) that is transmitted through the vegetation.
  
- The `UTC <https://en.wikipedia.org/wiki/Coordinated_Universal_Time>`__ offset is needed to accurately estimate the sun position, positive numbers for easterly position and negative for westerly. For example, Gothenburg is located in CET which is UTC +1.


Use a post-prcessing tool to animate shadows
--------------------------------------------

The UMEP plugin consist of three parts; a pre-processor, a processor and a post-processor. The pre-processor prepares spatial and meteorological data as inputs to the modelling system. The processor includes all the main models for the main calculations. To provide initial “quick looks” the post-processor will enable results to be plotted, statistics calculated etc. based on the model output. Now you will use a post-processing plugin to more cosely examine the shadow pattern you have generated in this tutorial.

Go to *UMEP -> Post-Processor -> Outdoor Thermal Comfort -> SOLWEIG Analyzer*. We will "borrow" this tool to animate the shadows we created earlier.

- Load one of three output folders you have used in this exercise.
- Click on *Show Animation*

Now you see a short animation of the shadow patterns at 30 minute interval for the data that you generated with the **Daily Shadow Pattern**-tool.
