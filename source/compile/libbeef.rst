Download and Install
====================

Download
########

1. Download :download:`version master <https://github.com/vossjo/libbeef>` of libbeef.

.. code-block:: Bash

    git clone https://github.com/vossjo/libbeef.git

Compile
########

1. Create configure file compile.sh

.. code-block:: Bash

    ./configure CC=icc \
        --prefix=/opt/software/libbeef/build

2. configure libbeef

.. code-block:: Bash

    bash compile.sh

3. compile and test libbeef

.. code-block:: Bash

    make && make check && make install
