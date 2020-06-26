.. _SUEWSAdvanced:

Urban Energy Balance - SUEWS Advanced
=====================================

Introduction
------------

The tutorial :ref:`IntroductionToSuews` should be completed first. This tutorial is designed to work with QGIS 3.x.

Objectives
~~~~~~~~~~

#. To explore the link between QGIS and SUEWS to include new
   site-specific information
#. To examine how it affects the energy fluxes

Overview of steps
~~~~~~~~~~~~~~~~~

#. Initially become familiar with SUEWS advanced which is a
   plugin that makes it possible for you to set all parameters that can
   be manipulated in SUEWS as well as execute the model on multiple grids (:ref:`SuewsSpatial`).
#. Derive new surface information
#. Run the model

How to run SUEWS Advanced from the UMEP-plugin
----------------------------------------------

#. Open the plugin which is located at *UMEP -> Processor -> Urban Energy
   Balance -> Urban Energy Balance (SUEWS/BLUEWS, Advanced)*. This has
   most of the general settings (e.g. activate the snow module etc.)
   which are related to
   `RunControl.nml <http://suews-docs.readthedocs.io/en/latest/input_files/RunControl/RunControl.html>`__.
#. Use the Input folder: *C:/Users/your_user_name/AppData/Roaming/QGIS/QGIS3/ profiles/default/python/plugins/UMEP/suewsmodel/Input/*

#. Create or enter an **Output directory** of your choice.
#. From the **Input folder** - confirm the data are in there.
#. Tick in **Obtain temporal...** and set **Temporal resolution of output (minutes)** to 60.
#. Click Run
#. Make sure that output files are created.
#. You can now close the **SUEWS/BLUEWS (Advanced)**-plugin again.

.. figure:: /images/SUEWSAdvanced_SuewsAdvanced.png
   :width: 75%
   :align: center
   :alt:  None

   Interface for SUEWS Advanced version.

   
Sensitivity Test
----------------

The default dataset included in **Suews Simple** has parameters
calculated from a `source area
model <http://umep-docs.readthedocs.io/en/latest/pre-processor/Urban%20Morphology%20Source%20Area%20(Point).html>`__
to obtain the appropriate values for the input parameters. Roughness
parameters such as roughness length (z\ :sub:`0`) and zero plane
displacement length (z\ :sub:`d`) are calculated using `morphometric 
models <http://umep-docs.readthedocs.io/en/latest/pre-processor/Urban%20Morphology%20Morphometric%20Calculator%20(Point).html>`__.
Now you will explore the differences in fluxes using the default
settings or using input parameters from the geodata included in the test
datasets available for this tutorial. Download the zip-file (see below)
and extract the files to a suitable location where you have both reading
and writing capabilities.

Data for the tutorial can be downloaded
`here <https://github.com/Urban-Meteorology-Reading/Urban-Meteorology-Reading.github.io/tree/master/other%20files/DataSmallAreaLondon.zip>`__.

.. list-table::

   * - **Geodata**
     - **Name**
   * - Ground and building DSM 
     - DSM_LondonCity_1m.tif (m asl)
   * - Vegetation DSM 
     - CDSM_LondonCity_1m.tif (m agl)
   * - DEM (digital elevation model) 
     - DEM_LondonCity_1m.tif (m asl)
   * - Land cover 
     - LC_londoncity_UMEP_32631
 

They are all projected in UTM 31N (EPSG:32631). The three surface models
originate from a LiDAR dataset. The land cover data is a mixture of
Ordnance Survey and the LiDAR data.

#. Open the geodatasets. Go to *Layer > Add layer > Add Raster Layer*.
   Locate the files you downloaded before (see above). You can also *drag and drop* the data from the *Browser*-panel to the *Layer*-panel in QGIS.
#. A QGIS style file (**landcoverstyle.qml**) is available for the land cover grid. It can
   found in *C:/Users/your_user_name/AppData/Roaming/QGIS/QGIS3/profiles/*
   *default/python/plugins/UMEP/LandCoverReclassifier*. Load it in the *Layer > Properties > Symbology
   > Style* (lower left) **Load Style**.
#. Click **Apply** before you close so that the names of the classes also
   load. You can also get the properties of a layer by right-clicking on a
   layer in the *Layers*-window.
#. If you have another land cover dataset you can use the
   `LandCoverReclassifier <http://umep-docs.readthedocs.io/en/latest/pre-processor/Urban%20Land%20Cover%20Land%20Cover%20Reclassifier.html>`__
   in the UMEP pre-processor to populate with the correct values
   suitable for the UMEP plugin environment.
#. Now take a moment and investigate the different geodatasets. What is
   the spatial (pixel) resolution? How is ground represented (what values) in the
   CDSM?

Generating data from the geodatasets
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

#. Make certain that you have the four geodatafiles open. The file at the top
   of the *layers* window is the one that is shown on the
   canvas (figure below). You can swap their order using the *layers* window.
#. Open *SUEWS Simple*.
#. Begin by adding the test dataset again.
#. You will now update the building morphology parameters (top left panel in Suews
   Simple) by generating new values from the geodata. Click **Open tool...**
#. This is another plugin within UMEP that can be used to generate
   morphometric parameters

   .. figure:: /images/SUEWSAdvanced_QGIS_SuewsSimple.png
      :width: 100%
      :align: center
      :alt:  None

      QGIS where Suews Simple and Image Morphometric Parameters (Point) is opened.

#. First, clear the map canvas from your two other plugin windows, e.g.
   as figure above.
#. If you use the default test data in *SUEWS Simple* - you can overwrite
   is as you go.
#. Locate the eddy covariance tower position on the Strand building,
   King's College London. To find the position, consult Figure 1 (KSS)
   in `Kotthaus and Grimmond
   (2014) <http://www.sciencedirect.com/science/article/pii/S2212095513000503>`__.
#. Use Select point on canvas and put a point at that location.
#. Generate a study area. Use 500 m search distance, 5 degree interval
   and click *Generate study area*. A circular area will be considered.
#. Enter the DSM and DEM files (i.e.
   the files you currently have in the viewer).
#. Use **Kanda et al. (2013)** as *Roughness calculation method* and **build** as *File prefix*.
#. Click *Run*.

   .. figure:: /images/SUEWSAdvanced_SUEWS_MorphometricParametersBuild.jpg
      :width: 75%
      :align: center
      :alt:  None

      Settings for Image Morphometric Parameters for buildings.
	  
#. In the folder you specified two additional files will be present (i)
   isotropic - averages of the morphometric parameters (ii) anisotropic
   - values for each wind sector you specified (5 degrees).
#. Close this plugin
#. Click on *Fetch file from...* in the building morphology panel.
#. Choose the isotropic file (just generated).
#. Do the same for vegetation (upper left panel, right). See figure below.
#. Instead of locating the point again you can use the existing point.
#. You still need to generate a separate study area for the vegetation
   calculation.
#. Examine the **CDSM** (vegetation file) in your map canvas. As you can
   see, this data has no ground heights (ground = 0). Therefore, this
   time Tick the box *Raster DSM (only buildings) exist*.
#. Enter the *CDSM* as your *Raster DSM (only 3D objects)*.
#. Click *Run*.

   .. figure:: /images/SUEWSAdvanced_SUEWS_MorphometricParametersVeg.jpg
      :width: 75%
      :align: center
      :alt:  None

      Settings for Image Morphometric Parameters for vegetation

#. A warning appears that your vegetation fractions between the
   morphology dataset and land cover dataset are large. You can ignore
   this for now since the land cover dataset also will change.
#. Repeat the same procedure for land cover as you did for buildings and vegetation but instead using the Land Cover
   Fraction (Point) plugin. Use **lc** as the *File prefix*
#. Enter the meteorological file, Year etc. This should be the same as the first run you made.
#. Now you are ready to run the model. Click *Run*.


You are now familiar with the full capabilities of the Suews Simple plugin. Your next task is to
choose another location within the geodataset domain, generate data and
run the model. Try to choose an area where the fraction of buildings and
paved surfaces are low. Consider lowering the *Population density* to get
more realistic model outputs. Compare the results for the different areas.

References
----------

-  Grimmond CSB and Oke 1999: Aerodynamic properties of urban areas
   derived, from analysis of surface form. `Journal of Applied
   Climatology 38:9,
   1262-1292 <http://journals.ametsoc.org/doi/abs/10.1175/1520-0450(1999)038%3C1262%3AAPOUAD%3E2.0.CO%3B2>`__
-  Grimmond et al. 2015: Climate Science for Service Partnership: China,
   Shanghai Meteorological Servce, Shanghai, China, August 2015.
-  Järvi L, Grimmond CSB & Christen A 2011: The Surface Urban Energy and
   Water Balance Scheme (SUEWS): Evaluation in Los Angeles and Vancouver
   `J. Hydrol. 411,
   219-237 <http://www.sciencedirect.com/science/article/pii/S0022169411006937>`__
-  Järvi L, Grimmond CSB, Taka M, Nordbo A, Setälä H &Strachan IB 2014:
   Development of the Surface Urban Energy and Water balance Scheme
   (SUEWS) for cold climate cities, , `Geosci. Model Dev. 7,
   1691-1711 <http://www.geosci-model-dev.net/7/1691/2014/>`__
-  Kormann R, Meixner FX 2001: An analytical footprint model for
   non-neutral stratification. `Bound.-Layer Meteorol., 99,
   207-224 <http://www.sciencedirect.com/science/article/pii/S2212095513000497#b0145>`__
-  Kotthaus S and Grimmond CSB 2014: Energy exchange in a dense urban
   environment - Part II: Impact of spatial heterogeneity of the
   surface. `Urban Climate 10,
   281â€“307 <http://www.sciencedirect.com/science/article/pii/S2212095513000497>`__
-  Onomura S, Grimmond CSB, Lindberg F, Holmer B, Thorsson S 2015:
   Meteorological forcing data for urban outdoor thermal comfort models
   from a coupled convective boundary layer and surface energy balance
   scheme. Urban Climate. 11:1-23 `(link to
   paper) <http://www.sciencedirect.com/science/article/pii/S2212095514000856>`__
-  Ward HC, L Järvi, S Onomura, F Lindberg, A Gabey, CSB Grimmond 2016
   SUEWS Manual V2016a, http://urban-climate.net/umep/SUEWS Department
   of Meteorology, University of Reading, Reading, UK
-  Ward HC, Kotthaus S, Järvi L and Grimmond CSB 2016b: Surface Urban
   Energy and Water Balance Scheme (SUEWS): Development and evaluation
   at two UK sites. `Urban Climate
   http://dx.doi.org/10.1016/j.uclim.2016.05.001 <http://www.sciencedirect.com/science/article/pii/S2212095516300256>`__
-  Ward HC, S Kotthaus, CSB Grimmond, A Bjorkegren, M Wilkinson, WTJ
   Morrison, JG Evans, JIL Morison, M Iamarino 2015b: Effects of urban
   density on carbon dioxide exchanges: observations of dense urban,
   suburban and woodland areas of southern England. `Env Pollution 198,
   186-200 <http://dx.doi.org/10.1016/j.envpol.2014.12.031>`__


