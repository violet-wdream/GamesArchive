## 三国志幻想大陆  Spine  Cocos2D 无加密 资源混杂-分类 

其实另一个帖子[关于三国志幻想大陆 - 讨论 - Live2DHub](https://live2dhub.com/t/topic/2676/61)已经讨论过了，但是有点混乱，我这里总结一下。

9.0分 夯，能活5年还是有点东西的

<img src="https://cdn.jsdelivr.net/gh/violet-wdream/Drawio/PNG/202601231428593.png" alt="image-20260123142809361" style="zoom:50%;" />

<img src="https://cdn.jsdelivr.net/gh/violet-wdream/Drawio/PNG/202601231430486.png" alt="image-20260123143040257" style="zoom:50%;" />

<img src="https://cdn.jsdelivr.net/gh/violet-wdream/Drawio/PNG/202601231431622.png" alt="image-20260123143145428" style="zoom:50%;" />

<img src="https://cdn.jsdelivr.net/gh/violet-wdream/Drawio/PNG/202601231432750.png" alt="image-20260123143252562" style="zoom:50%;" />

<img src="https://cdn.jsdelivr.net/gh/violet-wdream/Drawio/PNG/202601231435277.png" alt="image-20260123143558002" style="zoom:50%;" />

<img src="https://cdn.jsdelivr.net/gh/violet-wdream/Drawio/PNG/202601231437694.png" alt="image-20260123143710553" style="zoom:50%;" />

<img src="https://cdn.jsdelivr.net/gh/violet-wdream/Drawio/PNG/202601231438753.png" alt="image-20260123143829588" style="zoom:50%;" />

国服5周年进度，其他服务器只有3年，进度落后很多，自行斟酌。台服可以参考隔壁那个帖子。

以下是国服也就是官服，模拟器下载的。

### 资源路径

#### APK资源

`assets > hash`

![image-20260122174821384](https://cdn.jsdelivr.net/gh/violet-wdream/Drawio/PNG/202601221748456.png)

#### 热更资源

这个部分的资源并不是更新进度条显示的，你第一次登录游戏会让你更新100M资源，但实际上游戏新增了4G的内容，更新的时候你打开这个目录观察，可以发现最下面有个tmp文件，最后会自动拆开分散文件。

等到这个tmp文件消失了就说明下载完毕了。

<img src="https://cdn.jsdelivr.net/gh/violet-wdream/Drawio/PNG/202601230011210.png" alt="image-20260123001117110" style="zoom:50%;" />

miniRes目录下面有个cri_res，应该是一些CG之类的，这里用不到，可以删除。

这里还有upd目录（update），应该是刚才热更新的资源。

这里还需要登录游戏手动下载额外资源。会下载到MiniRes

<img src="https://cdn.jsdelivr.net/gh/violet-wdream/Drawio/PNG/202601231344716.png" alt="image-20260123134423394" style="zoom: 80%;" />

### 分类

可以看到这里所有文件都是hash命名的。应该会有对应的映射表来还原实际路径，但是有可能这个映射表也是这些混乱的文件的一部分，根据我的经验来看，映射表一般都很大，会有2~8M左右

使用脚本分别找到hash目录和miniRes目录下的前5个较大文件，依次人工检查一下即可，如果还没找到可以扩大查找范围。



![image-20260122205158900](https://cdn.jsdelivr.net/gh/violet-wdream/Drawio/PNG/202601222051025.png)

hash目录下的最大文件是`5e2a38241be9dfd4fe063d7f668334ec`是一个二进制文件，发现里面有一些类似编码用的整齐大小写字母数字串，可能是用来编码相关的。经过考证是个TTF字体文件。

<img src="https://cdn.jsdelivr.net/gh/violet-wdream/Drawio/PNG/202601222143581.png" alt="image-20260122214301447" style="zoom:50%;" />

看下一个，发现这个`27c02f0ca9782d7edadcb0e42d810ee8`在hash和miniRes中都有这个文件。

而miniRes目录下的最大文件是`27c02f0ca9782d7edadcb0e42d810ee8`，到这里不难猜出这个27开头的文件就是映射表了。值得注意的是这里的两份`27`大小并不相同。

hash目录下的这个表全是空的，而miniRes很多为空，部分有对应MD5。所以hash目录的那个表就不用管了。

![image-20260122210004896](https://cdn.jsdelivr.net/gh/violet-wdream/Drawio/PNG/202601222100087.png)

理论上来说应该全部Path都有对应的值，只要有文件就会有MD5。所以这个表也是残缺的。

注意到论坛里，有人提到了upd这个目录下面还有一个表`27c02f0ca9782d7edadcb0e42d810ee8`，名字跟之前的也是完全一样。

![image-20260123001309409](https://cdn.jsdelivr.net/gh/violet-wdream/Drawio/PNG/202601230013535.png)

发现这个表的所有项都有MD5值，所以是全表。另存为`config.json`

![image-20260123001425097](https://cdn.jsdelivr.net/gh/violet-wdream/Drawio/PNG/202601230014248.png)

现在需要解析一下这张表怎么用了。

注意到所有的文件名都是32位的hash，猜测是MD5。

这里有两个着手方向，一个是文件本身的MD5，一个是对应的路径的MD5；

这里随便找一个在线工具，挑一个文件算一下MD5：[文件md5在线计算-ME2在线工具](https://www.metools.info/other/o21.html)

经过这两种计算的排列组合得出结论：

对于json中的记载的每一个对(KEY , VALUE)

KEY 路径名的MD5 = 文件名， VALUE MD5是真实文件的MD5 （只是用来检验文件完整性，对分类没什么用）

也就是说，可以对config的Path求MD5，得到一个新的表MD5 -> Path

然后遍历需要分类文件，通过文件名（MD5）查表得到Path，然后移动/重命名完成分类操作。

随机取样发现，有几个文件存在路径，但是找不到源文件。表里有15w行，实测文件数量：APK 3w + 热更 10w = 13w 剩下的我也不知道啊🤓🤓🤓 不会要进游戏手动下载吧 😩😩😩

OK啊，又是经典的品鉴新手引导环节😩

![image-20260123134423394](https://cdn.jsdelivr.net/gh/violet-wdream/Drawio/PNG/202601231344716.png)

更新的资源都在MiniRes这里。

工作目录

```python
.
├─ ERRORRes
├─ Res
│  └─ hash
│  └─ MiniRes
│  └─ upd
├─ SortedRes
└─ config.json
```

可以参考下面这个脚本。

[.Scripts/SanHuan/SortFilesByMD5.py at main · violet-wdream/.Scripts](https://github.com/violet-wdream/.Scripts/blob/main/SanHuan/SortFilesByMD5.py)

设置了DRY_RUN模式，True是测试，正常后可以调为False正式移动文件。

看了下没有被分类的文件，几张宣传图和之前讨论用到的表，以及acb usm文件。

最后得出的立绘路径`.\SortedRes\res\spine\illustration`

部分角色预览图`.\SortedRes\res\new_ui\icon\knight\big`

这个有点像海报用的，角色全身预览图`.\SanHuan\SortedRes\res\new_ui\handbook\card`会显示角色的名字。

洗澡图`.\SortedRes\res\new_ui\homeland\bath`是静态图片。

350234

<img src="https://cdn.jsdelivr.net/gh/violet-wdream/Drawio/PNG/202601231540837.png" alt="10700" style="zoom:50%;" />

<img src="https://cdn.jsdelivr.net/gh/violet-wdream/Drawio/PNG/202601231541041.png" alt="41000" style="zoom:50%;" />

还有个`.\SortedRes\res\spine\goodwill` 不知道什么东西，反正是人形角色

器灵`.\SortedRes\res\spine\hero_weapon`

2026-1-23  775个，包括一些杂项，

### 抓包

暂时没兴趣弄，以后再说吧，这里挖个坑。

```c
https://p10488-ob-version.ejoy.com:8443/get_version/?chan=android_511314&patch_version=6.3.50&platform=android&randnum=2821397&utdid=first_login
```