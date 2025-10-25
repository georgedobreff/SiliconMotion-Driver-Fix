This is a quick fix for EVDI-DKMS module related error when installing the <a href='https://www.siliconmotion.com/downloads/SM770-drivers.html'> SM77x Driver from SiliconMotion</a> on Arch Linux.

## The Issue:

My second monitor stopped working after the last kernel update and I figured it's the SiliconMotion driver for the USB-HDMI adapter but reinstalling threw an error.

The EVDI-DKMS module fails to build after the last kernel update (6.17.1-arch1-1) and the installer is hardcoded to use its own EVDI module (1.14.9), completely ignoring the existing version. No idea why they would do that but I'm sure they had a good reason for it.

The error:
```
Building module(s)...(bad exit status: 2)

Failed command:

make -j16 KERNELRELEASE=6.17.1-arch1-1 all INCLUDEDIR=/lib/modules/6.17.1-arch1-1/build/include KVERSION=6.17.1-arch1-1 DKMS_BUILD=1



Error! Bad return status for module build on kernel: 6.17.1-arch1-1 (x86_64)
```

## My simple solution:

I extracted the installer and modified the install.sh script to remove the code blocks that try to install evdi-dkms.

## How to install this:

0. Download/clone this repo (duh)

1. Make sure you already have evdi-dkms installed on your system. On Arch use yay:
```
yay -S evdi-dkms
```

2. Open a terminal in the downloaded folder (or navigate there) and execute
```
sudo ./install.sh
```

3. Reboot (if needed)

#### Quick Tip:

Reinstalling the driver will reset your display settings including which display is the main one, so you need to reconfigure those.


# For OMARCHY-MAC testers:

You need to update the monitors.conf file to enable your external monitor.

First run ```hyprctl monitors all``` to find the name of your external monitor (e-DP1 is your built in screen). If nothing else shows please make sure you reboot after the driver installation.

Once you have the monitor name:

``` nvim .config/hypr/monitors.conf```

add the following line (press i for insert mode)

```monitor = *monitor name*, 1920x1080@60, auto, 1```

and save (esc then :w)

Report in the Discord if your monitor turned on and what its name is.
