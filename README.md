# How to Use Bmap Tool to speed image creation

## Introduction
Bmaptool is a samsung provided way to more efficiently burn images to cards by omitting the necessity of copying empty blocks to flash media. It can speed up flashing by an order of magnitude. It does have its peculiatiaries though. 

### Pre Process
Images need to be scanned in uncompressed format first, in order to create a 'bmap' xml file which is used in the image burning process. The scanning process looks for 'holes' in the filesystem and notes this in the XML file. Once this is done, the image can be compressed and burned from a compressed image.

The command to create a bitmap is as follows (on a RAW image only!!!)

    bmaptool create -o <image.bmap> <imagetoscan.raw>
    
### Post Process

The post process is quite simple and keep in mind that images can burned from 'compressed' masters, greatly saving time and space (since the system doesn't have to read all the sectors from the disk). 

Assuming you have performed the above process, to burn an image to a raw device of /dev/sdb type in the following:

    sudo bmaptool copy --bmap <image.bmap> <imagetoburn.img.xz> /dev/sdb
    
The system will copy the image to disk in about 1/10th the time of dd (depending on how much of the image is sparse). It also checksums the process so any failures will be reported prior to boot on the system.

Time to copy a 6GB image to a samsung uhs-1 sd card 16gb was about 5 minutes. Normal time with dd is about 10-20 minutes. Plus I don't have to uncompress the original with bmaptool.