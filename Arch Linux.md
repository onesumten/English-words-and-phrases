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
