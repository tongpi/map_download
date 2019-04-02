# map_download

## 用 python 实现的一个 `地图下载` 的小工具
支持如下
- Google 混合影像、天地图影像、Cesium 地形下载
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

### 觉得好用的话记得给个 Star 啊
