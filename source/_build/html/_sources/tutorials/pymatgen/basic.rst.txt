Basic Module
=============

1. Lattice, Structure, Molecule Module

.. code-block:: python

    from pymatgen.core import Lattice, Structure, Molecule
    # Read a POSCAR and write to a CIF.
    structure = Structure.from_file("POSCAR")
    structure.to(filename="CsCl.cif")

    # Read an xyz file and write to a Gaussian Input file.
    methane = Molecule.from_file("methane.xyz")
    methane.to(filename="methane.gjf")

2. VASP and cif Module

.. code-block:: python
    
    from pymatgen.io.cif import CifParser
    # read structure from cif file
    parser = CifParser("mycif.cif")
    structure = parser.get_structures()[0]
    # read POSCAR file and write it to cif structure
    from pymatgen.io.vasp import Poscar
    from pymatgen.io.cif import CifWriter
    p = Poscar.from_file('POSCAR')
    w = CifWriter(p.structure)
    w.write_file('mystructure.cif')

