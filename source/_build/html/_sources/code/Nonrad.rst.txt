Nonrad程序: 计算载流子非辐射捕获系数
=====================================

**介绍**

半导体晶体中的 **点缺陷** 为 **载流子非辐射复合** 提供了一种手段，这种复合过程会影响器件的性能。Alkauskas等人在2014年首先提出了基于量子力学方法来评估捕获过程[Phys. Rev. B 90, 075202 (2014)]，详细来说他们提出了一种评估投影缀加平面波中电子与声子耦合的方法。最近加州大学圣塔芭芭拉分校 **Chris G. Van de Walle教授** 课题组在Alkauskas等人的基础上开发了 **Nonrad程序** 用于评估载流子非辐射捕获系数。他们还表明，用高斯替换Dirac德尔塔函数的通用过程会在所得的捕获率中引入误差，并实现一种替代方案来适当考虑振动展宽。最后，他们通过与直接数值评估进行比较来评估对Sommerfeld参数使用解析近似的准确性。

* Nonrad下载链接和官方文档：

:download:`Github <https://github.com/mturiansky/nonrad>`

`Document <https://nonrad.readthedocs.io/en/latest/>`_

* Nonrad安装方法：

.. code-block:: Bash
    
    pip install nonrad

如果科研中使用了Nonrad，请帮忙引用下面的文献: `Ref <https://arxiv.org/pdf/2011.07433.pdf>`_

* 文献截图

.. image:: ../fig_code/20210202005.png

* 实际例子

.. image:: ../fig_code/20210202006.png

* 实际结果

.. image:: ../fig_code/20210202007.png

