1) copy file unbind_ehci

/etc/initramfs-tools/scripts/init-top/unbind_ehci



change mode

sudo chmod a+x /etc/initramfs-tools/scripts/init-top/unbind_ehci

2) create udev rule /etc/udev/rules.d/10_disable-ehci.rules

ACTION=="add", SUBSYSTEM=="pci", DRIVER=="ehci_hcd", \
    RUN+="/bin/sh -c 'echo -n %k > %S%p/driver/unbind'"

3) update initramfs

sudo update-initramfs -k all -u

#############################################
##NOT NEEDED MAYBE
4) this script will fix touchpad after resume

/etc/pm/sleep.d/99zcyapa http://pastebin.com/xajaskQr

sudo chmod a+x /etc/pm/sleep.d/99zcyapa
#############################################


5) enshure that in /etc/default/grub you have "tpm_tis.interrupts=0"

there is my settings
GRUB_CMDLINE_LINUX_DEFAULT="  boot=local  i915.modeset=1 tpm_tis.interrupts=0 " //Example

update grub

sudo update-grub2

GRUB_CMDLINE_LINUX_DEFAULT="quiet splash tpm_tis.interrupts=0 modprobe.blacklist=ehci_hcd,ehci_pci nmi_watchdog=0" //Maybe needed
sudo update-grub2