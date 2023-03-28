.. _SpatialTC:

Thermal Comfort - Spatial Thermal Comfort
=========================================

Introduction
------------

In this tutorial you will show you how to produce maps of thermal comfort indices using outputs from two different models in UMEP. 

The two different models used are SOLWEIG (radiation model) and URock (wind model). These two models are combined in the **SpatialTC**-tool to generate raster maps on thermal indices such as PET, UTCI and COMFA. This tutorial will make use of PET as an example.


Steps
~~~~~

#. Produce wind raster from URock
#. Produce T\ :sub:`mrt` raster from SOLWEIG
#. Produce map of PET with SpatialTC

Data for this exercise
~~~~~~~~~~~~~~~~~~~~~~

The UMEP tutorial datasets can be downloaded from our here repository
`here <https://github.com/Urban-Meteorology-Reading/Urban-Meteorology-Reading.github.io/raw/master/other%20files/Annedal_EPSG3006.zip>`__.

-  Download, extract and add the raster layers (DSM, CDSM, DEM), the building polygon layer and the line profile layer (not used in this tutorial) into a new QGIS session. Coordinate system of the grids is Sweref99 TM (EPSG:3006).

Produce result with URock
-------------------------

The output data generated from the introduction tutorial on Urock will be used for this exercise. If you have not gone through `IntroToURock`, do so and make sure that you produce data with vegetation information included (last section of the tutorial).

Produce T\ :sub:`mrt` raster from SOLWEIG
-----------------------------------------

It is recommend to get familiar with the SOLWEIG model before you produce your input for SpatialTC by looking at `IntroToSOLWEIG` but below you will also find instructions on how to generate the data needed. All tools should be executed via **UMEP for Processing**.

#. Some additional data has to be prepared in order to execute the SOLWEIG-model.
   
   -  Open *UMEP -> Pre-Processor -> Urban geometry -> Sky View Factor*.
   -  To run SOLWEIG various sky view factor (SVF) maps for both
      vegetation and buildings must be created (see `Lindberg and
      Grimmond
      (2011) <http://link.springer.com/article/10.1007/s00704-010-0382-8>`__
      for details).
   -  You can create all SVFs needed (vegetation and buildings) at the
      same time. Use the settings as shown below. Use an appropriate
      output folder for your computer. 
	  
    .. figure:: /images/spatialtc_svf.jpg
       :alt:  None
       :width: 100%
       :align: center
       
       Settings for the SkyViewFactorCalculator.
      
   -  If you look in your output folder you will find a zip-file and a .npz-file containing all the
      necessary SVF maps needed to run the SOLWEIG-model.

   -  Another pre-processing plugin is needed to create the building wall heights and aspect. Open *UMEP -> Pre-Processor -> Urban geometry -> Wall height and aspect* and use the settings as shown below. QGIS scales the loaded rasters by a *cumulative count out* approach (98%). As the height and aspect layers are filled with zeros where no wall are present it might appear as if there is no walls identified. Rescale your results to see the walls identified (*Layer Properties > Symbology*).
   
    .. figure:: /images/spatialtc_wallheightaspect.jpg
       :alt:  None
       :width: 100%
       :align: center
       
       Settings for the Wall height and aspect plugin.

#. Open the SOLWEIG plugin and use the settings shown below (see both figures). Do not 
   forget to tick *Save Mean Radiant Temperature raster(s)*. The metfile is found in the downloaded tutorial data (*metfile.txt*) and is a clear (and not very warm) Summer day. Click **Run**. 
   
    .. figure:: /images/spatialtc_solweig1.jpg
       :alt:  None
       :width: 100%
       :align: center
       
       The settings for your SOLWEIG run (click on figure for larger image).
      
    .. figure:: /images/spatialtc_solweig2.jpg
       :alt:  None
       :width: 100%
       :align: center
       
       Continuing.. The settings for your SOLWEIG run (click on figure for larger image).
       

Details of the model inputs and outputs are provided in the `SOLWEIG manual <http://umep-docs.readthedocs.io/en/latest/OtherManuals/SOLWEIG.html>`__. As the focus of  this tutorial is to run **SpatialTC**, only the most critical parameters are used. Many other parameters can be modified to more appropriate values, if applicable.

Produce map of PET with SpatialTC
---------------------------------

Now you will run SpatialTC based on the output from the SOLWEIG and URock run in the previous sections.

You need to load three rasters: the mean radiant temperature that has been produced by SOLWEIG, the pedestrian wind speed produced by URock, and the one that exclude buildings from the analysis (produced by SOLWEIG). You also need to set the meteorological text file that has been used for the calculation of the Tmrt map.

Last you need to select the thermal comfort index to map (PET for this tutorial.  Advanced parameters describing the person to consider for the comfort index can also be defined but the default values are kept for this tutorial. Then click **Run**. 

HERE I AM...


    .. figure:: /images/spatialtc.jpg
       :alt:  None
       :width: 411px
       :align: center
       
       Settings for your Spatial TC tool 
    
When the computation is finished, you should have 

Tutorial finished.

References
----------

`Lindberg F & Grimmond CSB 2011: The influence of vegetation and building morphology on shadow patterns and mean radiant
temperatures in urban areas: model development and evaluation. Theoretical and Applied Climatology, 105, 311-323. <https://doi.org/10.1007/s00704-010-0382-8>`__

