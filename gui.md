## gui.md

### tested with:
    
    archlinux legacy bios boot systemd  asus.k50
    archlinux uefi boot systemd         giga.am1

    artix legacy bios boot runit        asus.k50



#### -- following archlinux wiki

    [wiki](https://wiki.archlinux.org/title/General_recommendations)



#### -- install xorg

    lspci | g vga
    pacman -Ss xf86-video
    pi xf86-video-intel
    pi xorg
    pi xorg-xinit
    pi xterm



#### -- install suckless

    dwm
    st
    dmenu
    download:
    pi wget
    wget dl.suckless.org/dwm/dwm-6.4.tar.gz
    tar -xvf dwm-6.4.tar.gz
    cd dwm-6.4
    pi make gcc
    sudo make clean install




