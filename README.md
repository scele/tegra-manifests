# How to get started with nouveau on Jetson-TK1

Adapted from:

* http://elinux.org/Tegra/Mainline_SW/Nouveau
* https://raw.github.com/NVIDIA/tegra-uboot-flasher-scripts/master/README-developer.txt

Works on my Ubuntu 12.04.

```
sudo apt-get install build-essential autoconf automake libtool libusb-1.0-0-dev libcrypto++-dev flex bison g++-arm-linux-gnueabihf libwayland0 libwayland-dev

mkdir jetson-nouveau
cd jetson-nouveau

export TOP=$PWD
export CROSS_COMPILE=arm-linux-gnueabihf-
repo init -u git@github.com:scele/tegra-manifests.git --manifest-name=nouveau.xml
repo sync -j4

# Flash uboot
./uboot-flasher/scripts/build-tools
./uboot-flasher/scripts/build build
./uboot-flasher/scripts/tegra-uboot-flasher flash jetson-tk1
./uboot-flasher/scripts/tegra-uboot-flasher flash jetson-tk1 # yes, I need to run it twice, the first run fails :/

# Build linux
./scripts/build-linux

# Build nouveau
./scripts/build-nouveau

# In u-boot console, mount the root filesystem into the host machine
ums 0 mmc 0

# Install the upstream kernel image, dtb, firmware, and the nouveau module
export BOARD_ROOT=/mnt/your_mounted_emmc
./scripts/install-linux
./scripts/install-firmware
./scripts/install-nouveau
```
