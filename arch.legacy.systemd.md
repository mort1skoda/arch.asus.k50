#### archlinux tutorial
#### legacy bios systemd
#### asus k50



#--- preparation -----------------------------{{{



<pre>
    wget https://mirror.rackspace.com/archlinux/iso/2023.02.01/archlinux-x86_64.iso
</pre>
<pre>
    wget https://mirror.rackspace.com/archlinux/iso/2023.02.01/archlinux-x86_64.iso.sig 
</pre>
<pre>
    wget https://mirror.rackspace.com/archlinux/iso/2023.02.01/b2sums.txt
</pre>
<pre>
    wget https://mirror.rackspace.com/archlinux/iso/2023.02.01/sha256sums.txt
</pre>



#### download an iso image file

   [download iso file](https://archlinux.org/download)

#### wget iso file

    wget https://mirrors.dotsrc.org/archlinux/iso/2023.02.01/archlinux-x86_64.iso

#### wget sig file

    wget https://mirros.dotsrc.org/archlinux/iso/2023.02.01/archlinux-x86_64.iso.sig

#### verify signature

    pacman-key -v archlinux-x86_64.iso.sig

#### make bootable usb

    sudo dd bs=4M if=archlinux-x86_64.iso of=/dev/sdx conv=fsync oflag=direct status=progress

#---------------------------------------------}}}



#### boot the live environment ------------------{{{
    asus k50 spam ESC, select boot device

---> provide image of archlinux live boot screen <---

#### --------------------------------------------}}}



#### -- initial settings -----------------------{{{

    loadkeys no
    set -o vi
    alias l='ls -la --color --group-directories-first'
    passwd
#### -- ------- -------- -----------------------}}}



#### -- connect to internet --------------------{{{

    iwctl
    [iwd]# device list
    [iwd]# station wlan0 get-networks
    [iwd]# station wlan0 connect '103B 2.4'
    Passphrase: ********
    [iwd]# ctrl-d
    ip a
    ping -c4 archlinux.org
#### -- ------- -- -------- --------------------}}}



#### -- work on the host -----------------------{{{

    Switch between tty1-6: Alt+arrow
    You can use lynx to read instructions.
    lynx archlinux.org
    o=option to set vi keys
#### -- ---- -- --- ---- -----------------------}}}



#### -- work via ssh client --------------------{{{

    rm .ssh
    ssh root@10.0.0.56
    set -o vi
    alias l='ls -la --color --group-directories-first'
#### -- ---- --- --- ------ --------------------}}}



#### -- partiton ----------------------------{{{

    fdisk -l
    lsblk
    blkid

    fdisk /dev/<the_disk_to_be_partitioned>
#### -- -------- ----------------------------}}}

<pre>
At this point I went into Windows 10 and run MiniTool Partition Wizard
To make a swap partition and 3 linux installation partitons.
</pre>

#### -- format ------------------------------{{{

    lsblk
    mkfs.ext4 /dev/sda6
    mkswap /dev/sda5

---

Provide an image here to see the layout of the ssd on asus.k50

---

#### -- ------- -----------------------------}}}



#### -- mount the file system ----------------{{{

    lsblk
    mount /dev/sda6 /mnt
    swapon /dev/sda5
    lslbk
#### -- ----- --- ---- ------ ----------------}}}



#### -- pacstrap -------------------------------{{{

    lscpu | grep -i Vendor
    pacstrap -K /mnt intel-ucode
    pacstrap -K /mnt base
    pacstrap -K /mnt linux linux-firware
    pacstrap -K /mnt vim sudo openssh 
    pacstrap -K /mnt networkmanager
#### -- -------- ------------------------------}}} 



#### -- fstab --------------------------------{{{

    cat /mnt/etc/fstab
    genfstab -U /mnt >> /mnt/etc/fstab
    cat /mnt/etc/fstab
#### -- ----- --------------------------------}}}



#### -- chroot ---------------------------------{{{

    arch-chroot /mnt
    set -o vi
    alias l='ls -la --color --group-directories-first'
#### -- ------ ---------------------------------}}}



#### -- time zone ------------------------------{{{

    ln -svf /usr/share/zoneinfo/Europa/Oslo /etc/localtime
    hwclock --systohc
#### -- ---- ---- ------------------------------}}}



#### -- localiztion ----------------------------{{{

    vim /etc/locale.gen     [uncomment en_US.UTF-8 UTF-8]
    locale-gen

    vim /etc/locale.conf    [LANG=en_US.UTF-8]
    vim /etc/vconsole.conf  [KEYMAP=no]
#### -- ----------- ----------------------------}}}



#### -- network confiuration -------------------{{{

    vim /etc/hostname       [arch.k50]

    pacman -S networkmanager
    systemctl enable NetworkManager.service

    pacman -S openssh
    systemctl enable sshd
#### -- ------- ------------ -------------------}}}



#### -- root password --------------------------{{{

    passwd
#### -- ---- -------- --------------------------}}}



#### -- bootloader ----------------------------{{{

    pacman -S grub os-prober

    grub-install --target=i386-pc /dev/sda

    grub-mkconfig -o /boot/grub/grub.cfg

    vim /etc/default/grub   [uncomment: GRUB_DISABLE_OS_PROBER=false] 

    pacman -S ntfs-3g       (if windows)

    mount /dev/sda1 /mnt/sda1
    grub-mkconfig -o /boot/grub/grub.cfg
#### -- ---------- ----------------------------}}}



#### -- reboot --------------------------------{{{

    ctrl-d
    umount -R /mnt
    reboot
#### -- ------ --------------------------------}}}



#### -- first time reboot settings ------------{{{

    login as root
    set -o vi
    alias l='ls -la --color --group-directories-first'
#### -- ----- ---- ------ -------- ------------}}}    



#### -- connect to wifi -----------------------{{{

    nmcli device wifi list
    nmcli device wifi connect '103B 2.4' password sdbyorgufjuad
    ip -color a
    ping -c4 archlinux.org
#### -- ------- -- ---- -----------------------}}}



#### -- useradd ------------------------------{{{

    useradd -m -G wheel m
    passwd m
#### -- --- ---- -----------------------------}}}



#### -- visudo -------------------------------{{{

    EDITOR=/usr/bin/vim visudo
        [add at top: Defaults editor=/usr/bin/vim]
        [uncomment %wheel]
#### -- ------ -------------------------------}}}



#### -- logout login ------------------------{{{

    exit to logout
    login as m
#### -- ------ ----- ------------------------}}}



#### -- git ---------------------------------{{{

    sudo pacman -S git github-cli
    sudo mkdir /rep
    sudo chown -R m:m /rep

    cd /rep
    git clone https://github.com/mort1skoda/dotfiles.git
#### -- --- ---------------------------------}}} 



#### create symlinks ---------------------------{{{

   "Integrate your archlinux installation with dotfiles repo"
   
   Create symlink in ~ that point to
   /rep/dotfiles.

   In this way the git repo located in
   /rep/dotfiles will refelect any
   ajustments to config files in ~

    ln -svf /rep/dotfiles/.config        ~
    ln -svf /rep/dotfiles/.vifm          ~
    ln -svf /rep/dotfiles/.vim           ~
    ln -svf /rep/dotfiles/.bash_aliases  ~
    ln -svf /rep/dotfiles/.bash_logout   ~
    ln -svf /rep/dotfiles/.bash_profile  ~
    ln -svf /rep/dotfiles/.bashrc        ~
    ln -svf /rep/dotfiles/.lynxrc        ~
    ln -svf /rep/dotfiles/.tmux.conf     ~
    ln -svf /rep/dotfiles/.vimrc         ~
    ln -svf /rep/dotfiles/.xinitrc       ~

There is a bash (sh) file in /rep/dotfiles/ named:
create.symlinks.sh that creates theese symlinks.

    /rep/dotfiles/create.symlinks.sh
#### ------ -------- ---------------------------}}}



