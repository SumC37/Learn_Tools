**ndarray**

# 数组基础
- 对列表进行切片会复制元素到新列表，而对数组切片则返回视图——一个指向原数组数据的对象

# 数组属性
- `ndim`存储数组的维度数量
- `shape`由非负整数组成的元组，指定每个维度上的元素数量
- `size`存储数组中的固定元素总数
- `dtype`存储数据类型

# 创建数组
- 填充的数组
	- `np.zeros(2)`创建填充 `0` 的数组
	- `np.ones(3)`
	- `np.empty(4)`数组初始内容是随机的，取决于内存状态
	- `np.arange(5,10,2)`包含等间距数值的数组
	- `np.linspace(0,10,num=5)`在指定区间内线性间隔数值
	- 默认数据类型是浮点型,可以使用 `dtype` 关键字显式指定所需的数据类型

# 操作元素
- 排序
	- `np.sort()`返回数组排序副本
	- `argsort` 沿指定轴进行间接排序
	- `lexsort` 基于多键的间接稳定排序
	- `searchsorted` 用于在已排序数组中查找元素
	- `partition` 这是一种部分排序
- `np.concatenate()` 连接元素`np.concatenate((x, y), axis=0)`
- `np.delete(arr, 2)`使用索引来选择想保留的元素
- `reshape()` 重塑数组`np.reshape(a, shape=(1, 6), order='C')`，返回副本，不修改数组本身
	- `a`待重塑的数组
	- `shape`新形状
	- `order`索引顺序读写元素可选
		- `C`
		- `F`
		- `A`当数组在内存中是 Fortran 连续时采用类 Fortran 顺序，否则采用类 C 顺序
- 为数组添加新轴
	- `np.newaxis` 会将数组的维度增加一维
		- `a2 = a[np.newaxis, :]`加入一个行向量
	- `np.expand_dims`在指定位置插入新轴来扩展数组
		- `b = np.expand_dims(a, axis=1)`在索引位置1添加新轴。索引例：`（6，）`

# 索引和切片
```python
>>> (a2%2==0)
array([[False,  True],
       [False,  True],
       [False,  True]])
>>> a2[a2%2==0]
array([2, 4, 6])
>>> a2[1][1]
np.int64(4)
```
- 用圆括号返回布尔值
- `&`与
- `|`或
- `np.nonzero()` 从数组中选择元素或索引，返回一个由每个维度对应一个数组组成的元组
- `zip()`将多个可迭代对象中对应的元素打包成一个个元组，然后返回这些元组组成的对象，传入的迭代器长度不一致，会以最短的迭代器为准，多出来的元素将被忽略
- `list()`将任何可迭代数据转换为列表类型，并返回转换后的列表

# 从现有数据创建新数组
- `vstack()` 垂直堆叠
- `hstack()` 水平堆叠
- `hsplit()` 将数组拆分为多个较小的数组。可以指定要返回的等形状数组数量，或者指定拆分发生的列位置
- `view()` 方法创建一个新数组对象，该对象与原始数组共享相同数据（浅拷贝）
- 修改切片也会修改原对象中对应的数据
- `copy()` 方法会创建数组及其数据的完整副本（深拷贝）

# 操作数组
- 数组维度必须兼容——例如当两个数组维度相等，或其中一个维度为 1 时
- `sum() min() max()`,`mean` 计算平均值， `prod` 获取元素相乘结果， `std` 计算标准差，可用`axis`对轴操作

# 随机数
- 当 NumPy 打印 N 维数组时，最后一个轴的变化速度最快，而第一个轴的变化速度最慢
- 初始化数组的值
	- `ones()`   `zeros()`
	-  `random.Generator类`随机数生成
```python
>>> rng = np.random.default_rng() #实例化一个随机数生成器对象
>>> rng.random(3) #随机生成均匀分布的浮点数
array([0.63696169, 0.26978671, 0.04097352])
#或者
>>> np.random((3,2))
```
- `Generator.integers`生成从低值（包含）到高值（不包含）的随机整数。设置 `endpoint=True` 可以使高值变为包含
```python
>>> tth = np.random.default_rng()
>>> tth.integers(5,9, size=(2, 4)) #随机生成离散均匀分布的整数
array([[7, 6, 7, 8],
       [5, 7, 6, 7]])
```

# 获取唯一项&计数
- `np.unique` 打印数组中的唯一值，自动排序，
	- `return_index` 参数，获取索引
	- `return_counts`参数，获取出现频次
	- `axis=0`获取唯一行；`axis=1`获取唯一列。未设置`axis`参数，返回值将被展平

# 矩阵转置&重塑
- `data.T`转置，属性，不接受任何参数，返回视图
- `transpose()` 转置，方法，根据指定的值反转或更改数组的轴形状，返回视图
- `reshape()`切换维度

# 反转数组
- `np.flip()` 沿指定轴翻转或反转数组内容，需指定要反转的数组及轴向。若不指定轴向， 将沿输入数组的所有轴向反转内容
- `arr_2d[:,1] = np.flip(arr_2d[:,1])`反转单轴

# 重塑&展平多维数组
- `.flatten()` 创建副本
- `.ravel()`创建视图

# **使用`help()`查询对象功能及用法**
- ipyhon用`?`和`??`

# 数学公式运算
- `square()`平方
- `sum()`求和

# 保存&加载numpy对象
- `np.save`存储单个 ndarray 对象，保存为.npy 文件。指定要保存的数组和文件名
- `np.savez`在单个文件中存储多个 ndarray 对象，将其保存为.npz 文件
- `savez_compressed` 将多个数组以压缩的 npz 格式保存到单个文件中
- `np.load()` 重载数组
- `np.savetxt`将 NumPy 数组保存为纯文本文件，例如.csv 或.txt 文件，可以指定页眉、页脚、注释等内容
- `loadtxt()` 加载已保存的文本文件
- `savetxt()` 和 `loadtxt()` 函数接受额外的可选参数，例如表头、页脚和分隔符
- 文本文件更便于共享，但.npy 和.npz 文件体积更小且读取速度更快
- `genfromtxt`对文本文件进行更复杂的处理（例如处理包含缺失值的行）

# 导入&导出CSV文件
- 用pandas读取包含现有信息的 CSV 文件
```python
import pandas as pd

# If all of your columns are the same type:
x = pd.read_csv('music.csv', header=0).values

# You can also simply select the columns you need:
x = pd.read_csv('music.csv', usecols=['Artist', 'Plays']).values
```
- 用 Pandas 导出数组
```python
a = np.array([[1,2,3],[2,3,4],[3,4,5],[4,5,6]]) #创建数组
df = pd.DataFrame(a) #创建pandas数据框
df.to_csv('pd.csv') #保存数据
data = pd.read_csv('pd.csv') #读取
```
- 使用命令行时可以用`$ cat np.csv`读取文件