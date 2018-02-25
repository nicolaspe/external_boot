
## Resources
To create this guide, I followed the steps from these sites. Sadly, only by following one of them, I could not get this done. Each was missing some key pieces.
- https://hackernoon.com/how-to-run-bootcamp-windows-10-on-a-usb3-86551dc3def8
- https://bleeptobleep.blogspot.cl/2013/02/mac-install-windows-7-or-8-on-external.html
- https://discussions.apple.com/thread/5431182?tstart=0



## Steps

### 1. Preliminary downloads
- Download Windows 10 (64 bits) iso
- Download and install [Virtual Box OSX](virtualbox.org) (used 5.2.6)


### 2. OS Virtualization

#### Identify your USB Drive’s device location
On the terminal, type
```bash
$: diskutil list
```
... COPY


#### Bootable partition
http://www.theinstructional.com/guides/disk-management-from-the-command-line-part-2



Disconnect from MacOSX
Create a Virtual Disk mapping to USB Drive
```bash
$: diskutil eject /dev/disk2
$: sudo VBoxManage internalcommands createrawvmdk -filename "bootcamp.vmdk" -rawdisk /dev/diskX
$: sudo /Applications/VirtualBox.app/Contents/MacOS/VirtualBox
```

Unmount again


**install**

Prepare Windows Support Utilities in a USB Disk

### 3.


It will reload A TON of times
