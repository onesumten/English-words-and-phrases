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

| 文件或目录路径 | 功能描述 |
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
