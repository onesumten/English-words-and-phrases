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
* /mnt: 
* /boot:
* /home:
* /etc

# Expand your font size of your screen
```
setfont -d
```
* setfont: the command of set font
* -d: double your font size

# Creat a Swap File instead of Swap partition
## 创建交换文件 (分配空间)
* 我们创建一个 8GB 的文件。在终端输入：
```
dd if=/dev/zero of=/swapfile bs=1M count=8192 status=progress
```
* if=/dev/zero: 读取零填充。
* of=/swapfile: 在根目录下创建一个名为 swapfile 的文件。
* bs=1M count=8192: 1M * 8192 = 8GB。
## 设置权限 (安全保护)
* 交换文件记录的是内存里的数据，必须严格限制权限，否则会有安全隐患：
```
chmod 600 /swapfile
```
* 这确保了只有 root 用户可以读写这个文件。
## 格式化为交换格式
* 告诉系统这个文件不是普通文本，而是交换空间：
```
mkswap /swapfile
```
## 启用交换文件
* 让系统立刻开始使用它：
```
swapon /swapfile
```
## 永久挂载 (最关键的一步)
* 如果不把这一行写进 fstab，重启后 Swap 就消失了
  * 打开配置文件：
  ```
  nano /etc/fstab
  ```
  * 在文件的最后一行，添加以下内容：
  ```
  /swapfile none swap defaults 0 0
  ```
## 验证
* 输入以下命令，你应该能看到一行关于 /swapfile 的记录：
```
swapon --show
```
* 或者输入 free -h，看 Swap 那一行是否显示为 8.0G。