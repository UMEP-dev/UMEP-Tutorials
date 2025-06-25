.. _tutorials_intro:


Welcome to the UMEP tutorial website
====================================

.. image:: https://readthedocs.org/projects/tutorial-docs/badge/?version=latest
   :target: https://umep-docs.readthedocs.io/projects/tutorial/en/latest/?badge=latest
   :alt: Documentation Status


To help users getting started with UMEP, the community is working on
setting up tutorials and instructions for different parts of the UMEP
tool. The tutorials available are found in the table below.

.. note:: One essential part when working with geodata in a GIS is to make sure that a common coordinate reference system (CRS) is used, both for the data itself and the current QGIS-project you are working in. For more info, see `here <https://docs.qgis.org/3.10/en/docs/user_manual/working_with_projections/working_with_projections.html>`__. It is strongly recommended to reproject/transform all geodatasets into the same projected coordinate system before any processing starts as well using a CRS that is based on meters.


.. list-table::
   :widths: 13 17 23 35 12
   :header-rows: 1

   * - Topic
     - Parts of UMEP
     - Name
     - Application
     - Location
   * - Introducing UMEP - Daily shading
     - Processor and Post-processor
     - `DailyShading`
     - Shadow patterns for pedestrians
     - Gothenburg, Sweden
   * - Solar Energy
     - Pre-Processor, Processor and Post-processor
     - `SEBE`
     - Amount of solar energy received on building facets
     - Gothenburg, Sweden; London, UK
   * - Source Area Footprint
     - Pre-Processor
     - `Footprint`
     - Interpretation of eddy covariance flux source areas
     - London, UK
   * - Spatial data
     - Pre-Processor
     - `DSMGenerator`
     - Generating DSMs using `Open Street Map <https://www.openstreetmap.org/>`__ data
     - Gothenburg, Sweden (World)
   * - Spatial data
     - Pre-Processor and `FUSION/LDV <http://forsys.cfr.washington.edu/FUSION/fusion_overview.html>`__
     - `LidarProcessing`
     - Generating UMEP input data from a LiDAR point cloud
     - Gothenburg, Sweden
   * - Urban energy balance
     - Processor
     - `IntroductionToSuews`
     - Energy, water and radiation fluxes for one location
     - London, UK
   * - Urban energy balance
     - Pre-Processor and Processor
     - `SUEWSAdvanced`
     - Energy, water and radiation fluxes for one location
     - London, UK
   * - Urban energy balance
     - Pre-Processor, Processor and Post-processor
     - `SUEWSSpatial`
     - Energy, water and radiation fluxes for a spatial grid
     - New York City, US
   * - Urban energy balance
     - Pre-Processor, Processor and Post-processor
     - `SUEWSWUDAPT`
     - Making use of `WUDAPT <http://www.wudapt.org/>`__ local climate zones in `SUEWS  <http://suews-docs.readthedocs.io>`__
     - New York City, US
   * - Urban Energy Balance
     - Pre-Processor, Processor, Post-Processor
     - `SUEWSDatabase`
     - Energy, water and radiation fluxes for a spatial grid using a urban typology database 
     - Gothenburg, Sweden
   * - Anthropogenic heat
     - Processor
     - `LQF`
     - Anthropogenic heat modelling in London using LQF (LUCY methodology)
     - London, UK
   * - Anthropogenic heat
     - Processor
     - `GQF`
     - Anthropogenic heat modelling in London using GQF (GreaterQF methodology)
     - London, UK
   * - Outdoor thermal comfort
     - Pre-Processor, Processor and Post-processor
     - `IntroToSOLWEIG`
     - Mean radiation temperature modelling in complex urban settings
     - Gothenburg, Sweden
   * - Outdoor Thermal Comfort
     - Pre-Processor, Processor
     - `IntroToTreePlanter`
     - Locating trees to mitigate heat stress
     - Gothenburg, Sweden
   * - Urban Wind Fields
     - Pre-Processor, Processor, Post-Processor
     - `IntroToURock`
     - Modelling of 3D wind in urban areas
     - Gothenburg, Sweden
   * - Outdoor Thermal Comfort
     - Pre-Processor, Processor, Post-Processor
     - `SpatialTC`
     - Generating raster maps of outdoor thermal comfort
     - Gothenburg, Sweden
   * - Urban Heat Island
     - Pre-Processor, Processor, Post-processor
     - `TARGETTutorial`
     - Spatial variation of air temperature 
     - Gothenburg, Sweden
   * - Urban Heat Island
     - Pre-Processor, Processor, Post-processor
     - `UWGSpatial`
     - Spatial variation of air temperature 
     - Gothenburg, Sweden
   * - UMEP for Processing
     - Pre-Processor, Processor
     - `ProcessingSEBE`
     - Create UMEP workflows using the Processing Toolbox in QGIS
     - Gothenburg, Sweden
   * - Python Scripting
     - Processor
     - `PythonProcessing1`
     - Python in QGIS/UMEP and Jupyter Notebook for Windows 
     - Gothenburg, Sweden     
   * - Python Scripting
     - Pre-Processor, Processor
     - `PythonProcessing2`
     - Python in QGIS/UMEP for processing in Linux/Mac
     - Gothenburg, Sweden     


**Conference specific tutorials**


.. list-table::
   :widths: 13 17 23 35 12
   :header-rows: 1

   * - Topic
     - Parts of UMEP
     - Name
     - Application
     - Location
   * - Outdoor thermal comfort
     - Pre-Processor, Processor and Post-processor
     - `IntroToSOLWEIGICUC12`
     - Mean radiation temperature modelling in complex urban settings
     - Rotterdam, The Netherlands
   * - Solar Energy in Norrköping - Daily shading
     - Pre-procesoor, Processor and Post-processor
     - `Link to pdf <https://github.com/UMEP-dev/UMEP-Tutorials/blob/master/docs/source/data/UMEP_Workshop_QGISNorrkoping2025.pdf>`__
     - Specially designed tutorial for the QGIS 2025 User Confernce in Norrköping
     - Norrköping, Sweden

..   * - Urban energy balance
..     - Pre-Processor, Processor and Post-processor
..     - `SUEWSWUDAPT_Beijing`
..     - Making use of `WUDAPT <http://www.wudapt.org/>`__ local climate zones as well as `WATCH <http://www.eu-watch.org/>`__ meteorological forcing data in `SUEWS  <http://suews-docs.readthedocs.io>`__
..     - Beijing, China

Direct link back to `the UMEP Manual <https://umep-docs.readthedocs.io/>`__.

.. toctree::
   :caption: UMEP Tutorials
   :maxdepth: 2
   :hidden:

   Tutorials/DailyShading
   Tutorials/SEBE
   Tutorials/Footprint
   Tutorials/DSMGenerator
   Tutorials/LidarProcessing
   Tutorials/IntroductionToSuews
   Tutorials/SuewsAdvanced
   Tutorials/SuewsSpatial
   Tutorials/SuewsWUDAPT
   Tutorials/SUEWSDatabase
   Tutorials/LQF
   Tutorials/GQF
   Tutorials/IntroductionToSolweig
   Tutorials/IntroductionToTreePlanter
   Tutorials/IntroductionToURock
   Tutorials/SpatialTC
   Tutorials/TARGETTutorial
   Tutorials/UWGSpatial
   Tutorials/IntrodutionToProcessingSEBE
   Tutorials/PythonProcessing1
   Tutorials/PythonProcessing2
   
   
.. Tutorials/SUEWSWUDAPT_Beijing

