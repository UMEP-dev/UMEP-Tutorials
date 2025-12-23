.. _SUEWSDatabase:

Urban Energy Balance - SUEWS Database
=====================================

.. note:: TUTORIAL FOR UPCOMING PLUGIN. WORK IN PROGRESS. NOT READY.

**Introduction**

In this tutorial you will acquaint yourself with a database system designed to facilitate provision of input data for the SUEWS model (https://docs.suews.io/stable/inputs/index.html). This database can be used for any type of SUEWS application, but it is especially designed to work with different *urban typologies* (classification schemes to characterise similar buildings forms, materials, functions etc.).

*Overview of activities:* (1) Familiarization with the database itself. (2) Generation of input data for the SUEWS model for an area in Gothenburg, Sweden. (3) Addition of new information into the database. (4) Investigate changes in the urban energy balance when changes are made to an urban typology class within the study area.

In this tutorial detailed spatial data (high resolution) are used. If this kind of data are unavailable locally, other datasets derived from e.g. `Open Street Map <https://www.openstreetmap.org/>`__ can be exploited. However, we recommend going through the tutorial prior to working with user-specific datasets.

**Objectives**

-  To become familiar with the urban database for SUEWS (and other models), including adding information to the database.

-  To explore the influence of installing solar panels on roofs in a residential area Gothenburg, Sweden on urban energy exchanges.

**Initial Steps**

UMEP is a Python plugin used in conjunction with `QGIS <http://www.qgis.org/>`__. To install the software and the UMEP plugins see the `Getting started <http://umep-docs.readthedocs.io/en/latest/Getting_Started.html>`__ section of the `UMEP manual <https://umep-docs.readthedocs.io/>`__. For this tutorial you will need both UMEP and UMEP for Processing.

*Loading and visualise the spatial data*

All the geodata used in this tutorial can be downloaded from `here <https://github.com/Urban-Meteorology-Reading/Urban-Meteorology-Reading.github.io/blob/master/other%20files/Kville_Goteborgs_SWEREF99_1200.zip>`__.

-  Unzip and place the data in a folder that you have read and write access.

-  Load all the raster and vector datasets into an empty QGIS project. All layers are in SWEREF99 1200 (EPSG:3007).

The order in the *Layers Panel* determines what layer is visible. You can choose to show a layer (or not) with the tick box. You can modify layers by right-clicking on a layer in the *Layers Panel* and choose *Properties*. Note for example that that CDSM (vegetation) is given as height above ground (metre) and that all non-vegetated pixels are set to zero. This makes it hard to get an overview of all the 3D objects (buildings and trees). As QGIS’s default styling for a raster uses the 98 percentile of the values, the entire data range is not shown in the layer window to the left.

-  Right-click on your **CDSM** layer and go to *Properties > Style* and choose **Singleband pseudocolor** with a min value of 0 and max of 35. Choose a colour scheme of your liking.

-  Go to *Transparency* and add an additional no data value of 0. Click ok.

-  Now put your **CDSM** layer at the top and your **DSM** layer second in your *Layers Panel*. Now you can see both buildings and vegetation 3D object in your map canvas.

The land cover grid comes with a specific QGIS style file.

-  Right-click on the land cover layer (**lc_kville**) and choose *Properties*. At the bottom left of the window there is a *Style*-button. Choose *Load Style* and open **landcoverstyle.qml** and click OK.

-  Make only your land cover class layer visible to examine the spatial variability of the different land cover classes.

The land cover grid has already been classified into the seven different classes used in most UMEP applications (see `Land Cover Reclassifier <http://umep-docs.readthedocs.io/en/latest/pre-processor/Urban%20Land%20Cover%20Land%20Cover%20Reclassifier.html>`__). If you have a land cover dataset that is not UMEP formatted, you can use the *Land Cover Reclassifier* found at *UMEP > Pre-processor > Urban Land Cover > Land Cover Reclassifier* in the menu-bar to reclassify your data.

Furthermore, a polygon grid (400 m x 400 m) to define the study area and individual grids is included (**gridKville.shp**). Such a grid can be produced directly from the *Processing Toolbox* in QGIS (*Create Grid*), or an external grid can be used.

-  Load the vector layer **gridKville.shp** into your QGIS project.

-  In the *Style* tab in layer *Properties*, choose a *Simple fill* and set *Fill style* to *No Brush*, to be able to see the spatial data within each grid.

-  Also, add the label IDs for the grid to the map canvas in *Properties > Labels > Single Labels* to make it easier to identify the different grid squares later on in this tutorial.

-  There is also a vector polygon layer (**topologiesKville.shp**) locating different urban topologies within the area. Open the attribute table associated with this layer. Here you see an attribute called *urbantype* which describes the various urban typologies in the area. This is in Swedish so below you find a translation and description for each type.

+---------+-------------------+---------------------------------------+
| **Sw    | **English**       | **Description**                       |
| edish** |                   |                                       |
+=========+===================+=======================================+
| i       | industry          | Extensive low buildings               |
| ndustri |                   |                                       |
+---------+-------------------+---------------------------------------+
| bland   | Mixed city        | Modern high density residential       |
|         |                   | neighbourhood                         |
+---------+-------------------+---------------------------------------+
| kvarter | Traditional       | Early 20th century urban development  |
|         | neighbourhood     |                                       |
+---------+-------------------+---------------------------------------+
| Milton  | The Million       | Mid 20th century modernistic suburban |
|         | Programme         | neighbourhood development             |
+---------+-------------------+---------------------------------------+
| folkhem | Nordic            | low dense early/mid 20th century      |
|         | Functionalism     | suburban development                  |
+---------+-------------------+---------------------------------------+
| smahus  | detached houses   | sub-urban residential houses          |
+---------+-------------------+---------------------------------------+

-  To make overview easier of the typology layer and the other data, move **typologiesKville** second to the to the top (**gridKville** should be at the top) in the *Layers*-panel. Then go to *Properties* for the typology layer and set the *Opacity* to **50%** as well as *Classify* to *Categorized* based on *Value* **urbantype** and click, *OK*.

.. figure:: /images/media/media/image1.jpeg
   :alt:  none
   :width: 100%

   QGIS project with all data loaded and visualised. Click on image for enlargement.

The typology polygon layer is used to describe types of buildings and paved surfaces within each urban typology.

**Exploring the SUEWS database**

-  Open the database plugin from the menus at the top (*UMEP>Pre-Processor>Urban Energy Balance>SUEWS Database Manager*).

This multi-purpose plugin can be used to explore the different available settings that currently exists within the database. It can also be used to add new information to the database as well as connect a polygon layer that represents different urban typologies to typologies available from the database, making it possible to create input data to SUEWS in later steps. The plugin consists of tabs describing different sections of the database. More detailed information about each tab can be found `here <https://umep-docs.readthedocs.io/en/latest/pre-processor/Urban%20Energy%20Balance%20SUEWS%20Database%20Manager.html>`__. Each tab has an information panel (upper right) that give some basic explanation on what the current tab is used for and what the content is for this particular tab.

-  Go to the *Typologies*-tab and select **Sub-urban, Sweden** as a *Base Type*. Here, you can see basic information related to this specific typology. As you can see, two land cover types are connected to this typology (Paved:Kumpula, Helsinki and Buildings:Detached houses, Wood, Sweden). To the left you see more detailed information related to this urban type as well as a representative picture of this typology. This tab will later be used when we create a new typology where roof solar panels have been introduced.

-  Let us now examine the Detached houses typology more in detail by clicking *Edit/create Land Cover types*. This will change the tab to Land Cover. Here, select **buildings** in the top combobox and choose **Detached houses, Wood, Sweden** as a *Base element*.

Now, you can see all parameters that are connected to this particular urban typology. Taking **albedo** as an example, you can also see the available building related albedo settings that is present in the database. You also see a reference to all the albedo entries available in the database.

Moving further right in the different tabs, *Parameters* gives the opportunity to add new parameters for various land cover related parameters such as albedo, leaf area index (LAI) etc.

-  Now go to the *Profiles*-tab. Here you can examine and create new profiles that for example decides the pattern for traffic for a specific country and type of day. Choose **Traffic** as *Profile Type* and Select **Sweden** as *Country*. Now you see the profile plotted and you see the difference between weekend and weekday where weekday have two peaks, one for people going to work and one for going home.

.. figure:: /images/media/media/image2.png
   :alt:  none
   :width: 100%

   Profile for weekday traffic in Sweden. Click on image for enlargement.

Go through the rest of the tabs to make yourself familiar with their different contents and functionalities.

**Preparing input data for standard case (no solar panels)**

For later comparison, we should now generate input data and run the model for a standard case with no solar panels installed.

-  Go to the *Main tab - Reclassifier*. We should now appoint the typologies in our polygon layer to existing typologies found in the database. Use the settings below and click *Reclassify*.

.. figure:: /images/media/media/image3.jpeg
   :alt:  none
   :width: 100%

   Reclassification of typology vector layer into existing typologies found in SUEWS Database. Click on image for enlargement.

After that, click on **Close** in the right corner. A new window will pop-up asking you to update the database. As you have not yet made any changes you can click **No** and continue with the tutorial. If you like, you can also change the symbology of your new reclassified shapefile in your QGIS project as explained in the beginning of this tutorial.

Now, we need to derive surface fractions etc. from the geodata for each grid. We start with calculate roughness parameters based on the building geometry and vegetation within your grids. Open *UMEP > Pre-Processor > Urban Morphology: Morphometric Calculator (Grid)*.

-  Use the settings as in the figure below and press *Run*. Do not close the plugin after execution is completed.

.. figure:: /images/media/media/image4.png
   :alt:  none
   :width: 100%

   The settings for calculating building morphology in the Morphometric Calculator (Grid). Click on image for enlargement.

This operation should have produced 16 different text files; 15 (*anisotropic*) that include morphometric parameters from each 5-degree section for each grid and one file (*isotropic*) that includes averaged values for each of the 15 grids. You can open **KvilleBaseBuild_IMPGrid_isotropic.txt** and compare the different values for different grids. Header abbreviations are explained `here <http://umep-docs.readthedocs.io/en/latest/Abbreviations.html>`__. In addition, by ticking in *Calculate parameters for SUEWS/SS [optional]*, 15 additional files were created describing vertical morphology parameters in 1-meter vertical resolution. This info will later be used in a scheme for calculating storage heat flux as presented in Liu et al. (2025).

-  When first calculation is done, go to the *Parameters*-tab in the plugin and tick in *Raster DSM (only 3D building or vegetation objects) exist*, select **csdm_kville** as *Raster DSM (only 3D objects)*, change your *File prefix* to **Kville BaseVeg**. Un-tick *Calculate parameters for SUEWS/SS [optional].* This process will calculate morphometric parameters based on 3D vegetation.

Moving on to land cover fraction calculations for each grid.

-  Open UMEP > Pre-Processor > Urban Land Cover > Land Cover Fraction (Grid).

-  Use the settings as in the figure below and press *Run*.

-  When calculation is done, close the plugin.

.. figure:: /images/media/media/image5.jpeg
   :alt:  none
   :width: 100%

   The settings for calculating land cover fractions. Click on image for enlargement.

Population density is a required input in this tutorial. For each grid, this should be given in *persons/hectare*.

-  Open the attribute table for your grid (right-click on **gridKville** in the *Layers*-panel and click on *Open Attribute Table*). Here, you see a column named **\_popsum** which is the total residence population for each grid. Now, let’s create a new column called **\_popdens** giving us population density (pp/ha). This is done via the *Field Calculator* that could be accessed via the abacus-button in the attribute table window. Use the settings as shown in the figure below.

.. figure:: /images/media/media/image6.jpeg
   :alt:  none
   :width: 100%

   The settings for adding an attribute column with population density (pp/ha). Click on image for enlargement.

Remember to save your attribute table and exit editing mode by clicking the yellow pencil button. Now you can close the attribute table window.

**SUEWS Prepare - Database Typologies**

Now all the required input information is pre-processed apart from the final step which is to create the actual aggregated input data for the SUEWS model. To do this, we will make use of *SUEWS Prepare - Database Typologies*. Open **SUEWS Prepare - Database typologies** (*UMEP > Pre-Processor > Urban Energy Balance > SUEWS Prepare - Database typologies*).

-  Starting in the upper left panel, here general settings all vector-based input geodata is set. Population density for this tutorial is available as an attribute in the grid polygon layer. Here we also set the urban typology layer to be used. If no typology layer is selected, the paved and building typology is set based on national parameters (upper right panel).

-  The panel below includes raster data required. These datasets are used when the tool aggregates properties from different typologies within each grid.

-  The lower left panel includes some general setting used in SUEWS. Here, for example, it is important set the UTC correct.

-  The middle panels make use of the morphology files creates earlier. Here you also specify meteorological forcing data. We have provided you with a ERA5 dataset from 2018 (January to July) for this tutorial.

-  In the upper right panel various national (and regional) parameters are specified based on country chosen. There are possibilities to change each parameter if you like, but the tool adds default info taken from the database for the country selected.

-  The last panel (lower right) includes settings for the vertical morphology used for different schemes in SUEWS.

Below you see the settings used for each panel. When all settings are made, click OK and wait for the tool to create input data needed to continue.

.. figure:: /images/media/media/image7.png
   :alt:  none
   :width: 100%

   Settings for the SUEWS Database Prepare plugin - Base case. Click on image for enlargement.

**Executing the model**

Open *UMEP -> Processor -> Urban Energy Balance: SUEWS v2025.10.15rc1* and use the settings below. The model will calculate one half year starting in January 2018. It is always good to include some time for spin-up, preferably a full year should be used but, in this case, we only include half year. We will later examine result for the last month in the calculation (June). To avoid issues with memory running low, we divide the calculation into 10 different chunks. If you still experience memory issues, increase the number of chunks. Click **Run**. This will take a couple of minutes so grab a cup of tea while waiting.

.. figure:: /images/media/media/image8.png
   :alt:  none
   :width: 100%

   Settings for the SUEWS plugin - Base case. Click on image for enlargement.

**Analysing the results**

The post-processing tools in UMEP are very basic and more advanced analysis of model output is recommended to be performed outside of QGIS/UMEP. Nevertheless, there are still some useful capabilities existing within UMEP. There are two different tools within UMEP that can be used to examine the results from SUEWS, either focusing more on time or focusing on space (creating maps). Let’s have a look at plotting some time series to check that the model produced reasonable results.

-  From the QGIS menu, open *UMEP -> Post Processor -> Urban Energy Balance -> SUEWS Analyzer* and load your yaml-file (**kv.yml**) that now should be located in your Base-folder you specified earlier. Start by plotting basic data for grid 1 (2018). Zoom in (magnifying glass) last month (June) similar as shown below:

.. figure:: /images/media/media/image9.jpeg
   :alt:  none
   :width: 100%

   Model output for June (Grid 1) - Base case. Click on image for enlargement.

If you have a look at grid 1 in your map canvas you see that it consists of low building density with dominating areas of detached residential houses and relatively high fraction of trees and grass surfaces. Looking at the basic plot, you can see that the anthropogenic energy flux is low (about 25 W m\ :sup:`-2` midday) and storage heat flux (Q\ :sub:`S`) during clear and warm weather (e.g. beginning of June) peaks around 150 W m\ :sup:`-2`. You can, for example, also see the fast response of latent heat flux (Q\ :sub:`E`) during and after periods of rain. Also, have a look at peak values of K\ :sub:`up` for e.g. 1 June which is about 100 W m\ :sup:`-2`.

Now let’s compare with a denser grid square.

-  Close the figure window and tick off *Plot basic data*. Instead, choose a time period between *153* and *183* day of year and scroll to the *Net storage heat flux*. Tick in *Include another variable* and choose the same variable but for grid 11 which is the most dense grid within the model domain. Click **Plot**.

Now you see very large differences since the buildings in the denser grid 11 store more heat compared to grid 1. Feel free to play around and compare other variables and grids with each other. You can also produce scatter plots. Plots could be saved by clicking the floppy disk at the top of the figure window.

**Preparing input data for solar panels case**

We will now examine the effect of adding solar panels on the roofs of detached residential buildings. To accomplish this, we need to re-visit the *SUEWS Database Manager*-plugin and create a new typology where the roof solar panels have been added. The urban typology used for the detached residential buildings in the base case is called Sub-urban, Sweden. Based on this typology, we will now create a new one and change the albedo value, assuming solar panels will change reflectivity of short-wave radiation as well as altering the storage of energy.

-  Open *SUEWS Database Manager* and go to the *Typologies* tab. There, choose **Sub-urban, Sweden** as your *Base Type*. Now you see that the building land cover type for this typology is called **Detached houses, Wood, Sweden**. Moving over to the next tab, select Buildings and **Detached houses, Wood, Sweden** as your *Base Element*. Here you see all the settings for this building type. At the top you see the *Albedo* is specified by **Wood, Gothenburg**.

..

   First we need to add a new roof material into the database.

-  Go to the go to the References tab and add the following information before clicking Add Reference:

.. figure:: /images/media/media/image10.png
   :alt:  none
   :width: 100%

   Adding a new reference. Click on image for enlargement.

This publication includes information about the thermal properties of a photovoltaic panel.

-  Move to the next tab. Here, we will add a solar panel as a new material type. Use roof tile; Generic as a Base Material, set the Name to be Solar panel, colour to Black and use the reference we just added. The material characteristics should be set as follows:

   -  Albedo = 0.1

   -  Emissivity = 0.9

   -  Thermal conductivity = 0.9

   -  Specific heat capacity = 900

   -  Density = 400

..

   Add the new material and move to next tab (Building facets)

-  We will now create a new Building facet based on Detached houses, Wood, Sweden according to settings below:

.. figure:: /images/media/media/image11.png
   :alt:  none
   :width: 100%

   Adding new building facets including roofs with solar panels. Click on image for enlargement.

This new added information will be used by Dynamic OHM (dyOHM), a scheme that estimate storage of heat in buildings (Liu et al., 2025a). The radiation method used requires an effective bulk albedo. Hence, we need to alter this parameter also since we added solar panels on our roofs.

-  Go to the *Parameters*-tab and choose **Wood, Gothenburg** as your *Base parameter* when selecting Albedo and Buildings as Surface. Then change according to the figure below and click *Add Parameter*.

.. figure:: /images/media/media/image12.png
   :alt:  none
   :width: 100%

   Adding a new building albedo parameter for solar panels. Click on image for enlargement.

-  Move over to the *Land Cover*-tab. Select Buildings as Table. Using **Detached houses, Wood, Sweden** as the *Base Element*, create a new land cover type called **Detached houses SP, Wood** and change the Albedo to the new one that you just created (Wood Solar Panels) and change Spartacus Surface to **Detached houses, Solar Panels, Wood, Sweden** Click **Add Land Cover**.

-  Move to the *Typologies*-tab. Base your new typology on **Sub-urban, Sweden** and use the settings as below. Click **Generate New Type**.

.. figure:: /images/media/media/image13.jpeg
   :alt:  none
   :width: 100%

   Adding a new urban typology for detached wooden houses with solar panels on roofs into the database. Click on image for enlargement.

-  Finally, go the Main tab – Reclassifier-tab. A new, updated typology layer should be created. Use the original polygon layer, typologiesKville, and reclassify according to the figure below.

.. figure:: /images/media/media/image14.png
   :alt:  none
   :width: 100%

   Reclassification of typology vector layer into existing typologies, now with a new typology representing sub-urban houses with solar panels on roofs. Click on image for enlargement.

-  Click **Close**. Answer Yes to the question in the pop-up window. That will save all your new entries made into the database.

When you made changes and want to share them with the UMEP community, you can export the database (**Export Full Database** in the Main-tab) and send it to fredrikl[at]gvc.gu.se. Then your changes will be added into the next version of the tool and others can benefit from your updates. Also, if you choose not to send the database, all your changes will be lost the next time you update UMEP.

-  Now open SUEWS Database Prepare and use the same settings as you did for the Base-case but change the *Building typology (from polygon layer)* to your new **typologiesKvilleSolar** and change your output folder to a new one named e.g. Solar (*C:/temp/SUEWSDB_tutorial/Solar*). Click *Generate*.

-  Run the model again as before but now change your input folder so that information found in your **Solar**-directory is used. Also, make sure that you save in a new output folder, e.g. **./Solar/out**.

**Comparing the two scenarios**

-  Create a basic plot from grid 1 from the solar panel case, the same way as you did for the base-case. Look at e.g. K\ :sub:`up` for 1 June you can now see a lowering of peak K\ :sub:`up` of 20 W m\ :sup:`-2`. The resulting energy flux e.g. sensible heat flux is small, just a couple of W m\ :sup:`-2`.

The changes in energy fluxes are small, but one can see a clear effect when just altering the albedo for a certain urban typology. Have in mind that the fraction of buildings within the detached residential typology for this area is relatively low (see e.g. grid 1), so altering the albedo for the buildings only will not create a very large effect. Nevertheless, there is a small difference evident.

Finally, we will produce a spatial comparison between the two scenarios. We will make use of our polygon grid (**gridKville**) and populate new attributes for each grid polygon. We will investigate daytime Q\ :sub:`H` for the month of June. First, we will calculate for the base-case

-  From the Processing toolbox in QGIS, open *UMEP -> Post-Processor -> Urban Energy Balance: SUEWS Analyzer*. Use the settings shown in the figure below.

.. figure:: /images/media/media/image15.png
   :alt:  none
   :width: 100%

   The SUEWS Analyzer calculating mean daytime QH for the month of June 2018. Click on image for enlargement.

-  Repeat the task but change to the solar panel case.

Two new attributes should now have appeared in the *gridKville\** attribute table.

Lastly, open the field calculator and create a new attribute, subtracting the **QH_1** column (solar panel case) with **QH** (base-case). Now, right-click on *gridKville* in your *Layer*-panel and click on *Properties*. Here, go to *Symbology* and make a colourful map of the difference between the two scenarios with regards to mean daytime *Q\ H* in June 2018.

Tutorial finished
