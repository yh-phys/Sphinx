Download and Install
====================

Download
########

1. Download :download:`version 6.7 <https://github.com/QEF/q-e/releases/download/qe-6.7.0/qe-6.7-ReleasePack.tgz>` of Quantum-ESPRESSO.

.. code-block:: Bash

    wget https://github.com/QEF/q-e/releases/download/qe-6.7.0/qe-6.7-ReleasePack.tgz

.. tip ::
    One should also download the thermo_pw :download:`version 1.4.1 <http://people.sissa.it/~dalcorso/thermo_pw/thermo_pw.1.4.1.tar.gz>` and Wannier90 :download:`version 3.1.0 <https://github.com/wannier-developers/wannier90/archive/v3.1.0.tar.gz>` codes

Compile
#######

.. tip ::
    Before compile Quantum-ESPRESSO package, one should compile the :doc:`libbeef <libbeef>`, :doc:`libxc_4.3.4 <libxc_4.3.4>` and :doc:`hdf5_1.12.0 <hdf5_1.12.0>` 

preparation
------------


* Unpack qe-6.7-ReleasePack

.. code-block:: Bash

    tar zxvf qe-6.7-ReleasePack.tgz && cd qe-6.7/test-suite

* Edit the check_pseudo.sh file as follows

.. code-block:: Bash

    #!/bin/bash
    #
    # Copyright (C) 2001-2016 Quantum ESPRESSO group
    # 
    # This program is free software; you can redistribute it and/or
    # modify it under the terms of the GNU General Public License
    # as published by the Free Software Foundation; either version 2
    # of the License. See the file 'License' in the root directory
    # of the present distribution.
    
    if test "`which curl`" = "" ; then
       if test "`which wget`" = "" ; then
          echo "### wget or curl not found: will not be able to download missing PP ###"
       else
          DOWNLOADER="wget -O"
          echo "wget found"
       fi
    else
       DOWNLOADER="curl -o"
       echo "curl found"
    fi
    
    inputs=`find $1* -type f -name "*.in" -not -name "test.*" -not -name "benchmark.*"`
    pp_files=`for x in ${inputs}; do grep UPF ${x} | awk '{print $3}'; done`
    
    for pp_file in ${pp_files} ; do
    if ! test -f ${ESPRESSO_PSEUDO}/${pp_file} ; then 
        echo -n "Downloading ${pp_file} to ${ESPRESSO_PSEUDO} ... "
        ${DOWNLOADER} ${ESPRESSO_PSEUDO}/${pp_file} ${NETWORK_PSEUDO}/${pp_file} 2> /dev/null
        if test $? != 0 ; then 
            echo "Download of" ${pp_file} "FAILED, do it manually -- Testing aborted!"
            exit -1
        else
            echo "done."
        fi
    else
        echo "No need to download ${pp_file}."
    fi
    done

* Download the pseudopotentials

.. code-block:: Bash

    make pseudo

* Unpack the thermo_pw and move it into Quantum-ESPRESSO main direction

.. code-block:: Bash

    tar zxvf thermo_pw.1.4.1.tar.gz && mv thermo_pw qe-6.7

* Rename the Wannier90 code and move it into Quantum-ESPRESSO

.. code-block:: Bash

    mv wannier90-3.1.0.tar.gz v3.1.0 && mv v3.1.0 qe-6.7/archive/

Compile Quantum-ESPRESSO
-------------------------

1. Create configure file compile.sh

.. code-block:: Bash

    ./configure MPIF90=mpiifort CC=mpiicc F90=ifort F77=mpiifort -enable-parallel \
            --with-libxc=yes \
            --with-libxc-prefix=/opt/software/libxc-4.3.4/bin \
            --with-libxc-include=/opt/software/libxc-4.3.4/bin/include \
            --with-scalapack=intel \
            --with-hdf5=yes \
            --with-hdf5-libs=/opt/software/hdf5-1.12.0_intelmpi/hdf5-1.12.0/hdf5/lib \
            --with-hdf5-include=/opt/software/hdf5-1.12.0_intelmpi/hdf5-1.12.0/hdf5/include \
            --with-libbeef=yes \
            --with-libbeef-prefix=/opt/software/libbeef/build/lib

2. configure Quantum-ESPRESSO

.. code-block:: Bash

    bash compile.sh

3. Edit the make.inc file

.. code-block:: Bash

    43  DFLAGS         =  -D__DFTI -D__LIBXC -D__MPI -D__SCALAPACK -D__NOBEEF
    109 FFLAGS         = -O3 -assume byterecl -g -traceback -xhost
    124 LD_LIBS        = -L/opt/software/libxc-4.3.4/bin/lib -lxcf03 -lxc
    131 BLAS_LIBS      =   -L${MKLROOT}/lib/intel64 -lmkl_scalapack_lp64 -lmkl_intel_lp64 -lmkl_sequential -lmkl_core -lmkl_blacs_intelmpi_lp64 -lpthread -lm -ldl
    137 LAPACK_LIBS    = -L${MKLROOT}/lib/intel64 -lmkl_scalapack_lp64 -lmkl_intel_lp64 -lmkl_sequential -lmkl_core -lmkl_blacs_intelmpi_lp64 -lpthread -lm -ldl
    140 SCALAPACK_LIBS = -L${MKLROOT}/lib/intel64 -lmkl_scalapack_lp64 -lmkl_intel_lp64 -lmkl_sequential -lmkl_core -lmkl_blacs_intelmpi_lp64 -lpthread -lm -ldl
    145 FFT_LIBS       = -L${MKLROOT}/lib/intel64 -lmkl_scalapack_lp64 -lmkl_intel_lp64 -lmkl_sequential -lmkl_core -lmkl_blacs_intelmpi_lp64 -lpthread -lm -ldl
    148 HDF5_LIBS = -L/opt/software/hdf5-1.12.0_intelmpi/hdf5-1.12.0/hdf5/lib -lhdf5
    155 MPI_LIBS       = -L/opt/intel/impi/2019.9.304/intel64/lib -lmpi
    163 BEEF_LIBS      = /opt/software/libbeef/build/lib/libbeef.a
    164 BEEF_LIBS_SWITCH = internal
    185 LIBXC_LIBS     = -L/opt/software/libxc-4.3.4/bin/lib -lxcf03 -lxc

4. compile Quantum-ESPRESSO

.. code-block:: Bash

    pp=4
    make -j $pp pw && make -j $pp ph && make -j $pp hp && make -j $pp pwcond && make -j $pp neb \
        && make -j $pp pp && make -j $pp cp && make -j $pp tddfpt && make -j $pp gwl \
        && make -j $pp ld1 && make -j $pp xspectra && make -j $pp couple && make epw 
    
    cd thermo_pw && make join_qe && cd ../ && make thermo_pw

5. test Quantum-ESPRESSO

.. code-block:: Bash

    cd test-suite && make run-tests-parallel


