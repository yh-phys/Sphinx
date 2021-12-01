GW计算过程
==========

* 计算所需的文件结构如下：

.. code-block:: Bash

    ├── 5-epsilon
    │   ├── bash
    │   └── epsilon.inp
    ├── 6-sigma
    │   ├── bash
    │   ├── eqp.py
    │   └── sigma.inp
    ├── 7-kernel
    │   ├── bash
    │   └── kernel.inp
    ├── 8-absorption
    │   ├── absorption.inp
    │   └── bash
    ├── 9-inteqp
    │   ├── bash
    │   └── inteqp.inp
    └── ESPRESSO
        ├── 0-kgrid
        │   ├── script_0
        │   ├── WFN_fi.in
        │   ├── WFN_fiq.in
        │   ├── WFN.in
        │   └── WFNq.in
        ├── 1-scf
        │   ├── bash
        │   ├── B.SG15.PBE.UPF
        │   ├── input
        │   └── N.SG15.PBE.UPF
        ├── 2-wfn
        │   ├── bash
        │   ├── degeneracy_check.x
        │   ├── input
        │   └── pp_in
        ├── 3-wfnq
        │   ├── bash
        │   ├── input
        │   └── pp_in
        ├── 5-wfn_fi
        │   ├── bash
        │   ├── input
        │   └── pp_in
        ├── 6-wfn_fiq
        │   ├── bash
        │   ├── input
        │   └── pp_in
        ├── 7-band
        │   ├── bash
        │   ├── input
        │   └── pp_in
        └── script_1

Quantum ESPRESSO计算
#######################

0-kgrid文件修改
-------------------


* WFN.in

.. code-block:: Bash
    
    5   5   1         # k点网格
    0.0  0.0  0.0
    0.0  0.0  0.0
          2.5124282300       0.0000000000       0.0000000000  # 晶格常数
         -1.2562141150       2.1758266724       0.0000000000  # 晶格常数
          0.0000000000       0.0000000000       7.7072650000  # 晶格常数

    4 # 原子总数
     1     0.6666666700       0.3333333300       0.7500000000 # 第一个原子分数坐标
     1     0.3333333300       0.6666666700       0.2500000000 # 第二个原子分数坐标
     2     0.6666666700       0.3333333300       0.2500000000 # 第三个原子分数坐标
     2     0.3333333300       0.6666666700       0.7500000000 # 第四个原子分数坐标
    24 24 81 # 傅里叶变换网格
    .false.

* WFNq.in

.. code-block:: Bash

    5   5   1
    0.0    0.0  0.0
    0.001  0.0  0.0 # 第一个数变为0.001
          2.5124282300       0.0000000000       0.0000000000
         -1.2562141150       2.1758266724       0.0000000000
          0.0000000000       0.0000000000       7.7072650000

    4
     1     0.6666666700       0.3333333300       0.7500000000
     1     0.3333333300       0.6666666700       0.2500000000
     2     0.6666666700       0.3333333300       0.2500000000
     2     0.3333333300       0.6666666700       0.7500000000
    24 24 81
    .false.

* WFN_fi.in

.. code-block:: Bash
    
    7   7   1    # k点网格变密，根据服务器计算能力修改
    0.0  0.0  0.0
    0.0  0.0  0.0
          2.5124282300       0.0000000000       0.0000000000
         -1.2562141150       2.1758266724       0.0000000000
          0.0000000000       0.0000000000       7.7072650000

    4
     1     0.6666666700       0.3333333300       0.7500000000
     1     0.3333333300       0.6666666700       0.2500000000
     2     0.6666666700       0.3333333300       0.2500000000
     2     0.3333333300       0.6666666700       0.7500000000
    24 24 81
    .false.

* WFN_fiq.in

.. code-block:: Bash
    
    7   7   1
    0.0    0.0    0.0
    0.001  0.001  0.0 # 第一个与第二个数变为0.001
          2.5124282300       0.0000000000       0.0000000000
         -1.2562141150       2.1758266724       0.0000000000
          0.0000000000       0.0000000000       7.7072650000

    4
     1     0.6666666700       0.3333333300       0.7500000000
     1     0.3333333300       0.6666666700       0.2500000000
     2     0.6666666700       0.3333333300       0.2500000000
     2     0.3333333300       0.6666666700       0.7500000000
    24 24 81
    .false.


* script_0

.. code-block:: Bash
    
    #!/bin/bash

    #
    KGRID="/home/gengzi/apps/BerkeleyGW-2.1/bin/kgrid.x" #修改BerkeleyGW路径

    #
    $KGRID ./WFN.in ./WFN.out ./WFN.log
    $KGRID ./WFNq.in ./WFNq.out ./WFNq.log
    #$KGRID ./WFN_co.in ./WFN_co.out ./WFN_co.log
    $KGRID ./WFN_fi.in ./WFN_fi.out ./WFN_fi.log
    $KGRID ./WFN_fiq.in ./WFN_fiq.out ./WFN_fiq.log

    cd ..

* 生成输入文件

.. code-block:: Bash
    
    bash script_0

链接全局环境变量
----------------

.. code-block:: Bash
    
    #!/bin/bash
    #

    sys=BN                          # 体系名称
    PO1=B.SG15.PBE.UPF              # 势文件名称
    PO2=N.SG15.PBE.UPF              # 势文件名称

    mkdir ./2-wfn/$sys.save
    ln -s ../1-scf/$PO1 ./2-wfn     # 连接势文件，如果体系原子增加，相应的势文件也要增加
    ln -s ../1-scf/$PO2 ./2-wfn
    #ln -s ../1-scf/vdW_kernel_table ./2-wfn
    ln -s ../../1-scf/$sys.save/data-file-schema.xml ./2-wfn/$sys.save
    ln -s ../../1-scf/$sys.save/charge-density.dat ./2-wfn/$sys.save
    #
    mkdir ./3-wfnq/$sys.save
    ln -s ../1-scf/$PO1 ./3-wfnq
    ln -s ../1-scf/$PO2 ./3-wfnq
    #ln -s ../1-scf/vdW_kernel_table   ./3-wfnq
    ln -s ../../1-scf/$sys.save/data-file-schema.xml ./3-wfnq/$sys.save
    ln -s ../../1-scf/$sys.save/charge-density.dat ./3-wfnq/$sys.save
    #
    #
    mkdir ./5-wfn_fi/$sys.save
    ln -s ../1-scf/$PO1 ./5-wfn_fi
    ln -s ../1-scf/$PO2 ./5-wfn_fi
    #ln -s ../1-scf/vdW_kernel_table   ./5-wfn_fi
    ln -s ../../1-scf/$sys.save/data-file-schema.xml ./5-wfn_fi/$sys.save
    ln -s ../../1-scf/$sys.save/charge-density.dat ./5-wfn_fi/$sys.save
    
    mkdir ./6-wfn_fiq/$sys.save
    ln -s ../1-scf/$PO1  ./6-wfn_fiq
    ln -s ../1-scf/$PO2  ./6-wfn_fiq
    #ln -s ../1-scf/vdW_kernel_table   ./6-wfn_fiq
    ln -s ../../1-scf/$sys.save/data-file-schema.xml ./6-wfn_fiq/$sys.save
    ln -s ../../1-scf/$sys.save/charge-density.dat ./6-wfn_fiq/$sys.save

    mkdir ./7-band/$sys.save
    ln -s ../1-scf/$PO1  ./7-band
    ln -s ../1-scf/$PO2  ./7-band
    #ln -s ../1-scf/vdW_kernel_table   ./6-wfn_fiq
    ln -s ../../1-scf/$sys.save/data-file-schema.xml ./7-band/$sys.save
    ln -s ../../1-scf/$sys.save/charge-density.dat ./7-band/$sys.save

    ln -s ../ESPRESSO/2-wfn/wfn.complex ../5-epsilon/WFN
    ln -s ../ESPRESSO/3-wfnq/wfn.complex ../5-epsilon/WFNq
    #
    ln -s ../ESPRESSO/2-wfn/vxc.dat ../6-sigma/vxc.dat
    ln -s ../ESPRESSO/2-wfn/rho.cplx ../6-sigma/RHO
    ln -s ../ESPRESSO/2-wfn/wfn.complex ../6-sigma/WFN_inner
    ln -s ../5-epsilon/eps0mat ../6-sigma
    ln -s ../5-epsilon/epsmat ../6-sigma
    #
    ln -s ../ESPRESSO/2-wfn/wfn.complex ../7-kernel/WFN_co
    ln -s ../5-epsilon/eps0mat ../7-kernel
    ln -s ../5-epsilon/epsmat ../7-kernel
    #
    ln -s ../ESPRESSO/2-wfn/wfn.complex ../8-absorption/WFN_co
    ln -s ../ESPRESSO/5-wfn_fi/wfn.complex ../8-absorption/WFN_fi
    ln -s ../ESPRESSO/6-wfn_fiq/wfn.complex ../8-absorption/WFNq_fi
    ln -s ../5-epsilon/eps0mat ../8-absorption
    ln -s ../5-epsilon/epsmat ../8-absorption
    ln -s ../7-kernel/bsedmat ../8-absorption
    ln -s ../7-kernel/bsexmat ../8-absorption
    #
    ln -s ../ESPRESSO/2-wfn/wfn.complex ../9-inteqp/WFN_co
    ln -s ../ESPRESSO/7-band/wfn.complex ../9-inteqp/WFN_fi
    ln -s ../5-epsilon/eps0mat ../9-inteqp
    ln -s ../5-epsilon/epsmat ../9-inteqp
    #
    ln -s ../8-absorption/eqp_co.dat ../9-inteqp

* 运行脚本

.. code-block:: Bash

    bash script_1

1-scf
--------

* input文件

.. code-block:: Bash
    
      &CONTROL
      calculation = 'scf'
      etot_conv_thr =   4.0000000000d-05
      forc_conv_thr =   1.0000000000d-04
      outdir = './'
      prefix = 'BN'
      pseudo_dir = './'
      wfcdir = './'
      tprnfor = .true.
      tstress = .true.
      verbosity = 'high'
      wf_collect = .false.
    /
    &SYSTEM
      ecutwfc =   4.0000000000d+01
      ibrav = 0
      nat = 4
      ntyp = 2
    /
    &ELECTRONS
      conv_thr =   1.0000000000d-7
      electron_maxstep = 80
      mixing_beta =   4.0000000000d-01
      mixing_mode = 'plain'
      diagonalization = 'david'
      diago_david_ndim = 8
      diago_full_acc = .true.
    /
    ATOMIC_SPECIES
    B      10.811 B.SG15.PBE.UPF
    N      14.0067 N.SG15.PBE.UPF 
    ATOMIC_POSITIONS crystal
    B            0.6666666700       0.3333333300       0.7500000000  ! 顺序与0-kgrid中设置相同
    B            0.3333333300       0.6666666700       0.2500000000 
    N            0.6666666700       0.3333333300       0.2500000000 
    N            0.3333333300       0.6666666700       0.7500000000 
    K_POINTS automatic
    15 15 5 0 0 0
    CELL_PARAMETERS angstrom
          2.5124282300       0.0000000000       0.0000000000
         -1.2562141150       2.1758266724       0.0000000000
          0.0000000000       0.0000000000       7.7072650000

* 提交任务脚本

.. code-block:: Bash

    #!/bin/sh
    #PBS -N hey
    #PBS -l nodes=1:ppn=40,walltime=500:00:00
    ##PBS -q workq


    cd $PBS_O_WORKDIR
    source /home/gengzi/apps/intel/19/parallel_studio_xe_2019/psxevars.sh

    date "+01 Today's date is: %D. The time execution %T" >> time.info
    mpirun -np 40 /home/gengzi/apps/qe-6.5/bin/pw.x -nk 4  -in ./input &> ./out
    rm ./*.wfc*
    date "+02 Today's date is: %D. The time finish %T" >> time.info

2-wfn
-------

* input

.. code-block:: Bash

     &CONTROL
                           title = 'BN' ,
                     calculation = 'bands' ,
                    restart_mode = 'from_scratch' ,
                      wf_collect = .false. ,
                          outdir = './' ,
                          wfcdir = './' ,
                      pseudo_dir = './' ,
                          prefix = 'BN' ,
                         tstress = .false. ,
                         tprnfor = .false. ,
     /
     &SYSTEM
                            ibrav =0,
                             nat = 4,
                            ntyp = 2,
                           ecutwfc =40,
                           nbnd = 80, !要求能带是占据态的10倍


     /
     &ELECTRONS
                electron_maxstep = 100
                conv_thr = 1.0d-7
                mixing_mode = 'plain'
                mixing_beta = 0.4
             diagonalization = 'david'
              diago_david_ndim = 8
              diago_full_acc = .true.
    /
    ATOMIC_SPECIES
    B      10.811 B.SG15.PBE.UPF
    N      14.0067 N.SG15.PBE.UPF
    ATOMIC_POSITIONS crystal
    B            0.6666666700       0.3333333300       0.7500000000
    B            0.3333333300       0.6666666700       0.2500000000
    N            0.6666666700       0.3333333300       0.2500000000
    N            0.3333333300       0.6666666700       0.7500000000
    CELL_PARAMETERS angstrom
          2.5124282300       0.0000000000       0.0000000000
         -1.2562141150       2.1758266724       0.0000000000
          0.0000000000       0.0000000000       7.7072650000
    K_POINTS crystal ! 来源于0-kgrid/WFN.out

* pp_in

.. code-block:: Bash

    &input_pw2bgw
       prefix = 'BN'              ! 体系名称
       real_or_complex = 2
       wfng_flag = .true.
       wfng_file = 'wfn.complex'
       wfng_kgrid = .true.
       wfng_nk1 = 5               ! k点网格，与WFN.in设置相同
       wfng_nk2 = 5               ! k点网格，与WFN.in设置相同
       wfng_nk3 = 1               ! k点网格，与WFN.in设置相同
       wfng_dk1 = 0.0
       wfng_dk2 = 0.0
       wfng_dk3 = 0.0
       rhog_flag = .true.
       rhog_file = 'rho.cplx'
       vxcg_flag = .false.
       vxcg_file = 'vxc.cplx'
       vxc_flag = .true.
       vxc_file = 'vxc.dat'
       vxc_diag_nmin = 4          
       vxc_diag_nmax = 13        
       vxc_offdiag_nmin = 0
       vxc_offdiag_nmax = 0
    /

* 提交任务脚本

.. code-block:: Bash

    #!/bin/sh
    #PBS -N hey
    #PBS -l nodes=1:ppn=40,walltime=500:00:00
    ##PBS -q workq


    cd $PBS_O_WORKDIR
    source /home/gengzi/apps/intel/19/parallel_studio_xe_2019/psxevars.sh

    date "+01 Today's date is: %D. The time execution %T" >> time.info
    mpirun -np 40 /home/gengzi/apps/qe-6.5/bin/pw.x -nk 1  -in ./input &> ./out 
    mpirun -np 40 /home/gengzi/apps/qe-6.5/bin/pw2bgw.x -in ./pp_in &> ./pp_out
    #rm ./*.wfc*
    #rm ./*.igk*
    date "+02 Today's date is: %D. The time finish %T" >> time.info


* degeneracy_check.x程序后处理

.. code-block:: Bash

    degeneracy_check.x complex >> aa

* aa文件分析

.. code-block:: Bash

    Reading eigenvalues from file wfn.complex
    Number of spins:               1
    Number of bands:              80
    Number of k-points:           25

    == Degeneracy-allowed numbers of bands (for epsilon and sigma) ==
               1
               2
               3
               4 ! 取为vxc_diag_nmin的值
               6
               8
               9
              10
              11
              13
              15 ! 取为vxc_diag_nmax的值
              16
              17
              18
              19
              20
              21
              22
              24
              26
              27
              29
              30
              32
              34
              36
              37
              38
              39
              40
              41
              43
              45
              47
              48
              50
              52
              53
              54
              55
              57
              58
              59
              61
              63
              64
              66
              68
              69
              70
              72
              74
              75
              76
              77
              78
              79  ! 取为5-epsilon/epsilon.inp中number_bands的值，通常比总能带数小1
    Note: cannot assess whether or not highest band     80 is degenerate.

    == Degeneracy-allowed numbers of valence bands (for inteqp, kernel, and absorption) ==
               2
               4
               5
               6
               7
               8

    == Degeneracy-allowed numbers of conduction bands (for inteqp, kernel, and absorption) ==
               1
               2
               3
               5
               7
               8
               9
              10
              11
              12
              13
              14
              16
              18
              19
              21
              22
              24
              26
              28
              29
              30
              31
              32
              33
              35
              37
              39
              40
              42
              44
              45
              46
              47
              49
              50
              51
              53
              55
              56
              58
              60
              61
              62
              64
              66
              67
              68
              69
              70
              71
    Note: cannot assess whether or not highest conduction band     72 is degenerate.

* 修改pp_in文件

.. code-block:: Bash

    &input_pw2bgw
       prefix = 'BN'            
       real_or_complex = 2
       wfng_flag = .true.
       wfng_file = 'wfn.complex'
       wfng_kgrid = .true.
       wfng_nk1 = 5             
       wfng_nk2 = 5               
       wfng_nk3 = 1               
       wfng_dk1 = 0.0
       wfng_dk2 = 0.0
       wfng_dk3 = 0.0
       rhog_flag = .true.
       rhog_file = 'rho.cplx'
       vxcg_flag = .false.
       vxcg_file = 'vxc.cplx'
       vxc_flag = .true.
       vxc_file = 'vxc.dat'
       vxc_diag_nmin = 4         ! 见Note说明
       vxc_diag_nmax = 13        ! 见Note说明        
       vxc_offdiag_nmin = 0
       vxc_offdiag_nmax = 0
    /


.. note:: 

    vxc_diag_nmin取价带顶下约5-10条带，其中取的最下面的一条带要求与下面的带连续，例如取的最下面的带是4，则下一条必须是3

    vxc_diag_nmax取导带顶上约5-10条带，其中取的最上面的一条带要求与上面的带连续，例如取的最上面的带是13，则再上一条带必须是14

* 提交任务重新计算

.. code-block:: Bash

    #!/bin/sh
    #PBS -N hey
    #PBS -l nodes=1:ppn=40,walltime=500:00:00
    ##PBS -q workq


    cd $PBS_O_WORKDIR
    source /home/gengzi/apps/intel/19/parallel_studio_xe_2019/psxevars.sh

    date "+01 Today's date is: %D. The time execution %T" >> time.info
    # mpirun -np 40 /home/gengzi/apps/qe-6.5/bin/pw.x -nk 1  -in ./input &> ./out 
    mpirun -np 40 /home/gengzi/apps/qe-6.5/bin/pw2bgw.x -in ./pp_in &> ./pp_out
    #rm ./*.wfc*
    #rm ./*.igk*
    date "+02 Today's date is: %D. The time finish %T" >> time.info

3-wfnq
------

* input

.. code-block:: Bash
    
     &CONTROL
                           title = 'BN' ,
                     calculation = 'bands' ,
                    restart_mode = 'from_scratch' ,
                      wf_collect = .false. ,
                          outdir = './' ,
                          wfcdir = './' ,
                      pseudo_dir = './' ,
                          prefix = 'BN' ,
                         tstress = .false. ,
                         tprnfor = .false. ,
     /
     &SYSTEM
                            ibrav =0,
                             nat = 4,
                            ntyp = 2,
                           ecutwfc =40,
                           nbnd = 8,     ! 设置为占据态数目

     /
     &ELECTRONS
                electron_maxstep = 100
                conv_thr = 1.0d-7
                mixing_mode = 'plain'
                mixing_beta = 0.4
             diagonalization = 'david'
              diago_david_ndim = 8
              diago_full_acc = .true.
    /
    ATOMIC_SPECIES
    B      10.811 B.SG15.PBE.UPF
    N      14.0067 N.SG15.PBE.UPF
    ATOMIC_POSITIONS crystal
    B            0.6666666700       0.3333333300       0.7500000000
    B            0.3333333300       0.6666666700       0.2500000000
    N            0.6666666700       0.3333333300       0.2500000000
    N            0.3333333300       0.6666666700       0.7500000000
    CELL_PARAMETERS angstrom
          2.5124282300       0.0000000000       0.0000000000
         -1.2562141150       2.1758266724       0.0000000000
          0.0000000000       0.0000000000       7.7072650000
    K_POINTS crystal  ! 来源于0-kgrid/WFNq.out中的k点

* pp_in

.. code-block:: Bash
    
    &input_pw2bgw
       prefix = 'BN'             ! 体系名称
       real_or_complex = 2
       wfng_flag = .true.
       wfng_file = 'wfn.complex'
       wfng_kgrid = .true.
       wfng_nk1 = 5              ! 与WFNq.in设置相同
       wfng_nk2 = 5              ! 与WFNq.in设置相同
       wfng_nk3 = 1              ! 与WFNq.in设置相同
       wfng_dk1 = 0.005          ! k点乘以0.001
       wfng_dk2 = 0.0
       wfng_dk3 = 0.0

* 提交任务脚本

.. code-block:: Bash
    
    #!/bin/sh
    #PBS -N hey
    #PBS -l nodes=1:ppn=40,walltime=500:00:00
    ##PBS -q workq


    cd $PBS_O_WORKDIR
    source /home/gengzi/apps/intel/19/parallel_studio_xe_2019/psxevars.sh

    date "+01 Today's date is: %D. The time execution %T" >> time.info
    mpirun -np 40 /home/gengzi/apps/qe-6.5/bin/pw.x -nk 1  -in ./input &> ./out
    mpirun -np 40 /home/gengzi/apps/qe-6.5/bin/pw2bgw.x -in ./pp_in &> ./pp_out
    rm ./*.wfc*
    rm ./*.igk*
    date "+02 Today's date is: %D. The time finish %T" >> time.info

5-wfn_fi
---------

* input

.. code-block:: Bash
    
     &CONTROL
                           title = 'BN' ,
                     calculation = 'bands' ,
                    restart_mode = 'from_scratch' ,
                      wf_collect = .false. ,
                          outdir = './' ,
                          wfcdir = './' ,
                      pseudo_dir = './' ,
                          prefix = 'BN' ,
                         tstress = .false. ,
                         tprnfor = .false. ,
     /
     &SYSTEM
                            ibrav =0,
                             nat = 4,
                            ntyp = 2,
                           ecutwfc =40,
                           nbnd = 80,        !占据态乘以10


     /
     &ELECTRONS
                electron_maxstep = 100
                conv_thr = 1.0d-7
                mixing_mode = 'plain'
                mixing_beta = 0.4
             diagonalization = 'david'
              diago_david_ndim = 8
              diago_full_acc = .true.
    /
    ATOMIC_SPECIES
    B      10.811 B.SG15.PBE.UPF
    N      14.0067 N.SG15.PBE.UPF
    ATOMIC_POSITIONS crystal
    B            0.6666666700       0.3333333300       0.7500000000
    B            0.3333333300       0.6666666700       0.2500000000
    N            0.6666666700       0.3333333300       0.2500000000
    N            0.3333333300       0.6666666700       0.7500000000
    CELL_PARAMETERS angstrom
          2.5124282300       0.0000000000       0.0000000000
         -1.2562141150       2.1758266724       0.0000000000
          0.0000000000       0.0000000000       7.7072650000
    K_POINTS crystal  ! k点来源于0-kgrid/WFN_fi.out

* pp_in

.. code-block:: Bash
    
    &input_pw2bgw
       prefix = 'BN'
       real_or_complex = 2
       wfng_flag = .true.
       wfng_file = 'wfn.complex'
       wfng_kgrid = .true.
       wfng_nk1 = 7            ! 与WFN_fi.in设置相同
       wfng_nk2 = 7            ! 与WFN_fi.in设置相同
       wfng_nk3 = 1            ! 与WFN_fi.in设置相同
       wfng_dk1 = 0.0
       wfng_dk2 = 0.0
       wfng_dk3 = 0.0
    /

* 提交任务脚本

.. code-block:: Bash
    
    #!/bin/sh
    #PBS -N hey
    #PBS -l nodes=1:ppn=40,walltime=500:00:00
    ##PBS -q workq


    cd $PBS_O_WORKDIR
    source /home/gengzi/apps/intel/19/parallel_studio_xe_2019/psxevars.sh

    date "+01 Today's date is: %D. The time execution %T" >> time.info
    mpirun -np 40 /home/gengzi/apps/qe-6.5/bin/pw.x -nk 1  -in ./input &> ./out
    mpirun -np 40 /home/gengzi/apps/qe-6.5/bin/pw2bgw.x -in ./pp_in &> ./pp_out
    rm ./*.wfc*
    rm ./*.igk*
    date "+02 Today's date is: %D. The time finish %T" >> time.info


6-wfn_fiq
-----------

* input

.. code-block:: Bash
    
     &CONTROL
                           title = 'BN' ,
                     calculation = 'bands' ,
                    restart_mode = 'from_scratch' ,
                      wf_collect = .false. ,
                          outdir = './' ,
                          wfcdir = './' ,
                      pseudo_dir = './' ,
                          prefix = 'BN' ,
                         tstress = .false. ,
                         tprnfor = .false. ,
     /
     &SYSTEM
                            ibrav =0,
                             nat = 4,
                            ntyp = 2,
                           ecutwfc =40,
                           nbnd = 8,     ! 设置为占据态

     /
     &ELECTRONS
                electron_maxstep = 100
                conv_thr = 1.0d-7
                mixing_mode = 'plain'
                mixing_beta = 0.4
             diagonalization = 'david'
              diago_david_ndim = 8
              diago_full_acc = .true.
    /
    ATOMIC_SPECIES
    B      10.811 B.SG15.PBE.UPF
    N      14.0067 N.SG15.PBE.UPF
    ATOMIC_POSITIONS crystal
    B            0.6666666700       0.3333333300       0.7500000000
    B            0.3333333300       0.6666666700       0.2500000000
    N            0.6666666700       0.3333333300       0.2500000000
    N            0.3333333300       0.6666666700       0.7500000000
    CELL_PARAMETERS angstrom
          2.5124282300       0.0000000000       0.0000000000
         -1.2562141150       2.1758266724       0.0000000000
          0.0000000000       0.0000000000       7.7072650000
    K_POINTS crystal   ! k点来源于0-kgrid/WFN_fiq.out


* pp_in

.. code-block:: Bash
    
    &input_pw2bgw
       prefix = 'BN'
       real_or_complex = 2
       wfng_flag = .true.
       wfng_file = 'wfn.complex'
       wfng_kgrid = .true.
       wfng_nk1 = 7            ! 与WFN_fiq.in设置相同
       wfng_nk2 = 7            ! 与WFN_fiq.in设置相同
       wfng_nk3 = 1            ! 与WFN_fiq.in设置相同
       wfng_dk1 = 0.007        ! k点乘以0.001
       wfng_dk2 = 0.007        ! k点乘以0.001
       wfng_dk3 = 0.0
    /

* 提交任务的脚本

.. code-block:: Bash
    
    #!/bin/sh
    #PBS -N hey
    #PBS -l nodes=1:ppn=40,walltime=500:00:00
    ##PBS -q workq


    cd $PBS_O_WORKDIR
    source /home/gengzi/apps/intel/19/parallel_studio_xe_2019/psxevars.sh

    date "+01 Today's date is: %D. The time execution %T" >> time.info
    mpirun -np 40 /home/gengzi/apps/qe-6.5/bin/pw.x -nk 1  -in ./input &> ./out
    mpirun -np 40 /home/gengzi/apps/qe-6.5/bin/pw2bgw.x -in ./pp_in &> ./pp_out
    rm ./*.wfc*
    rm ./*.igk*
    date "+02 Today's date is: %D. The time finish %T" >> time.info

7-band
--------

* input

.. code-block:: Bash
    
    &CONTROL
      calculation = 'bands'
      etot_conv_thr =   4.0000000000d-05
      forc_conv_thr =   1.0000000000d-04
      outdir = './'
      prefix = 'BN'
      pseudo_dir = './'
      wfcdir = './'
      tprnfor = .false.
      tstress = .false.
      verbosity = 'high'
      wf_collect = .false.
    /
    &SYSTEM
      ecutwfc =   4.0000000000d+01
      ibrav = 0
      nat = 4
      ntyp = 2
      nbnd = 80                   ! 占据态乘以10
    /
    &ELECTRONS
      conv_thr =   1.0000000000d-7
      electron_maxstep = 80
      mixing_beta =   4.0000000000d-01
      mixing_mode = 'plain'
      diagonalization = 'david'
      diago_david_ndim = 8
      diago_full_acc = .true.
    /
    ATOMIC_SPECIES
    B      10.811 B.SG15.PBE.UPF
    N      14.0067 N.SG15.PBE.UPF
    ATOMIC_POSITIONS crystal
    B            0.6666666700       0.3333333300       0.7500000000 
    B            0.3333333300       0.6666666700       0.2500000000 
    N            0.6666666700       0.3333333300       0.2500000000 
    N            0.3333333300       0.6666666700       0.7500000000 
    CELL_PARAMETERS angstrom
          2.5124282300       0.0000000000       0.0000000000
         -1.2562141150       2.1758266724       0.0000000000
          0.0000000000       0.0000000000       7.7072650000

    K_POINTS crystal {crystal_b} ! 能带计算的高对称点路径
      4
      0      0      0  50
      0.5    0      0  50
      0.3333 0.3333 0  50
      0      0      0  0

* pp_in

.. code-block:: Bash
    
    &input_pw2bgw
       prefix = 'BN'
       real_or_complex = 2
       wfng_flag = .true.
       wfng_file = 'wfn.complex'
       wfng_kgrid = .true.
       wfng_nk1 = 0
       wfng_nk2 = 0
       wfng_nk3 = 0
       wfng_dk1 = 0.0
       wfng_dk2 = 0.0
       wfng_dk3 = 0.0
       wfng_occupation=.ture
       wfng_nvmin=1          ! 价带最小值，一般设置为1
       wfng_nvmax= 8         ! 价带最大值，也就是占据态
    /

* 提交任务脚本

.. code-block:: Bash
    
    #!/bin/sh
    #PBS -N hey
    #PBS -l nodes=1:ppn=40,walltime=500:00:00
    ##PBS -q workq


    cd $PBS_O_WORKDIR
    source /home/gengzi/apps/intel/19/parallel_studio_xe_2019/psxevars.sh

    date "+01 Today's date is: %D. The time execution %T" >> time.info
    mpirun -np 40 /home/gengzi/apps/qe-6.5/bin/pw.x -nk 1  -in ./input &> ./out
    mpirun -np 40 /home/gengzi/apps/qe-6.5/bin/pw2bgw.x -in ./pp_in &> ./pp_out
    rm ./*.wfc*
    rm ./*.igk*
    date "+02 Today's date is: %D. The time finish %T" >> time.info

BerkeleyGW计算
###############

5-epsilon
------------

* epsilon.inp

.. code-block:: Bash

    epsilon_cutoff 10          ! 不需要修改
    number_bands 79            ! 取2-wfn/aa中能带的最大值
    band_occupation 8*1 71*0   ! 价带*1，导带*0
    cell_slab_truncation

    begin qpoints ! 来源于WFN.out，最后加1列，第一行为1，其余都是0
      0.001000000  0.000000000  0.000000000   1.0 1 ! 第一个数改为0.001
      0.000000000  0.200000000  0.000000000   1.0 0
      0.000000000  0.400000000  0.000000000   1.0 0
      0.000000000  0.600000000  0.000000000   1.0 0
      0.000000000  0.800000000  0.000000000   1.0 0
      0.200000000  0.000000000  0.000000000   1.0 0
      0.200000000  0.200000000  0.000000000   1.0 0
      0.200000000  0.400000000  0.000000000   1.0 0
      0.200000000  0.600000000  0.000000000   1.0 0
      0.200000000  0.800000000  0.000000000   1.0 0
      0.400000000  0.000000000  0.000000000   1.0 0
      0.400000000  0.200000000  0.000000000   1.0 0
      0.400000000  0.400000000  0.000000000   1.0 0
      0.400000000  0.600000000  0.000000000   1.0 0
      0.400000000  0.800000000  0.000000000   1.0 0
      0.600000000  0.000000000  0.000000000   1.0 0
      0.600000000  0.200000000  0.000000000   1.0 0
      0.600000000  0.400000000  0.000000000   1.0 0
      0.600000000  0.600000000  0.000000000   1.0 0
      0.600000000  0.800000000  0.000000000   1.0 0
      0.800000000  0.000000000  0.000000000   1.0 0
      0.800000000  0.200000000  0.000000000   1.0 0
      0.800000000  0.400000000  0.000000000   1.0 0
      0.800000000  0.600000000  0.000000000   1.0 0
      0.800000000  0.800000000  0.000000000   1.0 0
    end                                              

* 提交任务脚本

.. code-block:: Bash
    
    #!/bin/sh
    #PBS -N hey
    #PBS -l nodes=1:ppn=40,walltime=500:00:00
    ##PBS -q workq

    cd $PBS_O_WORKDIR
    source /home/gengzi/apps/intel/19/mkl/bin/mklvars.sh intel64
    export PATH=$PATH:/home/gengzi/apps/openmpi404/bin
    export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/home/gengzi/apps/openmpi404/lib

    date "+01 Today's date is: %D. The time execution %T" >> time.info
    mpirun -np 40 /home/gengzi/apps/BerkeleyGW-2.1/bin/epsilon.cplx.x &> ./OUT.eps
    date "+02 Today's date is: %D. The time finish %T" >> time.info

6-sigma
----------

* sigma.inp

.. code-block:: Bash
    
    screened_coulomb_cutoff 10
    bare_coulomb_cutoff 70

    number_bands 79
    band_occupation 8*1 71*0
    #frequency_dependence 1
    #no_symmetries_q_grid
    band_index_min  4  ! 取为2-wfn/pp_in中的vxc_diag_nmin值
    band_index_max  13 ! 取为2-wfn/pp_in中的vxc_diag_nmax值
    cell_slab_truncation

    screening_semiconductor
    begin kpoints ! k点来源于WFN.out，最后加一列，第一行为1，其余都是0
      0.000000000  0.000000000  0.000000000   1.0 1
      0.000000000  0.200000000  0.000000000   1.0 0
      0.000000000  0.400000000  0.000000000   1.0 0
      0.000000000  0.600000000  0.000000000   1.0 0
      0.000000000  0.800000000  0.000000000   1.0 0
      0.200000000  0.000000000  0.000000000   1.0 0
      0.200000000  0.200000000  0.000000000   1.0 0
      0.200000000  0.400000000  0.000000000   1.0 0
      0.200000000  0.600000000  0.000000000   1.0 0
      0.200000000  0.800000000  0.000000000   1.0 0
      0.400000000  0.000000000  0.000000000   1.0 0
      0.400000000  0.200000000  0.000000000   1.0 0
      0.400000000  0.400000000  0.000000000   1.0 0
      0.400000000  0.600000000  0.000000000   1.0 0
      0.400000000  0.800000000  0.000000000   1.0 0
      0.600000000  0.000000000  0.000000000   1.0 0
      0.600000000  0.200000000  0.000000000   1.0 0
      0.600000000  0.400000000  0.000000000   1.0 0
      0.600000000  0.600000000  0.000000000   1.0 0
      0.600000000  0.800000000  0.000000000   1.0 0
      0.800000000  0.000000000  0.000000000   1.0 0
      0.800000000  0.200000000  0.000000000   1.0 0
      0.800000000  0.400000000  0.000000000   1.0 0
      0.800000000  0.600000000  0.000000000   1.0 0
      0.800000000  0.800000000  0.000000000   1.0 0
    end                            

* 提交任务脚本

.. code-block:: Bash
    
    #!/bin/sh
    #PBS -N hey
    #PBS -l nodes=1:ppn=40,walltime=500:00:00
    ##PBS -q workq

    cd $PBS_O_WORKDIR
    source /home/gengzi/apps/intel/19/mkl/bin/mklvars.sh intel64
    export PATH=$PATH:/home/gengzi/apps/openmpi404/bin
    export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/home/gengzi/apps/openmpi404/lib

    date "+01 Today's date is: %D. The time execution %T" >> time.info
    mpirun -np 40 /home/gengzi/apps/BerkeleyGW-2.1/bin/sigma.cplx.x &> ./OUT.eps

    python eqp.py  eqp1  sigma_hp.log ../8-absorption/eqp_co.dat
    python eqp.py  eqp1  sigma_hp.log ../9-inteqp/eqp_co.dat

    date "+02 Today's date is: %D. The time finish %T" >> time.info

.. note:: eqp.py脚本位于安装文件bin文件夹中，使用python3环境

7-kernel
-----------

* kernel.inp

.. code-block:: Bash

    number_val_bands  5 ! 画图时包含的价带数
    number_cond_bands 5 ! 画图时包含的导带数

    screened_coulomb_cutoff 10
    bare_coulomb_cutoff  70

    use_symmetries_coarse_grid

    screening_semiconductor
    cell_slab_truncation

* 提交任务脚本

.. code-block:: Bash
    
    #!/bin/sh
    #PBS -N hey
    #PBS -l nodes=1:ppn=40,walltime=500:00:00
    ##PBS -q workq

    cd $PBS_O_WORKDIR
    source /home/gengzi/apps/intel/19/mkl/bin/mklvars.sh intel64
    export PATH=$PATH:/home/gengzi/apps/openmpi404/bin
    export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/home/gengzi/apps/openmpi404/lib

    date "+01 Today's date is: %D. The time execution %T" >> time.info
    mpirun -np 40 /home/gengzi/apps/BerkeleyGW-2.1/bin/kernel.cplx.x &> ./OUT.eps
    date "+02 Today's date is: %D. The time finish %T" >> time.info

8-absorption
---------------

* absorption.inp

.. code-block:: Bash
    
    number_val_bands_coarse 5  ! 画图时包含的价带数
    number_val_bands_fine 5    ! 画图时包含的价带数
    number_cond_bands_coarse 5 ! 画图时包含的导带数
    number_cond_bands_fine 5   ! 画图时包含的导带数

    use_symmetries_fine_grid
    use_symmetries_shifted_grid
    use_symmetries_coarse_grid
    diagonalization
    screening_semiconductor
    cell_slab_truncation
    #use_velocity

    eqp_co_corrections
    #q_shift 0.0006 0.0006 0.000
    use_momentum
    polarization 1.0 0.0 0.0

    energy_resolution 0.05
    gaussian_broadening
    #write_eigenvectors 10

* 提交任务脚本

.. code-block:: Bash
    
    #!/bin/sh
    #PBS -N hey
    #PBS -l nodes=1:ppn=40,walltime=500:00:00
    ##PBS -q workq

    cd $PBS_O_WORKDIR
    source /home/gengzi/apps/intel/19/mkl/bin/mklvars.sh intel64
    export PATH=$PATH:/home/gengzi/apps/openmpi404/bin
    export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/home/gengzi/apps/openmpi404/lib

    date "+01 Today's date is: %D. The time execution %T" >> time.info
    mpirun -np 40 /home/gengzi/apps/BerkeleyGW-2.1/bin/absorption.cplx.x &> ./OUT.eps
    date "+02 Today's date is: %D. The time finish %T" >> time.info

9-inteqp
-----------

* inteqp.inp

.. code-block:: Bash
    
    number_val_bands_coarse 5  ! 画图时包含的价带数
    number_val_bands_fine 5    ! 画图时包含的价带数
    number_cond_bands_coarse 5 ! 画图时包含的导带数
    number_cond_bands_fine 5   ! 画图时包含的导带数

    #use_symmetries_fine_grid
    #no_symmetries_shifted_grid
    #use_symmetries_coarse_grid
    use_momentum
    #comm_mpi

* 提交任务脚本

.. code-block:: Bash
    
    #!/bin/sh
    #PBS -N hey
    #PBS -l nodes=1:ppn=40,walltime=500:00:00
    ##PBS -q workq

    cd $PBS_O_WORKDIR
    source /home/gengzi/apps/intel/19/mkl/bin/mklvars.sh intel64
    export PATH=$PATH:/home/gengzi/apps/openmpi404/bin
    export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/home/gengzi/apps/openmpi404/lib

    date "+01 Today's date is: %D. The time execution %T" >> time.info
    mpirun -np 40 /home/gengzi/apps/BerkeleyGW-2.1/bin/inteqp.cplx.x &> ./OUT.inteqp
    date "+02 Today's date is: %D. The time finish %T" >> time.info

* 提交任务完成计算，得到`bandstructure.dat`文件

数据处理
#########

* kline.py (刘仕明开发)

* klable.txt

.. code-block:: Bash
    
      2.5124282300       0.0000000000       0.0000000000 !晶格常数
     -1.2562141150       2.1758266724       0.0000000000 !晶格常数
      0.0000000000       0.0000000000       7.7072650000 !晶格常数
      4 ! 能带路径设置
      0      0      0  50
      0.5    0      0  50
      0.3333 0.3333 0  50
      0      0      0  0

* bandstructure.dat

* 运行脚本

.. code-block:: Python
    
    python3 kline.py

* 得到kline.dat文件，画图即可
