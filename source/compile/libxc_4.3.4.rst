Download and Install
====================

Download
########

1. Download :download:`version 4.3.4 <http://www.tddft.org/programs/libxc/down.php?file=4.3.4/libxc-4.3.4.tar.gz>` of libxc.

.. code-block:: Bash

    wget http://www.tddft.org/programs/libxc/down.php?file=4.3.4/libxc-4.3.4.tar.gz

Compile
#######

1. Create configure file compile.sh

.. code-block:: Bash

    ./configure --prefix=/opt/software/libxc-4.3.4/bin \
	AR=ar \
	FC=ifort \
	F77=ifort \
	F90=ifort \
	CC=icc

2. configure libxc

.. code-block:: Bash

    bash compile.sh

3. compile and test libxc

.. code-block:: Bash

    make && make check && make Install

