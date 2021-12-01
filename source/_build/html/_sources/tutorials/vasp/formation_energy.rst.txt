Formation Energy Calculation
============================

.. code-block:: Python

    #!/usr/bin/env python3

    """
    读取POSCAR与OUTCAR文件，自动计算形成能
    """

    """
    以字典的形式定义每个原子的能量，需要从Materials Project库里查找补充
    """
    Energy_atom = {'O' : -4.9480, 'Mg': -1.6003, 'Ca': -1.9995, 'Zn': -1.2595, 'Cu': -4.0992, 
                   'Sr': -1.6895, 'Cd': -0.9062, 'Ba': -1.9190, 'Yb': -1.5396, 'Pt': -6.0709,
                   'Pd': -3.7126
                  }

    """
    读取POSCAR文件，计算原子的总数和总能
    """
    with open("POSCAR", 'r', encoding='utf-8') as poscar:
        list_poscar = poscar.readlines()
    for i in range(0, len(list_poscar)):
        list_poscar[i] = list_poscar[i].rstrip('\n')
    list_element = ' '.join(list_poscar[5].strip().split('\t')).split()
    # print("list_element = ", list_element)
    list_num_element = ' '.join(list_poscar[6].strip().split('\t')).split()
    # print("list_num_element = ", list_num_element)
    total_atoms = 0
    Toten_atoms = 0
    for i in range(0, len(list_element)):
        total_atoms = total_atoms + int(list_num_element[i])
        Toten_atoms = Toten_atoms + Energy_atom[str(list_element[i])]*int(list_num_element[i])
    # print("total_atoms = ", total_atoms)
    # print("Toten_atoms = ", Toten_atoms)

    """
    读取OUTCAR文件获得体系总能
    """
    with open("OUTCAR", 'r', encoding='utf-8') as f:
        lines = f.readlines()
        for line in lines:
            if ("TOTEN" in line):
                # print(line.rstrip('\n'))
                list_TOTEN = line
    Toten_system = float(list_TOTEN.rstrip().split()[-2])

    Ef = (Toten_system-Toten_atoms)/total_atoms
    print("Ef = ", '%.3f' % Ef, "eV/atom")
