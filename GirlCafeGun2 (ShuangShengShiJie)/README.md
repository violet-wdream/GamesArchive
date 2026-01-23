## 双生视界 (Girl Cafe Gun) 停服 Spine & Live2D 加密



7.0分，游戏里可玩性应该还可以，纯看立绘就没那么显眼了。

<img src="https://cdn.jsdelivr.net/gh/violet-wdream/Drawio/PNG/202601232115205.png" alt="image-20260123211549973" style="zoom:50%;" />

![image-20260123213427089](https://cdn.jsdelivr.net/gh/violet-wdream/Drawio/PNG/202601232134211.png)

<img src="https://cdn.jsdelivr.net/gh/violet-wdream/Drawio/PNG/202601232141320.png" alt="image-20260123214103145" style="zoom:50%;" />

<img src="https://cdn.jsdelivr.net/gh/violet-wdream/Drawio/PNG/202601232222074.png" alt="image-20260123222210788" style="zoom:50%;" />

![image-20260123222317421](https://cdn.jsdelivr.net/gh/violet-wdream/Drawio/PNG/202601232223632.png)



### 资源

Github上现成的模型。应该是日服的，只有一部分。我感觉他的模型有点乱，我的更全然后更整齐一点。

[Live2d-model/少女咖啡枪 girls cafe gun at master · Eikanya/Live2d-model](https://github.com/Eikanya/Live2d-model/tree/master/%E5%B0%91%E5%A5%B3%E5%92%96%E5%95%A1%E6%9E%AA%20girls%20cafe%20gun)

还得是贴吧，找遗产就看贴吧[谁能分享份关服时的数据包【双生视界吧】_百度贴吧](https://tieba.baidu.com/p/9599456574?pid=151869573488&cid=0#151869573488)

[数据包（包括安装包）](https://pan.baidu.com/share/init?surl=F6DHGN9vFi9NPV0TtQbCrQ&pwd=naxy)

[APK](https://pan.baidu.com/s/1QQsOYspVHuM56sAeHSz_XQ#list/path=%2F)

这里选择处理B服（官服）的资源。



游戏里的服饰对应的就是l2d，装扮是Spine。部分明显有和谐，自行找mod替换贴图。

`cardbig`是卡面，模型预览图

`l2d`是Live2D模型

`2dAnimation`是Spine模型，需要改asset后缀为json

[.Scripts/SpineFileProcess/SuffixConverter.py at main · violet-wdream/.Scripts](https://github.com/violet-wdream/.Scripts/blob/main/SpineFileProcess/SuffixConverter.py)

其他的没什么用。

### 解密

global-metadata应该是加壳了，没法用il2cppdumper直接处理。

刚好有前辈的作业可以抄，懒得自己逆向了[双生视界Live2D提取 | Perfare's Blog](https://www.perfare.net/archives/1564)

这里Perfare提到了关于moc文件的处理，但是也只是讲了个大概。但是好在分享了网盘资源，里面有特别修改过的版本。这里使用dnspy逆向分析一下。

![](https://cdn.jsdelivr.net/gh/violet-wdream/Drawio/PNG/202601232002213.png)

反正就是正常使用AS导出Live2d模型后，再批量处理一下moc3文件就行了。

这里导出模型会有warnings，不用管。总共有39个角色， 然后每个角色有快10套衣服，每个衣服都单独做了个模型，不知道为什么不做成exp或者motion。共计340个模型。

导出模型后会导出多余动作，就是同一个动作会有两个文件，这里不介意的可以不管。

下面的脚本都只要更改一下输入目录，然后按顺序执行即可。

处理moc

[.Scripts/GirlCafeGun2/DecryptMocFile.py at main · violet-wdream/.Scripts](https://github.com/violet-wdream/.Scripts/blob/main/GirlCafeGun2/DecryptMocFile.py)

可选，删除多余motion

[.Scripts/GirlCafeGun2/DelOtherMotionsFile.py at main · violet-wdream/.Scripts](https://github.com/violet-wdream/.Scripts/blob/main/GirlCafeGun2/DelOtherMotionsFile.py)

可选，更改model3配置 （如果没删除motion 就不用管这个）

[.Scripts/Live2DFileConvert/ProcessModel3ByMotion3.py at main · violet-wdream/.Scripts](https://github.com/violet-wdream/.Scripts/blob/main/Live2DFileConvert/ProcessModel3ByMotion3.py)