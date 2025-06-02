# 基础配置
在进入新系统后，进行一些基础配置可以帮助你更好地使用系统。以下是一些基本的配置步骤

## 新建用户并配置 sudo
使用 adduser 命令创建新用户，并将其添加到 wheel 组中，以便该用户可以使用 sudo 命令执行管理员任务。

```bash
useradd -m -G wheel newuser # 创建新用户并添加到 wheel 组
passwd newuser # 设置新用户密码
```
如果使用 sudo 命令，需要安装 sudo 包并编辑 sudoers 文件以允许 wheel 组的用户使用 sudo。

```bash
pacman -S sudo # 安装 sudo
EDITOR=nano visudo # 编辑 sudoers 文件，不要总是使用 EDITOR 指定编辑器，这样不安全
```
在 sudoers 文件中，取消注释以下行以允许 wheel 组的用户使用 sudo：

```bash
%wheel ALL=(ALL) ALL
```

## 设置网络

我们使用 systemd-networkd 和 systemd-resolved 来管理网络。首先，启用这两个服务：

```bash
systemctl enable systemd-networkd.service
systemctl enable systemd-resolved.service
```
然后，创建一个网络配置文件 `/etc/systemd/network/20-wired.network`，内容如下：

```ini
[Match]
Name=enp0s3 # 替换为你的网络接口名称
[Network]
DHCP=yes
```
网络接口名称可以通过运行 `ip link` 命令来查看。

将 systemd-resolved 的符号链接设置为 `/etc/resolv.conf`：

```bash
ln -sf /run/systemd/resolve/resolv.conf /etc/resolv.conf
```

重启网络服务以应用更改：

```bash
systemctl restart systemd-networkd.service
```

如果是无线网络，可以使用 `wpa_supplicant` 来连接 Wi-Fi。首先，安装 `wpa_supplicant`：

```bash
pacman -S wpa_supplicant # 这应该已经安装了，如果没有，可以回到 Live 中重新安装
```
然后，创建一个配置文件 `/etc/wpa_supplicant/wpa_supplicant.conf`，内容如下：

```ini
ctrl_interface=/run/wpa_supplicant
update_config=1
network={
    ssid="your_wifi_ssid" # 替换为你的 Wi-Fi SSID
    psk="your_wifi_password" # 替换为你的 Wi-Fi 密码
}
```
然后，启动 `wpa_supplicant` 服务：

```bash
systemctl enable wpa_supplicant.service
systemctl start wpa_supplicant.service
```
## 安装窗口管理器
推荐使用 Hyprland 作为窗口管理器。对于 Hyprland 的安装和配置，请参考 [Hyprland 官方文档](https://wiki.hyprland.org/) 和 [Arch Wiki Hyprland 页面](https://wiki.archlinuxcn.org/title/Hyprland)。

如果你不想从头配置 Hyprland，可以使用现有的 dotfiles 项目，目前，比较推荐的是 [ml4w-hyprland](https://www.ml4w.com/) 和 [End-4 Hyprland](https://github.com/end-4/dots-hyprland)。注意，ml4w-hyprland 应该使用滚动更新版本，否则系统更新可能会破坏配置。

以下是 End-4 Hyprland 的安装步骤，仅供参考，不保证可用：

```bash
bash <(curl -s "https://end-4.github.io/dots-hyprland-wiki/setup.sh")
```
按照脚本的提示进行操作，完成后重启系统即可。

对于 Hyprland 的中文化，在 Hyprland 的配置文件中添加以下内容：

```ini
# ~/.config/hypr/hyprland.conf
# 设置语言环境

env = LANG,zh_CN.UTF-8
exec-once=dbus-update-activation-environment --systemd LANG
```
!!! warning
    Hyprland 不需要 `env = GTK_IM_MODULE,fcitx
    env = QT_IM_MODULE,fcitx
    env = XMODIFIERS,@im=fcitx
    `，因为 Hyprland 使用 Wayland，而 Fcitx5 在 Wayland 下不需要这些环境变量。

!!! danger
    请审查安装脚本，尽管 shell 脚本可读性极差，必须保证知道脚本做了什么，才有可能恢复更改或修复问题。尽量不要使用来路不明的脚本，原因见下一节。

## Btrfs 快照

如果你使用 Btrfs 文件系统，可以使用 `snapper` 来管理快照。

```bash
pacman -S snapper
snapper -c config create-config /path/to/subvolume
```

> 这将会：
> 根据 /usr/share/snapper/config-templates/default 处的默认配置模板创建一个配置文件 /etc/snapper/configs/config。
> 
> 在 /path/to/subvolume/.snapshots 处创建一个子卷，用于存储未来该配置文件产生的子卷。子卷的路径将会是 /path/to/subvolume/.snapshots/#/snapshot，# 是子卷序号。

可以按需设置自动快照，具体参阅 [Snapper 的 Arch Wiki(https://wiki.archlinuxcn.org/wiki/Snapper#%E8%87%AA%E5%8A%A8%E6%8C%89%E6%97%B6%E5%88%9B%E5%BB%BA%E5%BF%AB%E7%85%A7)。请在重大更改前手动创建快照，以便在出现问题时可以恢复。

```bash
snapper -c config create --description "Before major change"
```

## 安装实用工具

安装 paru：
```bash
pacman -S base-devel git
git clone https://aur.archlinux.org/paru.git
cd paru
makepkg -si
```
如果你添加了 archlinuxcn 源，直接安装`paru` 即可。

以下均为推荐，按需安装

安装 Chrome：
```bash
paru -S google-chrome
```
更流行的选择是 Firefox，Arch Linux 的哲学中并不包括必须开源的意识形态，你也可以选择 Chroium。

安装 VsCode：
```bash
paru -S visual-studio-code-bin
```

可以选择 Arch Linux 开源版本`code`，但它不支持 Microsoft 的插件。也可以选择社区驱动的`
vscodium`，它是 VsCode 的开源版本。

安装中文字体：
```bash
paru -S wqy-zenhei noto-fonts-cjk noto-fonts-emoji
```

## Fcitx5

安装 Fcitx5 输入法框架：
```bash
paru -S fcitx5 fcitx5-chinese-addons
```

如果是 Hyprland，可以在 Hyprland 的配置文件中添加以下内容来启用 Fcitx5：

```ini
# ~/.config/hypr/hyprland.conf
exec-once=fcitx5
```

Chrome 需要启用 flag：`Preferred Ozone platform：Wayland` 和 ` Wayland text-input-v3：eEnabled`。

linuxqq 请编辑 `/usr/bin/linuxqq` ，将最后一行改为`exec /opt/QQ/qq ${QQ_USER_FLAGS[@]} --ozone-platform-hint=auto --enable-wayland-ime "$@"`

!!! tip
    hyprland 设置环境变量不需要 `/etc/environment`，可以直接在 Hyprland 的配置文件中设置。

