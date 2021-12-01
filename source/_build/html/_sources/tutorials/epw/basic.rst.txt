NSCF Kpoints
============

* Generate by Python script

.. code-block:: Python

    #!/usr/bin/env python3

    import numpy as np
    f = open("./nscf.txt", "w")

    X = 6
    Y = 6
    Z = 6
    print("K_POINTS crystal", file = f)
    print(X * Y * Z, file = f)

    for ii in np.arange(0,1.0,1.0/X):
        for jj in np.arange(0,1.0,1.0/Y):
            for kk in np.arange(0,1.0,1.0/Z):
                # print('%.10f' %(ii), ' ', '%.10f' %(jj), ' ', '%.10f' %(kk), ' ', '%.10f' %(1.0 / (X * Y * Z)))
                print('%.10f' %(ii), ' ', '%.10f' %(jj), ' ', '%.10f' %(kk), ' ', '%.10f' %(1.0 / (X * Y * Z)), file = f)
    f.close()
    


* Generate by kmesh.pl script, which can be found in wannier90-x.y.z/utility/kmesh.pl

.. code-block:: Bash

    ./kmesh.pl 3 3 3 >> nscf.txt
