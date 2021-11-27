pydefect程序: 非金属体系点缺陷计算
==================================

**介绍**

半导体材料中的缺陷(点缺陷、线缺陷、面缺陷等)对于器件性能具有重要影响，如何构建这些缺陷模型，并且计算这些缺陷(缺陷形成能等)，以及衡量缺陷对性能的影响至关重要。尤其是对于带电缺陷的处理较为复杂，涉及各种经验修正等等，如果没有合适的脚本借助工作量较大。 **pPydefect** 是一个开源dePython库，用于基于VASP第一性原理计算的 **非金属固体中的点缺陷处理** 。开发者是东京工业大学的 **Yu Kumagai教授** 课题组。该程序主要的逻辑框架如下所示：

.. image:: ../fig_code/20210202001.png
    :width: 500px

* 思维框架图:

.. image:: ../fig_code/20210202001.png
    :width: 500px

* pydefect下载链接：:download:`Github <https://github.com/kumagai-group/pydefect>`

.. attention:: 

    Pydefect现在仅支持维也纳ab-initio模拟程序包(VASP)，因此我们假设其输入和输出文件名(例如POSCAR，POTCAR，OUTCAR)以及VASP中使用的计算技术(例如周期性边界条件)

    遵循vasp约定，在pydefect中使用的单位是能量的eV和长度的埃

    仅假定非磁性主体材料

* pydefect官方文档：`Document <https://kumagai-group.github.io/pydefect/>`_

* 官方文档截图

.. image:: ../fig_code/20210202002.png


* 实际例子：

.. image:: ../fig_code/20210202003.png
    :width: 300px

.. image:: ../fig_code/20210202004.png
    :width: 300px


