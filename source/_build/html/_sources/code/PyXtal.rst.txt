PyXtal程序: 晶体结构生成和对称性分析
========================================

**介绍**

内华达大学物理与天文学系的助理教授 **ZHu Qiang** `课题组 <https://qzhu2017.github.io>`_ 开发了 **PyXtal**,这是一个基于Python编程语言的新软件包，用于为原子和分子系统生成具有特定对称性和化学成分的结构。该软件为各种体系提供支持，只需输入化学成分和对称基团信息，PyXtal便可以通过逐步合并方案自动找到Wyckoff位置的合适组合。此外，当给出分子几何结构时，PyXtal可以生成具有不同维度的有机晶体，且分子占据了一般和特殊的Wyckoff位置。PyXtal也可以选择接受用户定义的参数(例如，晶胞参数、最小距离和Wyckoff位置)。通常， **PyXtal具有三个功能**：

1. 生成自定义结构
2. 通过对称关系来调制结构
3. 连接需要生成随机对称结构的现有结构预测代码

此外，我们提供了一些实用程序，可简化结构分析，包括对称性分析、几何优化和粉末X射线衍射(XRD)的模拟。

* PyXtal内部原理框架：

.. image:: ../fig_code/20210204001.png


* PyXtal下载链接：:download:`Github <https://github.com/qzhu2017/PyXtal>`

* PyXtal主页

.. image:: ../fig_code/20210204002.png

* PyXtal官方文档: `Document <https://pyxtal.readthedocs.io/en/latest/>`_

* 官方文档

.. image:: ../fig_code/20210204003.png

* 参考文献

.. image:: ../fig_code/20210204004.png
