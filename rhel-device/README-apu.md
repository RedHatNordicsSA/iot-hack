# Installing and configuring APU2 box

[APU2](https://www.pcengines.ch/apu2.htm) does not have display adapter, thus installation needs to be done over serial, or automated way. This is automated.

# The RHEL9 Edge way

[Instructions here](https://access.redhat.com/documentation/en-us/edge_management/2022/html-single/create_rhel_for_edge_images_and_configure_automated_management/)

1. Create the image at [Red Hat Edge image builder](https://console.redhat.com/edge/manage-images).
2. Download it to local directory, you'll have fleet_out.iso image.
3. Create activation keys, and drop them to ```fleet_rhc_vars``` -file.
4. Add permissions to directory ```chmod 777 .```
5. re-create iso
  ```podman run -it --rm -v /full/path/to/myisodir:/isodir:Z quay.io/fleet-management/fleet-iso-util:latest```
6. optional steps if you want to customize the iso kickstart to e.g. include local users and any post install actions
   1. mount the disk and copy out the kickstart:
      ```sudo mount -t iso9660 -o loop ~/VirtualMachines/fleet_out.iso /mnt```
   2. unmount the disk ```sudo umount /mnt```
   3. edit the fleet.ks -file to your liking
   4. fix the isolinux file to use serial console, and change default to first
      entry. This skips DVD check as we didn't create new checksum for it.
      ```diff
      *** /mnt/isolinux/isolinux.cfg  2022-09-08 16:55:37.000000000 +0300
      --- myisodir/isolinux-ikke.cfg  2022-09-13 11:50:09.639614079 +0300
      *************** menu separator # insert an empty line
      *** 60,73 ****

        label linux
          menu label ^Install Red Hat Enterprise Linux 9.0
          kernel vmlinuz
      !   append initrd=initrd.img inst.stage2=hd:LABEL=RHEL-9-0-0-BaseOS-x86_64  quiet  None inst.ks=hd:LABEL=RHEL-9-0-0-BaseOS-x86_64:/fleet.ks None

        label check
          menu label Test this ^media & install Red Hat Enterprise Linux 9.0
      -   menu default
          kernel vmlinuz
      !   append initrd=initrd.img inst.stage2=hd:LABEL=RHEL-9-0-0-BaseOS-x86_64  rd.live.check quiet  None inst.ks=hd:LABEL=RHEL-9-0-0-BaseOS-x86_64:/fleet.ks None

        menu separator # insert an empty line

      --- 60,73 ----

        label linux
          menu label ^Install Red Hat Enterprise Linux 9.0
      +   menu default
          kernel vmlinuz
      !   append console=tty0 console=ttyS0,115200n8 initrd=initrd.img inst.stage2=hd:LABEL=RHEL-9-0-0-BaseOS-x86_64  quiet  None inst.ks=hd:LABEL=RHEL-9-0-0-BaseOS-x86_64:/fleet.ks None

        label check
          menu label Test this ^media & install Red Hat Enterprise Linux 9.0
          kernel vmlinuz
      !   append console=tty0 console=ttyS0,115200n8 initrd=initrd.img inst.stage2=hd:LABEL=RHEL-9-0-0-BaseOS-x86_64  rd.live.check quiet  None inst.ks=hd:LABEL=RHEL-9-0-0-BaseOS-x86_64:/fleet.ks None

        menu separator # insert an empty line

      *************** label text
      *** 83,89 ****
              Red Hat Enterprise Linux 9.0.
          endtext
          kernel vmlinuz
      !   append initrd=initrd.img inst.stage2=hd:LABEL=RHEL-9-0-0-BaseOS-x86_64  inst.text quiet  None inst.ks=hd:LABEL=RHEL-9-0-0-BaseOS-x86_64:/fleet.ks None

        label rescue
          menu indent count 5
      --- 83,89 ----
              Red Hat Enterprise Linux 9.0.
          endtext
          kernel vmlinuz
      !   append console=tty0 console=ttyS0,115200n8 initrd=initrd.img inst.stage2=hd:LABEL=RHEL-9-0-0-BaseOS-x86_64  inst.text quiet  None inst.ks=hd:LABEL=RHEL-9-0-0-BaseOS-x86_64:/fleet.ks None


          menu indent count 5
      *************** label rescue
      *** 93,99 ****
              and edit config files to try to get it booting again.
          endtext
          kernel vmlinuz
      !   append initrd=initrd.img inst.stage2=hd:LABEL=RHEL-9-0-0-BaseOS-x86_64 inst.ks=hd:LABEL=RHEL-9-0-0-BaseOS-x86_64:/osbuild.ks inst.rescue quiet

        label memtest
          menu label Run a ^memory test
      --- 93,99 ----
              and edit config files to try to get it booting again.
          endtext
          kernel vmlinuz
      !   append console=tty0 console=ttyS0,115200n8 initrd=initrd.img inst.stage2=hd:LABEL=RHEL-9-0-0-BaseOS-x86_64 inst.ks=hd:LABEL=RHEL-9-0-0-BaseOS-x86_64:/osbuild.ks inst.rescue quiet

        label memtest
          menu label Run a ^memory test
      ```
     5. Create new DVD with the changed kickstart and boot menu
        ``` shell
        xorriso -indev ~/VirtualMachines/fleet_out.iso \
        -outdev ~/VirtualMachines/fleet_out_serial.iso \
        -compliance no_emul_toc \
        -map "isolinux-ikke.cfg" "isolinux/isolinux.cfg" \
        -map "fleet.ks" "fleet.ks" \
        boot_image any replay
        ```
7. Create USB stick
  ```sudo dd if=/home/itengval/VirtualMachines/fleet_out_serial.iso of=/dev/<be pretty sure what you put here, eg. sda> bs=8M status=progress```
8. Boot the box with USB stick in it. You can monitor the progress from
   serial console. Network cable needs to be in the third rj45 counting
   from power connector. After few minutes you can see the box in fleet
   management, and you can ssh into it.


# The regular RHEL way

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

