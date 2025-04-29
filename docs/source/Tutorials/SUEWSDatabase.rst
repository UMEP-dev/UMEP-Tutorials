.. _SUEWSDatabase:

Urban Energy Balance - SUEWS Database
=====================================

.. note:: TURORIAL FOR UPCOMING PLUGIN. WORK IN PROGRESS. NOT READY.

Introduction
------------

In this tutorial you will make yourself acquainted with a database system that facilitates use of input data for the SUEWS model. This database can be used for any type of SUEWS application but it is especially designed to work with so called *urban typologies* (neighborhoods that are similar in buildings materials, functions etc.). First, you will make your self-acquainted with the database itself. After that, you will generate input data for the SUEWS model for an area in Gothenburg, Sweden. Lastly, you will add new information into the database and investigate changes in the urban energy balance when changes have been made for a certain urban typology within the study area.

This tutorial makes use of local high resolution detailed spatial data. If this kind of data is unavailable, other datasets derived from e.g. `Open Street Map <https://www.openstreetmap.org/>`__ can be exploited. However, it is strongly recommended to go through this tutorial before moving on to more user-specific datasets.

Objectives
----------

* To get familiar with the urban database connected to SUEWS and how to add information into the database.
* Explore the influence on urban energy balance and budget when installing solar panels on roofs in a sub-urban area in Gothenburg, Sweden. 

Initial Steps
-------------

UMEP is a Python plugin used in conjunction with `QGIS <http://www.qgis.org>`__. To install the software and the UMEP plugins see the `getting started <http://umep-docs.readthedocs.io/en/latest/Getting_Started.html>`__ section in the UMEP manual. For this tutorial you will need both **UMEP** and **UMEP for Processing**.

Loading and visualise the spatial data
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

All the geodata used in this tutorial can be downloaded from `here <https://github.com/Urban-Meteorology-Reading/Urban-Meteorology-Reading.github.io/blob/master/other%20files/Kville_Goteborgs_SWEREF99_1200.zip>`__. 

- Unzip and place in a folder that you have read and write access to.
- Start by loading all the raster and vector datasets into an empty QGIS project. All layers are in SWEREF99 1200 (EPSG:3007).

The order in the *Layers Panel* determines what layer is visible. You can choose to show a layer (or not) with the tick box. You can modify layers by right-clicking on a layer in the *Layers Panel* and choose *Properties*. Note for example that that CDSM (vegetation) is given as height above ground (meter) and that all non-vegetated pixels are set to zero. This makes it hard to get an overview of all 3D objects (buildings and trees). QGIS default styling for a raster is using the 98 percentile of the values. Therefore, not all the range of the data is shown in the layer window to the left.

- Right-click on your **CDSM** layer and go to *Properties > Style* and choose **Singleband pseudocolor** with a min value of 0 and max of 35. Choose a colour scheme of your liking.
- Go to *Transparency* and add an additional no data value of 0. Click ok.
- Now put your **CDSM** layer at the top and your **DSM** layer second in your *Layers Panel*. Now you can see both buildings and vegetation 3D object in your map canvas.

The land cover grid comes with a specific QGIS style file.

- Right-click on the land cover layer (**lc_kville**) and choose *Properties*. At the bottom left of the window there is a *Style*-button. Choose *Load Style* and open **landcoverstyle.qml** and click OK.
- Make only your land cover class layer visible to examine the spatial variability of the different land cover classes.

The land cover grid has already been classified into the seven different classes used in most UMEP applications (see `Land Cover Reclassifier <http://umep-docs.readthedocs.io/en/latest/pre-processor/Urban%20Land%20Cover%20Land%20Cover%20Reclassifier.html>`__). If you have a land cover dataset that is not UMEP formatted you can use the *Land Cover Reclassifier* found at *UMEP > Pre-processor > Urban Land Cover > Land Cover Reclassifier* in the menu-bar to reclassify your data.

Furthermore, a polygon grid (400 m x 400 m) to define the study area and individual grids is included (**gridKville.shp**). Such a grid can be produced directly from the *Processing Toolbox* in QGIS (*Create Grid*) or an external grid can be used.

- Load the vector layer **gridKville.shp** into your QGIS project.
- In the *Style* tab in layer *Properties*, choose a *Simple fill* and set *Fill style* to *No Brush*, to be able to see the spatial data within each grid.
- Also, add the label IDs for the grid to the map canvas in *Properties > Labels > Single Labels* to make it easier to identify the different grid squares later on in this tutorial.
- There is also a vector polygon layer (**topologiesKville.shp**) locating different urban topologies within the area. Open the attribute table associated with this layer. Here you see an attribute called *urbantype* which describes the various urban typologies in the area. This is in Swedish so below you find a translation and description for each type.

    .. list-table::
       :widths: 20 20 60
       :header-rows: 1

       * - Swedish
         - English
         - Description
       * - industri
         - industry
         - Extensive low buildings 
       * - bland
         - Mixed city
         - Modern high density residential neighbourhood
       * - kvarter
         - Traditional neighbourhood
         - Early 20th century urban development
       * - miljon
         - The Million Programme
         - Mid 20th century modernistic suburban neighbourhood development
       * - folkhem
         - Nordic Functionalism
         - low dense early/mid 20th century suburban development
       * - smahus
         - detached houses
         - sub-urban residential houses

- To make overview easier of the typology layer and the other data, move **typologiesKville** second to the to the top (**gridKville** should be at the top) in the *Layers*-panel. Then go to *Properties* for the typology layer and set the *Opacity* to **50%** as well as *Classify* to *Categorized* based on *Value* **urbantype** and click, *OK*.


.. figure:: /images/SUEWSDatabase_MapOverview.jpg
   :alt:  none
   :width: 100%

   QGIS project with all data loaded and visualised. Click on image for enlargement.

The typology polygon layer is used to describe types of buildings and paved surfaces within each urban typology.

Exploring the SUEWS database
----------------------------

- Open the database plugin from the menus at the top (*UMEP>Pre-Processor>Urban Energy Balance>SUEWS Database Manager*). 

This multi-purpose plugin can be used to explore the different available settings that currently exists within the database. It can also be used to add new information to the database as well as connect a polygon layer that represents different urban typologies to typologies available from the database, making it possible to create input data to SUEWS in later steps. The plugin consists of a number of tabs describing different sections of the database. More detailed information about each tab can be found `here <https://umep-docs.readthedocs.io/en/latest/pre-processor/Urban%20Energy%20Balance%20SUEWS%20Database%20Manager.html>`__. Each tab has a panel (upper right) that give some basic explanation on what the current tab is used for and what the content is for this particular tab. 

- Go to the *Typologies*-tab and select **Sub-urban, Sweden** as a *Base Type*. Here, you can see basic information related to this specific typology. As you can see, two lnd cover types are connected to this typology (Paved:Kumpula, Helsinki and Buildings:Detached houses, Wood, Sweden). To the left you see more detailed information related to this urban type as well as a picture. This tab will later be used when we create a new typology where roof solar panels have been introduced.
- Let us now examine the Detached houses typology more in detail by clicking *Edit/create Land Cover types*. This will change the tab to Land Cover. Here, select **buildings** in the top combobox and choose **Detached houses, Wood, Sweden** as a *Base element*. 

Now, you can see all parameters that are connected to this particular urban typology. Taking **albedo** as an example, you can also see the available building related albedo settings that is present in the database. You also see a reference to all the albedo entries available in the database. 

Moving further in the different tabs, *Parameters* gives the opportunity to add new parameters for various land cover related parameters such as albedo, leaf area index (LAI) etc. 

- Now go to the *Profiles*-tab. Here you can examine and create new profiles that for example decides the pattern for traffic for a specific country and type of day. Choose **Traffic** as *Profile Type* and Select **Sweden** as *Country*. Now you see the profile plotted and you see the difference between weekend and weekday where weekday have two peaks, one for people going to work and one for going home. 

.. figure:: /images/SUEWSDatabase_Profile.jpg
   :alt:  none
   :width: 100%

   Profile for weekday traffic in Sweden. Click on image for enlargement.

Go through the rest of the tabs to make yourself familiar with their different contents and functionalities. 


Preparing input data for standard case (no solar panels)
--------------------------------------------------------

For later comparison, we should now generate input data and run the model for a standard case with no solar panels installed.

- Go to the *Main tab - Reclassifier*. We should now appoint the typologies in our polygon layer to existing typologies found in the database. Use the settings below and click *Reclassify*.

.. figure:: /images/SUEWSDatabase_ReclassifyBase.jpg
   :alt:  none
   :width: 100%

   Reclassification of typology vector layer into existing typologies found in SUEWS Database. Click on image for enlargement.

After that, click on **Close** in the right corner. A new window will pop-up asking you to update the database. As you have not yet made any changes you can click **No** and continue with the tutorial. If you like, you can also change the symbology of your new reclassified shapefile in your QGIS project as explained in the beginning of this tutorial.

Now, we need to derive surface fractions etc. from the geodata for each grid. This is similar as done in many other tutorials e.g. `SuewsSpatial`. We start with calculate roughness parameters based on the building geometry and vegetation within your grids. Open *UMEP > Pre-Processor > Urban Morphology: Morphometric Calculator (Grid)*. 

.. note:: For mac users, use this workaround: manually create a directory, go into the folder above and type the folder name. It will give a warning *“—folder name--” already exists. Do you want to replace it?* Click *replace*.

- Use the settings as in the figure below and press *Run*. Do not close the plugin after execution is completed.

.. figure:: /images/SUEWSDatabase_IMCGBuilding.jpg
   :alt:  none
   :width: 100%

   The settings for calculating building morphology. Click on image for enlargement.
   
This operation should have produced 16 different text files; 15 (*anisotropic*) that include morphometric parameters from each 5 degree section for each grid and one file (*isotropic*) that includes averaged values for each of the 15 grids. You can open **KvilleBaseBuild_IMPGrid_isotropic.txt** and compare the different values for different grids. Header abbreviations are explained `here <http://umep-docs.readthedocs.io/en/latest/Abbreviations.html>`__.

- When first calculation is done, go the *Parameters*-tab in the plugin and tick in *Raster DSM (only 3D building or vegetation objects) exist*, select **csdm_kville** as *Raster DSM (only 3D objects)*, change your *File prefix* to **Kville BaseVeg**. This will calculate morphometric parameters based on 3D vegetation.

As you might have noticed, we did not calculate specific input information for SUEWS/SS (`Spartacus <https://link.springer.com/article/10.1007/s10546-019-00457-0>`__) which is a more advanced scheme to calculate radiation fluxes in the model. We will instead make use of the `NARP <https://journals.ametsoc.org/view/journals/apme/42/8/1520-0450_2003_042_1157_ponarf_2.0.co_2.xml>`__-scheme.

Moving on to land cover fraction calculations for each grid.

- Open *UMEP > Pre-Processor > Urban Land Cover > Land Cover Fraction (Grid)*.
- Use the settings as in the figure below and press *Run*.
- When calculation is done, close the plugin.

.. figure:: /images/SUEWSDatabase_LCBase.jpg
   :alt:  none
   :width: 100%
   
   The settings for calculating land cover fractions. Click on image for enlargement.
   
Population density is a required input in this tutorial. For each grid, this should be given in *persons/hectare*. 

- Open the attribute table for your grid (right-click on **gridKville** in the *Layers*-panel and click on *Open Attribute Table*). Here, you see a column named **_popsum** which is the total number of resident population for each grid. Now, lets create a new column called **_popdens** giving us population density (pp/ha). This is done via the *Field Calculator* that could be accessed via the abacus-button in the attribute table window. Use the settings as shown in the figure below.

.. figure:: /images/SUEWSDatabase_AttributeTable.jpg
   :alt:  none
   :width: 80%
   
   The settings for adding an attribute column with population density (pp/ha). Click on image for enlargement.

Remember to save your attribute table and exit editing mode by clicking the yellow pencil button. Now you can close the attribute table window.

SUEWS Prepare - Database Typologies 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Now all the required input information is pre-processed apart from the final step which is to create the actual input data for the SUEWS model. To do this, we will make use of *SUEWS Prepare - Database Typologies*. Open **SUEWS Prepare - Database typologies** (*UMEP > Pre-Processor > Urban Energy Balance > SUEWS Prepare - Database typologies*).

- Starting in the upper left panel, here general settings all vector-based input geodata is set. Population density for this tutorial is available as an attribute in the grid polygon layer. Here we also set the 

- The panel below includes raster data required. These datasets are used when the tool aggregates properties from different typologies within each grid.

- The lower left panel includes some general setting used in SUEWS. Here, for example, it is important set the UTC correct.

- The middle panels make use of the morphology files creates earlier. Here you also specify meteorological forcing data. We have provided you with a ERA5 dataset from 2018 (January to July) for this tutorial.

- In the upper right panel various national (and regional) parameters are specified based on country chosen. There are possibilities to change each parameter if you like, but the tool adds default info taken from the database for the country selected.

- The last panel (lower right) includes settings for the Spartacus radiation scheme but since we will use other settings for SUEWS later, we do not need to consider settings in this panel. Below you see the settings used for each panel. When all settings are made, click OK and wait for the tool to create all input data needed to continue.  

.. figure:: /images/SUEWSDatabase_PrepareBase.jpg
   :alt:  none
   :width: 100%

   Settings for the SUEWS Database Prepare plugin - Base case (click for a larger image).


Executing the model
-------------------

Open *UMEP -> Processor -> Urban Energy Balance: SUEWS v2020a* and use the settings below. The model will calculate one half year starting in January 2018. It is always good to include some time for spin-up, preferably a full year should be used but in this case we only include half year. We will later examine result for the last month in the calculation (June). To avoid issues with memory running low, we divide the calculation into 10 different chunks. If you still experience memory issues, increase the number of chunks. Click **Run**. This will take a couple of minutes so grab a cup of tea while waiting.

.. figure:: /images/SUEWSDatabase_SUEWSBase.jpg
   :alt:  none
   :width: 100%

   Settings for the SUEWS plugin - Base case (click for a larger image).
   

Analysing the results
---------------------

The post-processing tools in UMEP are very basic and more advanced analysis of model output is recommended to be performed outside of QGIS/UMEP. Nevertheless, there are still some useful capabilities existing within UMEP. There are two different tools within UMEP that can be used to examine the results from SUEWS, either focusing more on time or focusing on space (creating maps). Lets have a look at plotting some time series to check that the model produced reasonable results.

- From the QGIS menu, open *UMEP -> Post Processor -> Urban Energy Balance -> SUEWS Analyzer* and load your RunControl namelist (**RunControl.nml**) that now should be located in your input folder that you specified earlier. Start by plotting basic data for grid 1 (2018). Zoom in (magnifying glass) last month (June) similar as shown below: 

.. figure:: /images/SUEWSDatabase_PlotBasicBase.jpg
   :alt:  none
   :width: 100%

   Model output for June (Grid 1) - Base case (click for a larger image).

If you have a look at grid 1 in your map canvas you see that it consists of low building density with dominating areas of detached residential houses and relatively high fraction of trees and grass surfaces. Looking at the basic plot you can see that the anthropogenic energy flux is low (about 25 W m\ :sup:`-2` midday) and storage heat flux (Q\ :sub:`S`) during clear and warm weather (e.g. beginning of June) peaks around 150 W m\ :sup:`-2`. You can, for example,  also the fast response of latent heat flux (Q\ :sub:`E`) during and after periods of rain. Also, have a look at peak values of K\ :sup:`up` for e.g. 1 June which is about 100 W m\ :sup:`-2`. 

Now lets compare with a more dense grid square.

- Close the figure window and tick off *Plot basic data*. Instead, choose a time period between *153* and *183* day of year and scroll to the *Net storage heat flux*. Tick in *Include another variable* and choose the same variable but for grid 11 which is the most dense grid within the model domain. Click **Plot**.

Now you see very large differences since the buildings in the more dense grid 11 store more heat compared to grid 1. Feel free to play around and compare other variables and grids with each other. Your can also produce scatter plots. Plots could be saved by clicking the floppy disk at the top of the figure window.

Preparing input data for solar panels case
------------------------------------------

As stated above, we will now examine the effect of adding solar panels on the roofs of detached residential buildings. To accomplish this, we need to re-visit the SUEWS Database Manager-plugin and create a new typology where the roof solar panels have been added. The urban typology used for the detached residential buildings in the base case is called Sub-urban, Sweden. Based on this typology, we will now create a new one and change the albedo value assuming that dark solar panels will reduce reflectivity of short-wave radiation.

- Open *SUEWS Database Manager* and go to the *Typologies* tab. There, choose **Sub-urban, Sweden** as your *Base Type*. Now you see that the building land cover type for this typology is called **Detached houses, Wood, Sweden**. Moving over to the next tab, select Buildings and **Detached houses, Wood, Sweden** as your *Base Element*. Here you see all the settings for this building type. At the top you see the *Albedo* is specified by **Wood, Gothenburg**.

- Now go to the *Parameters*-tab and choose **Wood, Gothenburg** as your *Base parameter*. Then change according to the figure below and click *Add Parameter*.

.. figure:: /images/SUEWSDatabase_SUEWSSolarParameter.jpg
   :alt:  none
   :width: 100%

   Adding a new building albedo parameter for solar panels (click for a larger image).

- Move over to the *Land Cover*-tab. Using **Detached houses, Wood, Sweden** as the *Base Element*, create a new land cover type called **Detached houses SP, Wood** and change the albedo to the new one that you just created (Wood Solar Panels). Click **Add Land Cover** and move to the *Typologies*-tab. Base your new typology on **Sub-urban, Sweden** and use the settings as below. Click **Grenerate New Type**.

.. figure:: /images/SUEWSDatabase_SUEWSSolarTypology.jpg
   :alt:  none
   :width: 100%

   Adding a new urban typology for detached wooden houses with solar panels on roofs into the database (click for a larger image).

- Finally, go the the Main tab. A new, updated typology layer should be created. Use the original polygon layer, typologiesKville, and reclassify according to the figure below.

.. figure:: /images/SUEWSDatabase_ReclassifySolar.jpg
   :alt:  none
   :width: 100%

   Reclassification of typology vector layer into existing typologies, now with a new typology representing sub-urban houses with solar panels on roofs (click for a larger image).
   
- Click **Close**. Answer Yes to the question in the pop-up window. That will save all your new entries made into the database. 

.. note:: IMPORTANT! Remember to export the database (**Export Full Database** in the Main-tab) and send it to fredrikl[at]gvc.gu.se. Then your changes will be added into the next version of the tool and others can benefit from your updates. Also, if you choose not to send the database, all your changes will be lost the next time you update UMEP. 

- Now open SUEWS Database Prepare and use the same settings as you did for the Base-case but change the *Building typology (from polygon layer)* to your new **typologiesKvilleSolar** and change your output folder to a new one named e.g. Solar (*C:/temp/SUEWSDB_tutorial/Solar*). Click *Generate*.

- Run the model again as before but now change your input folder so that information found in your **Solar**-directory is used. Also, make sure that you save in a new output folder, e.g. **./Solar/out**.


Comparing the two scenarios
---------------------------
- Create a basic plot fro grid 1 from the solar panel case, the same way as you did for the base-case. Look at e.g. K\ :sub:`up` for 1 June you can now see a lowering of peak K\ :sub:`up` of 20 W m\ :sup:`-2`. The resulting energy flux e.g. sensible heat flux a small, just a couple of W m\ :sup:`-2`.

The changes in energy fluxes are small but one can see a clear effect when just altering the albedo for a certain urban typology. Have in mind that the fraction of buildings within the detached residential typology for this area is relatively low (see e.g. grid 1), so altering the albedo for the buildings only will not create a very large effect. Nevertheless, there is a small difference evident. 

Finally, we will produce a spatial comparison between the two scenarios. We will make use of our polygon grid (**gridKville**) and populate new attributes for each grid polygon. We will investigate daytime Q\ :sub:`H` for the month of June. First, we will calculate for the base-case

- From the Processing toolbox in QGIS, open *UMEP -> Post-Processor -> Urban Energy Balance: SUEWS Analyzer*. Use the settings shown in the figure below.

.. figure:: /images/SUEWSDatabase_SpatialAnalyser.jpg
   :alt:  none
   :width: 80%

   The SUEWS Analyzer calculating mean daytime Q\ :sub:`H` for the month of June, 2018 (click for a larger image). 
   
- Repeat the task but change to the solar panel case.

Two new attributes should now have appeared in the *gridKville** attribute table.

Lastly, open the field calculator and create a new attribute, subtracting the **QH_1** column (solar panel case) with **QH** (base-case). Now, right-click on gridKville in your *Layer*-panel and click on *Properties*. Here, go to *Symbology* and make a colourful map of the difference between the two scenarios with regards to mean daytime Q\ :sub:`H` in June, 2018.   

Tutorial finished.
