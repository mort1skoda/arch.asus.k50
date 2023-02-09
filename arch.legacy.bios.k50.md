#### archlinux tutorial
#### legacy bios
#### asus k50



#--- preparation -----------------------------{{{

#### download an iso image file

   [download iso file](https://archlinux.org/download)

#### wget iso file

    wget https://mirrors.dotsrc.org/archlinux/iso/2023.02.01/archlinux-x86_64.iso

#### wget sig file

    wget https://mirros.dotsrc.org/archlinux/iso/2023.02.01/archlinux-x86_64.iso.sig

#### verify signature

    pacman-key -v archlinux-x86_64.iso.sig

#### make bootable usb

    sudo dd bs=4M if=/home/m/Downloads/archlinux-x86_64.iso of=/dev/sdx conv=fsync oflag=direct status=progress

#---------------------------------------------}}}



# boot the live environment ------------------{{{
    on asus k50 spam ESC to select boot device
# --------------------------------------------}}}



# a few initial settings ---------------------{{{

    loadkeys no
    set -o vi
    alias l='ls -la --color --group-directories-first'
    passwd
# --------------------------------------------}}}




