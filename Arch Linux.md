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

# Expand your font size of your screen
```
setfont -d
```
* setfont: the command of set font
* -d: double your font size