# Pacman Command Logic
| Flag | Full Name | Description | Common use cases
| :-- | :-- | :-- | :--
| -S | Sync | Synchronizes with remote repositories. | Installing new apps, updating the system (-Syu), searching the repo.
| -Q | Query | Searches the local package database. | Listing installed apps, checking if a package exists on your disk.
| -R | Remove | Removes packages from the system. | Uninstalling software and its configuration (if specified).
| -U | Upgrade | Installs a local file or from a URL. | Installing a *.pkg.tar.zst* file you downloaded manually.
## Synchronize (Sync) Modifiers
* -y (refresh): Downloads a fresh copy of the master package database from the server.
* -u (sysupgrade): Upgrades all packages that are out of date.
* -s (search): Searches for packages in the remote repositories.
## Query Modifiers
* -i (info): Shows detailed information about a package (size, install date, description).
* -l (list): Lists all files owned by that package (the command we used to find init).
## Remove Modifiers
* -s (recursive): Removes a package and all its dependencies (as long as they aren't used by other apps).
* -n (nosave): Tells pacman to remove configuration files too (doesn't create .pacsave files).
  
### *Rule of Thumb: If the action involves the Internet/Server, start with -S. If the action involves Your Hard Drive, start with -Q, -R, or -U.*
# Modify the hostname
Whatever this is the first time or the second time to modify the hostname, you can use below code:
```
ehco yourHostName > /etc/hostname
```
* yourHostName: the hostname you want. 
* Furthermore, you need change the information in **/etc/hosts**.
```
127.0.0.1   localhost
::1         localhost
127.0.1.1   yourHostName.localdomain   yourHostName
```
* yourHostName: the hostname you want. 
* Beside, **127.0.0.1** and **::1** are default, just add the third line information which is **127.0.1.1   yourHostName.localdomain   yourHostName**.

# Disk
* lsblk: Showing a list of disk you have now.
* /mnt: The root director(sometimes you will see this as **/**).
* /boot: To power up the computer and load the operating system into this active memory.
* /home: The user directory.
* /etc: Store the configuration of your computer and operating system.

| File or Directory PATH | Function description |
| :--- | :--- |
| `/etc/fstab` | 规定系统启动时如何挂载硬盘分区。 |
| `/etc/passwd` | 存储系统中所有用户的基本信息。 |
| `/etc/pacman.conf` | **Arch Linux 特有**：配置软件包管理器的镜像源和行为。 |
| `/etc/hostname` | 存储你给这台电脑起的名字。 |
| `/etc/sudoers` | 控制哪些用户可以使用 `sudo` 命令获取管理员权限。 |
| `/etc/ssh/` | 存放远程登录 (SSH) 的相关加密密钥和配置。 |
| `/etc/systemd/` | 存放系统服务 (Services) 的启动和管理配置。 |

# Expand your font size of your screen
```
setfont -d
```
* setfont: the command of set font
* -d: double your font size

# Updating Package Databases and Installed Software
```
sudo pacman -Syu
```
* -S(Sync): Tell the package manager(pacman) to synchronize packages from the remote pository.
* -y(refresh): Download a fresh copy of the master package database from the server.
* -u(Sysupgrade): Upgrades all installed packages that have a new version available.

# Creat a Swap File instead of Swap partition
## Create the Swap File(Allocate Space)
* Create an 8GB file by entering the following in the terminal:
```
dd if=/dev/zero of=/swapfile bs=1M count=8192 status=progress
```
* if=/dev/zero: Reads zeroed data (null characters) as the input source.
* of=/swapfile: Creates a file named swapfile in the root directory.
* bs=1M count=8192: Block size of 1M $\times$ 8192 blocks = 8GB.
## Set Permissions(Security Protection)
* Since the swap file records data from the RAM, permissions must be strictly restricted to prevent security risks:
```
chmod 600 /swapfile
```
* This ensures that only the root user has read and write privileges for this file.
## Format as Swap Space
* Tell the system that this file is not a regular text file, but a designated swap area:
```
mkswap /swapfile
```
## Enable the Swap File
* Command the system to start using the swap file immediately:
```
swapon /swapfile
```
## Persistent Mounting(The Critical Step)
* If you do not add this entry to __fstab__, the Swap will disappear after a reboot.
  * Open the configuration file:
  ```
  nano /etc/fstab
  ```
  * Add the following line at the very end of the file:
  ```
  /swapfile none swap defaults 0 0
  ```
## Verification
* Enter the following command; you should see a record for __/swapfile__:
```
swapon --show
```
* Alternatively, run __free -h__ and check if the Swap row displays 8.0Gi.

# Network Service Configuration (Persistence & Activation)
* The following command is used to manage systemd units (services) to ensure networking is available both immediately and across reboots.
```
systemctl enable --now NetworkManager
```
* systemctl: The primary command-line utility for controlling the systemd __init system and service manager__.
  * When you need open or install **Servie** like ssh. You need this. 
  * If you install **Tools** like vim, python. You don't need this. 
* enable: This configures the service to start automatically at boot. Technically, it creates a symbolic link (symlink) from the service file (usually in /usr/lib/systemd/system/) to the location where systemd looks for autostart scripts (usually in /etc/systemd/system/). 
* --now: This is a flag that tells systemd to start the service immediately in the current session. Without this flag, enable would only take effect after the next reboot.
* NetworkManager: The specific daemon (background service) responsible for detecting and configuring network connections (WiFi, Ethernet, VPNs).

# Connecting to Internet via NetworkManager
* Tool: nmtui — A user-friendly, cursor-based interface for managing networks.
```
nmtui
```

# Exporting Package Lists
```
pacman -Qeq > list
```
* -e(Explicit): Filters out dependencies; shows only what you specifically asked to install.
* -q(Quiet): Strips version numbers for a clean list of names.

# Bulk Package Installation
```
pacman -Syu -- < list
```
* --(Double Dash): A universal Linux convention that marks the end of command options. Everything following it is treated as a positional argument (like a filename or package name), preventing accidental misinterpretation of names starting with a hyphen.
* <(Redirection): Redirects the contents of a file into the command's standard input.

# Cleaning Package Cache
```
# Safely remove old, unused package versions (keeps the current one)
sudo pacman -Sc

# Forcefully remove ALL cached packages (nuclear option)
sudo pacman -Scc
```
* Pro Tip: It's good practice to run sudo pacman -Sc once a month to reclaim disk space, especially on smaller SSDs.

# River Environment Components
* River is a **dynamic tiling compositor**(动态平铺合成器) based on the Wayland protocol.
* It manages windows using a **layout generator** and a flexible **tag system** instead of traditional workspaces.

| Component | Software | Purpose
| :--- | :--- | :---
| Terminal | foot | The primary interface for command input.
| Status Bar | waybar | Visual indicators for tags, battery, and clock.
| Launcher | fuzzel | Quick search and launch for GUI applications.
| Auth Agent | polkit-gnome | Handles graphical password prompts for sudo actions.

## Intel Graphics Drivers Installation
* 这条命令是 Linux(特别是 Arch Linux 及其衍生版，如 Manjaro)中用于安装 Intel 显卡驱动和 Vulkan 图形接口支持的指令。简单来说，如果你使用的是 Intel 集成显卡（比如笔记本电脑自带的显卡），这条命令能确保你的系统具备运行 3D 游戏、高清视频加速和现代图形应用的能力。
```
sudo pacman -S mesa lib32-mesa vulkan-intel
```
* mesa: 开源的图形驱动核心。它包含 OpenGL 和 Vulkan 驱动，让显卡能理解绘图指令。
* lib32-mesa: `mesa`的32位版本。主要用于运行旧应用或通过 Steam 运行 32 位游戏。
  ```
  target not found: lib32-mesa
  ```
  * 通常是因为系统尚未启用 multilib 存储库。
  * 进入`nano /etc/pacman.conf`
  * 找到下面的内容
  ```
  #[multilib]
  #Include = /etc/pacman.d/mirrorlist
  ```
  * 去掉`#`这个注释
  * 更改配置文件后，必须让系统重新读取远程仓库的索引
  ```
  pacman -Syu
  ```
* vulkan-intel: Intel 显卡的 Vulkan 实现。Vulkan 是比 OpenGL 更现代、更高效的绘图接口。

## Locating Files within a Package
```
pacman -Ql river | grep init
```
* -Q (Query): Operates on the local package database.
* -l (List): Lists all files owned by the specified package.
* | (Pipe): Redirects the output to the next command.
* grep [keyword]: Filters the list to show only lines containing the keyword.
* -Ql 是“地毯式搜索”：它会列出包在系统里留下的每一个脚印（包括二进制文件、库文件、文档、协议等）。
* 配合 grep 是“精准定位”：
  * 如果你想找 River 的手册页在哪里：`pacman -Ql river | grep man`
  * 如果你想找 River 的可执行文件在哪里：`pacman -Ql river | grep bin`
  * 如果你想找 River 的默认配置示例：`pacman -Ql river | grep examples`

## Execute the init file

```
chmod +x ~/.config/river/init
```
* When you modify the init file at `~/.config/river/init`, you must use the above command. 
* In river, the init file is executed as a shell script, not just read as a static config file. If the file lacks executable permissions, river will fail to load your keybindings, leaving you unable to interact with the compositor.


