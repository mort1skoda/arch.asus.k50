## arch.asus.k50



#--- preparation -------------------------------------------------------------------------{{{

#### download

    https://mirror.archlinux.no/iso/2023.02.01/



#### wget

    wget https://mirror.archlinux.no/iso/2023.02.01/archlinux-x86_64.iso
    wget https://mirror.archlinux.no/iso/2023.02.02/archlinux-x86_64.iso.sig



#### verify signature

    pacman-key -v archlinux-x86_64.iso.sig



#### make bootable usb

    dd bs=4M if=/home/m/Downloads/archlinux-x86_64.iso of=/dev/sdx conv=fsync oflag=direct status=progress

#-----------------------------------------------------------------------------------------}}}



#--- working on the host -----------------------------------------------------------------{{{

#### boot the live environment

    ESC to select boot device.


#### loadkeys
    
    loadkeys no
    set -o vi
    alias l='ls -la --color --group-directories-first'


#### connect to the internet

    iwctl
    [iwd]# device list
    [iwd]# station wlan0 scan
    [iwd]# station wlan0 get-networks
    [iwd]# station wlan0 connect NETGEAR87
    passphrase: roundsquash478
    exit=ctrl+d
    ip a
    ping -c4 archlinux.org
    passwd root


<pre>
Switch console (tty1-6), to view this guide with lynx.
Use the Alt+arrow shortcut. To edit configuration files, vim are available.
</pre>

#----------------------------------------------------------------------------------------}}}



#--- working via a ssh client ------------------------------------------------------------{{{

#### ssh

    ssh -o StrictHostKeyChecking=no "UserKnownHostsFile /dev/null" root@192.168.0.31
    ssh root@192.168.0.31
    set -o vi
    alias l='ls -la --color --group-directories-first'


#### update the system clock

    timedatectl status


#### partiton the disks ---------------------------------------------{{{

    fdisk -l
    lsblk
    blkid

    fdisk /dev/<the_disk_to_be_partitioned>

<pre>
At this point I went into Windows 10 and run MiniTool Partition Wizard
To make a swap partition and 3 linux installation partitons.
</pre>


---

Provide an image here to see the layout of the ssd on asus.k50

---

####-----------------------------------------------------------------}}}


#### fortmat the partitions

    mkfs.ext4 /dev/sda6
    mkswap /dev/sda5 


#### mount the file systems

    mount /dev/sda6 /mnt
    swapon /dev/sda5

    lsblk

<pre>
sda1    win10 ntfs
sda2    extended part
sda3    data fat32
sda5    linux swap
sda6    arch1
sda7    arch2
sda8    lfs
</pre>


#### install essential packages

    lscpu to see if you need amd-ucode or intel ucode.    
    pacstrap -K /mnt base linux linux-firmware amd-ucode vim


#### fstab

    genfstab -U /mnt >> /mnt/etc/fstab
    cat /mnt/etc/fstab


#### chroot

    arch-chroot /mnt
    set -o vi
    alias l='ls -la --color --group-directories-first'


#### time zone

    ln -svf /usr/share/zoneinfo/Europe/Oslo /etc/localtime
    hwclock --systohc


#### localization

    vim /etc/locale.gen         Uncomment en_US.UTF-8 UTF-8
    locale-gen

    vim /etc/vconsole.conf      add: KEYMAP=no 


#### network configuration

    vim /etc/hostname           add: arch.k50

    install network managment software:
   

------------------------------------------------------------------------------------------}}} 



