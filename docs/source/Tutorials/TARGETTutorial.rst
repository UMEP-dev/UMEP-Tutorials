.. _TARGETTutorial:

Urban Heat Island - TARGET
==========================

.. note:: WORK IN PROGRESS. NOT READY.

Introduction
------------

In this tutorial you will generate input data for the
`TARGET model <https://umep-docs.readthedocs.io/en/latest/processor/Urban%20Heat%20Island%20TARGET.html>`__ and simulate spatial
(and temporal) variations of air temperature for an area in Gothenburg, Sweden.

The **TARGET** tool is an implementation of the Python version of `TARGET <https://github.com/jixuan-chen/target>`__ application with the same name. The original TARGET model was presented in `Broadbent at al. 2019 <https://gmd.copernicus.org/articles/12/785/2019/>`__.

This tutorial makes use of local high resolution detailed spatial data. If this kind of data is unavailable, other datasets derived from e.g. `Open Street Map <https://www.openstreetmap.org/>`__ can be exploited. However, it is strongly recommended to go through this tutorial before moving on to more user-specific datasets.

Objectives
----------

To perform and analyse intra urban temperature variations within an area is Gothenburg, Sweden using the TARGET model.

Initial Steps
-------------

UMEP is a Python plugin used in conjunction with
`QGIS <http://www.qgis.org>`__. To install the software and the UMEP
plugins see the `getting started <http://umep-docs.readthedocs.io/en/latest/Getting_Started.html>`__ section in the UMEP manual. For this tutorial you will need both **UMEP** and **UMEP for Processing**.

Loading and analyzing the spatial data
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

All the geodata used in this tutorial can be downloaded from `here <https://github.com/Urban-Meteorology-Reading/Urban-Meteorology-Reading.github.io/blob/master/other%20files/GBG_torgg_3006.zip>`__. 

- Unzip and place in a folder that you have read and write access to.
- Start by loading all the raster and vector datasets into an empty QGIS project. All layers are in SWEREF99 TM (EPSG:3006) CRS.

The order in the *Layers Panel* determines what layer is visible. You can choose to show a layer (or not) with the tick box. You can modify layers by right-clicking on a layer in the *Layers Panel* and choose *Properties*. Note for example that that CDSM (vegetation) is given as height above ground (meter) and that all non-vegetated pixels are set to zero. This makes it hard to get an overview of all 3D objects (buildings and trees). QGIS default styling for a raster is using the 98 percentile of the values. Therefore, not all the range of the data is shown in the layer window to the left.

- Right-click on your **CDSM** layer and go to *Properties > Style* and choose **Singleband pseudocolor** with a min value of 0 and max of 35. Choose a colour scheme of your liking.
- Go to *Transparency* and add an additional no data value of 0. Click ok.
- Now put your **CDSM** layer at the top and your **DSM** layer second in your *Layers Panel*. Now you can see both buildings and vegetation 3D object in your map canvas.

The land cover grid comes with a specific QGIS style file.

- Right-click on the land cover layer (**torgg_lc**) and choose *Properties*. At the bottom left of the window there is a *Style*-button. Choose *Load Style* and open **landcoverstyle.qml** and click OK.
- Make only your land cover class layer visible to examine the spatial variability of the different land cover classes.

The land cover grid has already been classified into the seven different classes used in most UMEP applications (see `Land Cover Reclassifier <http://umep-docs.readthedocs.io/en/latest/pre-processor/Urban%20Land%20Cover%20Land%20Cover%20Reclassifier.html>`__). If you have a land cover dataset that is not UMEP formatted you can use the *Land Cover Reclassifier* found at *UMEP > Pre-processor > Urban Land Cover > Land Cover Reclassifier* in the menu-bar to reclassify your data. TARGET land cover infromation is in somewhat different classes that the standard UMEP classes. The updated Land Cover Reclassifier includes possibilities to pre-process data into all nine classes used by TARGET. In the table below you see the classes used and how they relate to the standard UMEP classes:

    .. list-table::
       :widths: 20 20 60
       :header-rows: 1

       * - TARGET class
         - UMEP class
         - Comment
       * - roof
         - buildings
         -  
       * - road
         - paved
         - Total impervious surface in UMEP 
       * - watr
         - water
         -  
       * - conc
         - NA
         - Part of paved. Impervious in TARGET is both divided up between road and concrete. 
       * - Veg
         - Conifer and Deciduous trees
         - TARGET only have on class of trees whereas UMEP as two.
       * - dry
         - grass
         - TARGET divides grass into two classes either dry or completely wet.
       * - irr
         - grass
         - TARGET divides grass into two classes either dry or completely wet.
       * - NA
         - bare soil
         - TARGET has no bare soil cless. Bare soil becomes dry grass if UMEP land cover is used.          

         
A polygon grid (300 m x 300 m) to define the study area and individual grids is included (**gbgcity_grid_300m.shp**). Such a grid can be produced directly from the *Processing Toolbox* in QGIS (*Create Grid*) or an external grid can be used.

- Load the vector layer **gbgcity_grid_300m.shp** into your QGIS project.
- In the *Style* tab in layer *Properties*, choose a *Simple fill* and set *Fill style* to *No Brush*, to be able to see the spatial data within each grid.
- Also, add the label IDs for the grid to the map canvas in *Properties > Labels > Single Labels* to make it easier to identify the different grid squares later on in this tutorial.

Preparing input data
--------------------

First we need to derive surface fractions etc. from the geodata for each grid. This is similar as done in many other tutorials e.g. `SuewsSpatial`. We start to calculate roughness parameters based on the building geometry within each grids. Open *UMEP > Pre-Processor > Urban Morphology > Morphometric Calculator (Grid)*. 

- Use the settings as in the figure below and press *Run*.
- When calculation is done, close the plugin.

.. note:: For mac users, use this workaround: manually create a directory, go into the folder above and type the folder name. It will give a warning *“—folder name--” already exists. Do you want to replace it?* Click *replace*.


.. figure:: /images/target_IMCGBuilding.jpg
   :alt:  none
   :width: 75%

   The settings for calculating building morphology. Click on image for enlargement.

This operation should have produced 21 different text files; 20 (*anisotrophic*) that include morphometric parameters from each 5 degree section for each grid and one file (*isotropic*) that includes averaged values for each of the 20 grids. You can open **torggbuild_IMPGrid_isotropic.txt** and compare the different values for different grids. Header abbreviations are explained `here <http://umep-docs.readthedocs.io/en/latest/Abbreviations.html>`__.

Moving on to land cover fraction calculations for each grid.

- Open *UMEP > Pre-Processor > Urban Land Cover > Land Cover Fraction (Grid)*.
- Use the settings as in the figure below and press *Run*.
- When calculation is done, close the plugin.

.. figure:: /images/target_LCF.jpg
   :alt:  none
   :width: 75%
   
   The settings for calculating land cover fractions

As you noticed, we did not tick in **Calculate fractions for TARGET..**. As our land cover grid only included the seven standard UMEP land cover classes, we will deal with the two extra classes in the next step.

Preparing input data for the TARGET model
-----------------------------------------

Now all input information required is pre-processed apart from the final step which is to create the actual input files and folder structure for TARGET.

- Open **TARGET Prepare** (*UMEP > Pre-Processor > Urban Heat Island > TARGET prepare*) and use the following settings.

.. figure:: /images/target_prepare.jpg
   :alt:  none
   :width: 75%

   Settings for the TARGET Prepare plugin (click for a larger image).

Here we add fractions to the two missing classes by ticking in **Use standard UMEP land cover...**. As you notice, this is a simplification and could be more detailed if a 9-class land cover grid was exploited.  


Meteorological forcing data
~~~~~~~~~~~~~~~~~~~~~~~~~~~

TARGET requires a meteorological forcing data flie. The TARGET make use of user-specific formatted weather data input. These files could be automatically generated from UMEP standard meteorological files (see `Metdata Processor <https://umep-docs.readthedocs.io/en/latest/pre-processor/Meteorological%20Data%20MetPreprocessor.html>`__). In this tutorial, you are provided with s dataset from ERA5 covering the year 2018 for the Gothenburg region. This data could have been dowmnload via the `Meteorological Data: Downlaod data (ERA5)  <https://umep-docs.readthedocs.io/en/latest/pre-processor/Meteorological%20Data%20Download%20data%20%28ERA5%29.html>`__ tool in UMEP but to save some time we have done it for you.


Executing the model
-------------------

Now, lets run the TARGET model. Open *UMEP -> Processor -> Urban Heat Island: TARGET* and use the settings below. Before starting the calculation, open the Python Console in QGIS to see a more detailed information from the model while is runs. The period selected is the month of May, 2018.



.. figure:: /images/target_processor.jpg
   :alt:  none
   :width: 75%

   Settings for the TARGET main plugin (click for a larger image).

Analysing the results
---------------------

If you take a look in your output folder, you see a number of UMEP-formatted meteorological files which is the output from the model, one for each grid. First, try to plot grid 11 between May 7 and 17 by opening *UMEP -> Post Processor -> Urban Heat Island -> UWGAnalyzer* and use the settings below beofre clicking **Plot**:


.. figure:: /images/target_postprocessor_plot11gui.jpg
   :alt:  none
   :width: 75%

   Settings for the UWG Post-processing plugin (click for a larger image).
   
The result should look something like this:

.. figure:: /images/target_postprocessor_plot11.jpg
   :alt:  none
   :width: 100%

   Above: Wind speed and global radiation from met forcing file (ERA5 in this case). Below: Air temperature from forcing data compared with grid 11 (click for a larger image). You can also try to plot grid 19 and see how vegetation and less buildings affect the result.
   
   
Finally, you can also make a spatial grid from your model reults, both as a raster of add output to your grid polygon layer. Open the same tool but in **UMEP for Processing** and use the following settings:

.. figure:: /images/target_analyzer_spatial.jpg
   :alt:  none
   :width: 80%

   Settings for the TARGET Post-processing plugin create an dirunal average temperature difference map (click for a larger image).

Tutorial finished.
