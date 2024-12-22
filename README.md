# Install Ubuntu 24.04 on asus rog g14 (2024) 
Install Ubuntu 24.04 on your ASUS ROG G14 without losing your mind. The installation is not straighfarward.
This repo is just a small aggregation of steps that I followed and that worked successfully. 

## Flash and install Ubuntu 24.04
- First thing first, get a USB stick and flash the Ubuntu version on it [1].
- Start the computer in Windows. 
- Create a partition and a free space to allocate Ubuntu.
- Disenable Device encryption & Standard BitLocker encryption [2]
- Turn off the computer
- Disenable the Fast Boot and the Secture Boot. When turning on, press f2 to enter the menu, then f7. In the Section Boot, disenable Fast Boot, and in Secturity, set Secure Boot Control on Disabled. Then press F10 to save. [3]
- Connect the USB stick. Turn on the computer and press repeatedly f2. This will start the GRUB menu. Place the Flesh driver first in the order, close, and save. This will start the Ubuntu 24.04.
- **Important**: In the ubuntu GRUB manyu, do not select, the first option but the (safe graphics) option. Otherwise, the installation will keep crashing [4].
- During the installation, when asked, please do not use set the wifi. There is a bug and this will make your installation crash, you can activate the wifi later on.

## Install a compatible kernel 6.12.0

  To install the new kernel, I used the mainline kernel.

  ```bash
  sudo apt update
  sudo add-apt-repository ppa:cappelikan/ppa
  sudo apt update && sudo apt full-upgrade
  sudo apt install -y mainline
  mainline install 6.12
  reboot
  ```
- The computer will restart; when it turns on, Check that the kernel is correct, i.e., 
   ```bash
  uname -r 
  ```
This should display ``` 6.12.0-061200-generic ```. 

- We are ready to install the Asus Control for Ubuntu. This package is necessary to control the brightness of the keyboard and key fix the audio control. Otherwise, the keyboard does not light up and the audio is just terrible. After that, you will be able to choose also the color of the keyboard, and this is very cool!
- You have to install rust and cargo first [5]
  ```bash
  sudo apt install libclang-dev libudev-dev libfontconfig-dev build-essential cmake libxkbcommon-dev
  curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
  make
  sudo make install
  ```
- [Download](https://gitlab.com/asus-linux/asusctl/-/archive/6.0.12/asusctl-6.0.12.zip) the version 6.0.12 from and extract the folder.
- Open a terminal and run
  ```bash
    cd ~/Downloads
    unzip asusctl-6.0.12.zip
    cd asusctl-6.0.12
    source $HOME/.cargo/env
    make
    sudo make install
    rog-control-center
  ```
## Install compatible GPU Drivers
- Purge the old drivers and install the required essential library. 
```bash
  sudo apt-get remove --purge nvidia-*
  sudo apt-get update
  sudo apt-get install build-essential libncurses-dev linux-headers-$(uname -r)
```
- Install GCC (GNU Compiler Collection) and configure the system to allow switching between GCC versions

```bash
  sudo apt install gcc-14 g++-14
  sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-14 60 --slave /usr/bin/g++ g++ /usr/bin/g++-14
```

- [Download](https://us.download.nvidia.com/XFree86/Linux-x86_64/565.77/NVIDIA-Linux-x86_64-565.77.run
) Nvidia Drivers that are compatible with the kernel 6.12, i.e., 565.77 [6].

  ``` bash 
    cd ~/Downloads
    sudo sh ./NVIDIA-Linux-x86_64-565.77.run
  ```

  In the menu from the installation, select the proprietary drivers. 

- Check that the GPU was correctly updated using ``` nvidia-smi ```. If the drivers are installed correctly, the terminal will display GPU information and used memory.

[1] https://ubuntu.com/tutorials/install-ubuntu-desktop#2-download-an-ubuntu-image. \
[2] https://www.asus.com/support/faq/1047461/ \
[3] https://www.asus.com/support/faq/1049829/ \
[4] https://askubuntu.com/questions/1525184/ubuntu-24-04-1-lts-desktop-installer-crashes \
[5] https://gitlab.com/asus-linux/asusctl \
[6] https://forums.developer.nvidia.com/t/build-nvidia-drivers-with-linux-6-12-series-kernels/316195 

