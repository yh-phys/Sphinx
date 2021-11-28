Matplotlib
==========

常用命令说明
############

1. plt.legend() 显示 **图例**，需要放到画图参数后面

2. plt.savefig('name.png', dpi = 300, bbox_inches = 'tight', facecolor='w')

* 参数说明

| name.png 指定图片格式
| dpi 设置分辨率
| bbox_inches 是否保留外部的空白区域，一般设置为 **tight**
| facecolor 设置背景颜色

3. plt.figure(figsize=(4, 5)) 创建画布

* 参数说明

| figsize 画布尺寸，单位为英寸

4. plt.gca() 获得当前的Axes对象ax
