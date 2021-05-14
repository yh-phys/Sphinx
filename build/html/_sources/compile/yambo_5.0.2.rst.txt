Download and Install
====================

Download
########

1. Download :download:`version 5.0.2 <https://github.com/yambo-code/yambo/archive/5.0.2.tar.gz>` of Yambo.

.. code-block:: Bash

    wget https://github.com/yambo-code/yambo/archive/5.0.2.tar.gz

2. One should also download the dependent packages of Yambo by following commands

.. code-block:: Bash

    tar zxvf yambo-5.0.2.tar.gz && cd yambo-5.0.2/lib/archive && \
        make -f Makefile.loc

.. important ::
    The yambo-libraries-0.0.2.tar.gz package should also be download

(a) 1. Download :download:`version 0.0.2 <https://codeload.github.com/yambo-code/yambo-libraries/tar.gz/0.0.2>`
(b) Unpack and move the files to yambo code

.. code-block:: Bash

    tar zxvf yambo-libraries-0.0.2.tar.gz && \
        mv yambo-libraries-0.0.2/* yambo-5.0.2/lib/yambo

Compile
#######

1. Create configure file compile.sh

.. code-block:: Bash

    ./configure FC="ifort" MPIFC="mpiifort" F77="ifort" MPIF77="mpiifort" CC="icc" MPICC="mpiicc" CPP="gcc -E -P" FPP="gfortran -E -P -cpp"   \
    --enable-mpi \
    --enable-open-mp \
    --enable-dp \
    --with-blas-libs="-lmkl_intel_lp64 -lmkl_intel_thread -lmkl_core -liomp5 -lpthread -lm" \
    --with-lapack-libs="-lmkl_intel_lp64 -lmkl_intel_thread -lmkl_core -liomp5 -lpthread -lm" \
    --with-fft-includedir=/opt/intel/compilers_and_libraries_2020.4.304/linux/mkl/include/fftw \
    --with-fft-libs="-lmkl_intel_lp64 -lmkl_intel_thread -lmkl_core -liomp5 -lpthread -lm" \
    --with-blacs-libs="-lmkl_scalapack_lp64 -lmkl_blacs_intelmpi_lp64 -lmkl_core -liomp5 -lpthread -lm" \
    --with-scalapack-libs="-lmkl_scalapack_lp64 -lmkl_blacs_intelmpi_lp64 -lmkl_core -liomp5 -lpthread -lm"

2. configure yambo

.. code-block:: Bash

    bash compile.sh

3. compile yambo

.. code-block:: Bash

    make core

