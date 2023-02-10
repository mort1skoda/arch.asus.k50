## artix linux
### artix linux runit legacy bios



#### wget iso

    wget http://ftp.ludd.ltu.se/artix/iso/artix-base-runit-20220713-x86_64.iso



#### wget sig

    wget http://ftp.ludd.ltu.se/artix/iso/artix-base-runit-20220713-x86_64.iso



#### pgp

    pgp --auto-key-retrieve --verify artix-base-runit-20220713-x86_64.iso.sig artix-base-runit-20220713-x86_64.iso



#### dd

    s dd bs=4M if=~/Downloads/artix-base-runit-20220713-x86_64.iso of=/dev/sdb conv=fsync oflag=direct status=progress



#### boot live medium

    asus k50ij spam ESC



#### loadkeys

    ls -R /usr/share/kbd/keymaps
    loadkeys no



#### mount

    swapon /dev/sda5
    mount /dev/sda7 /mnt



#### wpa_supplicant

    vi /etc/wpa_supplicant/wpa_supplicant.conf
    ctrl_interface=/run/wpa_supplicant
    update_config=1

    wpa_supplicant -B wlan0 -c /etc/wpa_supplicant/wpa_supplicant.conf
    wpa_cli

    >scan
    >scan_results
    >add_network
    >set_network 0 ssid "103B 2.4"
    >set_network 0 psk "sdbyorgufjuad"
    >enable_network 0
    >save_config
    >quit

    or
    wpa_passphrase "103B 2.4" "sdbyorgufjuad"
    wpa_supplicant -B wlan0 -c (wpa_passphrase "103B 2.4" "sdbyorgufjuad" 

    Note: Because of the process substitution,
    you cannot run this command with sudo
    and must use a root shell

    dhcpcd ???

    ip a
    ping -c4 artixlinux.org



#### clock

    sv up ntpd



#### base system

    basestrap /mnt base base-devel runit elogind-runit



#### kernel

    basestrap /mnt linux linux-firmware



#### ucode

    basestrap /mnt intel-ucode



#### openssh vim





