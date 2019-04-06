# Fix iSight Camera on Ubuntu Linux
## Problem Statement
I could not use cv2.VideoCapture(0) in python on Ubuntu Linux, and ultimately determined that the system could not see the MacBook Pro iSight camera.

## System Parameters
* Ubuntu 18.04.2 LTS 

## Solution

### Install APFS FUSE Driver for Linux
The first step in being able to resolve this issue is being able to mount the Apple hard drive in linux so that we can access the camera driver. Newer Macs use the APFS file system. The [APFS FUSE Driver for Linux](https://github.com/sgan81/apfs-fuse) will allow us to mount the Apple hard drive.

Install the pre-requisites:
```
sudo apt update
sudo apt install fuse libfuse-dev libicu-dev bzip2 libbz2-dev cmake gcc git libattr1-dev
```
Clone the repository:
```
cd ~/Projects
git clone https://github.com/sgan81/apfs-fuse.git
cd apfs-fuse
git submodule init
git submodule update
```

Compile the driver:
```
mkdir build
cd build
cmake ..
make
```

The binaries are located in the `build` directory. Running `apfsutil` on `/dev/sda2` yields:
```
jacqmin@dustin-MacBookPro:~/Projects/apfs-fuse/build$ sudo ./apfsutil /dev/sda2
Volume 0 07A4BFCC-6667-3DC3-8265-67F519E2669F
---------------------------------------------
Role:               No specific role
Name:               Macintosh HD (Case-insensitive)
Capacity Consumed:  147889016832 Bytes
FileVault:          Yes

Volume 1 99717721-0B71-444B-A9FC-94651EF68E32
---------------------------------------------
Role:               Preboot
Name:               Preboot (Case-insensitive)
Capacity Consumed:  45105152 Bytes
FileVault:          No

Volume 2 2BF7AF84-334B-4E34-8F89-C0606FAECC48
---------------------------------------------
Role:               Recovery
Name:               Recovery (Case-insensitive)
Capacity Consumed:  516956160 Bytes
FileVault:          No

Volume 3 90E3F4B7-912E-4DEA-9D2B-85D3DA3CD649
---------------------------------------------
Role:               VM
Name:               VM (Case-insensitive)
Capacity Consumed:  3221274624 Bytes
FileVault:          No

```

## Mount the Mac hard drive


