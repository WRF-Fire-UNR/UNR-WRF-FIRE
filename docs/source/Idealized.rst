.. _idealized:

=====================================
How to Run WRF-Fire in Idealized Case
=====================================

Linking the Input Files
-----------------------
The input files of the case should be linked before running the WRF-Fire model using soft links. Commands to soft link the input files are as follows:

.. raw:: html

   <p style="margin-left:1%;"> - Soft link for namelist.input:

::

   $ ln -sf (path to created input files)/(created namelist.input) namelist.input 

.. raw:: html

   <p style="margin-left:1%;">- Soft link for input_sounding:

::

   $ ln -sf (path to created input files)/(created input_sounding) input_sounding

.. raw:: html

   <p style="margin-left:1%;"> - Soft link for namelist.fire:

::

   $ ln –sf (path to created input files)/(created namelist.fire) namelist.fire

Running WRF-Fire model
In an idealized case, first, the initialization software should be run to generate the input files required by WRF-Fire model. After the input files are generated, the model can be run.

Running The Initialization Software
-----------------------------------

The following command can be used to run the initialization software, called “ideal.exe”:

::

   $ ./ideal.exe

For future references, the output of the above command can be redirected to an external file. For instance:

::
 
   $ ./ideal.exe >& ideal.out

If the initialization is successful, the last printed line in the terminal window or the external file should be:

::

   $ wrf: SUCCESS COMPLETE IDEAL INIT

Running WRF-Fire Model
----------------------

.. raw:: html

   After successful initialization, the WRF-Fire model can be run using the following commands in serial and parallel modes: <br>

For serial run:

::

   $ ./wrf.exe >& wrf.out

In successful WRF-Fire runs, the last printed line in the model output should be:

::

   wrf: SUCCESS COMPLETE WRF

For parallel run:

::

   $ mpirun –np (number of CPUs) ./wrf.exe
 
.. note ::

   The number of CPUs in parallel mode must be squares in order to have perfect and even decomposition of the model.

In parallel runs, pairs of “rsl.out.*” and “rsl.error.*” files will be created, which are standard output and error files, respectively. in successful parallel runs, the last of line of the “rsl.*.0000” file should be:

:: 

   wrf: SUCCESS COMPLETE WRF
   
Plotting Fire Perimeter and Topography in Idealized Cases
---------------------------------------------------------

.. raw:: html

This simple Python code plots snap snapshots of fire perimeter together with filled contours of the domain topography. This code requires several Python libraries to ran successfully. The key library is WRF-Python which is described in: <a href="https://wrf-python.readthedocs.io/en/latest/" target="_blank">https://wrf-python.readthedocs.io/en/latest/</a> The installation guide of WRF-Python library is also available in the above link. Other required libraries are:

.. raw:: html 
  
  <a href="https://numpy.org/" target="_blank">- Numpy</a> <br>
  
.. raw:: html 
  
  <a href="https://matplotlib.org/" target="_blank">- NetCDF 4</a> <br>
.. raw:: html 
  
  <a href="https://matplotlib.org/" target="_blank">- Matplotlib</a> <br><br>

.. Note:: It is highly recommended to install all the libraries using Conda and conda-forge repository in a separate environment dedicated to WRF-Fire visualization to avoid any complications.

.. Note:: Basic Python and Matplotlib knowledge is required to use this code.

Python Code Description
-----------------------

The Python code to plot fire perimeter and topography in idealized cases can be downloaded from here and is presented below.

::

   #Developed by Kasra Shamsaei, Ph.D. Student, Department of Civil and Environmental Engineering, University of Nevada Reno
   #Version: 2.0
   import numpy as np
   from netCDF4 import Dataset
   import matplotlib.pyplot as plt
   import math
   import wrf

   def wrf_out_read (dir, time):
       mins= time % 60
       hrs = math.floor(time / 60)
       wrfout = Dataset('{}wrfout_d01_0001-01-01_{:02d}:{:02d}:00'.format(dir, hrs, mins)) #Format of the name of the output file must be corrected accordingly
       return wrfout

   def relax_zone_remover (input, sr):
       output = input
       for _ in range(sr):
           output = np.delete(output, -1, 0)
           output = np.delete(output, -1, 1)
       return output
   def fire_perimeter_plot(xf, yf, lfn, color):
       #removing the relaxation zones of the level-set function
       lfn_reinit = relax_zone_remover(lfn, sr)
       xf = relax_zone_remover(xf, sr)
       yf = relax_zone_remover(yf, sr)
       #Plotting the fire perimeter
       ax.contour(xf, yf, lfn_reinit, 0, colors='r')
       ax.plot([], [], color=color, label='Fire Line ($\phi>0)$')

   def wind_plot (data, xcoords, ycoords, field_1, field_2, height_value, color):
       u = wrf.getvar(data, field_1, timeidx=wrf.ALL_TIMES, method='cat', meta=False)
       v = wrf.getvar(data, field_2, timeidx=wrf.ALL_TIMES, method='cat', meta=False)
       u = u[height_value, :, :]
       v = v[height_value, :, :]

   wf = plt.quiver(xcoords[::8,::8], ycoords[::8,::8], u[::8,::8], v[::8,::8], color = color, scale = 8, scale_units = 'xy', pivot = 'tail', width = 0.002) 
       return wf


   out_time = 65 #Output time to plot in minutes
   sr = 4 #sub-grid ratio

   #wrfout files with reinit
   outs_folder = '(path to WRF-Fire output files)'
   wrfouts_reinit = wrf_out_read(outs_folder, out_time)

   #reading coordiantes
   x = wrf.getvar(wrfouts_reinit, 'XLONG', timeidx=wrf.ALL_TIMES, method='cat', meta=False) / 1000   #converting coordinates to km
   y = wrf.getvar(wrfouts_reinit, 'XLAT', timeidx=wrf.ALL_TIMES, method='cat', meta=False) / 1000
   xf = wrf.getvar(wrfouts_reinit, 'FXLONG', timeidx=wrf.ALL_TIMES, method='cat', meta=False) / 1000   #converting coordinates to km #xf and yf indicate fire grid x and y
   yf = wrf.getvar(wrfouts_reinit, 'FXLAT', timeidx=wrf.ALL_TIMES, method='cat', meta=False) / 1000

   #reading data to single array
   lfn = wrf.getvar(wrfouts_reinit, 'LFN', timeidx=wrf.ALL_TIMES, method='cat', meta=False)    #Level-set values
   hgt = wrf.getvar(wrfouts_reinit, 'HGT', timeidx=wrf.ALL_TIMES, method='cat', meta=False)   #Terrain height

   fig = plt.figure()
   ax = plt.subplot2grid((1,1), (0,0))

   fire_perimeter_plot(xf, yf, lfn, ‘r’)

   #Plotting the terrain
   CS = ax.contourf(x, y, hgt)
   cbar = plt.colorbar()
   cbar.set_label('Terrain Height (m)')

   #plotting the wind arrows
   wf = wind_plot (wrfouts_reinit, x, y, 'ua', 'va', 0, 'w')
   plt.quiverkey(wf, 0.7, 0.9, U=5, label=r'$5 \frac{m}{s}$', labelpos='E', coordinates='figure', color = 'k')

   ax.tick_params(direction='in')
   ax.yaxis.set_ticks_position('both')
   ax.xaxis.set_ticks_position('both')

   plt.ylabel('Y (km)')
   plt.xlabel('X (km)')
   plt.legend()
   plt.xlim(0, 5)
   plt.xticks(np.arange(0, 5.5, 0.5))
   plt.ylim(0, 5)
   plt.yticks(np.arange(0, 5.5, 0.5))
   plt.show()

Description of the Code’s Workflow
----------------------------------

.. raw:: html

   In the first step, the user should specify the time that he wants to plot the fire perimeter in minutes using “out_time” variable, and then the user should specify the sub-grid ratio of the fire domain using “sr” variable. Next, the path to the WRF-Fire output files, meaning “wrfout” files, must be specified by “outs_folder” variable. <br>

The code starts by opening the WRF-Fire output file using the using-defined output path and output time. In the next step, the code extracts the user-defined required variables using WRF-Python library. These variables in this Python code are X and Y coordinates of the atmospheric and fire domains, level-set function values, and terrain height. Next, a matplotlib figure is defined and the fire perimeter is plotted using “fire_perimeter_plot” function which is described later on. After that, the terrain is plotted using matplotlib filled contour, the wind field is plotted using “wind_plot” followed by a quiver key that shows the reference wind vector. Finally, some customization is applied which can be modified based on user’s needs.

Description of Functions in the Code
------------------------------------

Four functions are used in this code: (1) “wrf_out_read”, (2) “relax_zone_remover”, (3) “wind_plot”, and (4) “fire_perimeter_plot”.

“wrf_out_read” Function
^^^^^^^^^^^^^^^^^^^^^^^

This function reads WRF-Fire output files using netCDF4 Python library. This function first extracts the hours and minutes of the user-specified output time which is in minutes. Then, it opens WRF-Fire output using “Dataset” function of netCDF 4 library and returns the loaded output file. The name of the WRF-Fire output file must be edited by the user based on its WRF-Fire output names.

“relax_zone_remover” Function
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

WRF-Fire applies a relaxation zone to the level-set variable at the top and right side of the domain meaning the level-set value at this zone is equal to zero. The size of this relaxation zone is equal to one atmospheric grid cell, i.e., “sr” cells of the fire grid where “sr” is the sub-grid ratio defined by the user. To avoid incorrectly determining this relaxation zone as fire perimeter, where level-set function is equal to zero, this relaxation zone must be removed. “relax_zone_remover” function removes this zone by deleting “sr” columns and rows of the level-set variable at the top and right side of the domain using Numpy library. Furthermore, to match the size of the level-set variable with X and Y, the relaxation zone must be also removed from X and Y matrices.

“fire_perimeter” Function
^^^^^^^^^^^^^^^^^^^^^^^^^

This function plots the fire perimeter using level-set function values and matplotlib contour function. In the first step, the function removes the relaxation zone from level-set, X, and Y variables by calling the “relax_zone_remover” function. In the next step, “fire_perimeter” function plots the fire perimeter using matplotlib contour function followed by a label definition used for creating figure’s legend. In the contour function, the contour level is set to zero since the fire perimeter is where the level-set value is equal to zero.


“wind_plot” Function
^^^^^^^^^^^^^^^^^^^^

.. raw:: html

   This function uses matplotlib’s quiver function to plot arrows indicating wind speed and direction. It starts by first reading the U and V components of the wind speed, and since these variables are 3 dimensional, the height value, which is a user-defined input of the function, is applied to achieve U and V wind components at the desired vertical level. Then, the wind vectors are plotted using matplotlib quiver function. Quiver function options can be modified by the user based on its needs. In this example, wind vectors are plotted with interval of 8 to avoid congesting the resulting figure. Moreover, scale of 8 is applied to make reading the vectors easier. Further description of matplotlib quiver function is available <a href="https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.quiver.html" target="_blank">here.</a>
 


