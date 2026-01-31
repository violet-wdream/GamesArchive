## 奥奇传说网页版 Spine 无加密 资源分类

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

参考[网页动画应该如何提取呢（已解决） - 讨论 - Live2DHub](https://live2dhub.com/t/topic/4008/2)

现在只获取到了90kb版本https://aoqi.100bt.com/h5/version.json，没什么用

之前有3621kb的，来自隔壁。[GamesArchive/AoQiChuanShuo/File/version~202510101760026048.json at main · violet-wdream/GamesArchive](https://github.com/violet-wdream/GamesArchive/blob/main/AoQiChuanShuo/File/version~202510101760026048.json)

**根据batman提供的信息，目标变得很明确了。**

**需要把当前时间转为UNIX时间戳，然后获取version，之前考虑过是不是这个Stamp的问题，但是实测这个Stamp好像可有可无，然后就删了。**

**不过得到了个更加优雅的方式来获取Spine的ID，确实没注意到这么个东西。**

**`https://aoqi.100bt.com/h5/config/pet/petspineicon.json`**

从马后炮的角度来看，中间的部分其实也是有缝隙的，只探测两边是不合理的。

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

从马后炮的角度来看，中间的部分其实也是有缝隙的，只探测两边是不合理的。

下面的留作纪念吧。

> > 不过好在命名比较整齐，然后数据范围相当可观，可以暴力破解获取，实际上序号大致是递增的（且递增步数最大为110，这里取150作为阈值），所以可以先测试一下1 ~ MIN - 1 的序号是否存在资源，这里检测测试了下左侧的区间1 ~ MIN - 1应该是没东西的。只需要测一下右边的区间就行。
> >
> > 接下来检测MAX + 1 ~ MAX + 150 的序号是否存在资源，如果存在一个资源序号为MAX1 > MAX，这里就可以更新MAX为MAX1然后继续探索，直到连续150个序号没有资源就停止。
> >
> > 随着游戏后续更新，序号的递增步长可能会超过150，所以可能需要动态更新这个阈值。
> >
> > 可供参考的数据：
> >
> > 1. 2025-10-10 获取的最大序号是6657 
> > 2. 2026-1-30 获取的最大序号是 7114 

### 处理

[直接通过`petspineicon.json`的ID拼接URL得到所有 output.txt](https://github.com/violet-wdream/.Scripts/blob/main/Games/AoQiChuanShuo/GetResListNew.py)

### 下载

可用Aria2c下载。

```c
aria2c -i output.txt -d output
```

或者

[DownLoader](https://github.com/violet-wdream/.Scripts/blob/main/DownLoad/UrlsDownLoader.py)

### 解压mix文件

[MIX文件处理](https://github.com/violet-wdream/.Scripts/blob/main/Games/AoQiChuanShuo/MixFileProcess.py)

### 检测完整性 （可选）

[检测Spine文件的完整性](https://github.com/violet-wdream/.Scripts/blob/main/SpineFileProcess/CheckSpineFiles.py)

### 总结

1. 直接通过`petspineicon.json`的ID拼接URL得到所有 output.txt
2. 批量下载mix
3. 解压mix
4. 校验完整性

修正了ID获取方式，总数1705， [GamesArchive/AoQiChuanShuo/File/Spine at main · violet-wdream/GamesArchive ](https://github.com/violet-wdream/GamesArchive/tree/main/AoQiChuanShuo/File/Spine)仅供参考，有很多重复的但是ID不同，其实没什么参考价值。