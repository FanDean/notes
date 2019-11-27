## 系统镜像下载

只讨论 **原生系统镜像**。

[在 Mac 上通过 Boot Camp 使用 Windows 10](https://support.apple.com/zh-cn/HT204990)
[从 Microsoft 下载 ISO 文件](https://www.microsoft.com/zh-cn/software-download/windows10ISO) 

MSDN我告诉你

Linux官网 或 国内镜像站点 



## rufus (Windows中使用)

只支持Windows和多数Linux的官方原版iso镜像文件。在Windows中最受欢迎。

对于**新版**Linux系统镜像，rufus无法做到立即同步，如果rufus提示需要而额外下载文件并且要求的版本号不一致时，建议在之后的选项（如果弹出该选项）中选择"..DD"模式而非推荐的“ISO..”模式，源于一次在写入最新版XUbuntu 18时使用 "ISO..."出现无法引导的情况。

RUFUS的推荐设置：默认设置就够了。在“  格式 选项”中，同时选中以下两项：

- 快速格式化。
- 创建扩展的标签和图标文件。

或者：

**1.使用UEFI**创建可启动的Windows 10 USB（2019年基本都用这个）

- 分区方案：  GPT。
- 目标系统类型：UEFI。
- 文件系统：FAT32。
- 簇大小：选择4096字节（默认）。

**2.使用旧版BIOS**创建可启动的Windows 10 USB

- 分区方案：  MBR。
- 目标系统类型：BIOS或UEFI-CSM。
- 文件系统：NTFS。
- 簇大小：选择4096字节（默认）。





## 多重引导U盘

主要有：

- YUMI ：[YUMI - Multiboot USB Creator | Pen Drive Linux](https://www.pendrivelinux.com/yumi-multiboot-usb-creator/) 
- WinSetupFromUSB ：[Downloads | WinSetupFromUSB](http://www.winsetupfromusb.com/downloads/) 
- SARDU：[SARDUcd](https://www.sarducd.it/multiboot-usb-builder.html)  
- XBoot：[XBOOT](https://sites.google.com/site/shamurxboot/download)
- MultiBootUSB：[MultiBootUSB](http://multibootusb.org/) 

操作方式大多相同，写入一个iso后接着再写入下一个iso即可（也有可以同时选择多个再写入）。



### YUMI

 YUMI – Multiboot USB Creator；YUMI (Your Universal Multiboot Installer)

网站： [YUMI - Multiboot USB Creator](https://www.pendrivelinux.com/yumi-multiboot-usb-creator/) 

 YUMI是创建多重引导USB的最受欢迎和推荐的软件。它可用于集成多个iso文件，例如Windows和Linux发行版，防病毒磁盘，磁盘克隆实用程序等。 

与使用grub直接从USB引导ISO文件的MultiBootISO相反，YUMI使用syslinux引导存储在USB设备上的提取发行版，并在必要时恢复使用grub从USB引导多个ISO文件。 除了一些发行版以外，所有文件都存储在Multiboot文件夹中，从而可以组织得井井有条，可移植的Multiboot USB驱动器仍然可以用于其他存储目的。 

 以下YUMI UEFI版本正在开发中，它将GRUB2用于UEFI和BIOS引导。请注意，它与标准YUMI不向后兼容。支持的发行版是有限的，并且您的USB驱动器必须格式化为`fat32`才能支持以`UEFI`模式引导。 （仅BIOS模式可用于NTFS格式的驱动器）。 

您的USB驱动器必须是Fat16 / Fat32 / NTFS格式的，否则Syslinux将失败并且您的驱动器将无法启动。 NTFS可能不适用于所有发行版，但存储4GB以上的文件需要使用NTFS。 YUMI UEFI必须使用Fat32格式 。

其支持的发行版和工具有很多，在其列表中发现有：

- Rescatux
- System Rescue CD

YUMI 是便携版，用法非常简单。

**如果U盘格式为Fat32 ，则YUMI、MultiBootUSB 并不需要格式化现有U盘**

写入 windows*.iso 的介绍：

- Single Windows Installer / PE 选项最可能适用于原始文件和修改后的ISO文件。使用时，每个USB驱动器只能存储一个Windows Installer（即，一个Win XP和一个Win Vista 7或10）。
- Multiple Windows Installer / PE 选项允许每个驱动器存储多个 Windows Installer。通常，只有未修改的 Windows ISO 文件才能使用此选项。 

-  `-wimboot`选项将提取的 Multi Windows Installers 存储在其自己的目录中。
-  `-bootmgr`选项将 bootmgr 和 bcd 文件移动到驱动器的根目录。 （注意：-bootmgr选项确实需要Windows Vista或更高版本的主机才能运行bcdedit）。 

> 写入Windows10后在虚拟机中测试时提示 autounattend.xml 无效。好吧，其实我们可以换一种方法，比如写入和win PE，只要PE写入成功，则可以在PE中选择想要安装的系统ISO。Win7可以。
>
> 还没尝试将调整为其他方式，比如单一、bootmgr。



YUMI尝试将大多数添加的发行版存储在`multiboot`文件夹中。这也是为syslinux设置的根目录。在某些情况下，YUMI还希望USB驱动器的卷标为MULTIBOOT，以便引导OpenSUSE，CentOS和其他发行版。 YUMI尝试自动创建该卷标，但是有时可能会失败。如果您希望发行版能够启动，请确保USB的卷标保持 MULTIBOOT。 当从某些笔记本电脑（例如带触摸屏的Lenovo Yoga）引导Linux发行版（如Ubuntu）时，可能需要 `acpi = off` 引导参数才能成功引导。 

但是 YUMI 并没有介绍卸载某个系统的方法，难道是手动删除？

YUMI确实支持NTFS，但是并非所有发行版都可以从NTFS格式化的设备启动。 Windows To Go 和包含 4GB 以上文件的发行版需要 NTFS。 

> 官方链接**最下面**有各种使用方法：[YUMI - Multiboot USB Creator | Pen Drive Linux](https://www.pendrivelinux.com/yumi-multiboot-usb-creator/#FAQ) **查看方式：**最下面有一组**嵌入在网页中的Tab标签**，切换标签即可看到相应的信息，包含的Tab有
>
> - **Requirements**：必要条件
> - Changelog：
> - How To：介绍了在Linux中如何使用系统命令将非fat32格式的U盘格式化为fat32，并且需要通过Wine来使用YUMI（感觉YUMI自身并不能对U盘进行格式）
> - Supported Distors：列出了所有支持的发行版
> - **FAQ**：常见问题



### WinSetupFromUSB

多重Windows和Linux

WinSetupFromUSB：在Windows系统中使用

创建多重引导USB磁盘的过程非常简单。对于Windows操作系统，可以使用称为WinSetupFromUSB的流行工具创建这些多引导USB磁盘。它允许您将多个ISO放在一个安装盘中。 

您可以将Windows和Linux放在相同的可启动磁盘中，也可以创建Windows 7，Windows 8.1和Windows 10的主安装磁盘。由您决定。此外，对于必须使用各种发行版，每个发行版都有自己的功能集的Linux用户而言，制作多重启动USB可能对帮助非常有用 。

添加第一个 ISO 文件到 Multiboot USB Disk ：

- 下载 [WinSetupFromUSB](http://www.winsetupfromusb.com/)。解压缩此文件。 

- 将闪存驱动器连接到计算机。 

- 打开WinSetupFromUSB（根据您的操作系统选择打开32位和64位版本）。无需任何安装即可运行。 

- 确保在下拉菜单中列出并选择了您的闪存驱动器。如果它不在列表中，请单击刷新。 

- 勾选FBinst自动格式化。 注意：您只需要为首次安装ISO勾选此选项。如果您的计算机设置为使用UEFI模式启动，或者UEFI听起来有些奇怪，请选择FAT32。否则，请使用NTFS选项。 

-  单击高级选项。

-  选中“ Vista / 7/8 /服务器源”的“自定义”菜单名称复选框。单击十字（X）按钮以退出“高级选项”。 

-  要为多引导USB添加ISO文件，请在“添加到USB磁盘”子标题下，选中与操作系统相对应的复选框。 例如，我正在使用Windows 8.1 ISO。

   注意：如果ISO的大小大于4 Gb，它将显示一条消息，将文件拆分为多个部分。这是因为您选择了FAT32选项。单击确定。 

   注意：WinSetupFromUSB不支持双重ISO，即单个ISO不能具有32位和64位版本。它将显示一条错误消息。 

-  单击执行。将显示数据删除警告消息。这是因为您选择了格式化闪存驱动器。单击是。 

  注意：在单击“是”之前，请务必先检查闪存驱动器的名称。否则，您最终将格式化其他连接的存储介质。

-  将会显示另一条警告消息，告诉您所有分区将被删除。单击是。 

-  接下来，它将询问文件夹名称。在30秒内输入所需的数字，否则它将自动选择。单击确定。 

-  它将询问启动菜单名称。当您在某些PC上运行多重引导USB并选择操作系统时，将出现此信息。键入所需的名称，例如Windows 8.1 64位。单击确定。 

-  该过程将需要几分钟才能完成。 

-  单击退出以完成  

添加第二或多个 ISO 文件到 Multiboot USB Disk ：

- 再次启动该工具。 
- 在下拉菜单中选择您的闪存驱动器。 
- 单击高级选项，然后查找Vista / 7/8/10 / Server Source的自定义菜单名称。 
- **不要点击FBinst的自动格式化**。那是因为它将删除您以前的ISO文件。 
- 为您的多重引导USB添加第二个ISO文件。 
- 单击“执行”，然后重复前面提到的步骤。 



现在，您已经启动并运行了多重引导USB，现在该看一下操作了。将闪存驱动器插入计算机，然后将启动设备设置为USB。大多数台式机和笔记本电脑都有专用的键来触发启动菜单。加载多引导USB后，从列表中选择所需的操作系统。 因此，这就是创建多引导USB闪存驱动器的方法，它使您可以一次在多个操作系统之间进行选择。使用此方法可将同一操作系统的32位和64位版本放入一个可启动设备中。



### MultiBootUSB 

 MultiBootUSB 的使用仅限于基于Linux Live CD的ISO 。



[MultiBootUSB]( http://multibootusb.org/page_features/ ) 开源

开源的 MultiBootUSB ：  MultiBootUSB是一个用python编写的**跨**平台软件（可以在Win和Linux中安装），它允许您无损地在USB磁盘上安装 **多个实时Linux** （**只能写入Linux发行版**），并可以选择卸载发行版。 （Linux推荐）

MultiBootUSB 使用Syslinux，因为许多发行版在其ISO中都使用Syslinux / Isolinux。 MultiBootUSB附带了不同版本的Syslinux，以减少不兼容性，并安装了特定于发行版的Syslinux版本。这里， Syslinux是默认的引导加载程序，用于使USB闪存驱动器可引导。

步骤：

- Step 1 - Insert USB disk and start the program
- Step 2 - Choose your ISO
- Step 3- Click `Install distro` button.
- 要安装多个 ISO 则多次重复上面步骤即可

还可通过其自带的 QEMU 功能**测试**ISO镜像或制作好的U盘（如果MultiBootUSB安装在Linux则还需要手动下载） ：

-  Go to `Boot ISO/USB --> Browse ISO --> Choose RAM size（最大2G） --> Click on Boot ISO或者USB`. 
-  但是我测试了一下不行。推荐在在虚拟机中测试。

 MultiBootUSB 支持创建具有数据持久性（persistence ）能力的USB启动器：

- 首先要了解，持久性（persistence ）是什么意思？安装过Linux系统的都应该知道，在安装系统时我们可以先选择直接加载系统，之后再选择是否需要安装；直接进入系统后你可以在系统中更改应用程序设置或**安装程序**或保存一些文件，但是当你之后再次进入这个实时系统时，会发现之前所作的修改全都丢失了；如果想保存这些修改，则需要创建具有数据持久性（persistence ）能力的USB。但是对系统文件比如内核文件的修改是不能保存的。
- 并不是所有的Linux Live CD支持持久性，这里Ubuntu、Fedora和Debian都支持
- 创建方法：当你选择了 ISO 的时候， MultiBootUSB将检测发行版的类型，并且“持久性大小选择器”滑块将出现在“ MultiBootUSB”标签页之中。  持久性的最大大小也会根据USB磁盘文件系统自动计算。 

卸载已经写入的系统：打开MultiBootUSB ，选择要卸载的系统即可。

使用图形化的dd命令直接将ISO写入U盘。如果上面任何一种方法均无法创建活动USB磁盘，则可以选择此选项，点击”Write image to disk“下的”Write image to USB”开始写入。 如果要想恢复U盘到原始状态并且不使用破坏性方法，则需要格式化U盘。 

重新安装 syslinux：有些情况下你需要重新安装syslinux，比如当你使用其他  live USB creator 的时候会移除由MultiBootUSB 创建的 syslinux。 首先，选择一个USB磁盘分区（例如`/dev/sdb1`或G :），然后选择方法并单击安装。 



我遇到了如下错误

```
syslinux 6.03 edd 2014-10-06 copyright(c) ... H. Peter Anvin et al
"Undef symbol FAIL: init_fpu"
"Failed to load COM32 file libcom32.c32".
"Failed to load COM32 file wichsys.c32"
```

它们也同样遇到此问题： [Debian 10.1 and SystemRescue6.0.3 don't boot · Issue #474 · mbusb/multibootusb](https://github.com/mbusb/multibootusb/issues/474)



## 其他工具



 Easy2Boot： [Easy2Boot](http://www.easy2boot.com/)

Multiboot 2



## 系统启动后如何进入BIOS



对于Windows 10 ，我们可以在设置 👉 恢复 👉 高级启动 👉 立即重启 👉  重启后选择使用设备，然后进行你想要的操作。

下面是各品牌电脑对应的按键：

| OEM/Brands        | Key to Enter BIOS or Boot into BIOS/UEFI  | Devices/Models                                               |
| ----------------- | ----------------------------------------- | ------------------------------------------------------------ |
| ACER              | **DEL** key or **F2** Key                 | *Aspire, Predator, Spin, Swift, Extensa, Ferrari, Power, Altos, TravelMate* |
| ASUS              | **Delete** key                            | *A-Series*                                                   |
| ASUS              | **F2** key or **Esc**                     | *B-Series, ROG-Series, Q-Series, VivoBook, Zen AiO, ZenBook* |
| COMPAQ            | **F10** key                               | *Presario, Prolinea, Deskpro, Systempro, Portable*           |
| DELL              | **F2** key                                | *XPS, Dimension, Inspiron, Latitude, OptiPlex, Precision, Alienware, Vostro* |
| HP                | **ESC** key or **F10** key or **F11** key | *EliteBook, ProBook, Pro, OMEN, ENVY, TouchSmart, Vectra, OmniBook, Tablet, Stream, ZBook* |
| HP PAVILLION      | **F1** key                                | *Pavilion*                                                   |
| LENOVO            | **F1** key or **F2** key                  | *ThinkPad, IdeaPad, Yoga, Legion, 3000 Series, N Series, ThinkCentre, ThinkStation* |
| SAMSUNG           | **F2** key                                | *Odyssey, Notebook 5/7/9, ArtPC PULSE, Series ‘x’ laptops*   |
| SAMSUNG ULTRABOOK | **F10** key                               | *Ultrabook*                                                  |
| SONY              | **F1** key or **F2** key or **F3** key    | *PCG-Series, VGN-Series*                                     |
| SONY VAIO         | **ASSIST BUTTON**                         | *VAIO*                                                       |
| TOSHIBA           | **F1** key or **ESC** key                 | *Portégé, Satellite, Tecra, Equium*                          |
| TOSHIBA EQUIUM    | **F12** key                               | *Equium*                                                     |

[How To Enter BIOS Utility (UEFI Settings) On All PCs And Boot From USB?](https://fossbytes.com/enter-bios-utility-uefi-settings-all-pc-boot-from-usb/) 

## 参考



国外好文：

- [How To Enter BIOS Utility (UEFI Settings) On All PCs And Boot From USB?](https://fossbytes.com/enter-bios-utility-uefi-settings-all-pc-boot-from-usb/)
- [How To Put Multiple ISO Files In One Bootable USB Disk | Create Multiboot USB Disk](https://fossbytes.com/how-to-put-multiple-iso-files-in-one-bootable-usb-disk-create-multiboot-usb-disk/) 必看
- [Best Ways to Create Multi Boot USB Device [How to]](https://www.techperiod.com/create-multi-boot-usb-device/) 必看
- [How To Install OTA Updates Using Android Recovery And ADB Sideload?](https://fossbytes.com/install-ota-updates-using-android-recovery-and-adb-sideload/) 这还有各安卓刷机

