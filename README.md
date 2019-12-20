# map_download

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
      Index('tile_idx', 'zoom_level', 'tile_column', 'tile_row')
  )
  
class MetaData(BaseModel):
    __tablename__ = 'metadata'
    name = Column(Text)
    value = Column(Text)
    __table_args__ = (
        PrimaryKeyConstraint('name'),
        Index('name', 'name')
    )

    def __init__(self, name, value):
        self.name = name
        self.value = value

    def __repr__(self):
        return '<name:%s, x:%s>' % (self.name, self.value)
  
```

## 使用
- 修改 map_download/ui/main_dialog.py 第 53 行 'your access token in cesium' 为你自己在 cesium 官网申请的 access token
- 确保 python3 环境
- 安装三方库 `pip install --no-cache-dir -i https://mirrors.aliyun.com/pypi/simple/ -r requirements.txt`
- 安装本库 `python setup.py develop`
- 运行 `python run.py`


### 觉得好用的话记得给个 Star 啊
### 有问题欢迎提 issue 啊

## 本库的修改在gds-dev分支上，主要修复了写数据库时未将元数据写入的问题以及tile_row的计算问题
