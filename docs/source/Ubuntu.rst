.. _ubuntu:

==================================================================
How to Automatically Compile WRF-Fire on Ubuntu Using Shell Script
==================================================================


How to Install Required WRF-Fire Libraries Automatically on Ubuntu
------------------------------------------------------------------
To successfully compile WRF-Fire, a number of libraries are required which their compilation can be difficult, time consuming, and confusing. The tutorial presented in this section along with the shell script presented can automize this process and easy. Before reading this section, you must read and follow :ref:`“Required Compilers and How to Install Them” <reqCompilers>` and :ref:`"Testing the compilers" <testCompilers>' to install and test the required compilers.

Required Environment Variables
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Before moving to running the shell script that installs all the required libraries, the following environmental variables must be set in the Shell’s initialization file:

::

    export DIR= (Path to Libraries directory, e.g., /Users/username /LIBRARIES)
    export CC=gcc
    export CXX=g++
    export FC=gfortran
    export FCFLAGS=-m64
    export F77=gfortran
    export FFLAGS=-m64
    export JASPERLIB=$DIR/grib2/lib 
    export JASPERINC=$DIR/grib2/include  
    export PATH="$DIR/mpich/bin:$DIR/netcdf/bin:$DIR/hdf5/bin:$PATH"
    export NETCDF=$DIR/netcdf
    export HDF5=$DIR/hdf5/lib
    export LDFLAGS="-L$DIR/grib2/lib -L${DIR}/hdf5/lib -L${DIR}/netcdf/lib"
    export CPPFLAGS="-I$DIR/grib2/include -I${DIR}/hdf5/include -I${DIR}/netcdf/include"
    export LD_LIBRARY_PATH=${DIR}/netcdf/lib:$DIR/hdf5/lib:$LD_LIBRARY_PATH

The above-mentioned Environment Variables should be set in “.bash_profile” file since Ubuntu uses Bash shell. This file is located in the Home directory and can be accessed and edited using the following command:

.. centered:: “nano ~/.bash_profile”

After setting the required Environment Variables in the shell initialization file, the user must log out and log back in for the changes to take effect.

.. Note:: The ‘CC’, ‘CXX’, ‘FC’, and ‘F77’ Environment Variables are used to call the compilers, and therefore, they should be changed according to the compilers. For instance, the presented environment variables are for GFortran and GCC compilers in an Ubuntu system with only one version of the compilers installed.

Shell Script to Install Required Libraries Automatically
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The shell scripted provided below will install the required libraries to compile WRF-Fire automatically on Ubuntu. This script is tested on Ubuntu 18.04, 20.04 LTS, and 20.04 Server.

::

    apt-get -y update
    apt-get -y install gcc gfortran g++ git wget build-essential libpng-dev libcurl4-gnutls-dev m4      software-properties-common
    add-apt-repository universe
    apt-get -y install csh

    export DIR= (Path to Libraries directory, e.g., /Users/username /LIBRARIES)    
    export CC=gcc
    export CXX=g++
    export FC=gfortran
    export FCFLAGS=-m64
    export F77=gfortran
    export FFLAGS=-m64
    export JASPERLIB=$DIR/grib2/lib 
    export JASPERINC=$DIR/grib2/include  
    export PATH="$DIR/mpich/bin:$DIR/netcdf/bin:$DIR/hdf5/bin:$PATH"
    export NETCDF=$DIR/netcdf
    export HDF5=$DIR/hdf5/lib
    export LDFLAGS="-L$DIR/grib2/lib -L${DIR}/hdf5/lib -L${DIR}/netcdf/lib"
    export CPPFLAGS="-I$DIR/grib2/include -I${DIR}/hdf5/include -I${DIR}/netcdf/include"
    export LD_LIBRARY_PATH=${DIR}/netcdf/lib:$DIR/hdf5/lib:$LD_LIBRARY_PATH

    mkdir -p $BASE_DIR/LIBRARIES

    cd $DIR

    wget https://support.hdfgroup.org/ftp/HDF5/releases/hdf5-1.10/hdf5-1.10.5/src/hdf5-1.10.5.tar.gz
    tar xzvf hdf5-1.10.5.tar.gz
    cd hdf5-1.10.5
    ./configure --prefix=$DIR/hdf5 --enable-fortran
    make
    make install
    cd .. 

    wget https://www.unidata.ucar.edu/downloads/netcdf/ftp/netcdf-c-4.7.2.tar.gz
    tar xzvf netcdf-c-4.7.2.tar.gz     
    cd netcdf-c-4.7.2
    ./configure --prefix=$DIR/netcdf --enable-netcdf-4 LDFLAGS="-L$DIR/hdf5/lib" CPPFLAGS="-I$DIR/hdf5/include" 
    make
    make install
    cd .. 

    wget https://www.unidata.ucar.edu/downloads/netcdf/ftp/netcdf-fortran-4.5.2.tar.gz
    tar xzvf netcdf-fortran-4.5.2.tar.gz
    cd netcdf-fortran-4.5.2
    ./configure --prefix=$DIR/netcdf LDFLAGS="$LDFLAGS" CPPFLAGS="$CPPFLAGS" 
    make 
    make install
    cd .. 

    wget http://www2.mmm.ucar.edu/wrf/OnLineTutorial/compile_tutorial/tar_files/mpich-3.0.4.tar.gz
    tar xzvf mpich-3.0.4.tar.gz      
    cd mpich-3.0.4
    ./configure --prefix=$DIR/mpich
    make
    make install
    cd .. 

    wget https://www.zlib.net/zlib-1.2.11.tar.gz
    tar xzvf zlib-1.2.11.tar.gz    
    cd zlib-1.2.11
    ./configure --prefix=$DIR/grib2
    make
    make install
    cd .. 

    wget http://www2.mmm.ucar.edu/wrf/OnLineTutorial/compile_tutorial/tar_files/jasper-1.900.1.tar.gz
    tar xzvf jasper-1.900.1.tar.gz  
    cd jasper-1.900.1
    ./configure --prefix=$DIR/grib2
    make
    make install
    cd ../../
The above shell script can be downloaded from here. Remember to change “DIR” environment variable to the correct location. The script can be run using the following command:

.. centered:: “bash (script_name)”

After the installation is finished, WRF-Fire can be compiled using the instruction provided in “Compiling WRF-Fire”. For creating the configuration file, option number 34 must be used when using the provided script to compile WRF-Fire using GNU compilers and in parallel using MPICH.
