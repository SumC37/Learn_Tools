#input #fleeting 

- `geopandas.GeoDataFrame` `pandas.DataFrame` 的子类,存储几何列并执行空间操作
- `geopandas.GeoSeries` `pandas.Series` 的子类,处理几何图形
- `GeoDataFrame` 实际上是传统数据（数值型、布尔型、文本等）与几何图形（点、多边形等）的结合体。可以拥有任意数量的几何列
- `GeoSeries.crs` 属性用于存储投影信息