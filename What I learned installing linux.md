---
id: What I learned installing linux
aliases: []
tags: []
---
Im installing [[Linux]] Mint on my mom's old laptop, im gonna write here all my learnings
gpt's guidelines:
| Step | Action                                      |
| ---- | ------------------------------------------- |
| 1    | Backup your Windows system                  |
| 2    | Download & verify Linux Mint ISO            |
| 3    | Create bootable USB with Linux Mint         |
| 4    | Shrink Windows partition to free space      |
| 5    | Boot laptop from USB and enter live session |
| 6    | Run installer → manual partitioning         |
| 7    | Install Mint and GRUB bootloader            |
| 8    | Reboot and test booting into both systems   |
| 9    | Install drivers and updates in Mint         |
| 10   | Revert to Windows if needed                 |


Making a backup of the windows system
    made it in disk D: because it was recommended (couldnt because i have no cds to spare, D is external disk)

download the iso and check the hash:
ccf482436df954c0ad6d41123a49fde79352ca71f7a684a97d5e0a0c39d7f39f *linuxmint-22.1-cinnamon-64bit.iso
d286306d0f40bd7268f08c523ece5fba87c0369a27a72465a19447e3606c5fa0 *linuxmint-22.1-mate-64bit.iso
6451496af35e6855ffe1454f061993ea9cb884d2b4bc8bf17e7d5925ae2ae86d *linuxmint-22.1-xfce-64bit.iso
```bash
> sha256sum linuxmint-22.1-xfce-64bit.iso
6451496af35e6855ffe1454f061993ea9cb884d2b4bc8bf17e7d5925ae2ae86d  linuxmint-22.1-xfce-64bit.iso
```
Great success! :)

Create partition with win + R and right click on disk (C:) I had 400 free but windows only let me do 200 for linux, whatever 

lsblk lists the storage devices
sda is the hard disk
sdb my usb
```bash
> sudo dd if=~/Downloads/linuxmint-22.1-xfce-64bit.iso of=/dev/sdb bs=4M status=progress conv=fdatasync
```
dd if is input file is the downloaded iso, of output file is the usb drive the rest comes from gpt


sudo eject /dev/sdb to safely take it out

go into bios in the new pc, 

Do not fear the kernel with the `[ OK ]` stuff or the 48 records in and out, nothing broke down 

install linux mint icon, install along side windows great success! :)


