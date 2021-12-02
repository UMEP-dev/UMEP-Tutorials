.. _UWGSpatial:

Urban Heat Island - Urban Weather Generator
===========================================


.. note:: Under Construction. NOT READY


Introduction
------------

In this tutorial you will generate input data for the
`Urban Weather Generator <http://suews-docs.readthedocs.io>`__ and simulate spatial
(and temporal) variations of urban heat island (UHI) in Gothenburg, Sweden.

The **Urban Weather Generator** (UWG) tool is an implementation of the `Ladybug <https://github.com/ladybug-tools/uwg>`__ application with the same name. The `original Urban Weather Generator <http://urbanmicroclimate.scripts.mit.edu/uwg.php>`__ was developed by Bruno Bueno for `his PhD thesis at MIT <https://dspace.mit.edu/handle/1721.1/59107>`__. Since this time, it has been validated 3 times and has been `enhanced by Aiko Nakano <https://dspace.mit.edu/handle/1721.1/108779>`__. In 2016, Joseph Yang also `improved the engine and added a range of building templates <https://dspace.mit.edu/handle/1721.1/107347>`__. For more detailed information on UWG, follow the links above.

This tutorial makes use of local high resolution detailed spatial data. If this kind of data is unavailable, other datasets derived from e.g. `Open Street Map <https://www.openstreetmap.org/>`__ can be exploited. However, it is strongly recommended to go through this tutorial before moving on to more user-specific datasets.

Objectives
----------

To perform and analyse UHI within an area is Gothenburg, Sweden.


Initial Steps
-------------

UMEP is a Python plugin used in conjunction with
`QGIS <http://www.qgis.org>`__. To install the software and the UMEP
plugin see the `getting started <http://umep-docs.readthedocs.io/en/latest/Getting_Started.html>`__ section in the UMEP manual.

**NOT READY BEYOND THIS POINT**

Loading and analyzing the spatial data
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

All the geodata used in this tutorial can be downloaded from here [NOT READY].

- Start by loading all the raster and vector datasets into an empty QGIS project.

The order in the *Layers Panel* determines what layer is visible. You can choose to show a layer (or not) with the tick box. You can modify layers by right-clicking on a layer in the *Layers Panel* and choose *Properties*. Note for example that that CDSM (vegetation) is given as height above ground (meter) and that all non-vegetated pixels are set to zero. This makes it hard to get an overview of all 3D objects (buildings and trees). QGIS default styling for a raster is using the 98 percentile of the values. Therefore, not all the range of the data is shown in the layer window to the left.

- Right-click on your **CDSM** layer and go to *Properties > Style* and choose **Singleband pseudocolor** with a min value of 0 and max of 35. Choose a colour scheme of your liking.
- Go to *Transparency* and add an additional no data value of 0. Click ok.
- Now put your **CDSM** layer at the top and your **DSM** layer second in your *Layers Panel*. Now you can see both buildings and vegetation 3D object in your map canvas.

The land cover grid comes with a specific QGIS style file.

- Right-click on the land cover layer (**NAME NOT READY**) and choose *Properties*. At the bottom left of the window there is a *Style*-button. Choose *Load Style* and open **landcoverstyle.qml** and click OK.
- Make only your land cover class layer visible to examine the spatial variability of the different land cover classes.

The land cover grid has already been classified into the seven different classes used in most UMEP applications (see `Land Cover Reclassifier <http://umep-docs.readthedocs.io/en/latest/pre-processor/Urban%20Land%20Cover%20Land%20Cover%20Reclassifier.html>`__). If you have a land cover dataset that is not UMEP formatted you can use the *Land Cover Reclassifier* found at *UMEP > Pre-processor > Urban Land Cover > Land Cover Reclassifier* in the menu-bar to reclassify your data.

Furthermore, a polygon grid (500 m x 500 m) to define the study area and individual grids is included (**NAME_NOT_READY**). Such a grid can be produced directly from the *Processing Toolbox* in QGIS (*Create Grid*) or an external grid can be used.

- Load the vector layer **NAME_NOT_READY** into your QGIS project.
- In the *Style* tab in layer *Properties*, choose a *Simple fill* and set *Fill style* to *No Brush*, to be able to see the spatial data within each grid.
- Also, add the label IDs for the grid to the map canvas in *Properties > Labels > Single Labels* to make it easier to identify the different grid squares later on in this tutorial.


Meteorological forcing data
~~~~~~~~~~~~~~~~~~~~~~~~~~~

NOT READY



Preparing input data for the Urban Weather Generator
----------------------------------------------------

A key capability of UMEP is to facilitate preparation of input data for the various models. UWG requires input information to model the urban heat island. The plugin *UWG Prepare* and *UWG Reclassifier* serves this purpose. 

- Open SUEWS Prepare (*UMEP > Pre-Processor > Urban Heat Island > UWG prepare*).

.. figure:: /images/SUEWSSpatial_Prepare1.png
   :alt:  none
   :width: 100%

   The dialog for the SUEWS Prepare plugin (click for a larger image).

Here you can see the various settings that can be modified. 

Tutorial finished.
