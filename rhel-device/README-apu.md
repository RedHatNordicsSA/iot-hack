Installing and configuring APU2 box

[APU2](https://www.pcengines.ch/apu2.htm) does not have display adapter, thus installation needs to be done manually.

In order to do so, you need two USB sticks:

1. Write [RHEL8 install](https://access.redhat.com/downloads/content/479/ver=/rhel---8/8.1/x86_64/product-software) iso image into on the first stick 
2. Initialize the second stick with filesystem labelled as OEMDRV, and copy ks.cfg kickstart file into root of it. [Instructions here](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html-single/performing_an_advanced_rhel_installation/index/#making-a-kickstart-file-available-on-a-local-volume-for-automatic-loading_making-kickstart-files-available-to-the-installation-program).

See [kickstart manual](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html-single/performing_an_advanced_rhel_installation/index/) for more details. The kickstart used in this exercise is in ks.cfg file in this directory.
