.. _visualization:

=============
Visualization
=============

.. _Ncview:

Ncview
------

.. raw:: html

   <a href="http://cirrus.ucsd.edu/~pierce/software/ncview/index.html" target="_blank">Ncview</a> is simple yet powerful visual browser to open and view netCDF files. This software is useful to check and view the input and output files of WRF-Fire. <br>

-----------------------

In-house Python codes
---------------------

.. _python:
   
Plotting Fire Perimeter and Topography in Idealized Cases
---------------------------------------------------------

.. raw:: html

   This simple Python code plots snap snapshots of fire perimeter together with filled contours of    the domain topography. This code requires several Python libraries to ran successfully. The key library is WRF-Python which is described in: <a href="https://wrf-python.readthedocs.io/en/latest/" target="_blank">https://wrf-python.readthedocs.io/en/latest/</a> The installation guide of WRF-Python library is also available in the above link. Other required libraries are: <br>

.. raw:: html 
  
  <br><a href="https://numpy.org/" target="_blank">- Numpy</a> <br>
  
.. raw:: html 
  
  <a href="https://matplotlib.org/" target="_blank">- NetCDF 4</a> <br>
.. raw:: html 
  
  <a href="https://matplotlib.org/" target="_blank">- Matplotlib</a> <br><br>

.. Note:: It is highly recommended to install all the libraries using Conda and conda-forge repository in a separate environment dedicated to WRF-Fire visualization to avoid any complications.

.. Note:: Basic Python and Matplotlib knowledge is required to use this code.


**How to Install Ncview on Linux**
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Linux users can install Ncview from Advanced Package Tool (apt-get) using the following command:

::

   $ sudo apt-get install ncview

**How to Install Ncview on Mac**
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. raw:: html

   Mac users can install Ncview using <a href="https://brew.sh/" target="_blank">Homebrew</a>: <br><br>
  
::

   $ brew install ncview

**How to Use Ncview**
~~~~~~~~~~~~~~~~~~~~~

netCDF files can be opened in Ncview using the following command on both Linux and Mac:

::

   $ ncview (path to the netcdf file)/(netcdf file)

After the file is opened in Ncview, variables in the netCDF file can be viewed from variables menu (see below figure).
 
.. image:: images/v3.png
  :align: center
  :width: 700
  :height: 400
  :alt: Alternative text

.. centered:: Ncview window after opening a netCDF file

After selecting the variable of interest, it will be shown in XY plane using color-filled contours.


.. image:: images/v2.png
  :align: center
  :width: 700
  :height: 400
  :alt: Alternative text
 
.. centered:: Ncview window after choosing a variable

.. image:: images/xy.png
  :align: center
  :width: 500
  :height: 400
  :alt: Alternative text
 
.. centered:: Output window of Ncview showing the value of the chosen variable in XY plane

Ncview can also plot 2D graphs of the selected variable. By left-clicking on a point of interest from the output window, Ncview generates a graph showing the value of the chosen variable with respect to a transect passing the chosen point.

.. image:: images/v1.png
  :align: center
  :width: 700
  :height: 400
  :alt: Alternative text

.. centered:: Sample of Ncview generated 2D graph



