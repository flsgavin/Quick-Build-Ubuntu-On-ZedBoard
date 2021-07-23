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

    - `git clone https://github.com/flsgavin/Quick-Build-Ubuntu-On-ZedBoard`

2. Make SD card and adjust file and directory structure
- Insert the SD card into the PC and check the mount node

    - `dmesg | tail -n 50` 

    The last part of the command output will indicate the node where the SD card is mounted, as shown in the figure, **sdb** is mounted

    **Note:** The following "sdb" should be replaced with your mount node (e.g. sda sdc..).
- Unmount SD card 
    - `sudo umount /dev/sdb1`

- Erase the first sector of SD card 
    - `sudo dd if=/dev/zero of=/dev/sdb bs=1024 count=1`

- Create SD card partition 
    - `sudo fdisk /dev/sdb`
    - `n`

    n: Create a new partition **1**, enter **+100M** in the *Last sector*, and the others keep the default.
    - `n`

    n: Create a new partition **2** and keep all defaults.
    - `w`

    w: Write partition table, save and exit.

3. Format partition

    - `sudo mkfs.vfat -F 32 -n boot /dev/sdb1`
    - `sudo mkfs.ext4 -L root /dev/sdb2`

4. Mount SD card

    - `sudo mkdir -p /media/ubuntu/boot`
    - `sudo mount /dev/sdb1 /media/ubuntu/boot`
    - `sudo mkdir -p /media/ubuntu/root`
    - `sudo mount /dev/sdb2 /media/ubuntu/root`

       **Note:** "ubuntu" here should be replaced with your user name.

5. Copy boot file to SD card boot partition

    - `cd $THIS_PROJECT_PATH/boot`
    - `sudo cp ./* /media/ubuntu/boot`

6. Build Ubuntu root file system

    - `cd $THIS_PROJECT_PATH/root`
    - `wget https://rcn-ee.com/rootfs/eewiki/minfs/ubuntu-18.04.2-minimal-armhf-2019-02-16.tar.xz`
    - `sudo tar -xJvf ubuntu-18.04-2-minimal-armhf-2019-02-16.tar.xz`
    - `cd ubuntu-18.04-2-minimal-armhf-2019-02-16`
    - `sudo tar xvf armhf-rootfs-ubuntu-bionic.tar`
    - `cd armhf-rootfs-ubuntu-bionic`
    - `sudo rsync -av ./ /media/ubuntu/root`

7. Startup Ubuntu on your ZedBoard

    - Insert SD card into ZedBoard
    - Connecting ZedBoard UART to PC
    - Poweron your ZedBoard
    - Open the serial software and set the baud rate to 115200
    - When When serial output:

        `Ubuntu 18.04.2 LTS arm ttyPS0` <br/> 
        `default username:password is [ubuntu:temppwd]`<br/> 
        `arm login:`<br/> 
        <br/>
    Ubuntu boot successfully!
    
