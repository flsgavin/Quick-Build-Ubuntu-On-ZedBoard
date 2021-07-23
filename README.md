# Quick-Build-Ubuntu-On-ZedBoard
This project provides a quick way to build Ubuntu on the Xilinx ZedBoard.
This project provides compiled boot image. You don't need to compile u-boot and Linux kernel, etc.

## Requirements
- Xilinx ZedBorad 
- SD card ( >4GB )
- A PC (running Ubuntu)

## Description of directory structure
|Directory|Description|
|-|-|
|boot|All necessary kernel boot files|
|root|Ubuntu root file system link|
## Build Steps
1. Download this project
> git clone https://github.com/flsgavin/Quick-Build-Ubuntu-On-ZedBoard

2. Make SD card and adjust file and directory structure
- Insert the SD card into the PC and check the mount node
> dmesg | tail -n 50 

The last part of the command output will indicate the node where the SD card is mounted, as shown in the figure, SDB is mounted

- Unmount SD card
> sudo umount /dev/sdb1

- Erase the first sector of SD card
> sudo dd if=/dev/zero of=/dev/sdb bs=1024 count=1

- Create SD card partition
> sudo fdisk /dev/sdb

> n

n: Create a new partition **1**, enter **+100M** in the *Last sector*, and the others keep the default.

> n

n: Create a new partition **2** and keep all defaults.

> w 

w: Write partition table, save and exit.
