# 令人头疼的英伟达

并不推荐使用英伟达的显卡，因为英伟达的驱动程序和 CUDA 工具包在 Linux 上经常会出现兼容性问题，导致无法正常使用。

## 安装驱动

安装驱动：
```shell
pacman -S nvidia-dkms nvidia-utils
```

对于笔记本双显卡用户，还需要安装 `nvidia-prime` 或 `optimus-manager`，以便切换显卡。

## 配置内核模块

编辑 `/etc/mkinitcpio.conf`，在 `MODULES` 一行添加：
```
MODULES=(nvidia nvidia_modeset nvidia_uvm nvidia_drm)
```
然后重新生成 initramfs：
```shell
mkinitcpio -P
```

## 启用 DRM 模式设置（可选）

为获得更好的 Wayland 支持和无撕裂体验，可以启用 DRM 模式设置。在 `/etc/modprobe.d/nvidia.conf` 添加：
```
options nvidia-drm modeset=1
```

## 更新 initramfs 并重启

```shell
mkinitcpio -P
reboot
```

## 检查驱动是否加载

重启后，使用以下命令检查驱动是否正常加载：
```shell
nvidia-smi
```
如果能看到显卡信息，说明驱动安装成功。

## 后日谈

更老的显卡，详细的安装步骤，常见问题排查，参考 [Arch Wiki NVIDIA](https://wiki.archlinuxcn.org/title/NVIDIA)。

!!! warning
    每次内核升级后，务必确保 nvidia-dkms 已重新编译，否则可能导致无法进入图形界面。