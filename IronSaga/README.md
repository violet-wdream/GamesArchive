https://live2dhub.com/t/topic/4137/41?u=twistzz

机动战队IronSaga

**总结**

<img src="https://cdn.jsdelivr.net/gh/violet-wdream/Drawio/PNG/202604030004072.png" alt="image-20260403000444925" style="zoom:33%;" />


1. 需要经过新手引导后到设置界面手动下载额外资源，并重新启动游戏等待系统处理文件

   <img src="https://cdn.jsdelivr.net/gh/violet-wdream/Drawio/PNG/202604011458968.png" alt="image-20260401145854628" style="zoom: 33%;" />

   <img src="https://cdn.jsdelivr.net/gh/violet-wdream/Drawio/PNG/202604011512570.png" alt="image-20260401151214241" style="zoom: 33%;" />

2. 骨骼和纹理集：

   1. APK中在`./assets/spine`找到一部分 `atlas+json` 
   2. 热更新部分在`/data/user/0/com.gameduchy.jdzd.jp/com.gameduchy.jdzd.jp/Local Store/hu/spine/`获取另一部分`atlas+json`

3. 图像：基础路径同上

   1. `./Local Store/hu/texture/`少部分

   2. `./Local Store/asset_apk/texture/bigMapComEtc/`大部分

      可以直接一起合并到`bigMapComEtc`里面，重名文件保留size较大者。

4. 获取还原文件`all_bin` 和 `desc.txt`：基础路径同上

   1. `all.bin` : `./Local Store/hu/bin`
   2. `desc.txt` + `desc_jp.txt` :  `/com.gameduchy.jdzd.jp/Local Store/hu/desc/` 手动将`desc_jp.txt`的内容粘贴到`desc.txt`尾部即可，因为最后只使用一个。

5. 还原图像/处理纹理集：以下脚本均只需要修改输入/输出目录，配置项即可。

   1. [IronSaga_process.py ](https://github.com/violet-wdream/.Scripts/blob/main/Games/IronSaga/IronSaga_process.py)处理完图像后会按照文件名分类，只保留最后一级文件名，spine相关图像在输出路径的`spine`路径下，相较于帖子前面给出的脚本，做出了一些额外的修正和处理（比如通过文件名分类，图像原始数据提取优化以及对etc1_rgb的支持）。
   2. [Atlas_process.py](https://github.com/violet-wdream/.Scripts/blob/main/SpineFileProcess/Atlas_process.py) ：使用之前，需要把输出路径下还原后的`spine`路径图像与先前获取的骨骼/纹理集放在同一个路径下，这个路径作为输入路径。临时搓的Atlas反序列化工具，在这里的作用是修正atlas中错误的图像名以及size，目前大部分处理正常，少部分难以还原。
   3. 可选 [SortAtlas&Skel&png(Any).py](https://github.com/violet-wdream/.Scripts/blob/main/SpineFileProcess/Sort/SortAtlas%26Skel%26png(Any).py) spine分类工具，仅仅只是把spine模型的文件单独放到一个目录下。

6. 存在的一些难以避免的问题：

   1. 经考证，有一些enc/cet并不是 `etc1_rgba` 或者 `etc1_rgb`，明显与其他图像不同，可能是某种其他形式的图像，或者格式处理过了，比如`jp.dynamic.frame.store.highk.enc` 以及 `.map.map01.cet` 类似图像

   2. 资源命名无规律，难以还原，需要手动处理，但是占少数。

      ```c
      [ERROR] bdCreate1: atlas 引用但缺失图片：
          - bdCreate1.png
      //ex.image.spine.new.bd.creat1.cet
      ```

      json名称写错`JHSD2025.json` -> `JHSD2005.json`，应该是2025，但实际上纹理集和贴图都是2005

      还有包括但不限于一个模型当两个用（使用同一个贴图，只是插槽开关不同）

      `daishen` 和 `daishen2` 用同一个贴图

      <img src="https://cdn.jsdelivr.net/gh/violet-wdream/Drawio/PNG/202604030958860.png" alt="image-20260403095829792" style="zoom:33%;" />

      <img src="https://cdn.jsdelivr.net/gh/violet-wdream/Drawio/PNG/202604030958280.png" alt="image-20260403095853215" style="zoom:33%;" />

      还有和谐之类的，比如`Dai_hexie` 和 `Dai_normal` 用同一个贴图

      <img src="https://cdn.jsdelivr.net/gh/violet-wdream/Drawio/PNG/202604030957781.png" alt="image-20260403095714675" style="zoom:33%;" />

      <img src="https://cdn.jsdelivr.net/gh/violet-wdream/Drawio/PNG/202604030957721.png" alt="image-20260403095700522" style="zoom:33%;" />

      以及一些未被使用的贴图或者某个旧模型新的版本贴图（new），不再列举。