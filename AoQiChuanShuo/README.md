## 奥奇传说网页版 Spine

鉴赏环节。8.0分。

时间过的真快😩，十年老兵，难凉热血

![image-20260130170114763](https://cdn.jsdelivr.net/gh/violet-wdream/Drawio/PNG/202601301701900.png)

现在已经变成邵g传说了。碍于这个web端的性能，分辨率属实可惜，不过也有可能是webp的问题，但是我比对了一下png和webp的两个版本，几乎看不出任何区别。

这个应该是新出的皮肤，7060

<img src="https://cdn.jsdelivr.net/gh/violet-wdream/Drawio/PNG/202601301954592.png" alt="image-20260130195235868" style="zoom:50%;" />

7114

<img src="https://cdn.jsdelivr.net/gh/violet-wdream/Drawio/PNG/202601301954873.png" alt="image-20260130195414677" style="zoom:50%;" />

这不是我们碧蓝航线的圣路易斯吗，下次记得标明出处🥵

<img src="https://cdn.jsdelivr.net/gh/violet-wdream/Drawio/PNG/202601301958750.png" alt="image-20260130195829531" style="zoom:50%;" />

<img src="https://cdn.jsdelivr.net/gh/violet-wdream/Drawio/PNG/202601302000002.png" alt="image-20260130200039830" style="zoom:50%;" />

<img src="https://cdn.jsdelivr.net/gh/violet-wdream/Drawio/PNG/202601301950614.png" alt="image-20260130195034338" style="zoom:50%;" />

<img src="https://cdn.jsdelivr.net/gh/violet-wdream/Drawio/PNG/202601301951710.png" alt="image-20260130195105517" style="zoom:50%;" />

<img src="https://cdn.jsdelivr.net/gh/violet-wdream/Drawio/PNG/202601302005769.png" alt="image-20260130200503560" style="zoom:50%;" />

![image-20260130220749205](https://cdn.jsdelivr.net/gh/violet-wdream/Drawio/PNG/202601302207356.png)

万物起源

<img src="https://aoqi.100bt.com/h5/peticon/background/peticon2858.webp" alt="img" style="zoom:50%;" />

<img src="https://aoqi.100bt.com/h5/peticon/background/peticon6367003.webp" alt="img" style="zoom:50%;" />

### 获取清单

现在只获取到了90kb版本https://aoqi.100bt.com/h5/version.json，没什么用

之前有3621kb的，来自隔壁。[GamesArchive/AoQiChuanShuo/File/version~202510101760026048.json at main · violet-wdream/GamesArchive](https://github.com/violet-wdream/GamesArchive/blob/main/AoQiChuanShuo/File/version~202510101760026048.json)

等待好心人提供新的version获取方式。

```C
VERSION 
https://aoqi.100bt.com/h5/version.json
BASE-URL
https: //aoqi.100bt.com/h5/
海报
https://aoqi.100bt.com/h5/peticon/background/peticon6657.webp
静态立绘
https://aoqi.100bt.com/h5/peticon/static/peticon6657.webp
Spine立绘
https://aoqi.100bt.com/h5/peticon/spine/peticon6657.mix
```

不过好在命名比较整齐，然后数据范围相当可观，可以暴力破解获取，实际上序号大致是递增的（且递增步数最大为110，这里取150作为阈值），所以可以先测试一下1 ~ MIN 的序号是否存在资源，这里检测测试了下左侧的区间1~MIN应该是没东西的。只需要测一下右边的区间就行。

接下来检测MAX+1~MAX+150 的序号是否存在资源，如果存在一个资源序号为MAX1 > MAX，这里就可以更新MAX为MAX1然后继续探索，直到连续150个序号没有资源就停止。

随着游戏后续更新，序号的递增步长可能会超过150，所以可能需要动态更新这个阈值。

可供参考的数据：

1. 2025-10-10 获取的最大序号是6657  
2. 2026-1-30 获取的最大序号是 7114 



[.Scripts/Games/AoQiChuanShuo/GetResList.py at main · violet-wdream/.Scripts](https://github.com/violet-wdream/.Scripts/blob/main/Games/AoQiChuanShuo/GetResList.py)

用于取出version文件中的mix路径并升序排序得到`output.txt`。

[GamesArchive/AoQiChuanShuo/File/output.txt at main · violet-wdream/GamesArchive](https://github.com/violet-wdream/GamesArchive/blob/main/AoQiChuanShuo/File/output.txt)

我导出的`output.txt`。

[.Scripts/Games/AoQiChuanShuo/GetUpdateRes.py at main · violet-wdream/.Scripts](https://github.com/violet-wdream/.Scripts/blob/main/Games/AoQiChuanShuo/GetUpdateRes.py)

通过`output.txt`拓展得到理论上的最新的`output.txt`，不保证完整性（包含春节更新的两个皮肤）。



### 下载

可用Aria2c下载。

```c
aria2c -i output.txt -d output
```

或者

[.Scripts/DownLoad/UrlsDownLoader.py at main · violet-wdream/.Scripts](https://github.com/violet-wdream/.Scripts/blob/main/DownLoad/UrlsDownLoader.py)

### 解压mix文件

[.Scripts/Games/AoQiChuanShuo/MixFileProcess.py at main · violet-wdream/.Scripts](https://github.com/violet-wdream/.Scripts/blob/main/Games/AoQiChuanShuo/MixFileProcess.py)

### 检测完整性

[.Scripts/SpineFileProcess/CheckSpineFiles.py at main · violet-wdream/.Scripts](https://github.com/violet-wdream/.Scripts/blob/main/SpineFileProcess/CheckSpineFiles.py)