# Install Ubuntu-24.04 on asus rog g14 (2024) 
Install Ubuntu 24.04 on your ASUS ROG G14 without losing your mind. 
This repo is just a small aggregation of steps that I followed and that worked successfully. 

## Flash and install Ubuntu OS
- First thing first, get a USB stick and flash the Ubuntu version on it [1].
- Create a partition and a free space to allocate Ubuntu. Then, turn off the computer. 
- Connect the USB stick. Turn on the computer and press repeatedly f2. This will start the GRUB menu. Place the Flesh driver first in the order, close, and save. This will start the Ubuntu 24.04.
- **Important**: In the ubuntu GRUB manyu, do not select, the first option but the (safe graphics) option. Otherwise, the installation will keep crashing [2].
- During the installation, when asked, please do not use set the wifi. There is a bug and this will make your installation crash, you can activate the wifi later on.

  ## Install a compatible kernel 6.12.0

  To install the new kernel, I used the mainline kernel.

  ```bash
  sudo apt update
  sudo add-apt-repository ppa:cappelikan/ppa
  sudo apt update && sudo apt full-upgrade
  sudo apt install -y mainline
  sudo mainline
  ```
  In the mainline graphical interface, click on 6.12 and hit install. Reboot the system. 

  ```bash
  reboot
  ```

- Check that the kernel is correct.
 ```bash
uname -r 
```
This should display ``` 6.12.0-061200-generic ```. 

- We are ready to install the Asus Control for Ubuntu. This package is necessary to control the brightness of the keyboard and key fix the audio control. Otherwise, the keyboard does not light up and the audio is just terrible. After that you will be able to choose also the color of the keyboard and this is very cool!
- You have to install rust and cargo first [3]
  ```bash
  sudo apt install libclang-dev libudev-dev libfontconfig-dev build-essential cmake libxkbcommon-dev
  curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
  make
  sudo make install
  ```
- Download the .zip of the version 6.0.12 from [here](https://gitlab.com/asus-linux/asusctl/-/releases) and extract the folder.
- Open a terminal and run
  ```bash
    cd asusctl-6.0.12
    source $HOME/.cargo/env
    make
    sudo make install
    rog-control-center
  ```
## Install compatible GPU Drivers
```bash
  sudo apt-get remove --purge nvidia-*
  sudo apt-get update
  sudo apt-get install build-essential libncurses-dev linux-headers-$(uname -r)
  sudo apt install gcc-14 g++-14
  sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-14 60 --slave /usr/bin/g++ g++ /usr/bin/g++-14
```

- [Download](https://us.download.nvidia.com/XFree86/Linux-x86_64/565.77/NVIDIA-Linux-x86_64-565.77.run
) Nvidia Drivers that are compatible with the kernel 6.12, i.e., 565.77. [4]

  ``` bash  sudo sh ./NVIDIA-Linux-x86_64-565.77.run
    cd Downloads
    sudo sh ./NVIDIA-Linux-x86_64-565.77.run
  ```

- Check that the GPU was correctly updated using ``` nvidia-smi ```. If the drivers are installed correctly, the terminal will display GPU information and used memory. 
[1] https://ubuntu.com/tutorials/install-ubuntu-desktop#2-download-an-ubuntu-image.
[2] https://askubuntu.com/questions/1525184/ubuntu-24-04-1-lts-desktop-installer-crashes
[3] https://gitlab.com/asus-linux/asusctl
[4] https://forums.developer.nvidia.com/t/build-nvidia-drivers-with-linux-6-12-series-kernels/316195

