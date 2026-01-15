
> [!tip ] 
> **地理空间数据处理，生成地图及开展其他地理空间数据分析**
> **面向对象的投影定义，在不同投影间转换点、线、矢量、多边形和图像**

# 坐标参考系统CRS
- `cartopy.crs.CRS` 类，cartopy 中所有坐标参考系统都以 `CRS` 作为父类
- `cartopy.crs.CRS(proj4_params, globe=None)`
	- `proj4_params` 这是一个可迭代的键值对集合，用于定义所需的CRS参数，**禁止**在 `proj4_params` 中包含椭球体模型参数（如 `a`, `b`, `f`, `ellps` 等），`proj4_params` 参数将覆盖 Globe 定义的任何参数
    - `globe` `Globe` 实例，可选，用于定义地球的椭球体模型（形状和大小），默认是 WGS84 椭球体
- `globe` `CRS`实例
- `as_geocentric()`返回一个与此 CRS 具有相同椭球体/基准面的新地心坐标系 CRS
- `as_geodetic()`返回一个与此 CRS 具有相同椭球体/基准面的新大地坐标系 CRS
- `transform_point(x,y,src_crs)`将给定源坐标系（ `src_crs` ）中的 float64 坐标对转换至当前坐标系。参数:
	- `x y`待转换的坐标
	- `src_crs`表示 `x` 和 `y` 坐标系的 `CRS` 类实例
	- `trap`是否捕获 proj 关于"纬度或经度超出限制"及"容差条件错误"的报错
- `transform_points(src_crs,x,y[,z])`将给定源坐标系（ `src_crs` ）中的坐标转换至当前坐标系。参数：
	- `src_crs`
	- `x`待转换的 x 坐标数组（基于 `src_crs` 坐标系）。可为一维或二维数组
	- `y`需要转换的 y 坐标（数组），采用 `src_crs` 坐标系。其形状必须与 x 坐标保持一致。
	- `z`（可选）需要转换的 z 坐标（数组），采用 `src_crs` 坐标系。默认为 None。若提供，其形状必须与 x 坐标保持一致
	- `trap`
	- 返回该坐标系下形状为 `x.shape + (3, )` 的数组
- `transform_vectors(src_proj,x,y,u,v)` 将给定向量分量（位于指定源坐标系 `src_proj` 中）转换至当前坐标系。向量分量必须相对于源投影的坐标参考系（网格东向和网格北向）给出。参数：
	- `src_proj`表示向量所在坐标系的 `CRS.Projection`
	- `x y`源投影中向量的x，y坐标
	- `u v`向量的网格东、北分向量
	- x、y、u 和 v 可以是一维或二维的，但必须保持形状一致
	- 返回变换后的矢量分量ut vt
	-  用于变换矢量的算法是近似而非精确变换，但其精度足以满足可视化需求
- `Global` 类用于封装任何 cartopy 坐标参考系统（CRS）的基础球体或椭球体。所有 CRS 都有一个关联的 `Globe` ，通常是默认的参考椭球体（即“WGS84”）`Globe` 。参数：
	- `datum_Proj` “datum“定义，默认为`None`
	- `ellipse_Proj`“ellps”定义，默认为`None`
	- `semimajor_axis`球体/椭球体的半长轴，~
	- `semiminor_axis`椭球体的半短轴，~
	- `flattening`扁率，~
	- `inverse_flattening`反扁率，~
	- `towgs84`直接传递给 Proj 定义的参数，~
	- `nadgrids`直接传递给 Proj 定义的参数，~
- `cartopy.crs.Projection` 类表示一个二维坐标系，可以直接绘制成地图。`Projection` 是 Cartopy 投影列表中所有投影的父类
	- `cartopy.crs.Projection(*args,**kwargs)`定义一个具有平面拓扑结构和欧几里得距离的投影坐标系。参数：
		- `proj4_params`
		- `globe`
	- `project_geometry(geometry,src_crs=None)`将给定几何图形投影至此坐标系中。参数：
		- `geometry`待（重新）投影的几何图形
		- `src_crs`默认采用目标坐标系的大地测量版本
		- 返回投影结果（shapely 几何对象）
	- `quick_vertices_transform(_vertices_, _src_crs_)`尽可能将给定形状为 `(n, 2)` 的顶点数组和源坐标参考系统转换为CRS，并返回转换后的顶点数组
		- 该方法可能返回 None 以表示顶点无法快速转换，需要更复杂的几何变换
- 存在一些非 `Projection` 子类。这些坐标系参考系统是三维的，无法直接在纸上绘制
	- `cartopy.crs.Geodetic(globe=None)`定义一个具有球面拓扑结构的经纬度坐标系，地理距离和坐标，以度为单位测量
	- `cartopy.crs.Geocentric(globe=None)`定义一个地心坐标系，其中 x、y、z 是从地球中心出发的笛卡尔坐标
	- `cartopy.crs.RotatedGeodedic(pole_longitude,pole_latitude,central_rotated_longitude=0.0,globe=None)`定义一个具有球面拓扑结构和地理距离的旋转经纬度坐标系，坐标以度为单位。使用 proj 执行 ob_tran 变换操作，首先通过 pole_longitude 设置 lon_0 参数，然后基于 pole_latitude 和 central_rotated_longitude 进行两次旋转。这相当于将新极点定位到由 globe 定义的 GeogCRS 坐标系中 pole_latitude 和 pole_longitude 值指定的位置，再使用 central_rotated_longitude 值绕该新极点旋转整个坐标系。参数
		- `pole_longitude`极点经度位置（以未旋转前的角度表示）
		- `pole_latitude ` 极点纬度位置（以未旋转前的角度表示）
		- `central_rotated_longitude`(可选)绕新极点的经度旋转角度（以度为单位），默认值为 0
- 有一个函数可通过指定代码调用 epsg.io，返回相应的 cartopy 投影
	- `crtopy.crs.epsg(code)`返回与给定 EPSG 代码对应的投影。EPSG 编码必须对应"投影坐标系"，因此诸如 4326（WGS-84）这类定义"大地坐标系"的 EPSG 编码将无法使用。该函数转换由 pyproj.CRS 执行

# 绘图
## 常用投影类型对比

| 投影类型     | 代码                        | 适用场景    | 变形特点  |
| -------- | ------------------------- | ------- | ----- |
| **等距圆柱** | `ccrs.PlateCarree()`      | 简单全球图   | 高纬度拉伸 |
| **墨卡托**  | `ccrs.Mercator()`         | 航海/网络地图 | 面积变形  |
| **兰勃特**  | `ccrs.LambertConformal()` | 中纬度地区   | 形状保持  |
| **正交**   | `ccrs.Orthographic()`     | 球形效果图   | 透视变形  |
| **罗宾森**  | `ccrs.Robinson()`         | 统计世界图   | 平衡变形  |
|          | `ccrs.Mollweide()`        |         |       |
```python
>>> import cartopy.crs as ccrs
>>> import matplotlib.pyplot as plt
>>>
>>> ax = plt.axes(projection=ccrs.Mollweide()) #实例化一个投影
>>> ax.stock_img() #添加参考底图
>>> ax.coastlines() #绘制海岸线
```

## 图幅
- 对于"全球"绘图，请使用 `set_global()` 方法
- 要基于任意坐标系中的边界框设置地图范围，可使用 `set_extent()` 方法
- `ax.set_xlim(left=None, right=None, emit=True, auto=False, *, xmin=None, xmax=None)`  `ax.set_ylim()`在 GeoAxes 的本地坐标系中使用标准限制设置方法。参数：
	- `emit`是否触发 'xlim_changed' 事件
	- `auto`是否启用自动缩放
- `ax.set_extent([x0, x1, y0, y1],crs=projection)`
- `img_extent(x0, x1, y0, y1)`精确指定栅格影像（如卫星图、地图瓦片等）在地理坐标系中的空间范围。

## 恒值图
- `contour([X,Y,]Z,/,[levels],**kwargs)`
- `contourf(*args, **kwargs)`

# 矢量绘图
- `quivers`
- `barbs`
- `streamplots`
- `cartopy.vector_transform.vector_scalar_to_grid()`将矢量场重新网格化为目标投影上规则网格

# 经纬网和刻度
- `from cartopy.mpl.ticker import (LongitudeFormatter LatitudeFormatter LatitudeLocator)`
	- `LongitudeFomatter LatitudeFomatter`经纬度格式化器
	- `LatitudeLocator`纬度刻度定位器
- `gridlines`(_crs=None_, _draw_labels=False_, _xlocs=None_, _ylocs=None_, _dms=False_, _x_inline=None_, _y_inline=None_, _auto_inline=True_, _xformatter=None_, _yformatter=None_, _xlim=None_, _ylim=None_, _rotate_labels=None_, _xlabel_style=None_, _ylabel_style=None_, _labels_bbox_style=None_, _xpadding=5_, _ypadding=5_, _offset_angle=25_, _auto_update=None_, _formatter_kwargs=None_, _**kwargs_)   在绘图时自动为坐标轴添加指定坐标系的网格线

```python
import matplotlib.pyplot as plt
import matplotlib.ticker as mticker
import cartopy.crs as ccrs

from cartopy.mpl.ticker import (LongitudeFormatter, LatitudeFormatter,
                                LatitudeLocator)

ax = plt.axes(projection=ccrs.Mercator()) #创建一个地理投影实例
ax.coastlines() #绘制海岸线

gl = ax.gridlines(crs=ccrs.PlateCarree(), draw_labels=True,
                  linewidth=2, color='gray', alpha=0.5, linestyle='--') #为坐标轴添加经纬线并添加标签
gl.top_labels = False #取消绘制顶部的标签
gl.left_labels = False #取消绘制左边的标签
gl.xlines = False #取消绘制经线
gl.xlocator = mticker.FixedLocator([-180, -45, 0, 45, 180]) #
gl.ylocator = LatitudeLocator()
gl.xformatter = LongitudeFormatter()
gl.yformatter = LatitudeFormatter()
gl.xlabel_style = {'size': 15, 'color': 'gray'}
gl.xlabel_style = {'color': 'red', 'weight': 'bold'}

plt.show()
```

# 要素接口
- `cartopy.feature`预定义库包

| Name      | Description                                                                                                                                                                                                                                                       |
| --------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| BORDERS   | Country boundaries.                                                                                                                                                                                                                                               |
| COASTLINE | Coastline, including major islands.                                                                                                                                                                                                                               |
| LAKES     | Natural and artificial lakes                                                                                                                                                                                                                                      |
| LAND      | Land polygons, including major islands.                                                                                                                                                                                                                           |
| OCEAN     | Ocean polygons.                                                                                                                                                                                                                                                   |
| RIVERS    | Single-line drainages, including lake centerlines.                                                                                                                                                                                                                |
| STATES    | Internal, first-order administrative boundaries (limited to the United States at this scale). Natural Earth have first-order admin boundaries for most countries at the 1:10,000,000 scale; these may be accessed with `cartopy.feature.STATES.with_scale('10m')` |
# projection和transform
- 坐标轴的投影方式(projection)与数据定义的坐标系(transfrom)相互独立

# cartopy shapereader
```python
import cartopy.io.shapereader as shpreader
#使用natural_earth()函数从Natural Earth获取国家数据集
shpfilename = shpreader.natural_earth(resolution='110m',category='cultural',name='admin_0_countries') #返回值shapefile文件路径
#利用Reader来获取 shapefile 中的第一个国家
reader = shpreader.Reader(shpfilename) #创建一个Reader对象来读取shapefile
countries = reader.records() #获取shapefile中所有记录的迭代器，只能遍历一次
country = next(countries) #从迭代器中取出第一个国家记录
#通过Record.attributes属性获取该国家的属性字典
print(type(country.attributes))
print(sorted(country.attributes.keys()))
#找到人口最少的5个国家
population = lambda country:country.attributes['POP_EST'] #创建匿名函数提取国家人口
countries_by_pop = sorted(reader.records(),key=population)[:5] #人口升序排序并取前5个
','.join([country.attributes['NAME_LONG'] for country in countries_by_pop]) #提取国家长名称并用逗号连接
```

```python
import cartopy.io.shapereader as shpreader

filename = shpreader.natural_earth(resolution='110m',category='cultural',name = 'admin_0_countries')#返回文件路径

file = shpreader.Reader(filename)#创建一个文件对象用于读取

countries = file.records()#创建一个迭代器对象

af = [country for country in countries if country.attributes["CONTINENT"]=='Africa']#获取非洲国家

pop = lambda country:country.attributes['POP_EST']#获取人口数据

country_pop_af = sorted(af,key=pop)[-4:]#排序并获取人口最多的4个国家

','.join([country.attributes['NAME_LONG'] for country in country_pop_af])#显示
```
- 迭代器是一个实现了迭代协议的对象，即拥有`__iter__`和`__next__`方法。`__iter__`方法返回迭代器对象本身，而`__next__`方法返回迭代器的下一个值。当没有更多的值可返回时，`__next__`会抛出`StopIteration`异常。
- 列表推导式，从一个可迭代对象中创建新列表
```python
squares = [x**2 for x in range(1, 11)]

even_squares = [x**2 for x in range(1, 11) if x % 2 == 0]

nested_list = [[1, 2], [3, 4], [5, 6]]
flattened_list = [x for sublist in nested_list for x in sublist]

pairs = [(x, y) for x in range(1, 4) if x % 2 == 1 for y in range(5, 8) if y % 2 == 0]
```
