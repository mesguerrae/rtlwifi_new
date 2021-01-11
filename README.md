rtlwifi_new
===========
### A repo for the newest Realtek rtlwifi codes.

This code will build on any kernel 4.19 and newer as long as the distro has not modified
any of the kernel APIs. IF YOU RUN UBUNTU, YOU CAN BE ASSURED THAT THE APIs HAVE CHANGED.
NO, I WILL NOT MODIFY THE SOURCE FOR YOU. YOU ARE ON YOUR OWN!!!!!

This repository includes drivers for the following cards:

RTL8822BE, RTL8822CE, RTL8821CE, and RTL8723DE.

### Installation instruction
##### Requirements
You will need to install "make", "gcc", "kernel headers", "kernel build essentials", and "git".
You can install them with the following command, on **Ubuntu**:
```bash
sudo apt-get update
sudo apt-get install make gcc linux-headers-$(uname -r) build-essentials git
```
If any of the packets above are not found check if your distro installs them like that. 

##### Installation
For all distros:
```bash
git clone https://github.com/mesguerrae/rtlwifi_new.git
cd rtlwifi_new
make
sudo make install
```
##### Blacklisting (needed if you want to use these modules)
Some distros provide `RTL8723DE` drivers. To use this driver, that one MUST be
blacklisted. How to do that is left as an exercise as learning that will be very beneficial.

If your system has ANY conflicting drivers installed, you must blacklist them as well. For kernels
5.6 and newer, this will include drivers such as rtw88_xxxx.
Here is a useful [link](https://askubuntu.com/questions/110341/how-to-blacklist-kernel-modules) on how to blacklist a module

Once you have reached this point, then reboot. Use the command `lsmod` and check if there are any
conflicting drivers. The correct ones are:
- `rtw_8723de  rtw_8723d  rtw_8822be  rtw_8822b  rtw_8822ce  rtw_8822c  rtw_core  and rtw_pci`

If you have other modules installed, see if you blacklisted them correctly.

##### How to disable/enable a Kernel module
 ```bash
sudo modprobe -r rtw_8723de         #This disable the module
sudo modprobe rtw_8723de            #This enables the module, you can add options like ant_sel=2
```

##### Option configuration
If it turns out that your system needs one of the configuration options, then do the following:
```bash
sudo nano /etc/modprobe.d/<dev_name>.conf 
```
There, enter the line below:
```bash
options <device_name> <<driver_option_name>>=<value>
```

***********************************************************************************************

When your kernel changes, then you need to do the following:
```bash
cd ~/rtlwifi_new
git pull
make
sudo make install
```

Remember, this MUST be done whenever you get a new kernel - no exceptions.

These drivers will not build for kernels older than 4.14. If you must use an older kernel,
submit a GitHub issue with a listing of the build errors. Without the errors, the issue
will be ignored. I am not a mind reader.

When you have problems where the driver builds and loads correctly, but fails to work, a GitHub
issue is NOT the best place to report it. I have no idea of the internal workings of any of the
chips, and the Realtek engineers who do will not read these issues. To reach them, send E-mail to
linux-wireless@vger.kernel.org. Include a detailed description of any messages in the kernel
logs and any steps that you have taken to analyze or fix the problem. If your description is
not complete, you are unlikely to get any satisfaction.

*********************************************************************************************
Para su instalación seguiremos los siguientes pasos:

 sudo apt install git build-essential dkms git clone -b extended https://github.com/lwfinger/rtlwifi_new.git sudo dkms add ./rtlwifi_new sudo dkms install rtlwifi-new/0.6 sudo modprobe -r rtl8723de && sudo modprobe rtl8723de
1
2
3
4
5
6
7
8
9
sudo apt install git build-essential dkms
 
git clone-bextended https://github.com/lwfinger/rtlwifi_new.git
 
sudo dkms add./rtlwifi_new
 
sudo dkms install rtlwifi-new/0.6
 
sudo modprobe-rrtl8723de&&sudo modprobe rtl8723de
En general con el primer paso ya lo tendreis solucionado, en algunos casos deberemos se ejecutar el siguiente comando alternando el número 3, por 1, 2 o 3 para cambiar la señal:

 echo "options rtl8723de ant_sel=3" | sudo tee /etc/modprobe.d/rtl8723de.conf
1
echo"options rtl8723de ant_sel=3"|sudo tee/etc/modprobe.d/rtl8723de.conf
