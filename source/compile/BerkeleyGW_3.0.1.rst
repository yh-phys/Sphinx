Download and Install
====================

Download
########

* Download :download:`version 3.0.1 <https://berkeley.box.com/s/1dgnhiemo47lhxczrn6si71bwxoxor8>` of BerkeleyGW.

Compile
#######

* Before compile BerkeleyGW package, one should compile hdf5-1.12.0 code Download :download:`version 1.12.0 <https://www.hdfgroup.org/package/hdf5-1-12-0-tar-gz/?wpdmdl=14582&refresh=618bc42bc76531636549675>` of HDF5.

2.1 Compile HDF5
-----------------

.. code-block:: Bash

    ./configure CC=mpiicc FC=mpiifort CXX=mpiicpc \
        --enable-fortran --enable-parallel --enable-shared \
        --prefix=/your_path
    make

    make install

2.2 Compile BerkeleyGW
-----------------------

* Create arch.mk file, one can modifiy the ./config/frontera.tacc.utexas.edu_impi.mk file as follows.

.. code-block:: Bash

    # arch.mk for BerkeleyGW codes
    #
    # suitable for Frontera at TACC, Cascade Lake (CLX) architecture
    # Uses intel compilers + impi
    #
    # BERKELEYGW DOES NOT WORK WITH INTEL/18 DUE TO A COMPILER BUG!
    # ------------------------------------------------------------
    #
    # You'll need to run:
    # module load arpack impi intel/19.0.5 phdf5
    #
    # Use 'make -j 8' for parallel build on Frontera
    #
    # Zhenglu Li
    # July 2020, Berkeley

    COMPFLAG  = -DINTEL
    PARAFLAG  = -DMPI -DOMP 
    MATHFLAG  = -DUSESCALAPACK -DUNPACKED -DUSEFFTW3 -DHDF5
    # Only uncomment DEBUGFLAG if you need to develop/debug BerkeleyGW.
    # The output will be much more verbose, and the code will slow down by ~20%.
    #DEBUGFLAG = -DDEBUG

    FCPP    = cpp -C -nostdinc
    F90free = mpiifort -free -qopenmp -ip -no-ipo
    LINK    = mpiifort -qopenmp -ip -no-ipo
    # We need the -fp-model procise to pass the testsuite.
    FOPTS   = -O3 -fp-model source
    FNOOPTS = -O2 -fp-model source -no-ip
    #FOPTS   = -g -O0 -check all -Warn all -traceback
    #FNOOPTS = $(FOPTS)
    MOD_OPT = -module 
    INCFLAG = -I

    C_PARAFLAG = -DPARA -DMPICH_IGNORE_CXX_SEEK
    CC_COMP = mpiicpc
    C_COMP  = mpiicc
    C_LINK  = mpiicpc
    C_OPTS  = -O3 -ip -no-ipo -qopenmp
    C_DEBUGFLAG =

    REMOVE  = /bin/rm -f

    # Math Libraries
    #
    MKLPATH      = $(MKLROOT)/lib/intel64

    FFTWLIB      =	-Wl,--start-group \
    		$(MKLPATH)/libmkl_intel_lp64.a \
    		$(MKLPATH)/libmkl_intel_thread.a \
    		$(MKLPATH)/libmkl_core.a \
    		-Wl,--end-group -liomp5 -lpthread -lm -ldl
    FFTWINCLUDE  = $(MKLROOT)/include/fftw


    LAPACKLIB    = -Wl,--start-group \
    		$(MKLPATH)/libmkl_intel_lp64.a \
    		$(MKLPATH)/libmkl_intel_thread.a \
    		$(MKLPATH)/libmkl_core.a \
    		$(MKLPATH)/libmkl_blacs_intelmpi_lp64.a \
    		-Wl,--end-group -liomp5 -lpthread -lm -ldl
    SCALAPACKLIB = $(MKLPATH)/libmkl_scalapack_lp64.a

    HDF5PATH     =  /opt/software/BerkeleyGW-3.0.1/hdf5/lib
    HDF5LIB      =	$(HDF5PATH)/libhdf5hl_fortran.a \
    		$(HDF5PATH)/libhdf5_hl.a \
    		$(HDF5PATH)/libhdf5_fortran.a \
    		$(HDF5PATH)/libhdf5.a \
    		-lz
    HDF5INCLUDE  = $(HDF5PATH)/../include

    TESTSCRIPT = 

* Compile

.. code-block:: Bash

    make -j N all-flavors (N is the number of cores)
