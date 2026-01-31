## 奥拉星网页版 Spine 无加密 资源分类

跟奥奇传说网页版资源获取方式几乎一致。

鉴赏环节。8.5分。

![img_petskinbackground_889](https://cdn.jsdelivr.net/gh/violet-wdream/Drawio/PNG/202601311452087.png)

![image-20260131144749806](https://cdn.jsdelivr.net/gh/violet-wdream/Drawio/PNG/202601311447864.png)

<img src="https://cdn.jsdelivr.net/gh/violet-wdream/Drawio/PNG/202601311443663.png" alt="image-20260131144330549" style="zoom:50%;" />

<img src="https://cdn.jsdelivr.net/gh/violet-wdream/Drawio/PNG/202601311443531.png" alt="image-20260131144358435" style="zoom:50%;" />

<img src="https://cdn.jsdelivr.net/gh/violet-wdream/Drawio/PNG/202601311444045.png" alt="image-20260131144452963" style="zoom:50%;" />

<img src="https://cdn.jsdelivr.net/gh/violet-wdream/Drawio/PNG/202601311446015.png" alt="image-20260131144559928" style="zoom:50%;" />

<img src="https://cdn.jsdelivr.net/gh/violet-wdream/Drawio/PNG/202601311446984.png" alt="image-20260131144634923" style="zoom:50%;" />

<img src="https://cdn.jsdelivr.net/gh/violet-wdream/Drawio/PNG/202601311447183.png" alt="image-20260131144707110" style="zoom:50%;" />

### 获取清单

现在只获取到了858kb版本https://aola.100bt.com/h5/version.json 

依旧只是部分数据，并不是完整的。同理，还是可以通过这些基础数据来推测范围然后爆破。

```python
VERSION
https://aola.100bt.com/h5/version.json
#序列帧动画
https://aola.100bt.com/h5/peticon/breath/peticon4213.png
#Spine立绘
https://aola.100bt.com/h5/peticon/newbreath/petmovie5275/petmovie5275.png
https://aola.100bt.com/h5/peticon/newbreath/petmovie5275/petmovie5275.json
https://aola.100bt.com/h5/peticon/newbreath/petmovie5275/petmovie5275.atlas
#静态立绘
https://aola.100bt.com/h5/peticon/newlarge/type1/peticon5841/peticon5841_1.png
#海报
https://aola.100bt.com/h5/pet/petskin/background/bg/img_petskinbackground_926.png
```

没什么可说的，和奥奇传说基本一致。 

可供参考的数据：

2026-1-30 理论可得：

1. 若干序列帧动画 
2. spine 光启图鉴515 +  皮肤图鉴569  = 1084  
3. 海报243

实际得到：

1. 序列帧动画没弄

2. spine 1193

3. 海报337，不知道怎么设计的，有很多一模一样的图，分辨率和size都是一样的。

   ![image-20260131135446474](https://cdn.jsdelivr.net/gh/violet-wdream/Drawio/PNG/202601311354581.png)



### 处理

1. [通过version生成基础 output.txt](https://github.com/violet-wdream/.Scripts/blob/main/Games/AoLaXing/GetResList.py)
2. [更新基础 output.txt 爆破得到其他的文件。](https://github.com/violet-wdream/.Scripts/blob/main/Games/AoLaXing/GetUpdateRes.py)
3. [得到海报和人物立绘。](https://github.com/violet-wdream/.Scripts/blob/main/Games/AoLaXing/GetBgAndPainting.py) 

### 下载

可用Aria2c下载。

```c
aria2c -i output.txt -d output
```

或者

[UrlsDownLoader](https://github.com/violet-wdream/.Scripts/blob/main/DownLoad/UrlsDownLoader.py)

### 分类spine

[分类](https://github.com/violet-wdream/.Scripts/blob/main/SpineFileProcess/Sort/SortAtlas%26Skel%26png(Any).py)

![image-20260131135736317](https://cdn.jsdelivr.net/gh/violet-wdream/Drawio/PNG/202601311357351.png)

### 检测完整性（可选）

[.Scripts/SpineFileProcess/CheckSpineFiles.py at main · violet-wdream/.Scripts](https://github.com/violet-wdream/.Scripts/blob/main/SpineFileProcess/CheckSpineFiles.py)

无效立绘，可以直接删除。https://aola.100bt.com/h5/peticon/newlarge/type1/peticon5080/peticon5080_1.png

![image-20260131140319246](https://cdn.jsdelivr.net/gh/violet-wdream/Drawio/PNG/202601311403288.png)

人工检测发现这个xshuimi应该是部件名称标错了，不用改。

![image-20260131141143093](https://cdn.jsdelivr.net/gh/violet-wdream/Drawio/PNG/202601311411121.png)

手动合成一下链接

```python
https://aola.100bt.com/h5/peticon/newbreath/petmovie4406/petmovie44062.png
https://aola.100bt.com/h5/peticon/newbreath/petmovie4407/petmovie44072.png
https://aola.100bt.com/h5/peticon/newbreath/petmovie4427/petmovie44272.png
https://aola.100bt.com/h5/peticon/newbreath/petmovie4427/petmovie44273.png
https://aola.100bt.com/h5/peticon/newbreath/petmovie4427/petmovie44274.png
https://aola.100bt.com/h5/peticon/newbreath/petmovie4512/petmovie45122.png
https://aola.100bt.com/h5/peticon/newbreath/petmovie5273/petmovie5273.png #这个url存在，可能是意外下载失败了。
```



### 总结

1. 获取version文件
2. 通过version文件得到petmovie目录文件路径，拼接为URL得到基础 output.txt
3. 更新基础output.txt
4. 批量下载
5. 校验完整性

一共1193，仅供参考。

2026-1-31导出资源：[GamesArchive/AoLaXing/File at main · violet-wdream/GamesArchive](https://github.com/violet-wdream/GamesArchive/tree/main/AoLaXing/File)