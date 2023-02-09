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

---> provide image of archlinux live boot screen <---
# --------------------------------------------}}}



# -- initial settings -----------------------{{{

    loadkeys no
    set -o vi
    alias l='ls -la --color --group-directories-first'
    passwd
# -- ------- -------- -----------------------}}}



# -- connect to internet --------------------{{{

    iwctl

[iwd]# device list
[iwd]# station wlan0 get-networks
[iwd]# station wlan0 connect '103B 2.4'
Passphrase: ********
[iwd]# ctrl-d

    ip a
    ping -c4 archlinux.org
# -- ------- -- -------- --------------------}}}



# -- work on the host -----------------------{{{

    Switch between tty1-6: Alt+arrow
    You can use lynx to read instructions.
    lynx archlinux.org
    o=option to set vi keys
# -- ---- -- --- ---- -----------------------}}}



# -- work via ssh client --------------------{{{

    rm .ssh
    ssh root@10.0.0.56
    set -o vi
    alias l='ls -la --color --group-directories-first'
# -- ---- --- --- ------ --------------------}}}



# -- partiton and format --------------------{{{

    fdisk -l
    lsblk
    blkid

    fdisk /dev/<the_disk_to_be_partitioned>

<pre>
At this point I went into Windows 10 and run MiniTool Partition Wizard
To make a swap partition and 3 linux installation partitons.
</pre>

#### fortmat the partitions

    lsblk
    mkfs.ext4 /dev/sda6
    mkswap /dev/sda5

---

Provide an image here to see the layout of the ssd on asus.k50

---

# -- -------- --- ------ --------------------}}}



# -- mount the file system ------------------{{{

    lsblk
    mount /dev/sda6 /mnt
    swapon /dev/sda5
    lslbk
# -- ----- --- ---- ------ ------------------}}}



# -- pacstrap -------------------------------{{{

    lscpu | grep -i Vendor
    pacstrap -K /mnt intel-ucode
    pacstrap -K /mnt base
    pacstrap -K /mnt linux linux-firware
    pacstrap -K /mnt vim sudo openssh 
    pacstrap -K /mnt networkmanager
# -- -------- ------------------------------}}} 



# -- fstab --------------------------------{{{

    cat /mnt/etc/fstab
    genfstab -U /mnt >> /mnt/etc/fstab
    cat /mnt/etc/fstab
# -- ----- --------------------------------}}}



# -- chroot ---------------------------------{{{

    arch-chroot /mnt
    set -o vi
    alias l='ls -la --color --group-directories-first'
# -- ------ ---------------------------------}}}



# -- time zone ------------------------------{{{

    ln -svf /usr/share/zoneinfo/Europa/Oslo /etc/localtime
    hwclock --systohc
# -- ---- ---- ------------------------------}}}



# -- localiztion ----------------------------{{{

    vim /etc/locale.gen     [uncomment en_US.UTF-8 UTF-8]
    locale-gen

    vim /etc/locale.conf    [LANG=en_US.UTF-8]
    vim /etc/vconsole.conf  [KEYMAP=no]
# -- ----------- ----------------------------}}}



# -- network confiuration -------------------{{{

    vim /etc/hostname       [arch.k50]

    pacman -S networkmanager
    systemctl enable NetworkManager.service

    pacman -S openssh
    systemctl enable sshd
# -- ------- ------------ -------------------}}}



# -- root password --------------------------{{{

    passwd
# -- ---- -------- --------------------------}}}



# -- bootloader ----------------------------{{{

    pacman -S grub os-prober

    grub-install --target=i386-pc /dev/sda

    grub-mkconfig -o /boot/grub/grub.cfg

    vim /etc/default/grub   [uncomment: GRUB_DISABLE_OS_PROBER=false] 

    pacman -S ntfs-3g       (if windows)

    mount /dev/sda1 /mnt/sda1
    grub-mkconfig -o /boot/grub/grub.cfg
# -- ---------- ----------------------------}}}



# -- reboot --------------------------------{{{

    ctrl-d
    umount -R /mnt
    reboot
# -- ------ --------------------------------}}}



# -- connect to wifi -----------------------}}}




