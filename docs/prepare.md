# 准备

!!! danger
    刷机有风险，出事我不管。

先下载一个 [Mt管理器](https://www.coolapk.com/apk/bin.mt.plus)，用来执行文件操作。

然后在 [Magisk仓库](https://github.com/topjohnwu/Magisk/releases/latest/) 安装最新版本的 Magisk，如果不能下载请复制Github的 apk 链接请填入[镜像站](https://ghproxy.com/) 下载


## 我的手机能 Root 吗？

第一看手机能不能解锁Bl,第二看你是否有全量包或者对应的第三方Recovery.

如果不是很小众，端口什么的也没有封锁(需要手动验证),大部分手机都可以 Root.

## 经过后面的尝试，我发现我不能Root,但是想用模块.....

你可以去试一下 LSPatch？

>LSPatch: A non-root Xposed framework extending from LSPosed

支持安卓9以上系统。

这里放上相关链接。

- [简单的 LSPatch 使用教程](https://duzhaokun123.github.io/2022/05/06/simple-lspatch-guide.html#q0-lspatch-%E6%98%AF%E4%BB%80%E4%B9%88)

- [项目与下载地址](https://github.com/LSPosed/LSPatch)

- [一个教程](https://www.jipa.work/lspatch)

嗯。

## 了解术语[^17]

**内存/运存/闪存/Ram/Rom**

首先**没有运存这种东西**，只有内存。

除了手机外，你把闪存/硬盘/存储空间当内存说都是会被骂的。


- 内存是什么？

内存是能够直连CPU的高速存储器。内存包括RAM和ROM两种。

- 外存是什么？

外存是不能直连CPU，需要通过内存中转的低速存储器。电脑的硬盘（HDD），固态硬盘（SSD），手机的存储空间（实际是和固态硬盘使用同样基本技术的闪存），都是外存。

- RAM是什么？

RAM是随机访问存储器（Random Access Memory），暂存正在运行的系统和程序的数据，断电后数据消失。这是对应于某些人所说的“运行内存”的组件。

- ROM是什么？

ROM是只读存储器（Read Only Memory），但是很多可以读可以写的介质也被称为了ROM。

手机的闪存也是可以被称为ROM的，当然称为闪存是更准确的说法，但说闪存不是ROM是错的

- 闪存是什么？

[^39]
闪存是最常见的存储芯片。固态硬盘、U盘，手机内的存储空间均使用闪存芯片，但芯片的组织方式和主控有所区别。

**手机的存储空间到底应该叫什么？**

叫外存，叫闪存，叫存储空间都行。叫ROM属于错误，叫内存属于有毒。




**双清:** 

Dalvik/ART Cache ,Cache 

清除分区以及数据，简称重置手机。

**三清:** 

Dalvik/ART Cache ,Cache ,Data 

刷机前基本上必选三清，使新系统的兼容性达到最佳。

**四清** 

Dalvik/ART Cache ,Cache ,Data ,System 

四清针对版本差异过大的系统,四清后不刷入系统无法进系统(因为系统分区寄掉了)
 
!!! warning  end

     重要！
     四清后不刷入系统无法开机进系统！！只能电脑刷或者储存卡刷。


**五清** 

Dalvik/ART Cache ,Cache ,Data ,System ,Internal Storage（内置储存） 
!!! warning  end

    如果选择这个方式，手机内置存储上的东西会被删除，也就不能从手机选择卡刷包了。

**六清** 

Dalvik/ART Cache ,Cache ,Data ,System ,Internal Storage（内置储存）,USB OTG.

**底包**

既不是ROM也不是OTA软件包，它是一组**低级驱动程序**，可帮助操作系统完成其想做的任何事情。 它包括调制解调器，蓝牙，引导程序，DSP等各种内容。支持所有Snapdragon和MTK设备，包括仅限中国的设备。

底包就相当于一个纯净版或者内核版的系统包，包含基带，字库。

**MTK**

联发科。

MTK和高通是生产手机CPU的厂家。MTK平台和高通平台指的是这两家的操作系统。

!!! info
    [^28]
    联发科不建议玩机，尤其是动分区，什么 logo, dtbo, boot, vendor_boot 等等，特别容易寄，当然它大部分时候也不是真砖，就是只能反复一屏重启，也不让你进 Bootloader。不过不让你进 Bootloader 基本也就等于砖了，因为你只能去售后了。（补充0，如果只是解锁之后搞下 Magisk、LSPosed 这种，那还是没什么问题的）
    
    要是以前也还好，漏洞允许我们深刷绕过小米售后账号权限。现在新机深刷不能自己来不说，联发科他自己的深刷工具对动态分区的支持还不好，如果刷错分区死在 B 槽位，很大概率深刷也刷不好，等着换主板吧。

    虽然我们制作了这些处理器的 TWRP，但并不建议任何没有丰富经验的人刷入使用。由于 GKI2.0 内核的特性，dtb 和 内核模块 都位于 vendor_boot 内，而 Recovery 也被移动到这，一旦官方有更新这部分，就会导致无法引导进系统，这种情况还好，Bootloader 是可以进的，恢复官方的 vendor_boot 即可。而一旦遇到反复重启到第一屏的情况，就只能去售后深刷。根据最新的测试结果来看，现在单纯刷入我们制作的 TWRP 理论上不会再遇到这种问题了，但用它来刷包等操作，还是不能保证。
    （补充1，由于联发科设备 fastboot boot 命令是暴毙的，所以临时启动 twrp 是做不到的，只能通过直接 fastboot flash 刷入分区来使用，因此大大增加了暴毙的几率。）
    （补充2，也由于 GKI2.0 的原因，整个 boot 分区只有 kernel，所以理论上为其编译内核只需要使用 android common kernel 合入 mtk 专有 gki 部分，就可以正常开机了）

**手机分区**

内容来自[^30]

- boot

引导分区：顾名思义，一个引导进入系统的分区，包含Android的kernel（内核）和ramdisk（内存盘）。我们日常启动Android系统，就是通过启动boot分区的kernel并加载ramdisk，完成内核启动，进入系统。

一旦引导分区遭到不当改动，手机通常无法进入系统，主要表现为，无限重启，卡fastboot，卡第一屏等。

- system

组合体,system分区遭到损坏，手机就无法正常开机。

- data&userdata

用户数据分区：用户所有的数据都包含在这个分区当中，也包括内部存储中的数据。

- persist

保存着用于FRP（factory reset protect）机制的一些信息，例如账号，密码等重要信息。

而且还包含DRM（数字版权管理）相关文件，传感器注册表，对我们的wifi，蓝牙，mac地址来说必不可少。

!!! info
    恢复出厂设置并不能清空persist分区，另外线刷包不包含persist分区，一旦出问题我们需要动手修复。

- modem&radio

基带分区,控制手机通讯功能的分区，此分区一旦损坏，通讯相关功能大概率会寄寄，具体表现为不读卡，丢失imei等。

!!! help
    Qualcomm（高通）基带分区：fsg、fsc、modemst1、modemst2 可选分区dsp、bluetooth、modem、persist、sec

    Mediatek（MTK）基带分区：nvcfg、nvdata、persist、protect1、protect2、seccfg、nvram分区

    路径：/dev/block/bootdevice/by-name/
    /dev/block/platform/bootdevice/by-name/

- vbmeta

AVB/DM启动验证分区，主要是为了防止启动镜像（boot.img）被篡改。vbmeta启动效验通常导致MTK机型刷入magisk或者三方Recovery后陷入无限重启的情况。可以去掉验证，面具中也有选项。

```sh
fastboot --disable-verity --disable-verification flash vbmeta vbmeta.img 
```

- recovery

备用引导分区，在boot分区（主引导分区）损坏后，仍可以进入rec分区进行系统的备份和恢复，发挥着相当于电脑pe的作用。

- misc

一个非常小的分区，4 MB左右。
这个分区供 recovery 来保存一些关于升级的信息，应对升级过程中的设备掉电重启的状况。

- cache

安卓系统缓存分区，清除此分区不会影响个人数据，缓存将会在日用中重新生成。

- dtbo

控制屏幕刷新率和频率的分区，变更前记得先备份，否则得不偿失。

- splash&logo

存储着安卓开机第一屏图片，fastboot模式下图片，及系统损坏图片等。


**A/B分区**

Android从7.0开始引入新的OTA升级方式，A/B System Updates，这里将其叫做A/B系统。顾名思义，A/B系统就是设备上有A和B两套可以工作的系统（用户数据只有一份，为两套系统共用），简单来讲，可以理解为一套系统分区，另外一套为备份分区。其系统版本可能一样；也可能不一样，其中一个是新版本，另外一个旧版本，通过升级，将旧版本也更新为新版本。当然，设备出厂时这两套系统肯定是一样的。之所以叫套，而不是个，是因为Android系统不是由一个分区组成，其系统包括boot分区的kernel和ramdisk，system和vendor分区的应用程序和库文件，以及userdata分区的数据

A/B系统实现了无缝升级(seamless updates)，有以下特点：出厂时设备上有两套可以正常工作的系统，升级时确保设备上始终有一个可以工作的系统，减少设备变砖的可能性，方便维修和售后。

!!! info  end

    OTA升级在Android系统的后台进行，所以更新过程中，用户可以正常使用设备，数据更新完成后，仅需要用户重启一次设备进入新系统。如果OTA升级失败，设备可以回退到升级前的旧系统，并且可以尝试再次更新升级。

**VAB架构**

又称虚拟AB分区，出厂安卓11的新机型，**几乎都是VAB架构**。

安卓分区架构发展史为：onlyA，AB，onlyA动态分区，AB动态分区，VAB架构。 所谓的VAB架构，其实就是AB分区，套上了动态分区，再解决了AB分区的空间占用问题。

刷机时经常会刷写的分区(system,vendor,boot,recovery等等)。userdata分区就是用户分区，格式化data就是格式化的这个分区。需要注意的是，格式化data和清空data，是两个不同的概念，经常会有小白把这两个概念搞混淆。

格式化data就是把userdata的分区进行格式化操作，就像你格式化U盘一样，是格式化操作。

而清空data，是删除data分区的所有文件及文件夹。当你遇到data挂载不上时，你清空data是没有效果的，这个时候，你需要进行格式化data操作，才能挂载data，所以，这两个不要搞混淆了

官方文档见 [这里](https://source.android.com/devices/tech/ota/virtual_ab)

附 [对Virtual A/B 分区工作方式的进一步探索](https://blog.xzr.moe/archives/30/)

**通用系统映像(GSI/SGSI)**

[https://source.android.google.cn/devices/tech/ota/dynamic_partitions/implement](https://source.android.google.cn/devices/tech/ota/dynamic_partitions/implement)

Android 8 引入 Project Treble 后，手机的系统文件和底层的厂商硬件驱动开始分离存放，更新系统时只需要更新系统文件即可。此项举措意在方便厂商加快 Android 大版本更新的步伐，自然也同样方便了第三方 ROM 的开发和更新

而 Android 9 开始，Google更改了要求，所有设备都必须使用[system-as-root]

GSI由此以A-only和A/B进行区分。

GSI则是一种可以忽略厂商定制的通用刷机包，sgsi只支持高通设备且不自带内核。，而gsi理论上支持任何设备。

[^45]

它的优点是在机器还没有适配第三方 ROM 的时候，可以提前体验到类原生系统。但是因为没有针对具体机型进行优化，所以会存在部分问题。

贴一个 通用系统镜像的列表:[Generic System Image (GSI) List](https://github.com/phhusson/treble_experimentations/wiki/Generic-System-Image-%28GSI%29-list)


**Project Treble(PT)**

方便手机厂商给手机升级系统。

为了解决Android碎片化问题，减少技术支持层面的拖累，谷歌将原本由芯片厂商负责的代码修改工作纳入到Android项目中，绕过芯片厂而直接将打包好处理器适配性的系统发送给手机厂商，从而大大节省时间和研发难度，让手机厂商升级系统的门槛变得更低。

pt的分区又有着不同区别，分别是a-only和a/b。部分机子升级至安卓8.0级以上安卓版本时，部分良心手机厂商的机子会支持pt计划的新分区，叫vendor分区

只要你的手机支持pt，也就能以支持pt的rom为底包，双清刷入gsi，尝鲜类原生，但因为是通用系统镜像，部分机型刷入后可能会有部分传感器丢失的现象，比如指纹，相机不可用，得自己搞定驱动。


**动态分区**

Google在Android 10开始引入了动态分区（Dynamic Partitions） 简单来说，就是把原来的system , vendor , product还有odm分区整合到了一起，构成super分区 在刷入设备的时候动态调整system等分区的大小

**什么是GSI**

GSI代表通用系统映像。这是一个文件系统映像，您可以将其刷到设备的系统分区。之所以具有通用性，是因为它使用新的标准化硬件API访问硬件（因此它可以在任何启用treble的设备上运行）。

**Bootloader**
[^36]
Bootloader是好几个启动阶段的统称，bootloader与电脑的启动管理器相似，负责启动手机的用户层面操作系统。

手机开机时进行开机自检和初始化手机硬件，它会指引手机找到系统分区并启动操作系统.

如果BL锁了，即使你强行刷了非官方boot，因为BL会校验boot分区的数字签名，所以无法加载Android内核到RAM，也就无法启动system。

锁定 Bootloader 一方面是为了用户安全（解锁就会发生格式化），另一方面也是为了维护厂商自身利益。


部分厂商对于 Bootloader 采用硬件层面上的熔丝机制，一旦你解锁了就回不到出厂 locked 状态，就是失去了保修。




![](https://s3.bmp.ovh/imgs/2022/08/16/f010caf0576cf10b.png)


**字库(分区)**

字库是硬件，就相当于电脑的硬盘。

功能机时代，很多手机程序、控制信息、字库信息是存储在一个专用芯片里面，芯片中主要部分是字库，所以一些售后和维修人员就习惯把这个存储芯片称做字库芯片。不过，到了智能机时代，这个存储芯片的功能已经远远超越了存储字库这么简单，所以它也远不是“字库”所能概括的，更准确的表述应该为eMMC芯片(embedded MultiMediaCard）。　

简单来说，“字库”(eMMC芯片)就相当于电脑中的BIOS+硬盘，一方面，它里面固化有手机的启动程序、基本输入输出程序、系统设置信息等等；另一方面，它还起到了存储照片、音乐等文件的作用，也就是我们经常提到的手机xxGB存储空间，而且手机的ROM(系统固件)也在这颗芯片当中，由此可见它对于一部手机的重要性。　

与手机一样，“字库”(eMMC芯片)也有相应的分类。

1、原装专用字库，即原厂生产、针对相应型号专门使用的芯片；

2、原装代用字库，同样是由生产，在原装字库货源短缺的情况下，替代原装字库使用的芯片，由于不是专门适用某型号机器的字库芯片，所以在体积上与原装字库存在差异，一般都要稍大一些；

3、其他品牌的字库，例如某芝部分型号的芯片，可以替换原装字库使用。

!!! tip "简单来说"
    字库要是坏了，换主板吧

**ROM包**

华为，小米，OPPO等都是安卓手机，但它们的系统又各不相同。这些系统本质上都是基于安卓系统的，他们对安卓系统进行定制，加入自己特有的服务。比如我们看到很多手机出厂就自带APP，这些就是 定制化之后的安卓系统 , 这些安卓系统就叫做ROM包。

**Recovery**

当手机系统文件被损坏从而不能正常启动时，我们可以进入recovery小系统对主体系统进行修复。

**第三方Recovery**

官方的recovery功能较少，一般可以清除缓存，擦除所有数据，刷入官方指定的系统，而不能刷第三方系统，所以就有人做 Recovery 去代替官方 (即第三方Recovery) 的，这样就能刷非官方的ROM了。

分为CMW和TWRP两种recovery

我们开机进入系统自带Re清除数据时音量上下键操作的界面就是CMW的版本。

TWRP就是可以触摸的版本，而TWRP的功能更为强大。
　　


**Fastboot**

fastboot 主要是用来与bootloader的USB通讯的PC命令行工具，也用来向bootloader传送刷机文件进行文件分区重烧。

开机后它会初始化硬件环境，实现一个小系统，然后和PC通讯，将PC上的刷机包写入至Emmc中，实现刷机。Recovery此时不起作用。


**FastbootD**

据我所知, fastbootd 是用户空间中的 fastboot。

在动态分区手机. `data`, `system`等原来的物理分区, 现在都被放到一个共同的`super`分区下. 这种"虚拟分区"只在用户空间(Android系统里)可见, 也就是说原版fastboot只能识别到整个`super`, 而`super`里的`data`这些却不行.

所以fastbootd 就是动态分区手机的 fastboot(指非动态分区手机的).

![a5nZlt.png](https://s1.328888.xyz/2022/08/13/TGOJ6.png)

[What is FastbootD? How to Boot to FastbootD Mode - DroidWin](https://www.droidwin.com/fastbootd-mode/)

**ADB Android 调试桥 (ADB)**

Android 调试桥 (ADB) 可让您将开发工作站直接连接到 Android 设备进行通信，以便安装软件包并评估更改。

![adb](https://oss-emcsprod-public.modb.pro/wechatSpider/modb_20220420_bc3dbe04-c08f-11ec-8f56-fa163eb4f6be.png)


了解更多：https://source.android.com/docs/setup/build/adb



## 了解刷机

### **早期**

早先的Android手机要想获取Root权限可以有以下几种方式[]：

1）使用本地提权漏洞利用工具来直接Root，这是最原始最纯洁的方式。随着厂商对Rom的升级，这些内核的漏洞随时都可能被修补，因此，这种Root方法在时间与空间上都有着很大的局限性。

2）由于手机厂商对硬件的封闭，加上内核补丁修补很完全，这个时候获取Root权限就更难了，这个时候刷机与Root就联合起来了，由于不能从系统内部通过Exploits来获取Root权限，只能通过修改Rom包来达到Root的目的，这也是目前很多第三方Rom包自带了Root的原因，然而手机厂商也不是吃干饭的，手机厂商在OTA升级时使用Recovery对包签名进行验证来防止用户刷入修改过的包。对于这种变态的厂商，只能通过FastBoot来线刷了，这里内容就不再展开了。

3）当然，还有一部分厂商，为了吸引更多用户购买他们的手机，还是在手机中偷偷的留了后门的，比如不锁BootLoader，让用户刷第三方的Recovery，又或是干脆留个以前的漏洞不补，让用户自己来Exploits等等

[^33]


### **深刷**

深刷就是底层刷机，顾名思义，这是一种从底层刷写手机分区的方式。与正常的线刷卡刷相比，这种方式更为彻底，不仅不需要开机，还可以强行解BL锁。

底层刷机模式常用于使用卡刷（Recovery下以及一系列使用ADB刷机方式的统称）或者线刷（Bootloader下使用Bootloader命令刷机的方式）刷成砖后救砖的场景下。按照主流处理器，高通为9008刷机模式，MTK为MTK PreLoader模式，海思麒麟为eRecovery模式（类rec操作）和eDownload模式。

!!! note
    MTK联发科芯片机型的深刷口被小mi提交修复，目前 天矶920 以后少有能使用此方法的。高通需要9008端口即可。详见刷机部分。

[底层相关刷机教程示例？](https://wiki.pchelper666.com/%E5%BA%95%E5%B1%82%E5%88%B7%E6%9C%BA%E6%95%99%E7%A8%8B)

### **OTA**

**这是升级，谈不上刷机**

OTA 意思就是**增量升级**，就是在原先系统的基础上增加新功能，也许是给手机打个补丁，也许是对性能的优化。**手机要活着能进系统且未修改才能用**。OTA 不会删除资料和系统的设定。备份资料放的位置要问官方，但通常不需要知道位置，内建备份也有还原功能，还能选择要还原的东西，还原的时候有问题的部分可以选择略过。

有些档案用 patch 的方式处理了, 有些用覆盖的方式 (要看做这个 OTA 包的人怎么做), 所以, 如果有档案与预期的不同 (通常是 root 后会删除或修改档案),  OTA 会失败。

你也可以解 OTA 的包进行修补获取Root权限。


### **Mtk 深刷**

是一直底层的刷机方法

### **高通 9008**

是一种最底层的联机模式，机器rec，fb都被篡改或清除时，唯一能进入还不会被摧毁的就是download模式。联机后驱动会显示9008或9007或900e等。所以大家都叫9008深度刷机。



### **软刷**

软件刷就是刷机大师，刷机精灵等的第三方刷机软件刷，现在已经绝迹了；

### **厂刷**

寄回厂子……


### **线刷**

线刷是 Fastboot 模式刷机，因为需要有一个PC机并且USB线要始终联着。所以这种方式称为线刷。

线刷是用 Fastboot，一般都是直接刷镜像，由 uboot 以直接写入闪存的办法把镜像直接写到闪存对应的位置（或者说分区）。

线刷时备份资料可以使用PC端的程式来管理，出问题的话三清之后再选择性恢复，再不行就全部设定和资料重头来过。线刷 要有电脑, 执行特定的程式. (通常就算手机挂到都进不去 Recovery 也能刷)

所以线刷包实际一般就是包含了 Fastboot 程序和各个系统镜像以及一个可执行的脚本的包，用户直接运行那个脚本，脚本调用 Fastboot 来刷。


### **卡刷 /第三方 REC**

卡刷就是 Rcovery 模式刷机

!!! tip "Do you know?"
    区别卡刷包 和线刷包最明显的区别是 (卡刷包的文件内容）里有 **Recovery 文件，而（**线刷包的文件内容）里有 FLASH 文件，如果你注意到上方路径，从文件名可以看到线刷包文件名里有 “FASTBOOT” 。

一般是在 recovery 里进行的，有直接刷镜像的比如 kernel 部分，但像 system 都是挂载 system 分区后再个别的更新里面的文件（差分或者直接覆盖），而不是像线刷那样把整个 system 镜像重刷一次。如果是通过打二进制补丁差分更新的话（绝大部分官方 OTA 包的做法），就要求被更新的文件和出厂时一样，否则就会失败，这是 OTA 失败的原因。优点是比较简单快捷，非常适合不会刷机的新手。

具体卡刷应该分两种, 一种是**小的更新包** (作法与 OTA 一样, 当然结果也一样), 另一种是完整的系统包, 这种通常是把系统的 partition 重新 format 再把资料放上去. (不会动到使用者资料的分区)。

卡刷可以用手机直接下载卡刷包，更名Update.zip后进入recovery刷机，刷后资料还会在，但是通常不做资料清除的话很容易发生问题，严重的就是一直出现系统错误，轻的则是偶尔出现闪退。

所以基本上刷完机最好进Recovery三清，刷前做三清也行，重点就是该清的要清一清，不然问题很多。接着要还原备份的时候如果系统本差异太大最好不要还原系统设定和App之类的资料，还原一些使用者data就好。卡刷需要存储空间，一般能进 recovery 就能刷。

卡刷包有比较复杂些的目录结构，除了用来更新的文件外，也包括一个可执行文件和脚本，但这两个脚本是给recovery来用的，而不是用户。

#### 第三方Recovery

第三方Recovery有很多种，最常用的是 `TWRP` ，还有很多基于 `TWRP` 修改的种类，比如 `橙狐Recovery`，`SHRP`，`PBRP`，`奇兔Recovery` 等等。

可以去 TWRP 官网 [Here](https://twrp.me/Devices/) 搜索，或者在 TWRP 官方APP下载，但是速度可能不是很好,也可以去[橙狐官网](https://orangefox.download/zh-CN)找。


如果以上途径都找不到，可以去酷安找你机型的话题，在话题内搜索关键词：`TWRP，rec，橙狐，oringe，pbrp，沥青，shrp。`

!!! info  "**什么是第三方REC？？**"
    Recovery像是一个独立的微型系统，可以不依赖于安卓操作系统主体单独运行。Recovery的中文名是恢复，顾名思义，当手机系统文件被损坏从而不能正常启动时，我们可以进入recovery小系统对主体系统进行修复。可是官方的recovery功能较少，一般可以清除缓存，擦除所有数据，刷入官方指定的系统，而不能刷第三方系统，所以就有人做 Recovery 去代替官方 (即第三方Recovery) 的，这样就能刷非官方的ROM了。像是所有的官改包，第三方是配的包，国际版的包以及原生系统，都是用第三方REC刷入。可以说，有了第三方REC，是你刷机的出发点，想刷什么都可以（当然刷的东西兼容你的设备）有了它，你就可以各种不同的系统，开始真正的刷机之路。但是，大部分的第三方REC不支持OTA升级，也就是说，每次你收到系统更新的时候，都必须要下载完整包更新。


### 刷机原理

见关于页面





## 准备设备

---

本教程用到的工具我**已经打入附带的文件包** [^1]

### 提前注意

**安卓系统的版本需要是 5.0~12.0 之间 (2022)**

!!! warning "注意"
    **解锁BL或救砖都会让你的文件被清空，需要备份**


!!! warning "注意"
    - 解锁设备将允许修改系统重要组件，并有可能在一定程度上导致设备受损
    - 解锁后设备安全性将失去保证
    - 解锁后部分对系统安全性依赖高的功能和服务失效
    - 解锁后部分系统功能遭到修改后，将影响系统新版本升级
    - 解锁后由于刷机导致的硬件故障，售后维修网点可以按非保修处理
    - 三星设备解锁后会永久性熔断 KNOX 安全认证
    - 大部分手机的版权认证 DRM 等级也会从 L1 下降至 L3、无法通过 Play 商店认证等。
    - 谷歌安全认证下降

不建议你在主力机上解锁 Bootloader，而且，如果厂商明确表示不能解锁 Bootloader ，**请放弃**。如果一定要刷机并且报着变砖的觉悟，可以尝试**深刷**强解。

### 准备驱动文件

准备你的手机对应的机型的驱动文件，文件包提供 Vivo 和 Oppo 的两种驱动文件[1^]。
个人建议下载一个泛用型驱动 [universal adb drivers](https://adb.clockworkmod.com/)。少数情况下不能识别的话，需要我们用「手机厂商名 + adb driver」的关键词搜索得到相关的下载和安装教程。

安装完驱动请重启。 [相关手机驱动下载-FiimeROM](https://mi.fiime.cn/qudong)
[相关驱动站点](https://kamiui.ml/E52shuaji/)



!!! tip "**对于非深刷玩家如何检查是否链接？**"  
    💡 重启后在设备开机状态下连接电脑，打开终端，输入`adb devices`，如果返回了设备名称，说明 adb 配置完成；用 `adb reboot bootloader`进入 fastboot 界面（这步不适用fastbootd设备,即安卓十），键入 `fastboot reboot`后，若设备重启，说明 fastboot 正常。



### 准备设备和平台工具

Win7 或以上电脑一台，能传输文件的数据线一条（**最好是原装线，不能用充电线**），电脑下载解压[安卓平台文件包](https://dl.google.com/android/repository/platform-tools_r33.0.2-windows.zip)或[Linux版本安卓平台工具包](https://dl.google.com/android/repository/platform-tools-latest-linux.zip)或[Mac版本](https://dl.google.com/android/repository/platform-tools-latest-darwin.zip)，退出所有手机助手类软件。

解压工具包，你会看到 adb 和 fastboot ，这是我们针对 Android 设备进行高级调试和安装的工具。

如果你不想配置ADB全局环境的话，以后执行 Adb 命令需要在工具目录下，按住 Shift 右键鼠标打开终端，命令替换为`.\adb`或者`.\fastboot`，如果需要配置全局环境，请按照[这篇文档](https://www.sunzn.com/2018/08/02/Windows-10-%E4%B8%8B%E9%85%8D%E7%BD%AE-ADB-%E7%8E%AF%E5%A2%83%E5%8F%98%E9%87%8F/)配置。

Linux 需要使用其自带包管理器安装 `android-platform-tools`

!!! tip "提示[^3]"
    如果你已经安装了 [choco](https://chocolatey.org/) 或 [homebrew](https://brew.sh/) 等包管理工具的话，Windows 输入`choco install adb universal adb-drivers -y`，Mac 输入 `brew install android-platform-tools`能最方便的完成 adb 和 fastboot 的配置。Windows 用户可以参照  [Windows 操作系统下的 ADB 环境配置](https://sspai.com/post/40471) 这篇文章；macOS 用户可以尝试  [此脚本](https://github.com/corbindavenport/nexus-tools) 或是参考 [使用 Mac 为 Android 手机刷原生系统](https://sspai.com/post/38535) 进行手动配置。最后最最不济，可以尝试在 Google  [开发者页面](https://developer.android.com/studio/releases/platform-tools?hl=zh-cn) 下载对应 adb 包，解压后在对应的目录下执行指令亦可，或者是尝试 [WebADB](https://app.webadb.com/#/) 或  [adb 在线执行器](https://adb.http.gs/) 这样的在线 adb 工具，比较考验浏览器的兼容性。


然后打开手机的USB调试开关允许计算机调试，确认你的**驱动线和驱动**都**没有问题**！

!!! info
    通过adb devices命令确认已经连上手机。


华为工具在软件包内(看引用)

### 准备深度测试或申请解锁BL（深刷可跳过）

> BL 是 bootloader 的简称 就是 手机开机时，最先运行的小程序：开机引导程序 ，Bootloader 锁，主要是在引导过程中对系统签名，内核签名及 Recovery 签名进行检验，如果签名不一致，即终止引导。绿机器人儿用它来进行开机自检和初始化手机硬件，它会指引手机找到系统分区并启动操作系统，相当于电脑上的BIOS。
> 
厂商通常会对手机的bootloader上锁，这样它就只能运行厂商认证过的操作系统和recovery了。如果boatloader发现要运行的系统不是指定的系统，就会阻止它运行。

在开发者选项中打开「OEM 解锁」（除了少部分流入我国市场的国外运营商有锁机外，此选项基本都可供用户开启。）

!!! danger 
    **解bl锁会清除手机（恢复出厂设置）所有数据，记得提前备份好。**

不同的手机解锁方式不同，你甚至可以去线下店让服务人员解锁。或者从自己的社区中获取本机型的解锁信息。部分手机解锁很麻烦，比如华为，想要解锁只能去淘宝买解锁码，而且当你刷回官方 ROM 时，会自动加锁。

而对于小米手机，可以通过这个地址 [申请解锁](https://www.miui.com/unlock/download.html) 下载工具，然后打开手机设置，进入关于手机–>系统版本点10下，在`设备解锁状态`中绑定账号和设备，进入`Fastboot`模式(关机后，同时按住开机键和音量下键)，打开刚才下载的工具，点击 `解锁` 后系统会恢复出厂系统并解锁。也可以通过OEM解锁

!!! danger "不可逆"
    解锁 Bootloader 的手机会发生熔断，熔断是硬件级别的，你选择了确定解锁的那一刻就熔断了，操作不可逆，不是你再次上锁就能恢复的。
    熔断丢保修，不能用pay，外版本来就没保修不能用pay你爱怎么熔怎么熔。

    线刷官方四件套固件无需解锁，所以不会断溶。


**如果你的设备不能进行官方解锁，可以尝试深刷强解 。**

!!! note
    一些古董机型是没有BL锁的，比如红米Note。

附上[小米解锁教程](https://web.vip.miui.com/page/info/mio/mio/detail?postId=28646781&boardId=5415551&isComment=&isRecommend=0&app_version=dev.211029&ref=share).



### **什么，你是华为？**


这节的教程来自酷安@某贼 [^41]

推荐直接看[知乎上他的文章](https://zhuanlan.zhihu.com/p/397173427)

[华为解锁相关驱动与工具](https://miao202.lanzouj.com/igAzW09qew7e)

[解锁华为 bootloader 工具，适用 Kirin 960/95х/65x/620](https://github.com/mashed-potatoes/PotatoNV) ，如果不能下载请复制Github的 zip 链接请填入[镜像站](https://ghproxy.com/) 下载。[^42]

!!! danger
   如果你选择了强解Bl,将不能更新系统。官方解锁可以正常OTA。

**拆机短接解锁深刷端口**

!!! danger
    如果你没有拆机经验，建议找你附近的手机店帮拆一下，以免操作不当造成物理损坏。一般只需要拆后盖即可，但有几个机型还需要拆屏蔽罩，具体参考猎人短接图上的点位。


>猎人短接图使用方法：打开后在左下方搜索手机名称，比如荣耀9i就搜索9i即可,可能会搜索出两个结果，选择带 `TP` 字样的即可。

找到短接图后就可以按图短接了。一般是关机状态下短接再插线连电脑。**注意不要拆电池**。

其中有的图箭头指向两个触点，意思是用镊子短接这两个点。有的图是一个箭头，意思是用镊子一端接这个点，另一端悬空或接旁边的金属保护罩。有的图两个箭头分别指向触点和保护罩，意思是短接这个触点和保护罩。


电脑提前打开设备管理器，短接插线后注意观察电脑反应，如果成功的话电脑会有设备连接提示音并在设备管理器里刷新出华为麒麟深刷端口。

如果有提示音但显示未知设备USB-SER说明你没装驱动。

如果电脑没任何反应，你应该检查短接工具，数据线，电脑插口，手机是否关机状态，然后多短接几次试试。出端口之后就可以松手了。

不用看手机，注意端口就行，进入深刷后手机是黑屏无显示的。

**更改解锁码**

短接成功之后打开PotatoNV，第一项一般会被自动识别，在Bootloader一项里选你的处理器，通常情况下659选方案A即可。

点击 Start ，软件会随机生成一个16位解锁码显示在输出信息里。改好解锁码重启。

**华为**的解锁码是和硬件相关的，如果有解锁码进 FASTBOOOT 输入`fastboot oem unlock 解锁码`就可以了。


---------


**解锁困难**

如果你是华为手机，**不推荐** 麒麟990以及更高的手机解锁，解锁过程困难且限制大。

之所以说 **华为解BL锁很困难**，是因为华为官方在18年就**关停**了官方获取解锁码通道，所以无法从官方拿解锁码。但是我们仍然可以选择通过第三方软件，淘宝商家获取解锁码或者手动拆机短接改解锁码。

!!! info
    顺带一提，华为联发科解不了BL

而且华为通过解锁码解锁的BL叫做用户BL，只开放少数分区比如system，boot，recovery，userdata等。工厂BL则开放全分区，但需要第三方软件解锁（需要很多钱），而且只能在Fastboot里临时解锁，重启即失效。

普通用户就只能解特供用户BL。


**data分区是敏感肌**

!!! danger  
    如果你在使用官方系统，华为喜欢把某些系统组件放到data里,请避免双清＆格式化data,执意操作会导致这些组件丢失，不能开机。如果你需要格式化，可以恢复官方的 Recovery 来格式化。如果格式化导致无法开机，可以提取官方包里的userdata分区，进入Fastboot用fastboot flash userdata 刷入来恢复。
    
    华为这里很多TWRP不支持解密data，那就别解密了，插SD卡或者adb sideload刷包。




**救砖困难**

华为官方并没有给用户提供传统意义上的卡刷和线刷方式。一般是在升级模式用的（即所谓“三键强刷”包）华为官方包是.APP格式，由一堆分区镜像组合而成。

但是升级模式是官方 Rec 的一部分，刷包校验十分严格，包括华为签名（是否官方包），系统版本等等。升级模式无法满足救砖的需要。

!!! tip
    官方没有卡刷包。APP那个zip一般不能直接拿来卡刷。

如果使用Fastboot,借助第三方工具可以用APP包线刷，但是上面说了用户特供用户BL是没有解锁全部分区的，这将导致设备的分区大半无法刷入。实现全分区临时解锁除了借助第三方工具（moneymoney很多钱），唯一自己能操作的办法就是拆机短接。救砖教程请看下一节。

!!! info
    华为官方给用户提供的官方救砖途径是eRecovery恢复和Hisuite修复，但是华为封锁了这两条途径……
    这两个方法都是从官方服务器下载包然后恢复，但很多旧机型已经无法再从官方获取到包，也就无法再进行官方救砖。


**刷第三方包容易变砖**

包括官改移植类原生。


**华为各个模式**


Recovery：不连接电脑，关机状态长按电源键和音量加，出现 logo 松开电源键继续按住音量加，直到进入。

eRecovery：关机状态,**数据传输线**连接电脑，长按电源键和音量加。

Fastboot：关机状态,**数据传输线**连接电脑，长按电源键和音量减。

升级模式（所谓三键强刷）：关机状态下，长按电源键，音量加和音量减。如果你的系统包在手机存储里，则不需要链接直接按，如果你想要用电脑工具辅助，则需要使用**数据传输线**连接电脑。

??? info "关于 Recovery 和 eRecovery"
    Recovery和eRecovery是有区别的。Recovery就是我们熟知的那个Recovery，而eRecovery是独立于Recovery的另一个分区。二者功能不同，在Recovery里我们可以做恢复出厂，清除cache等本地操作，而在eRecovery里可以连接WiFi下载适用于你的手机的系统包并自动安装恢复。刷TWRP只是替换掉了Recovery，eRecovery仍然保留。











[^1]:**所需资料打包**<https://push.dianas.cyou/LIS/Share/Root/>
[^3]:[Android 玩家必备神器入门：从零开始安装 Magisk - 少数派 (sspai.com)](https://sspai.com/post/67932)
[^17]:常识基础 [https://mi.fiime.cn/tutorial](https://mi.fiime.cn/tutorial)
[^28]:联发科不建议玩机 https://www.coolapk1s.com/feed/37080982
[^30]:简单认识手机各个分区 https://www.coolapk1s.com/feed/38367093

[^33]:Android Root原理分析及防Root新思路 https://blog.csdn.net/hsluoyc/article/details/50560782

[^36]:Bootloader 原理 https://www.zhihu.com/question/47496619/answer/195494376

[^39]:[为什么内存不叫运存？](https://www.zhihu.com/question/327171923/answer/716602933)

[^41]:[[新手必看]华为刷机你一定要知道的](https://zhuanlan.zhihu.com/p/416456337)

[^42]:[部分华为麒麟手动获取BL解锁码](https://zhuanlan.zhihu.com/p/397173427)

[^45]:[关于Project Treble和Android GSI](https://bbs.liuxingw.com/t/9315/2.html)
