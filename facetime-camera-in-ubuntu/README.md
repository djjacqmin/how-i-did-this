# Fix Facetime HD Camera on Ubuntu Linux
## Problem Statement
I could not use cv2.VideoCapture(0) in python on Ubuntu Linux, and ultimately determined that the system could not see the MacBook Pro iSight camera.

## System Parameters
* Ubuntu 18.04.2 LTS 

## Solution

### Install a Linux driver for the Facetime HD
The first step in being able to resolve this issue is installing a Linux driver for the Facetime HD. A driver can be found here: [bcwc_pcie](https://github.com/patjak/bcwc_pcie). The installation instrutions are found on the wiki for the repository.

Install the pre-requisites:
```
sudo apt update
sudo apt install curl cpio
```
Clone the repository:
```
cd ~/Projects
git clone https://github.com/patjak/bcwc_pcie.git
```

Download and extract firmware, then copy to `/lib/firmware/facetimehd`:
```
cd bcwc_pcie/firmware
make
sudo make install
```

Install additional dependencies:
```
sudo apt-get install linux-headers git kmod libssl-dev libelf-dev checkinstall
```

Build the kernal module:
```
make
```

Generate a dkpg and install the kernel module (this will make it easier to uninstall later):
```
sudo checkinstall
```
Here is the output from that step:
```
**********************************************************************

 Done. The new package has been installed and saved to

 /home/jacqmin/Projects/bcwc_pcie/bcwc-pcie_20190406-1_amd64.deb

 You can remove it from your system anytime using: 

      dpkg -r bcwc-pcie

**********************************************************************
```

Install the kernel module:
```
sudo make install
```

Run depmod for the kernel to be able to find and load it: 
```
sudo depmod 
```

Load kernel module:
```
sudo modprobe facetimehd
```

Try it out with `cheese`
