.. _IntroductionToSuews:

Urban Energy Balance - SUEWS Introduction
=========================================

Introduction
------------

In this tutorial you will use a land-surface model,
`SUEWS <http://suews-docs.readthedocs.io>`__ to simulate energy
exchanges in a city (London is the test case).

SUEWS (Surface Urban Energy and Water Balance Scheme) allows the energy
and water balance exchanges for urban areas to be modelled (Järvi et al.
`2011 <http://www.sciencedirect.com/science/article/pii/S0022169411006937>`__, `2014 <http://www.geosci-model-dev.net/7/1691/2014/>`__, Ward et al.  `2016a <http://www.sciencedirect.com/science/article/pii/S2212095516300256>`__). The model is applicable at the
neighbourhood scale (e.g. 10\ :sup:`2` to 10\ :sup:`4` m). The fluxes
calculated are applicable to height of about 2-3 times the mean height
of the roughness elements; i.e. above the `roughness sublayer
(RSL) <http://glossary.ametsoc.org/wiki/Roughness_sublayer>`__. The use
of SUEWS within Urban Multi-scale Environmental Predictor (`UMEP <http://umep-docs.readthedocs.io>`__)
provides an introduction to the model and the processes simulated, the
parameters used and the impact on the resulting fluxes. Tools such as this, once appropriately assessed for an area, can be used
for a broad range of applications. For example, for climate services
(e.g. `Grimmond et al (2020) <https://doi.org/10.1016/j.uclim.2020.100623>`_). 

SUEWS can be run in a number of different ways:

#. Within UMEP via the `Simple <https://umep-docs.readthedocs.io/en/latest/processor/Urban%20Energy%20Balance%20Urban%20Energy%20Balance%20(SUEWS,%20simple).html>`__ selection. This is useful for familiarising yourself with the model.
#. Within UMEP via the `Advanced <https://umep-docs.readthedocs.io/en/latest/processor/Urban%20Energy%20Balance%20Urban%20Energy%20Balance%20(SUEWS.BLUEWS,%20advanced).html>`__ selection. This can be used to exploit
   the full capabilities of the model.
#. Via the `UMEP for Processing <https://umep-docs.readthedocs.io/en/latest/UMEPforProcessing.html>`__-plugin.
#. SUEWS standalone (see
   `manual <http://suews-docs.readthedocs.io>`__).
#. `SuPy <https://supy.readthedocs.io/>`__, a Python-enhanced tool for SUEWS.   
#. Within other larger scale models (e.g. `WRF <https://www.mmm.ucar.edu/weather-research-and-forecasting-model>`__).

Tutorial Objectives
-------------------

This tutorial introduces SUEWS and demonstrates how to run the model within UMEP. 

Overview
~~~~~~~~

#. An introduction to the model and how it is designed (above)
#. Different kinds of input data that are needed to run the model
#. How to run the model
#. How to examine the model output

Initial Steps
-------------

UMEP is a python plugin used in conjunction with
`QGIS <http://www.qgis.org>`__. To install the software and the UMEP
plugin see the `getting started <http://umep-docs.readthedocs.io/en/latest/Getting_Started.html>`__ section in the UMEP manual.

As UMEP is constantly under development, some documentation may be missing and/or
there may be instability. Please report any issues or suggestions to our
`repository <https://github.com/UMEP-dev/UMEP>`__.

SUEWS Model Inputs
------------------

Details of the model inputs and outputs are provided in the `SUEWS
manual <http://suews-docs.readthedocs.io>`__. As this tutorial is
concerned with a **simple application**, only the most critical
parameters are shown. Other versions allow many other parameters to be
modified to more appropriate values if applicable. The table below
provides an overview of the parameters that can be modified in the
Simple application of SUEWS.

.. list-table::
   :widths: 20 40 40
   :header-rows: 1
   
   * - Type
     - Definition
     - Reference/Comments
   * -
     - *Building/ Tree Morphology*
     -
   * - Mean height of Building/Trees
     - Mean height of objects (`m agl <https://umep-docs.readthedocs.io/en/latest/Abbreviations.html>`__)
     - Grimmond and Oke  (1999)
   * - Frontal area index 
     - Area of the front face of a roughness element exposed to the wind relative to the plan area.
     - Grimmond and Oke (1999), Fig 2
   * - Plan area index 
     - Area of the roughness elements relative to the total plan area. 
     - Grimmond and Oke (1999), Fig 2 
   * -  
     -
     -     
   * - 
     - *Land cover fraction*
     - Should sum to 1 
   * - Paved 
     - Roads, sidewalks, parking lots, impervious surfaces that are not buildings  
     -  
   * - Buildings
     - Buildings
     - Same as the plan area index of buildings in the morphology section.
   * - Evergreen trees
     - Trees/shrubs that retain their leaves/needles all year round.  
     - Tree plan area index will be the sum of evergreen and deciduous area. Note: same as the plan area index of vegetation in the morphology section.
   * - Deciduous trees
     - Trees/shrubs that lose their leaves
     - Same as above 
   * - Grass
     - Grass
     - 
   * - Bare soil
     - Bare soil – non vegetated but water can infiltrate
     -      
   * - Water
     - Rivers, lakes, ponds, swimming pools, fountains 
     - 
   * -  
     -
     -
   * - 
     - *Initial conditions*
     - What is the state of the conditions when the model run begins? 
   * - Days since rain (days)
     - This will influence irrigation behaviour in the model. If there has been rain recently then it will be longer before irrigation occurs.
     - If this is a period or location when no irrigation is permitted/occurring then this is not critical as the model will calculate from this point going forward.
   * - Daily mean temperature (°C)
     -      
     - Influences irrigation and anthropogenic heat flux.
   * - Soil moisture status (%)
     - Available moisture in soil. This will influence both evaporation and runoff processes.
     - If close to 100% then there is plenty of water for evaporation but also a higher probability of flooding if intense precipitation occurs.
   * -  
     -
     -
   * - 
     - *Other*
     -
   * - Latitude (°)
     - Solar related calculations
     - 
   * - Longitude (°) 
     - Solar related calculations  
     -      
   * - UTC (h)  
     - Time zone
     - Influences solar related calculations 
   * - Height of the meteorological forcing data (m)  
     - 
     - 
   * - Population density (pp/ha) 
     - Influences the antropogenic heat flux 
     - Average daytime and nighttime population density. Can be separated in SUEWS Advanced.
   * - File code 
     - A prefix that can be used to recognise output files from a particular model run. 
     - 


How to Run SuewsSimple from the UMEP-plugin
-------------------------------------------

#. Open SuewsSimple from *UMEP -> Processor -> Urban Energy Balance ->
   Urban Energy Balance (SUEWS, Simple)*. The `GUI <https://umep-docs.readthedocs.io/en/latest/Abbreviations.html>`__ that opens looks quite
   extensive but it is actually not that complicated to start a basic
   model run (figure below). Some additional information about the plugin is
   found in the window on the right had of the GUI. As you can read, a **test dataset** from
   observations for London, UK (`Kotthaus and Grimmond
   2014 <http://www.sciencedirect.com/science/article/pii/S2212095513000503>`__,
   `Ward et al.
   2016a <http://www.sciencedirect.com/science/article/pii/S2212095516300256>`__)
   is included within the plugin.
   
.. figure:: /images/SUEWSIntro_Interface.jpg
    :alt:  none
    :width: 100%

    The interface for SUEWS, simple version (click on image to make it larger).
   
#. To make use of this dataset click on **Add settings from test
   dataset** (see near bottom of the box). The land cover fractions and
   all other settings originate from Kotthaus and Grimmond (2014). They
   used a source area model to obtain the different input parameters
   (their `Fig. 7 in Kotthaus and Grimmond,
   2014 <http://www.sciencedirect.com/science/article/pii/S2212095513000497>`__).
#. Before you start the model, change the location of the output data to
   any location of your choice. Also, make notes on the settings such as
   *Soil moisture state* etc.
#. Do a model run and explore the results by clicking **Run**. Once the calculations are done, some of
   the results are shown in two summary plots (if **Show basic plots of model reults** is ticked).

.. figure:: /images/SUEWSIntro_SuewsSimplefig1.jpg
    :alt:  none
    :width: 100%

    Model output from SUEWS (simple) using the default settings and data (click on image to make it larger).   

    
.. figure:: /images/SUEWSIntro_SuewsSimplefig2.jpg
    :alt:  none
    :width: 100%
    
    Model output from SUEWS (simple) using the default settings and data (click on image to make it larger). 

    
The graphs in the upper figure are the monthly mean energy (left) and water (right)
balance. The lower graphs show the radiation fluxes,
energy fluxes, and water related outputs throughout the year. This plot
includes a lot of data and it might be difficult to examine it in
detail.

To zoom into the plot: use the tools in the top left corner, to zoom to
a period of interest. For example, the Zoom in to about the last ten
days in March (figure below).

.. figure:: /images/SUEWSIntro_SuewsSimplefig2zoom.jpg
    :alt:  none
    :width: 100%
    
    Zoom in on end of March from the daily plot (click on image to make it larger). 


To save a figure, simply use the disk tool in the upper left corner of the figure window. 

Output data Files
-----------------

In the output folder (you selected earlier) you will find (at least)
two files:

#. **Kc1_2011_60.txt** – provides the 60 min model results for the year 2011. **Kc** is the gridcode specified earlier and **1** is the grid number (only one grid in this case). 
#. **Kc1_DailyState.txt** – this provides the daily mean state (see
   SUEWS manual for detailed explanation). This allows you to see, for
   example, the daily state of the LAI (leaf area index).
#. **RunControl.nml** - includes overall settings used in the model run. 

If you open these files in a text editor. To understand the header
variables read the `SUEWS manual <http://suews-docs.readthedocs.io>`__.

Sensitivity to land surface fractions
-------------------------------------

The previous results are for a densely build-up area in
London, UK. In order to test the sensitivity of SUEWS to some surface
properties you can think about changing some of the surface properties
in the SUEWS Simple. For example, change the land cover fraction by:

#. Reducing paved by 20% (0.230) and increase
   grass surface by 20% (0.230). You can also change other values as long as all 
   the fractions *add up to 1.0*. If you change the building or tree fractions you also
   need to change the `PAI <https://umep-docs.readthedocs.io/en/latest/Abbreviations.html>`__ fraction in the two top panels as they are connected to land cover fractions.
#. Save the output to a different folder by selecting *output folder*.
#. Click *Run*.
#. Examine the results and compare with the default run you made previously, e.g. for the ten last days in March 2011. To reproduce the daily basic plot you can exploit *UMEP > Post-Processor > Urban Energy Balance > SUEWS Analyzer*.

    - Open SUEWS Analyzer.
    - Load the **RunControl.nml** found in your output folder for a specific run.
    - In the left panel (**Plot data**), Specify grid no. 1 and Year to investigate 2011.
    - Tick *Plot basic data* and click **Plot**.

Tutorial finished.

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
   207–224 <http://www.sciencedirect.com/science/article/pii/S2212095513000497#b0145>`__
-  Kotthaus S and Grimmond CSB 2014: Energy exchange in a dense urban
   environment – Part II: Impact of spatial heterogeneity of the
   surface. `Urban Climate 10,
   281–307 <http://www.sciencedirect.com/science/article/pii/S2212095513000497>`__
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


Further explanation
-------------------

Morphometric Methods to determine Roughness parameters:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

For more and overview and details see `Grimmond and Oke
(1999) <http://journals.ametsoc.org/doi/abs/10.1175/1520-0450%281999%29038%3C1262%3AAPOUAD%3E2.0.CO%3B2>`__
and `Kent et al.
(2017a) <https://link.springer.com/article/10.1007%2Fs10546-017-0248-z>`__.
This uses the height and spacing of roughness elements (e.g. buildings,
trees) to model the roughness parameters. For more details see `Kent et
al.
(2017a) <https://link.springer.com/article/10.1007%2Fs10546-017-0248-z>`__,
`Kent et al.
(2017b) <http://www.sciencedirect.com/science/article/pii/S0167610516307346?via%3Dihub>`__
and [Kent et al. (2017c)]. UMEP has tools for doing this: *Pre-processor
> Urban Morphology*.

Source Area Model
~~~~~~~~~~~~~~~~~

For more details see `Kotthaus and Grimmond
(2014b) <http://www.sciencedirect.com/science/article/pii/S2212095513000497>`__
and `Kent et al.
(2017a) <https://link.springer.com/article/10.1007%2Fs10546-017-0248-z>`__.
The `Kormann and Meixner
(2001) <https://link.springer.com/article/10.1023%2FA%3A1018991015119>`__
model is used to determine the probable area that a turbulent flux
measurement was impacted by. This is a function of wind direction,
stability, turbulence characteristics (friction velocity, variance of
the lateral wind velocity) and roughness parameters.

