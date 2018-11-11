# How to: Bootcamp Windows on an external SSD

This tutorial intends to show the steps to perform a clean install of Windows on an external drive. The obvious advantage of doing this is that you don't use up that precious and limited SSD space on your Mac.

There is some material out there, but sadly not one guide worked 100% on its own for me. Still, they pointed in the right direction and got me to figure a successful method. And as I recently managed to recreate my own process, I decided to write this guide for anyone wanting to do the same.



## 0. Requirements
- A SSD drive (du'h)
- Download [Windows 10 ISO](https://www.microsoft.com/en-us/software-download/windows10ISO) (64 bits)
- Download and install [Virtual Box OSX](virtualbox.org) (I used 5.2.6)
- Access to a Windows 10 Machine **OR** create a Windows 10 temporary Virtual Machine
- Download the **Windows Support Software** (see below)
- External basic USB Keyboard and Mouse
- A 2 Gb+ USB Drive -and/or- a software to write on NTFS disks (personally, I use [Tuxera NTFS](https://www.tuxera.com/products/tuxera-ntfs-for-mac/))

The **Windows Support Software** is a set of Boot Camp drivers for your specific Macbook hardware. To download this open the *Boot Camp Assistant* app, go to  `Action > Download Windows Support Software`. Save that folder on an external USB Drive, as you'll need it later.

This software will let you update the drivers and control the settings of you Macbook hardware, such as trackpad behaviour and gestures, support for the keyboard backlight, etc.

![Boot Camp Windows Support Software menu](/imgs/00_winsupportsw.png)




## 1. Preparing you SSD
In order to get your SSD ready, we need to erase its content and create the a bootable partition plus another partition where to install Windows.

**The following steps have to be performed on a Windows machine (or Virtual Machine).**

1. On the Start menu, look for the Command Prompt (or type `cmd.exe`), right click, and select "Run as Administrator"
2. Open the [DiskPart tool](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-vista/cc766465(v=ws.10)) by typing `diskpart` and perform the following commands:
	- `list disk`: to list and identify the number of the target disk
	- `select disk #`: to select it
	- `clean`: to erase its content
	- `convert MBR`:	to create the [Master Boot Record](https://technet.microsoft.com/en-us/library/cc976786.aspx), which sets the data structure of the disk
	- `create partition primary size=350`: to create the boot partition
	- `format fs=FAT32 quick`: to perform a quick format with the [FAT32](https://en.wikipedia.org/wiki/File_Allocation_Table#FAT32) file system
	- `active`: to set it as an active partition
	- `assign letter=b` to assign a letter to the partition (you can whichever you want, I just use 'b' to remember it's the "boot" partition)

	At this point, a new Explorer window will pop-up with the disk unit we just created. Just close it and proceed with the creation of the second partition, where Windows will be installed:
	- `create partition primary`: this time by not specifying the size it uses all the remaining space
	- `format fs=ntfs quick`: to format this time with the [NTFS](https://en.wikipedia.org/wiki/NTFS) file system
	- `assign letter=w` to assign a letter to the partition (again, whichever letter you want)

This is only the first half we have to perform on a Windows machine. After the next step, we'll have to come back one more time to finalize and activate the boot partition.



## 2. Virtually installing to an external drive
There are a number of limitations when it comes to installing Windows, using Boot Camp or even mounting a Virtual Machine on an external drive. Thankfully, there are ways to bypass them and get everything working our way.

`diskutil list`

`diskutil unmount dev/disk#`

`VBoxManage internalcommands createrawvmdk -filename "bootcamp.vmdk" -rawdisk /dev/disk#`


Start Virtual Box with administrator privileges

`sudo /Applications/VirtualBox.app/Contents/MacOS/VirtualBox`

Create a Virtual Machine from the disk file created
![VirtualBox create Virtual Machine]()

Load the Virtual Optical Disk Drive with the Windows 10 ISO file
![VirtualBox load ISO]()

Run the Windows 10 installation
**Don't let Windows restart!**
![video]()




## 3. Let's make it bootable
With Windows almost fully installed, we just need to write the bootable information to the corresponding partition.

**The following steps have to be performed on a Windows machine (or Virtual Machine).**

1. Initialize again the Command Prompt as an Administrator (right click and select "Run as Administrator")
2. Type `w:\windows\system32\bcdboot w:\windows /f ALL /s b:` to create the boot section

	Remember to replace the `w:` and `b:` drive names with the ones you used.

	If this gives you any problems, try removing the `/f ALL` part.

Now your SSD is ready to boot!!! Let's go back and finish the installation.



## 4. Finalize and install drivers
Turn down your Macbook, plug in the SSD and press the option (⌥) key to open the boot menu. With the arrow keys, select your Windows drive (EFI boot) and press Enter.

Windows now will finish installing. It might restart again which might result in booting into OS X, but just shut down and press the option key again to select the Windows drive.

It is worth noting that the built-in keyboard and trackpad might not work, but a simple USB keyboard and mouse will be detected and work without any problem.

After the configuration ends, you'll only need to install the **Windows Support Software**. Plug in the USB drive (alternatively, you can put this folder directly in the SSD drive after step 2, but you'll need an additional software to write in the NTFS drive), extract the contents and follow the instructions.



## 5. Congratulations!
Now you have Windows 10 running on an external SSD!

Some pieces of advice from issues I've experienced:
1. DON'T UNPLUG THE SSD WHILE RUNNING WINDOWS! (this should be obvious, but you can never be too careful)
2. Don't close the lid to make your laptop sleep. It seems that the USB ports get disconnected when you do this, because my computer simply restarts when I open it again.
3. There might be some issues when trying to update Windows, as it's not installed in an UEFI disk. This is Microsoft's fault, adding weird "security" measures which are mostly a nuisance. There are workarounds, but I still haven't gotten to solve them. I'll post about that as soon as I do!



## Acknowledgements and credits
- Thanks to [Hacker Noon](https://hackernoon.com/how-to-run-bootcamp-windows-10-on-a-usb3-86551dc3def8) for the basic steps on mounting the Virtual Machine to install Windows
- And to [BleepToBleep](https://bleeptobleep.blogspot.cl/2013/02/mac-install-windows-7-or-8-on-external.html) for how to create the boot partition
