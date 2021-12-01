Download and Install
====================

Download
########

1. Download :download:`version 1.12.0 <https://support.hdfgroup.org/ftp/HDF5/releases/hdf5-1.12/hdf5-1.12.0/src/hdf5-1.12.0.tar.gz>` of HDF5.

.. code-block:: Bash

    wget https://support.hdfgroup.org/ftp/HDF5/releases/hdf5-1.12/hdf5-1.12.0/src/hdf5-1.12.0.tar.gz

Compile
#######

1. Create configure file configure.sh

.. code-block:: Bash

    #!/bin/bash

	./configure CC=mpiicc FC=mpiifort CXX=mpiicpc \
        --enable-fortran \
        --enable-parallel \
        --enable-shared \
        --prefix=/opt/software/hdf5-1.12.0 \
        --with-zlib=/opt/software/zlib-1.2.11

2. configure HDF5

.. code-block:: Bash

    bash configure.sh

3. compile and test HDF5

.. code-block:: Bash

    make && make check && make Install
