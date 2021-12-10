Matplotlib
===========

常用命令说明
############

* plt.legend() 显示 **图例**，需要放到画图参数后面

* plt.savefig('name.png', dpi = 300, bbox_inches = 'tight', facecolor='w')

.. attention:: 
    
    name.png 指定图片格式
    
    dpi 设置分辨率
    
    bbox_inches 是否保留外部的空白区域，一般设置为 **tight**
    
    facecolor 设置背景颜色

* plt.figure(figsize=(4, 5)) 创建画布

.. attention:: 
    
    figsize 画布尺寸，单位为英寸
    
    plt.gca() 获得当前的Axes对象ax

* Matplotlib画图通用设置参数

.. code-block:: Python

    import matplotlib.pyplot as plt
    from matplotlib.ticker import MultipleLocator

    plt.rcParams['text.usetex'] = True

    plt.rcParams['font.size'] = 18
    plt.rcParams['font.family'] = "Times New Roman"
    tdir = 'in'
    major = 5.0
    minor = 3.0
    plt.rcParams['xtick.direction'] = tdir
    plt.rcParams['ytick.direction'] = tdir
    plt.rcParams['xtick.major.size'] = major
    plt.rcParams['xtick.minor.size'] = minor
    plt.rcParams['ytick.major.size'] = major
    plt.rcParams['ytick.minor.size'] = minor

