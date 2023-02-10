## artix linux
### artix linux runit legacy bios

#### wget iso

    wget http://ftp.ludd.ltu.se/artix/iso/artix-base-runit-20220713-x86_64.iso
  
#### wget sig

    wget http://ftp.ludd.ltu.se/artix/iso/artix-base-runit-20220713-x86_64.iso

#### pgp

    pgp --auto-key-retrieve --verify artix-base-runit-20220713-x86_64.iso.sig artix-base-runit-20220713-x86_64.iso
