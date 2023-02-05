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

#### boot the live environment

    on the asus k50:
    esc = select boot device <---
    ------------------------ 
    F2  = bios setup
    
#### loadkeys
    
    loadkeys no
    set -o vi
    alias=l='ls -la --color --group-directories-first'


<pre>
Switch console to view this guide with lynx.
Use the Alt+arrow shortcut. To edit configuration files, vim are available.
</pre>


#### connect to the internet

    iwctl
    [iwd]# device list
    [iwd]# station wlan0 scan
    [iwd]# station wlan0 get-networks
    [iwd]# station wlan0 connect NETGEAR87
    passphrase: roundsquXXNNN
    exit=ctrl+d
    ping -c4 archlinux.org

#### update the system clock

    timedatectl status

#### partiton the disks

    fdisk -l
    fdisk /dev/<the_disk_to_be_partitioned>

<pre>
At this point I went into Windows 10 and run MiniTool Partition Wizard
To make a swap partition and 3 linux installation partitons.
</pre>


---

Provide an image here to see the layout of the ssd on asus.k50

---



