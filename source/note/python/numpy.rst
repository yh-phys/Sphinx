Numpy
=====

常见函数说明
############

* np.linspace 主要用来创建 **等差数列**

| numpy.linspace(start, stop, num=50, endpoint=True, retstep=False, dtype=None, axis=0)
| Return evenly spaced numbers over a specified interval.(在start和stop之间返回均匀间隔的数据)
| Returns num evenly spaced samples, calculated over the interval [start, stop].
| (返回的是 [start, stop]之间的均匀分布)
| The endpoint of the interval can optionally be excluded.
| Changed in version 1.16.0: Non-scalar start and stop are now supported.
| (可以选择是否排除间隔的终点)

| start:返回样本数据开始点
| stop:返回样本数据结束点
| num:生成的样本数据量，默认为50
| endpoint：True则包含stop；False则不包含stop
| retstep：If True, return (samples, step), where step is the spacing between samples.(即如果为True则结果会给出数据间隔)
| dtype：输出数组类型
| axis：0(默认)或-1
