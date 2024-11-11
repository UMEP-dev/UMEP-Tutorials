.. _SUEWSDatabase:

Urban Energy Balance - SUEWS Database
=====================================

.. note:: WORK IN PROGRESS. NOT READY.

Introduction
------------

In this tutorial you will make yourself acquainted with a database system that facilitates use of input data for the SUEWS model. This database can be used for any type of SUEWS application but it is also designed to work with so called *urban typologies* (neighborhoods that are similar in buildings materials etc.)


.. note:: NOT READY BEYOND THIS POINT.

 generate input data for the
`Urban Weather Generator <https://umep-docs.readthedocs.io/en/latest/processor/Urban%20Heat%20Island%20Urban%20Weather%20Generator.html>`__ and simulate spatial (and temporal) variations of urban heat island (UHI) in Gothenburg, Sweden.

The **Urban Weather Generator** (UWG) tool is an implementation of the `Ladybug <https://github.com/ladybug-tools/uwg>`__ application with the same name. The `original Urban Weather Generator <http://urbanmicroclimate.scripts.mit.edu/uwg.php>`__ was developed by Bruno Bueno for `his PhD thesis at MIT <https://dspace.mit.edu/handle/1721.1/59107>`__. Since this time, it has been validated 3 times and has been `enhanced by Aiko Nakano <https://dspace.mit.edu/handle/1721.1/108779>`__. In 2016, Joseph Yang also `improved the engine and added a range of building templates <https://dspace.mit.edu/handle/1721.1/107347>`__. For more detailed information on UWG, follow the links above.

This tutorial makes use of local high resolution detailed spatial data. If this kind of data is unavailable, other datasets derived from e.g. `Open Street Map <https://www.openstreetmap.org/>`__ can be exploited. However, it is strongly recommended to go through this tutorial before moving on to more user-specific datasets.

Objectives
----------

To perform and analyse intra urban temperature variations within an area in Gothenburg, Sweden using the Urban Weather Generator.

Initial Steps
-------------

UMEP is a Python plugin used in conjunction with
`QGIS <http://www.qgis.org>`__. To install the software and the UMEP
plugins see the `getting started <http://umep-docs.readthedocs.io/en/latest/Getting_Started.html>`__ section in the UMEP manual. For this tutorial you will need both **UMEP** and **UMEP for Processing**.

Loading and analyzing the spatial data
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
- There is also a vector polygon layer (**topologiesKville.shp**) describing the different urban topologies within the area. Open the attribute table associated with this layer. Here you see an attribute called *urbantype* which describes the various urban typologies in the area. This is in Swedish so below you find a translation and description for each type.

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
       * - slut
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

The typology poloygon layer is used to describe types of buildings which will affect the building energy model within UWG.

Preparing input data
--------------------

First we need to derive surface fractions etc. from the geodata for each grid. This is similar as done in many other tutorials e.g. `SuewsSpatial`. We start with calculate roughness parameters based on the building geometry within your grids. Open *UMEP > Pre-Processor > Urban Morphology > Morphometric Calculator (Grid)*. 

- Use the settings as in the figure below and press *Run*.
- When calculation ids done, close the plugin.

.. note:: For mac users, use this workaround: manually create a directory, go into the folder above and type the folder name. It will give a warning *“—folder name--” already exists. Do you want to replace it?* Click *replace*.


.. figure:: /images/uwg_IMCGBuilding.jpg
   :alt:  none
   :width: 75%

   The settings for calculating building morphology. Click on image for enlargement.

This operation should have produced 16 different text files; 15 (*anisotrophic*) that include morphometric parameters from each 5 degree section for each grid and one file (*isotropic*) that includes averaged values for each of the 15 grids. You can open **kv_IMPGrid_isotropic.txt** and compare the different values for different grids. Header abbreviations are explained `here <http://umep-docs.readthedocs.io/en/latest/Abbreviations.html>`__.

Moving on to land cover fraction calculations for each grid.

- Open *UMEP > Pre-Processor > Urban Land Cover > Land Cover Fraction (Grid)*.
- Use the settings as in the figure below and press *Run*.
- When calculation is done, close the plugin.

.. figure:: /images/uwg_LCF.jpg
   :alt:  none
   :width: 75%
   
   The settings for calculating land cover fractions

Finally, you need to reclassify the urban typology layer layer into typologies that UWG use.

- Open *UMEP -> Pre-processor -> Urban Heat Island -> UWG Reclassifier* and use the settings below:

.. figure:: /images/uwg_reclassifier.jpg
   :alt:  none
   :width: 60%

   Settings used to reclassify urban typologies into UWG building types.
   


Preparing input data for the Urban Weather Generator
----------------------------------------------------

Now all input information required is pre-processed apart from the final step which is to create the uwg-files used by the model.

- Open **UWG Prepare** (*UMEP > Pre-Processor > Urban Heat Island > UWG prepare*) and use the following settings.

.. figure:: /images/uwg_prepare.jpg
   :alt:  none
   :width: 75%

   Settings for the UWG Prepare plugin (click for a larger image).

Here you can see the various settings that can be modified. 


Meteorological forcing data
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Before we execute the Urban Weather Generator, meteorological forcing data is required. The UWG make use of Energy Plus Weather (EPW) files (.epw). These files are generated for purposes of building energy simulation and are one full year in length (hourly resolution). However, the UWG can preferably model just a portion of a year and not always a full year which will take long computation time, especially if multiple grids are inverstigated. Information on EWP-files and possible downloads for your location can be found `here <https://energyplus.net/weather>`__. In the zip-file downloaded for this tutorial, a .epw file from a nearby airport is availabe. This is a so-called typical meteorological dataset where a typical year in the Gothenburg region is created including natural variation within a year.

Executing the model
-------------------

Open *UMEP -> Processor -> Urban Heat Island: Urban Weather Generator* and use the settings below. Before starting the calculations, open the Python Console in QGIS to see a more detailed information from the model while is runs. The period selected is a warm week in June.

.. figure:: /images/uwg_processor.jpg
   :alt:  none
   :width: 75%

   Settings for the UWG main plugin (click for a larger image).

Analysing the results
---------------------

If you take a look in your output folder, you see a number of UMEP-formatted meteorological files which is the output from the model, one for each grid. First, try to plot grid 9 by opening *UMEP -> Post Processor -> Urban Heat Island -> UWGAnalyzer* and use the settings below beofre clicking **Plot**:


.. figure:: /images/uwg_postprocessor_plot9.jpg
   :alt:  none
   :width: 75%

   Settings for the UWG Post-processing plugin (click for a larger image).
   
The result should look something like this:

.. figure:: /images/uwg_plot.jpg
   :alt:  none
   :width: 100%

   Above: Wind speed and global radiation from epw-file. Below: Air temperature from airport compared with grid 9 (click for a larger image).
   
   
Finally, you can also make a spatial grid from your model reults, both as a raster of add output to your grid polygon layer. You will add a new attribute to your grid polygon layer. Open the same tool but in UMEP for Processing and use the following settings:

.. figure:: /images/uwg_analyzer_spatial.jpg
   :alt:  none
   :width: 80%

   Settings for the UWG Post-processing plugin adding a new attribute field (click for a larger image).

Open the attribute table for your grid and you should have a column called *mean*. As you can see the differences of nocturnal temperature differences between the airport and any specific grid is about 3.5 degrees celsius. However, the differences between the grids are very small. The reason for this could have many answers but one main explanation is that UWG is not very sensitive to vegetation changes that could create temperature variations within a city. Also, the model seem to be unsensitive to small changes in building density and regional climate. See `here <https://gupea.ub.gu.se/handle/2077/76418>`__ for a more detailed investigation on the performance of UWG for the Gothenburg region. The UMEP development team is also adding a new UHI-model (`TARGET <http://umep-docs.readthedocs.io/en/latest/processor/Urban%20Heat%20Island%20TARGET.html>`__) into UMEP that is more sensitive to blue and green infrastructure in urban areas.

Tutorial finished.
