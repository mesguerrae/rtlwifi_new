rtlwifi_new
===========
### A repo for the newest Realtek rtlwifi codes.

This code will build on any kernel 4.19 and newer as long as the distro has not modified
any of the kernel APIs. IF YOU RUN UBUNTU, YOU CAN BE ASSURED THAT THE APIs HAVE CHANGED.
NO, I WILL NOT MODIFY THE SOURCE FOR YOU. YOU ARE ON YOUR OWN!!!!!

This repository includes drivers for the following cards:

RTL8822BE, RTL8822CE, RTL8821CE, and RTL8723DE.

### General Requirements
Before proceeding with either installation method, you will need to install essential packages.
For **Ubuntu** and similar Debian-based distributions, run:
```bash
sudo apt-get update
sudo apt-get install make gcc linux-headers-$(uname -r) build-essential git
```
For other distributions, please use your package manager to install "make", "gcc", "kernel headers", "kernel build essentials", and "git".

## Recommended Installation: Using DKMS
Dynamic Kernel Module Support (DKMS) will automatically rebuild and install the driver when your kernel is updated. This is the preferred method.

##### 1. Install DKMS
If you haven't already, install `dkms`. On **Ubuntu** and similar Debian-based distributions:
```bash
sudo apt-get install dkms
```
Ensure the general requirements (make, gcc, kernel headers, etc.) listed above are also installed.

##### 2. Clone this Repository
```bash
git clone https://github.com/mesguerrae/rtlwifi_new.git
cd rtlwifi_new
```

##### 3. Add and Install Driver using DKMS
```bash
sudo dkms add .
sudo dkms install rtlwifi-new/0.6
```
(The package name `rtlwifi-new` and version `0.6` are taken from the `dkms.conf` file in this repository.)

DKMS will now monitor for kernel updates and automatically rebuild the `rtlwifi-new` modules.

### Manual Installation (Alternative)
This method requires you to manually rebuild and reinstall the driver every time your kernel is updated.

##### 1. Clone this Repository (if you haven't already for DKMS)
```bash
git clone https://github.com/mesguerrae/rtlwifi_new.git
cd rtlwifi_new
```

##### 2. Build and Install
```bash
make
sudo make install
```

##### Important: Manual Reinstallation After Kernel Updates
If you use this manual method, you **MUST** re-run the `make` and `sudo make install` commands every time your kernel is updated.
To do this:
```bash
cd /path/to/your/rtlwifi_new_clone # Navigate to where you cloned the repo
git pull # To get the latest driver updates
make
sudo make install
```
Failure to do so will result in the driver not working with the new kernel.

### Post-Installation Steps (For Both DKMS and Manual Installation)

##### Blacklisting Conflicting Modules
Some distributions provide built-in drivers that may conflict with these. For example, the stock `rtl8723de` driver might need to be blacklisted. For kernels 5.6 and newer, this might also include drivers like `rtw88_xxxx`.

You must identify and blacklist any conflicting drivers. Learning how to do this is beneficial.
Here is a useful [link](https://askubuntu.com/questions/110341/how-to-blacklist-kernel-modules) on how to blacklist a kernel module.

After blacklisting, reboot your system.

Use the command `lsmod | grep rtw` to check for conflicting drivers. The correct modules from this driver start with `rtw_` (e.g., `rtw_8723de`, `rtw_8822be`, `rtw_pci`, `rtw_core`). If other Realtek drivers are loaded (e.g., `r8723de`), ensure your blacklisting is effective.

##### How to disable/enable a Kernel module
```bash
sudo modprobe -r rtw_8723de         # This disables the module
sudo modprobe rtw_8723de            # This enables the module, you can add options like ant_sel=2
```
Replace `rtw_8723de` with the specific module name you wish to manage.

##### Option Configuration
If your wireless card requires specific driver options (e.g., for antenna selection):
Create or edit a `.conf` file in `/etc/modprobe.d/`. For example:
```bash
sudo nano /etc/modprobe.d/rtw_8723de.conf
```
Add a line with the option, for example:
```bash
options rtw_8723de ant_sel=2
```
Replace `rtw_8723de` with the correct module name and adjust the option and value as needed.

### Troubleshooting
These drivers will not build for kernels older than 4.14. If you must use an older kernel,
submit a GitHub issue with a listing of the build errors. Without the errors, the issue
will be ignored.

When you have problems where the driver builds and loads correctly, but fails to work, a GitHub
issue is NOT the best place to report it. The Realtek engineers who have internal knowledge of the
chips will not read these issues. To reach them, send E-mail to
linux-wireless@vger.kernel.org. Include a detailed description of any messages in the kernel
logs and any steps that you have taken to analyze or fix the problem. If your description is
not complete, you are unlikely to get any satisfaction.
