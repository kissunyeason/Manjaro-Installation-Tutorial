# Manjaro 安装与优化指南

## 一、安装准备

### 1. 下载 Manjaro 安装包

* **下载地址**：[Manjaro 官方下载页面](https://manjaro.org/products/download/x86)

* **选择版本**：根据需求选择桌面环境（如 KDE、GNOME、XFCE 等）

* **提示**：下载后记录文件名和大小，便于后续验证。

### 2. 准备刻录工具

* **推荐工具**：[Ventoy](https://www.ventoy.net/cn/index.html)

* **优点**：开源免费，支持多个 ISO 文件共存，方便测试不同系统。

* **下载方式**：访问官网，选择适合你操作系统的版本（Windows/Linux）。

### 3. 磁盘分区

* **操作环境**：Windows 系统。

* **步骤**：

   * 右键点击 **Windows 键** -> 选择 **磁盘管理**。

   * 选中一个分区 -> 右键 -> **压缩卷**。

   * 输入压缩大小（建议至少 400GB），留出未分配空间。

* **建议**：

   * Manjaro 根分区 (`/`) 至少需要 50GB。

##    - 若计划长期使用，可额外为 `/home` 分区预留空间。

## 二、刻录启动 U 盘

### 1. 使用 Ventoy 刻录 U 盘

1. **安装 Ventoy**：

   1. 插入 U 盘，打开 Ventoy 软件。

   2. 选择你的 U 盘，点击 **安装**，等待约 10 分钟完成。

3. **添加镜像**：

   1. 将下载的 Manjaro ISO 文件直接复制到 U 盘的 Ventoy 分区。

4. **验证文件完整性**：

   1. 使用工具检查 ISO 文件的 MD5 哈希值，确保未损坏。

      1. Windows：推荐 [WinMD5](https://www.winmd5.com/)。

      2. Linux：运行 ``md5sum 文件名.iso``。

## - **提示**：若文件损坏，可能导致安装失败，务必验证。

## 三、系统安装

### 1. 启动设置

1. **进入 BIOS/UEFI**：

   1. 插入 U 盘，重启电脑。

   2. 按键进入 BIOS（常见按键：F2、F12、Del 或 Esc，依主板型号而定）。

   3. 关闭 **安全启动（Secure Boot）**。

4. **选择启动设备**：

   1. 保存设置并重启，按 F12（或类似键）选择 U 盘启动。

5. **配置启动选项**：

   1. 在 Manjaro 启动菜单中选择：

      1. 地区（如中国）

      2. 语言（如中文）

      3. 键盘布局（如默认 US 或中文）

      4. 驱动类型（建议选择 **开源驱动**，兼容性更好）。

6. **进入 Live 系统**：

   1. 进入后，点击桌面上的 **“安装 Manjaro”**。

### 2. 分区配置

1. **选择手动分区**：

   1. 在安装向导中选择 **“手动分区”**。

2. **设置分区**：

   1. **根分区（**`/`**）**：

      1. 选中未分配空间，新建分区。

      2. 文件系统：`btrfs`（推荐，性能优于 `ext4`）。

      3. 大小：至少 50GB。

   2. **启动分区（**`/boot/efi`**）**：

      1. 选择 Windows 的 EFI 分区（通常为 100-500MB 的 FAT32 分区）。

      2. 挂载点设为 `/boot/efi`。

   3. **可选：**`/home`**分区**：

      1. 若需要分离数据和系统，可新建分区并挂载到 `/home`。

3. **完成安装**：

   1. 按提示继续，等待安装完成。

##    - 重启电脑并拔出 U 盘。

## 四、系统前期设置

### 1. 基础配置

* **修复双系统时间不同步**：

```bash
sudo timedatectl set-local-rtc 1
```
* **更新系统**：

```bash
sudo pacman -Syyu
```
* **说明**：确保系统为最新版本，避免兼容性问题。

### 2. 国内镜像加速（可选）

* **加速软件源**：

```bash
sudo pacman-mirrors -c China
```
```bash
sudo pacman -Syyu
```
* **好处**：使用中国镜像源，下载速度更快。

### 3. 输入法安装

* **安装rime**：

```bash
sudo pacman -S ibus-rime
```
* **配置步骤**：

   * 注销并重新登录。

   * 打开 **设置** -> **键盘** -> **输入源**。

   * 添加 **“汉语(中国)”** -> 选择 **“中文(智能拼音)”**。

* **可选：雾凇输入法插件**：

```bash
sudo pacman -S ruby
```
```bash
git clone --depth=1 https://github.com/Mark24Code/rime-auto-deploy.git --branch latest
```
```bash
cd rime-auto-deploy
```
```bash
./installer.rb
```
* **提示**：雾凇适合需要高级输入法功能的用户。

### 4. 软件源扩展

* **安装 AUR 管理器**：

```bash
sudo pacman -S yay base-devel git
```
* **配置 Flatpak**：

```bash
sudo pacman -S flatpak
sudo flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
flatpak update
```
* **安装 Flatpak 插件**：

```bash
sudo pacman -S gnome-software-plugin-flatpak
```
(如果找不到该软件包，请在应用商店的设置中检查是否已开启第三方或 Flatpak 支持)
* **安装 Flatpak 管理工具**：

```bash
flatpak install flathub io.github.flattool.Warehouse ## 管理器
```
```bash
flatpak install flathub com.github.tchx84.Flatseal ## 权限管理
```
* **建议**：新手优先使用 Flatpak 安装软件，依赖管理更简单。

### 5. 调整分辨率

```bash
gsettings set org.gnome.mutter experimental-features "['scale-monitor-framebuffer']" ## 开启分数缩放
```
```bash
gsettings set org.gnome.mutter experimental-features "['scale-monitor-framebuffer', 'xwayland-native-scaling']" ## 解决高分屏字体模糊的问题
```
### 6.dock点击最小化

```bash
gsettings set org.gnome.shell.extensions.dash-to-dock click-action 'minimize'
```
### 7.面部识别登录

* 安装howdy-beta

```bash
yay -S howdy-beta-git ## 安装测试版本
```
* 修改配置

```bash
sudo howdy config
```
```bash
device_path = /dev/video2 ## 这里需要找到自己红外线摄像头的编号
```
* 添加面部数据

```bash
sudo howdy add
```
* 测试

```bash
sudo howdy test ## 测试是否能识别
```
* 添加howdy验证到系统

```bash
sudo nano /etc/pam.d/gdm-password ## 这里只添加了gdm的pam验证，root和其余的可以自己添加
```
##  

## 五、常用软件安装

以下软件按功能分类，涵盖办公、娱乐和开发需求。

### 办公与生产力

1. **WPS Office（办公套件）**

```bash
yay -S wps-office-365
```
2. **Xournal++（手写笔记）**

```bash
yay -S xournalpp
```
3. **Apostrophe（Markdown 编辑器）**

```bash
flatpak install flathub org.gnome.gitlab.somas.Apostrophe
```
4. **Evolution（邮件/日历管理）**

```bash
flatpak install flathub org.gnome.Evolution
```
5. **Endeavour（待办事项）**

```bash
flatpak install flathub org.gnome.Todo
```
### 网络与通讯

1. **Clash Verge Rev（网络代理）**

```bash
yay -S clash-verge-rev-bin
```
2. **Google Chrome（浏览器）**

```bash
flatpak install flathub com.google.Chrome
```
3. **Materialgram（Telegram 客户端）**

```bash
flatpak install flathub io.github.kukuruzka165.materialgram
```
4. **WeChat（微信）**

```bash
flatpak install flathub com.tencent.WeChat
```
5. **WeWork（企业微信）**

   1. **步骤 1**：安装 Bottles (Wine 容器管理器)

```bash
flatpak install flathub com.usebottles.bottles
```
* **步骤 2**：配置 Bottles 并安装企业微信

   * 打开 Bottles。

   * 在 Bottles 的设置中，找到运行环境 (Runners) 部分，下载并选择一个合适的运行环境，如 `Proton GE` 或最新的 `Wine GE`。

   * （可选）在 Bottles 中为企业微信创建的 Bottle 的设置中，进入“首选项”或“显示设置”，可以尝试关闭“窗口装饰”以获得更好的视觉效果。

   * 在 Bottle 的依赖项 (Dependencies) 设置中，安装中文字体 (`allfonts`, `cjkfonts` 等)。

   * 使用此配置好的 Bottle 来安装和运行企业微信 `.exe` 安装包。

6. **FileZilla（FTP 客户端）**

```bash
yay -S filezilla
```
### 娱乐

1. **OSD Lyrics（歌词显示）**

```bash
yay -S osdlyrics
```
2. **Spotify（音乐流媒体）**

```bash
flatpak install flathub com.spotify.Client
```
3. **无暇 (G4Music)（简洁音乐播放器）**

```bash
flatpak install flathub com.github.neithern.g4music
```
4. **Steam（游戏平台）**

```bash
yay -S steam
```
5. **Play Timer (MPRIS Timer)（音乐计时器）**

```bash
flatpak install flathub io.github.efogdev.mpris-timer
```
### 系统工具

1. **Timeshift（系统备份）**

```bash
yay -S timeshift
```
2. **Resources（资源监控）**

```bash
flatpak install flathub net.nokyan.Resources
```
3. **GDM Settings（显示管理器配置）**

```bash
flatpak install flathub io.github.realmazharhussain.GdmSettings
```
4. **扩展管理器 (Extension Manager)（GNOME 扩展管理）**

```bash
flatpak install flathub com.mattjakeman.ExtensionManager
```
5. **Ignition（启动项管理）**

```bash
flatpak install flathub io.github.flattool.Ignition
```
6. **BleachBit（系统垃圾清理）**

```bash
flatpak install flathub org.bleachbit.BleachBit
```
### 多媒体

1. **Easy Effects（音频处理）**

```bash
flatpak install flathub com.github.wwmm.easyeffects
```
2. **Shotwell（照片管理）**

```bash
flatpak install flathub org.gnome.Shotwell
```
3. **图像查看器 (Loupe)（图片浏览）**

```bash
flatpak install flathub org.gnome.Loupe
```
4. **Transmission（轻量级 BitTorrent 下载客户端）**

```bash
yay -S transmission-gtk
```
### 开发与教育

1. **Alpaca（AI聚合）**

```bash
flatpak install flathub com.jeffser.Alpaca
```
2. **GeoGebra（数学工具）**

```bash
flatpak install flathub org.geogebra.GeoGebra
```
### 其他实用工具

1. **Pins（软件图标编辑）**

```bash
flatpak install flathub io.github.fabrialberio.pinapp
```


## 六、补充配置

### 1. GNOME 扩展

* **Customize IBus**：

   * **下载地址**：[GNOME Extensions](https://extensions.gnome.org/extension/4112/customize-ibus/)

   * **功能**：自定义输入法界面和行为。

### 2. 硬件支持

* **数位板驱动**：

   * **配置指南**：[libwacom 添加设备](https://github.com/linuxwacom/libwacom/wiki/Adding-a-new-device)

   * **适用**：Wacom 等数位板用户。

### 3. 软件仓库

* **Flatpak 应用商店**：

   * **地址**：[Flathub](https://flathub.org/zh-Hans)

##    - **说明**：提供丰富的 Flatpak 软件资源。

