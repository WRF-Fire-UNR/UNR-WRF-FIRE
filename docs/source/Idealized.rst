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
 


