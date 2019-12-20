# map_download


### 本库与上游项目的差异代码 在gds-dev分支上，主要修复了写数据库时未将元数据写入的问题以及tile_row的计算问题

## 用 python 实现的一个 `地图下载` 的小工具
支持如下
- Google 混合影像、天地图注记、Cesium 地形下载
- 多线程下载
- 下载数据写入 sqlite 数据库，支持快速拷贝读取等，其中数据库结构如下：
```
class Tiles(BaseModel):
  __tablename__ = 'tiles'
  zoom_level = Column(Integer)
  tile_column = Column(Integer)
  tile_row = Column(Integer)
  tile_data = Column(LargeBinary)
  __table_args__ = (
      PrimaryKeyConstraint('zoom_level', 'tile_column', 'tile_row'),
      Index('data_idx', 'zoom_level', 'tile_column', 'tile_row')
  )
```

## 使用
- 修改 map_download/ui/main_dialog.py 第 53 行 'your access token in cesium' 为你自己在 cesium 官网申请的 access token
- 确保 python3 环境
- 安装三方库 `pip install --no-cache-dir -i https://mirrors.aliyun.com/pypi/simple/ -r requirements.txt`
- 安装本库 `python setup.py develop`
- 运行 `python run.py`


### 觉得好用的话记得给个 Star 啊
### 有问题欢迎提 issue 啊

## 工作流程及参考资料
1、使用map_download工具下载google瓦片数据
https://github.com/cugxy/map_download.git
2、使用mbutil或tippecanoe生成mbtiles
http://github.com/mapbox/mbutil.git
3、使用geoserver生成发布mbtiles。需要先安装一个支持mbtiles的geoserver插件
参考：https://blog.csdn.net/u013592964/article/details/53337968

资料：
https://blog.csdn.net/u013420816/article/details/83828134 栅格瓦片转mbtiles文件离线部署

https://blog.csdn.net/u013323965/article/details/53213298 利用MBTiles技术原理减轻离线地图的存储量
https://zhuanlan.zhihu.com/p/31185974 开源工具生成本地矢量瓦片
python mb-util E:\ws\map\map_download\data\label-google-17\Google\ E:\ws\map\map_download\data\label-google-17\tianhe2.mbtiles --image_format=jpg

https://blog.csdn.net/supermapsupport/article/details/72781962 MBTiles离线包生成和使用

####  实验数据：1*1° 16级2G 17级4G 18级7G 19级google影像jpg格式13G 20级25G 21级50G
