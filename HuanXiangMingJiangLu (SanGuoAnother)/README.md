## 三国another (幻想名将录/三国志アナザー～星将の願い～) 停服了  Spine Cocos2D Astc-Beeplay

赏析环节。给个7.5分，还是挺好看的。第一次处理cocos引擎游戏，真是一路折腾🤯🤯🤯，感觉收获颇丰。



<img src="https://cdn.jsdelivr.net/gh/violet-wdream/Drawio/PNG/202601182156623.png" alt="image-20260118215653483" style="zoom: 67%;" />

<img src="https://cdn.jsdelivr.net/gh/violet-wdream/Drawio/PNG/202601182147237.png" alt="image-20260118214756071" style="zoom:50%;" />

<img src="https://cdn.jsdelivr.net/gh/violet-wdream/Drawio/PNG/202601182136231.png" alt="image-20260118213610034" style="zoom:50%;" />

<img src="https://cdn.jsdelivr.net/gh/violet-wdream/Drawio/PNG/202601181232250.png" alt="image-20260118123247069" style="zoom:50%;" />

<img src="https://cdn.jsdelivr.net/gh/violet-wdream/Drawio/PNG/202601181228721.png" alt="image-20260118122757462" style="zoom:50%;" />

<img src="https://cdn.jsdelivr.net/gh/violet-wdream/Drawio/PNG/202601181229708.png" alt="image-20260118122911544" style="zoom:50%;" />

<img src="https://cdn.jsdelivr.net/gh/violet-wdream/Drawio/PNG/202601181230021.png" alt="image-20260118123043984" style="zoom:50%;" />

<img src="https://cdn.jsdelivr.net/gh/violet-wdream/Drawio/PNG/202601181231199.png" alt="image-20260118123152014" style="zoom:50%;" />

### 游戏资源

依旧来自Qoo[[Download\] 三国志アナザー 星将の願い - QooApp Game Store](https://apps.qqaoop.com/en/app/23343)

[【公式】三国志アナザー～星将の願い～（三国another） (@another3594) / X](https://x.com/another3594)

官网半天找不到，大概也是半截入土了。

2026-1-22

我们做了一个艰难的决定，mamba out🤣🤣🤣

![image-20260124133644069](https://cdn.jsdelivr.net/gh/violet-wdream/Drawio/PNG/202601241336196.png)

### 资产路径

#### APK

都在APK里面，这里忘记看了，所以安装了。应该也是APK里面有分块APK，找到下面这个。

![image-20260117113506697](https://cdn.jsdelivr.net/gh/violet-wdream/Drawio/PNG/202601171135783.png)

在以下这个路径里面

![image-20260117170405787](https://cdn.jsdelivr.net/gh/violet-wdream/Drawio/PNG/202601171704896.png)

这里的native就是所有的资源文件，import和 index.jsc 没什么用。

config.json很重要，是用来还原文件名的。这里先解压出来。

#### 热更资源

还剩下很多热更资源是要你登录游戏后才会下载，而且是后台下载，不会提示你。你进游戏后随便点几下出去，你会发现游戏体积大了1.5G左右。

在下面这个位置。

<img src="https://cdn.jsdelivr.net/gh/violet-wdream/Drawio/PNG/202601181458666.png" alt="image-20260118145852589" style="zoom:50%;" />

### key （可以跳过）

这里得到的key并不重要，因为后续也用不到，只是用来反编译jsc使用。如果只需要获取spine，这里可以跳过

参考

[关于Cocos2dx-js游戏的jsc文件解密(二) - 吾爱破解 - 52pojie.cn](https://www.52pojie.cn/thread-1449982-1-1.html)

[群英风华录解密有教的吗 - 讨论 - Live2DHub](https://live2dhub.com/t/topic/4788/9)

[关于Cocos2dx打包游戏的jsc文件解密(一) - 知乎](https://zhuanlan.zhihu.com/p/408170731)

cocos引擎，xxtea加密

`libcocos2djs.so`搜索`Cocos Game` 后面跟着的第一个串就是key`wf-game-card`

```nginx
77 66 2D 67 61 6D 65 2D 63 61 72 64 00 00 00 00 #自动填充到16 bytes
wf-game-card
```

![image-20260117172729244](https://cdn.jsdelivr.net/gh/violet-wdream/Drawio/PNG/202601171727352.png)

或者IDA里面搜索xxtea相关的函数也能找到。

验证了论坛里的一个说法`applicationDidFinishLaunching`，大多都是在这里可以找到xxtea的key

```nginx
AppDelegate::applicationDidFinishLaunching(void)
```

![image-20260117173020279](https://cdn.jsdelivr.net/gh/violet-wdream/Drawio/PNG/202601171730416.png)



### 还原astc为png

从散落的文件里看到了atlas文件，所以是spine无疑了。然后bin文件就是二进制skel骨骼，直接改为skel后缀就能用了。

绝大部分文件都是astc，压缩且加密，没法直接转换，解压astc发现文件头变成了`beeplay`，说明第一步是解压，然后再去掉beeplay进行XOR解密

逆向`libcocos2djs.so`搜索astc以及image相关函数

`cocos2d::Image::initWithImageData`

发现检测文件头7bytes，就是检测beeplay签名

```c
if ( !memcmp(cocos2d::Image::initWithImageData(unsigned char const*,long)::ENCRYPT_SIGNATURE, p[0], 7u) )
{
    v8 = v4 - 7;
    v9 = (unsigned __int8 *)malloc(v4 - 7);
    memcpy(v9, v7 + 7, v4 - 7);
    if ( v4 - 7 >= 1 )
    {
        if ( v8 < 0x20 )
        {
            v10 = 0;
            do
                LABEL_15:
            v9[v10++] ^= 0x17u;
            while ( v8 != v10 );
            goto LABEL_16;
        }
        v11 = 0;
        v10 = v8 & 0xFFFFFFFFFFFFFFE0LL;
        v12.n128_u64[0] = 0x1717171717171717LL;
        v12.n128_u64[1] = 0x1717171717171717LL;
        do
        {
            v13 = (int8x16_t *)&v9[v11];
            v14 = *(int8x16_t *)&v9[v11];
            v15 = *(int8x16_t *)&v9[v11 + 16];
            v11 += 32;
            *v13 = veorq_s8(v14, v12);
            v13[1] = veorq_s8(v15, v12);
        }
        while ( v10 != v11 );
        if ( v8 != v10 )
            goto LABEL_15;
    }
}
```


这里大概就是说把beeplay文件头去掉后，直接对所有的字节XOR `0x17`

```c
伪astc文件  
  └─ GZIP解压
       └─ beeplay ASTC
             └─ 去 7 字节头
                  └─ XOR 解密
                      └─ 标准 ASTC
                          └─ astcenc 解码 = 正常astc文件
```

astc处理为正常格式后，还需要分类，

![image-20260117134749840](https://cdn.jsdelivr.net/gh/violet-wdream/Drawio/PNG/202601171347917.png)

上面提到了很多种图片格式，这里统一处理为png格式的（spine的贴图是png的），这里只会处理astc

文件，skel 和 atlas不会处理，可以放心使用。后文会给出脚本。

### 资产分类/名称还原

所有文件的名称都是没什么规律的，没法进行分类，所以也没法通过名称找到同一个spine的三个文件。尽管可以尝试用atlas里面记载的图片名称和尺寸进行匹配，但是skel的匹配却别无他法。所以最优解法还是找到映射表。

根据上文找到的key，可以通过jsc反编译器[jsc解加密工具-Orange.zip - 蓝奏云](https://beisheng.lanzoui.com/iSxKhplgnch)来查看源码`index.jsc`。

<img src="https://cdn.jsdelivr.net/gh/violet-wdream/Drawio/PNG/202601171748158.png" alt="image-20260117174828082" style="zoom:50%;" />

这里得到`index.jsc`，目前看来没什么用。推测是引擎自己使用的逻辑，有兴趣可以自行了解一下。

理论上来说通过config.json应该就可以搞定了，但是这里uuid里记载的都是22位的，实际文件基本都是36位，有少数是9位的。在config里面都是22位的根本找不到，应该还做了其他处理。

通过抓包发现，下载的本地manifest里面记载的也是22位数据，同时远程的manifest也是22位数据，所以说要处理的话只能在本地处理了。要去找到游戏内部的处理逻辑。

```nginx
# 热更新
http://hotter-hxmjljp.wengames.com/japan/cardjapan-update/1.0.17.73/ios_bundle/assets/resources/import/51/5127c5e3-3e3e-4150-b8bf-904386ffe7fc.json?md5=b120aa6f54754ce2834e617c2ddc4e7f
# 本地manifest
http://hotter-hxmjljp.wengames.com/japan/cardjapan-update/1.0.17.73/ios_bundle/project.manifest
```

经过一天的瞎折腾，终于找到了答案：

[不懂就问：UUID的压缩算法是怎么样的呢？ - Creator 3.x - Cocos中文社区](https://forum.cocos.org/t/topic/170531)

[有什么方法能从cocos creator构建的游戏里还原出live2d或spine动画资源? - 讨论 - Live2DHub](https://live2dhub.com/t/topic/2671/8)

config.json里记载的uuid是22位的被压缩的uuid，而文件名称都是36位的uuid （不是hash）

处理顺序：读取config.json建表（path的key对应uuid的index）dictionary<uuid22, path>，遍历所有文件（uuid36 -> uuid22 -> path）找到对应path，重命名并移动。后文会给出处理脚本。



---

处理有点慢，要个5s吧。但是处理完发现还有大约900张图片是没有分类的，因为config里面根本没有对应的路径，只能找到uuid。

小人`spines/panel/`  立绘`spines/beauty ` 预览图`icon/heroHead`

到这里基本上就搞定了，接下来就是把spine相关文件的名称改为模型名称。

从马后炮的角度来看，其实path里面记载的路径比如`"3": ["icon/heroHead/11068", 1],`

最后一部分`11068`其实是文件名，`icon/heroHead`才是路径。这样可以理解为什么会有这样的`"spines/panel/SG_SHU_OR_pangtong/SG_SHU_OR_pangtong"`后面两段重复的路径了。



发现还缺了挺多东西的，不知道是不是鉴权，下面这几个角色有一大半没有。

![image-20260118124502024](https://cdn.jsdelivr.net/gh/violet-wdream/Drawio/PNG/202601181245110.png)

从头像数量来看，应该有211个的spine，config的路径里面`spines/beauty/spine/`开头的有168个，分类后实际42个。

我找了一个没有的角色头像，同时也是未被分类的，计算出来的path应该是8790，而这里刚好没有。这就产生了一个疑点。

![image-20260118130301744](https://cdn.jsdelivr.net/gh/violet-wdream/Drawio/PNG/202601181303813.png)

搜索武则天，在官网也能找到对应的立绘，但是本地只能找到小人，config里面也只有小人的相关文件，并没有立绘的。

<img src="https://pbs.twimg.com/media/GLA4Q3UaUAAtajc?format=jpg&name=large" alt="图像"  />

实测游戏内可以看到立绘，而且是动态的。

<img src="https://cdn.jsdelivr.net/gh/violet-wdream/Drawio/PNG/202601181432989.png" alt="image-20260118143258827" style="zoom:50%;" />

在游戏里面切换立绘十分流畅，几乎可以肯定都是本地资源。

又经过了一番探索，发现了问题的端倪，APK里的只有500MB，而游戏体积占大头的一定是资产，所以这1.69G里面肯定有资产。

<img src="https://cdn.jsdelivr.net/gh/violet-wdream/Drawio/PNG/202601181457189.png" alt="image-20260118145735093" style="zoom:50%;" />

在`/data/data/包名/`这个路径下，remote目录

<img src="https://cdn.jsdelivr.net/gh/violet-wdream/Drawio/PNG/202601181458666.png" alt="image-20260118145852589" style="zoom:50%;" />

这里还有个很重要的表`cacheList.json`，一并保存下来。

打开remote发现有两个特别大的json文件，果不其然是config。为什么会有两个？

猜测因为把native和import放一起了，所以既有atsc bin atlas 还有json文件。

但是这俩玩意里面很多重复的条目，这个也无从分辨，不知是怎么考虑的，当前的策略是第一个config为主体，如何没有查到，就查另一个config。不过从结果来看，用那个size更大的config好像就行了，因为这个查不到的话，另一个也还是查不到。

注意到文件的命名都不是36位的uuid，而是类似`17686595069780`的一串数字。这里`cacheList.json`就发挥作用了，这里又是一层映射关系，url -> uuid36

![image-20260118153929977](https://cdn.jsdelivr.net/gh/violet-wdream/Drawio/PNG/202601181539097.png)

然后接下来的处理方式跟之前一致。



### 总结

1. APK资源：Qoo下载APK，解压找到`split_install_time_assets.apk`，再解压找到 `assets > assets > resources `这个目录保存`native`文件夹和`config.json` （configAPK），native应该有400+MB。
2. Cache资源：在上文提到路径，保存remote目录和`cacheList.json`。将文件按照大小排序，保存最大的那个json`17686595437760.json` （configCommon 4528 KB），然后按照文件类型排序，取出其中的atsc atlas bin文件，json以及其他类型的不要，保存到`cache`目录。
3. native和cache目录统一放在`Res`目录下

形成以下工作目录结构

```python
.
├─ ERRORRes
├─ Res
│  └─ native
│  └─ cache
├─ SortedRes
├─ cacheList.json
├─ configAPK.json
└─ configCommon.json
```

先分类，再处理。分别使用以下两个脚本来分类和还原。具体使用方法看注释或者问AI

1. 分类：把脚本里面的输入目录改为工作目录，同时修改这三个json的路径

   [.Scripts/SanGuoAnother/SortFiles.py at main · violet-wdream/.Scripts](https://github.com/violet-wdream/.Scripts/blob/main/SanGuoAnother/SortFiles.py)

2. 还原：分类完毕后，把还原脚本里的输入路径改为你想要还原的部分（比如`.\SortedRes\spines\beauty\spine`），不建议全部还原，因为文件太多了，astcenc处理速度有点慢。

   [.Scripts/SanGuoAnother/Astc2Png (SanGuoAnother).py at main · violet-wdream/.Scripts](https://github.com/violet-wdream/.Scripts/blob/main/SanGuoAnother/Astc2Png (SanGuoAnother).py)

部分文件无法分类放在了ERRORRes，需要手动处理。我看了下都是些乱七八糟的玩意，无伤大雅。

总共172个，有个模型缺了atals，应该是这个`nvzhulihui_pifu2`，其他的该有的都有了。

<img src="https://cdn.jsdelivr.net/gh/violet-wdream/Drawio/PNG/202601182204500.png" alt="nv_2" style="zoom:50%;" />