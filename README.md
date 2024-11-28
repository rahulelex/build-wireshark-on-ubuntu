# Build wireshark on Ubuntu

## Wireshark Setup Guide

This guide walks you through the process of setting up Wireshark from source on a Linux-based system. The steps include cloning the repository, installing necessary dependencies, building, and running Wireshark.

**Tested on Ubuntu 22.04 LTS**

## Prerequisites

Make sure you have the following tools installed before starting:
- Git
- CMake
- Ninja (for faster builds)
- Qt5 (since we're not using Qt6)
- Build-essential packages for compiling

You will also need `sudo` privileges to install dependencies and configure certain permissions for the `dumpcap` binary.

## Steps to Build and Install Wireshark

### 1. Clone the Wireshark repository

Clone the Wireshark repository with a depth of 5000 commits to minimize the size of the clone:

```bash
git clone https://gitlab.com/wireshark/wireshark.git --depth=5000
```

### 2. Install dependencies for building
Navigate to the tools directory and install the necessary dependencies for Qt5:
```bash
cd wireshark/tools
sudo ./debian-setup.sh --install-qt5-deps
```

### 3. Prepare the build directory
Create a build directory and navigate into it:
```bash
cd ..
mkdir build && cd build
```
### 4. Configure the build with CMake
Run CMake to configure the build process. We disable Qt6 support and specify the installation path:
```bash
sudo cmake -GNinja -DUSE_qt6=OFF -DCMAKE_INSTALL_PREFIX=$PWD/.. ..
```
### 5. Configure additional settings (optional)
You can configure additional build settings using ccmake:
```bash
sudo ccmake .
```
In the configuration menu, ensure the following option is enabled:

- ENABLE_CCACHE set to YES for caching purposes.
### 6. Build Wireshark
Once configured, you can start the build process with Ninja:
```bash
sudo ninja
```

### 7. Install Wireshark
Install Wireshark to the specified location:
```bash
sudo ninja install
```
### 8. Set capabilities for dumpcap
Grant dumpcap the necessary permissions to capture network traffic:
```bash
cd ../bin
sudo setcap cap_net_raw,cap_net_admin+ep dumpcap
```
### 9. Run Wireshark
You can now run Wireshark:
```bash
./wireshark
```
Wireshark should open, and you should be able to start capturing network traffic.

## License
**Free Software, Hell Yeah!**

## Authors
- [Rahul Gupta](https://github.com/rahulelex)
