# arch.asus.k50

#### download

    https://mirror.archlinux.no/iso/2023.02.01/

#### wget

    wget https://mirror.archlinux.no/iso/2023.02.01/archlinux-x86_64.iso
    wget https://mirror.archlinux.no/iso/2023.02.02/archlinux-x86_64.iso.sig

#### verify signature

    pacman-key -v archlinux-x86_64.iso.sig

#### make bootable usb

    dd bs=4M if=/home/m/Downloads/archlinux-x86_64.iso of=/dev/sdx conv=fsync oflag=direct status=progress


