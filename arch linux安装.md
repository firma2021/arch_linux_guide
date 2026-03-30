## 选择boot loader

- 如果你想要一个现代的，集成Btrfs快照的, 开箱即用的，同时支持 BIOS、UEFI 和 Windows 双引导的boot loader，请选择 Limine。
- 如果你想要使用稳定成熟，适用于几乎所有机器的boot loader，请选择 GRUB。同时你可以接受这些缺点：庞大且复杂，包含许多文件系统驱动程序；明显比 systemd-boot、Limine 慢。
- 如果你倾向于最简单的设置且不需要快照或高级功能，请选择 systemd-boot。

## 选择文件系统

- 对于需要快照/备份功能和透明压缩功能的用户，推荐使用 BTRFS。
- 对于不需要高级功能，只想获得一个快速可靠的文件系统的用户，推荐使用 XFS。

## 什么是挂载

从“直觉”上看，“先有设备 → 格式化 → 在里面建目录 → 才能用”，这和我们现实里用硬盘、U 盘的感觉是一致的。

Linux 的“挂载”却是：**先有目录，再把设备挂到这个目录上**，表面上反直觉，实际上带来了一套非常统一、强大的抽象。

Linux/Unix 把“路径/目录树”当成第一公民，把“设备上的文件系统”当成可以插拔的实现，所以需要先有挂载点目录，再把设备上的文件系统挂进去。这样所有资源——无论来自哪块盘、网络还是内存——对用户来说都只是一个统一目录树中的一段路径。上层应用只关心“路径 + 权限 + 文件内容”，不关心物理介质。如果让“设备成为一切的起点”，就会把“物理细节（哪块盘）”泄漏到应用层和用户的日常使用中，破坏了一致的命名空间。

挂载动作其实是在告诉 VFS：  

> “从这个路径开始，后面的访问，交给某个具体文件系统实现。”

例如把`/dev/sdb1` 挂载到`/mnt/usb` 下时，当路径解析到 `/mnt/usb` 这个挂载点时，“切换”到 `/dev/sdb1` 上的那个文件系统实现，由它来回答后续的目录/文件访问请求。

## 分区，文件系统和挂载

1. 硬盘：一整张空白的纸。

2. 分区：把纸裁剪成几个大小不同的部分（A4纸、便签纸等）。
3. 文件系统：规定每张裁剪好的纸的书写格式（比如横线格、方格、五线谱）。
4. 挂载：把画好格子的纸贴到某个文件夹里，这样你就能随时打开文件夹使用它了。

- 分区是物理/逻辑的空间划分，目的是隔离与组织。
- 文件系统是空间内部的管理规则，目的是高效、安全地存取数据。
- 挂载是建立访问通道，目的是将已管理好的空间，无缝接入到现有的、更大的访问体系（目录树）中。

## 交换分区

使用ZRAM 交换分区。ZRAM将待交换的数据在内存中进行实时压缩，压缩后的数据仍存储在物理内存中，不会发生磁盘 I/O，速度更快。

## 键盘布局

键盘布局设置为 **English (US) / default**，而不是中文键盘布局。

这是因为，国内绝大多数物理键盘，其键帽上的字母、符号排列就是标准的美式英语布局。

> [!TIP]
>
> 解决 failed to start virtual console setup：
> 编辑`/etc/vconsole.conf`，将`KEYMAP=cn`改为`KEYMAP=us`，再执行`mkinitcpio -P`命令

## 解决双系统下时间错乱

Linux 和 Windows 对系统硬件时钟（RTC）的解读方式不同。当你在一个系统中设置正确时间后，另一个系统启动时会因为解读方式不同，显示错误的时间。

- Windows 默认将硬件时钟（RTC）视为本地时间（Local Time）‍。
- 大多数 Linux 发行版 默认将硬件时钟视为协调世界时（UTC）‍，然后根据系统设置的时区来换算显示本地时间。

> [!TIP]
>
> 最佳实践：修改 Windows 注册表，让 Linux 保持 UTC 默认。

在 Windows 中，以管理员身份打开“命令提示符”或“PowerShell”，执行以下命令：

```powershell
reg add "HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\TimeZoneInformation" /v RealTimeIsUniversal /t REG_DWORD /d 1
```

## 避免关机时间过长

在`/etc/systemd/system.conf`中指定 systemd 在启动或停止一个服务时的超时时间：

```ini
DefaultTimeoutStartSec=5s
DefaultTimeoutStopSec=5s
```

> [!TIP]
>
> 执行`journalctl -p5`，查看是哪一个进程导致了 timeout。p5表示查看等级为0-5的日志。在 `journalctl` 的优先级系统中，**数字越小，代表日志的紧急和严重程度越高**。

## CachyOS安装

推荐安装基于arch linux的的发行版CachyOS。

手动分区，分三个区：
|大小 |文件系统 |挂载点 |标记|
| ------- | ------- | ------- |------- |
|4096MiB |fat32 |/boot |boot|
|8192MiB |linuxswap |无 |swap|
|剩余所有空间| btrfs| / |无|

取消勾选：
- CachyOS Packages下的cachyos-wallpapers，如果不使用micro编辑器，可以取消勾选cachyos-micro-settings
- CachyOS shell configuration
- Base-devel + Common packages下的：
  - filesystem下的nilfs-utils
  - fonts下的ttf-bitstreamm-vera, ttf-dejavu, ttf-opensans
  - Some applications selection下的alacrittty, btop, glances, nano-syntax-highlighting, micro, nano, vim
- KDE-Desktop下的cachyos-emerald-kde-theme-git, cachyos-*, char-white, kdeconnect
-  
- cachyos-plymouth-bootanimation

在安装后，执行`pacman -Qs cachyos`，查看安装的CachyOS包，移除多余的主题包，如`paru -Rsn cachyos-plymouth-theme cachyos-plymouth-bootanimation`






