# 安装前的准备

需要注意的是，这篇指南只提供在 UEFI 设备上安装的方法。

## 我们需要足够的空间

根据 Arch Wiki 上的建议，EFI 至少需要 1 GB 的空间，根目录文件系统推荐至少 40 GB, 如果您的 EFI 是由 Windows Setup 创建的，那大概率只有 300 MB，我们需要扩容 EFI，大概步骤是在 Live 中备份您的 EFI，扩容之后再复制回去，详细的教程在 [Wiki](https://wiki.archlinuxcn.org/wiki/EFI_%E7%B3%BB%E7%BB%9F%E5%88%86%E5%8C%BA#%E6%9B%BF%E6%8D%A2%E4%B8%BA%E6%9B%B4%E5%A4%A7%E7%9A%84%E5%88%86%E5%8C%BA) 中，请自行查阅。

### 双系统的提示

打开磁盘管理，右键想要使用的磁盘中的分区，点击`压缩`，输入 40960 作为留出的空间，之后不需要进行操作，更不要用 DiskGeinus 处理未分配的空间。

由于本指南使用 systemd-boot，确保所有的硬盘上只有一块硬盘上有 EFI 分区，否则，请使用 Grub2 作为引导加载程序。

## 制作 Arch Live 驱动器

Arch Live ISO 可以在[这里](https://archlinux.org/download/)找到，下载它并使用 Ventoy 制作启动盘，为了防止额外的问题，请使用 UEFI/GPT 安装 Ventoy。

## 关闭安全启动

这是必要的，一个是为引导加载程序签名很麻烦（也不安全），而且一些主板不能导入您自己的签名，在一些联想设备上，导入自己的签名甚至会使设备无法启动（需要编程器刷写 BIOS 固件修复）。另外，官方的 Arch ISO 也没有签名。

## 关闭快速启动

快速启动会让 Windows 使用的硬盘带有 dirty 标志，这会导致每次启动时都进入紧急模式，如果不介意，手动清除 dirty 标志 `ntfsfix --clear-dirty /dev/devices`。这可以通过 force 标志或 ntfs3g 解决，但这不优雅，不是吗？此外，一些报告称快速启动会导致网卡故障。所以，最好关闭快速启动。

## 双系统的时间问题

一些报告称 Windows 的时间与 Linux 冲突，在 Windows 上执行 `reg add "HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\TimeZoneInformation" /v RealTimeIsUniversal /d 1 /t REG_DWORD /f` 以使 Windows 使用 UTC 而非默认的 localtime.
