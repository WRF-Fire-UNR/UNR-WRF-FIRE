.. WildfireWiki documentation master file, created by
   sphinx-quickstart on Sun Jul 11 11:17:23 2021.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Welcome to WRF-Fire Wiki Page!
==============================

Introduction to WRF-Fire
------------------------
 
How to Use this Wiki and Where to Begin?
----------------------------------------
This Wiki aims to tutor WRF-Fire using a variety of examples with incrementally increasing level of complexity, ranging from basic uncoupled models to nested coupled models in the LES mode. In addition, tutorials are provided on :ref:`Compiling WRF-Fire for Mac <compileMac>`, :ref:`Compiling WRF-Fire for Linux <compileLin>`, :ref:`Running WRF-Fire <runWRF>`, :ref:`Key WRF-Fire and WRF commands <keyCommands>`, and :ref:`Visualization tools <visualization>`.

**If you have preliminary experience with WRF and/or WRF-Fire,** start with the examples along with the commands tutorials to learn WRF-Fire. You can also review the visualization tutorials, which include Python codes developed exclusively for visualizing WRF-Fire outputs.

**If you are a new user to WRF world,** we suggest to pursue the following steps to get acquainted with WRF-Fire. 

 * :ref:`Step 1:<DCR>` Run WRF-Fire for idealized cases.
 * :ref:`Step 2:<idealized>` Compile ne of the test cases, namely “hill_simple” and “two_fires”, distributed with WRF-Fire.
 * :ref:`Step 3:<Ncview>` Visualize the outputs of test cases using Ncview.
 * :ref:`Step 4:<tutorials>` After getting acquainted with WRF-Fire, follow the examples and commands tutorials to learn how to create different WRF-Fire models. You can also follow visualization tutorials for more visualization tools and options.

Enjoy learning WRF-Fire!

About Us 
--------
This Wiki has been developed through collaboration between teams from the University of Nevada, Reno and University Corporation for Atmospheric Research (UCAR). The development of this wiki page has been sponsored by the `NSF LEAP-HI Grant #1953333 <https://www.nsf.gov/awardsearch/showAward?AWD_ID=1953333&HistoricalAwards=false>`_ and UCAR funding.

The contributing team from the University of Nevada, Reno includes:

.. raw:: html
   
   <p style="margin-left:2.5%;">- <a href="https://www.unr.edu/cee/people/hamed-ebrahimian" target="_blank">Hamed Ebrahimian</a>, Assistant Professor<br>

.. raw:: html

   <p style="margin-left:2.5%;">- Kasra Shamsaei, Ph.D. Student <br>

.. raw:: html

   <p style="margin-left:2.5%;">- Honggang Wang, Postdoctoral Fellow <br>

.. raw:: html

   <p style="margin-left:2.5%;">- Evan Chinn, B.Sc. Student<br>

.. raw:: html

   <p style="margin-left:0%;">To read more about our project and progress, <a href="https://packpages.unr.edu/wildfireproject" target="_blank">visit here</a>.

The contributing team from the UCAR includes:

.. raw:: html

   <p style="margin-left:2.5%;">- <a href="https://staff.ucar.edu/users/branko" target="_blank">Branko Kosovic</a>, Director of the Weather Systems and Assessment Program, NCAR <br>

.. raw:: html

   <p style="margin-left:2.5%;">- <a href="https://staff.ucar.edu/users/tjuliano" target="_blank">Timothy Juliano</a>, Project Scientist I, NCAR <br>

For questions and comment about this wiki page, please contact Dr. Hamed Ebrahimian at hebrahimian@unr.edu. 

For technical questions about WRF-Fire, please contact Dr. Branko Kosovic at branko@ucar.edu. 

.. note::

   This project is under active development.

.. toctree::
   :maxdepth: 2
   :caption: Contents

   DCR
   input_files
   case_studies 
   visualization
   faq
   forum
