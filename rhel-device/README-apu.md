# Installing and configuring APU2 box

[APU2](https://www.pcengines.ch/apu2.htm) does not have display adapter, thus installation needs to be done manually.

# Prepare two USB sticks

In order to do so, you need two USB sticks:

1. Write [RHEL8 install](https://access.redhat.com/downloads/content/479/ver=/rhel---8/8.1/x86_64/product-software) iso image into on the first stick 
2. Initialize the second stick with filesystem labelled as OEMDRV, and copy ks.cfg kickstart file into root of it. [Instructions here](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html-single/performing_an_advanced_rhel_installation/index/#making-a-kickstart-file-available-on-a-local-volume-for-automatic-loading_making-kickstart-files-available-to-the-installation-program).

See [kickstart manual](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html-single/performing_an_advanced_rhel_installation/index/) for more details. The kickstart used in this exercise is in ks.cfg file in this directory.

Replace the root ssh public key in the ks.cfg with yours before proceeding.

# Connect the USB sticks and power on

Make sure to have both USB sticks connected to APU box before powering it on.

# Use serial terminal while booting

It might be that you did everything correct, and it takes some time, and device installs itself and reboots.

However, while doing this I did the following to see the process of installation.

1. Open minicom for terminal window: minicom -b 115200 -D /dev/ttyUSB0 -o -8
   You'll see Bios to go by, and will end up into grub entry stating installing options
2. Press <tab> while at installer prompt in order to get to edit kernel parameters
3. Remove the ```inst.check quiet``` parts from the end, and replace them with ```console=ttyS0,115200 inst.text```
4. Press enter, and install should proceed in serial terminal

If you did the ks.cfg part above, this is not really needed. But in case you want to do the normal text based install, this leads into normal text based installer if no ks.cfg is provided.

# Post install config

This workshop aims to configure everything by ansible, so we don't want to do any manual configs. You are done now if you can ssh into box as root.

