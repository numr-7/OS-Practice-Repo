---
# Task 1 - NgeBOOT

## A. Langkah-langkah & Potongan Kode, Screenshot, Kode Penuh  
_(Steps & Code Snippets, Screenshot, Full Code)_

### Langkah-langkah & Potongan Kode _(Steps & Code Snippets)_
Jelaskan langkah-langkah yang dilakukan dan berikan potongan kode dari langkah-langkah yang kalian jelaskan jika ada.  
_Explain the steps performed and include relevant code snippets from the steps you describe if applicable._

1. Pertama-tama masuk ke directory osboot, kemudian menjalankan `wget` untuk mendownload `linux-6.1.1.tar.xz`. Lalu mengekstrak file tar tersebut menggunakan command `tar`. Lalu masuk ke directory yang sudah terbuat, yaitu `linux-6.1.1`
```bash
cd osboot # osboot/ (Masuk ke directory osboot)
wget https://cdn.kernel.org/pub/linux/kernel/v6.x/linux-6.1.1.tar.xz
tar -xvf linux-6.1.1.tar.xz
cd linux-6.1.1 # osboot/linux-6.1.1 (Masuk ke directory linux-6.1.1)
```

2. Meng-copy file .config kedalam directory `linux-6.1.1`
```bash
cp /home/ubuntu/sisop-modul-3-a10/task-1/.config .config
```

3. Melakukan kompilasi kernel menggunakan `make -j$(nproc)` untuk menghasilkan file `bzImage` yang kemudian akan di copy kedalam directory `osboot/`
```bash
make -j$(nproc)
cp arch/x86/boot/bzImage ..
cd .. # osboot/
```

4. Menghapus file dan directory `linux-6.1.1.tar.xz`
```bash
rm linux-6.1.1.tar.xz
rm -r linux-6.1.1
```

### Screenshot _(Screenshot)_
Masukkan screenshot hasil eksekusi program atau proses yang relevan.  
_Insert screenshots of program execution results or other relevant processes._

<img width="770" height="258" alt="!KERNEL4" src="https://github.com/user-attachments/assets/f69fd3ab-18a9-4e1a-aa96-7ad8e1a898e5" />

### Kode Penuh _(Full Code)_
Masukkan kode lengkap yang digunakan untuk menyelesaikan bagian ini.  
_Insert the full source code used to solve this section._

```bash
#!/bin/bash

cd osboot # osboot/
wget https://cdn.kernel.org/pub/linux/kernel/v6.x/linux-6.1.1.tar.xz
tar -xvf linux-6.1.1.tar.xz
cd linux-6.1.1 # osboot/linux-6.1.1
cp /home/ubuntu/sisop-modul-3-a10/task-1/.config .config

make -j$(nproc)
cp arch/x86/boot/bzImage ..
cd .. # osboot/

rm linux-6.1.1.tar.xz
rm -r linux-6.1.1
```

## B. Langkah-langkah & Potongan Kode, Screenshot, Kode Penuh  
_(Steps & Code Snippets, Screenshot, Full Code)_

### Langkah-langkah & Potongan Kode _(Steps & Code Snippets)_
Jelaskan langkah-langkah yang dilakukan dan berikan potongan kode dari langkah-langkah yang kalian jelaskan jika ada.  
_Explain the steps performed and include relevant code snippets from the steps you describe if applicable._

1. Masuk ke directory `osboot` dan membuat directory-directory `bin, dev, proc, sys`
```bash
cd osboot # osboot/
mkdir -p single/{bin,dev,proc,sys}
```

2. Meng-copy file device dari system host ke dalam directory `/dev`
```bash
cp -a /dev/null single/dev
cp -a /dev/tty* single/dev
cp -a /dev/zero single/dev
cp -a /dev/console single/dev
```

3. Meng-copy file `BusyBox` ke dalam directory `/bin` di dalam `single`. Lalu menginstall utilitas `BusyBox`
```bash
cp /usr/bin/busybox single/bin
cd single/bin # osboot/single/bin
./busybox --install .
```

4. Kemudian kembali ke directory `osboot/single` dan membuat file `init` yang berisi instruksi untuk melakukan mounting dan memulai shell interaktif
```bash
cd .. # osboot/single
touch init
cat <<EOF > init
#!/bin/sh
/bin/mount -t proc none /proc
/bin/mount -t sysfs none /sys

clear
exec /bin/sh
EOF
```

5. Terakhir, menambahkan permission untuk execute pada file `init`. Lalu mengompresi dan membuat file `initramfs` dengan cara menggunakan perintah cpio dan mengompresnya dengan `gzip`
```bash
chmod +x init
find . | cpio -oHnewc | gzip > ../single.gz
cd .. # osboot/
sudo rm -r single
```


### Screenshot _(Screenshot)_
Masukkan screenshot hasil eksekusi program atau proses yang relevan.  
_Insert screenshots of program execution results or other relevant processes._

<img width="616" height="56" alt="image" src="https://github.com/user-attachments/assets/b3132260-b715-47c4-b712-4aed2e56a991" />

### Kode Penuh _(Full Code)_
Masukkan kode lengkap yang digunakan untuk menyelesaikan bagian ini.  
_Insert the full source code used to solve this section._

```bash
#!/bin/bash

cd osboot # osboot/
mkdir -p single/{bin,dev,proc,sys}

cp -a /dev/null single/dev
cp -a /dev/tty* single/dev
cp -a /dev/zero single/dev
cp -a /dev/console single/dev

cp /usr/bin/busybox single/bin
cd single/bin # osboot/single/bin
./busybox --install .

cd .. # osboot/single
touch init
cat <<EOF > init
#!/bin/sh
/bin/mount -t proc none /proc
/bin/mount -t sysfs none /sys

clear
exec /bin/sh
EOF

chmod +x init
find . | cpio -oHnewc | gzip > ../single.gz
cd .. # osboot/
sudo rm -r single
```

## C. Langkah-langkah & Potongan Kode, Screenshot, Kode Penuh  
_(Steps & Code Snippets, Screenshot, Full Code)_

### Langkah-langkah & Potongan Kode _(Steps & Code Snippets)_
Jelaskan langkah-langkah yang dilakukan dan berikan potongan kode dari langkah-langkah yang kalian jelaskan jika ada.  
_Explain the steps performed and include relevant code snippets from the steps you describe if applicable._

1. Masuk ke dalam directory `osboot/` untuk membuat directory `multi/` yang memiliki directory-directory lain seperti `bin/`, `dev/`, `proc/` dan lainnya.
```bash
cd osboot # osboot
mkdir -p multi/{bin,dev,proc,sys,etc,root,home/{blue,green,red,pink}/}
```

2. Meng-copy file milik system host seperti `null`, `tty`, `zero` dan `console` kedalam directory `multi/dev`
```bash
cp -a /dev/null multi/dev
cp -a /dev/tty* multi/dev
cp -a /dev/zero multi/dev
cp -a /dev/console multi/dev
```

3. Menyalin file `BusyBox` dan melakukan instalasi utilitas milik `BusyBox`
```bash
cp /usr/bin/busybox multi/bin
cd multi/bin # osboot/multi/bin
./busybox --install .
```

4. Masuk ke dalam directory `etc` kemudian membuat file `passwd` yang akan berisi password setiap user yang sudah di hash menggunakan command `openssl passwd` 
```bash
cd .. # osboot/multi
cd etc # osboot/multi/etc

touch passwd
cat<<EOF > passwd
root:$(openssl passwd -1 root123):0:0:root:/root:/bin/sh
blue:$(openssl passwd -1 blue123):1000:67:userBlue:/home/blue:/bin/sh
green:$(openssl passwd -1 green123):1001:68:userGreen:/home/green:/bin/sh
red:$(openssl passwd -1 red123):1002:69:userRed:/home/red:/bin/sh
pink:$(openssl passwd -1 pink123):1003:76:userPink:/home/pink:/bin/sh
EOF
```

5. Lalu membuat file `group` yang akan berisi grup-grup setiap usernya.
```bash
touch group
cat<<EOF > group    
root:x:0:
bin:x:1:root
sys:x:2:root
tty:x:5:root,blue,green,red,pink
disk:x:6:root
wheel:x:10:root,blue
blue:x:67:blue
green:x:68:blue,green
red:x:69:blue,green,red
pink:x:76:blue,green,red,pink
EOF
```

6. Melakukan perbuahan permission untuk setiap directory `home/[user]` agar setiap user yang bisa mengakses user lainnya, hanya bisa membaca dan mengeksekusi saja
```bash
cd .. # osboot/multi
chown 1000:67 home/blue
chmod 750 home/blue
chown 1001:68 home/green
chmod 750 home/green
chown 1002:69 home/red
chmod 750 home/red
chown 1003:76 home/pink
chmod 750 home/pink
```

7. Membuat file `init` yang berisi instruksi untuk mounting sistem file dan menjalankan login ke user
```bash
touch init
cat<<EOF > init
#!/bin/sh
/bin/mount -t proc none /proc
/bin/mount -t sysfs none /sys

while true
do
        /bin/getty -L tty1 115200 vt100
        sleep 1
done
EOF
chmod +x init
```

8. Masuk ke dalam directory `/etc` dan membuat directory `loginArt` untuk memuat `ASCII art` yang akan di tampilkan setiap kali user login
```bash
cd etc # osboot/multi/etc
mkdir loginArt
cd loginArt # osboot/multi/etc/loginArt

touch blue.txt
cat<<EOF > blue.txt
 __  __     __        ______     __         __  __     ______    
/\ \_\ \   /\ \      /\  == \   /\ \       /\ \/\ \   /\  ___\   
\ \  __ \  \ \ \     \ \  __<   \ \ \____  \ \ \_\ \  \ \  __\   
 \ \_\ \_\  \ \_\     \ \_____\  \ \_____\  \ \_____\  \ \_____\ 
  \/_/\/_/   \/_/      \/_____/   \/_____/   \/_____/   \/_____/ 
EOF

touch green.txt
cat<<EOF > green.txt
 __  __     __        ______     ______     ______     ______     __   __    
/\ \_\ \   /\ \      /\  ___\   /\  == \   /\  ___\   /\  ___\   /\ "-.\ \   
\ \  __ \  \ \ \     \ \ \__ \  \ \  __<   \ \  __\   \ \  __\   \ \ \-.  \  
 \ \_\ \_\  \ \_\     \ \_____\  \ \_\ \_\  \ \_____\  \ \_____\  \ \_\\"\_\ 
  \/_/\/_/   \/_/      \/_____/   \/_/ /_/   \/_____/   \/_____/   \/_/ \/_/ 
EOF

touch red.txt
cat<<EOF > red.txt
 __  __     __        ______     ______     _____    
/\ \_\ \   /\ \      /\  == \   /\  ___\   /\  __-.  
\ \  __ \  \ \ \     \ \  __<   \ \  __\   \ \ \/\ \ 
 \ \_\ \_\  \ \_\     \ \_\ \_\  \ \_____\  \ \____- 
  \/_/\/_/   \/_/      \/_/ /_/   \/_____/   \/____/ 
EOF

touch pink.txt
cat<<EOF > pink.txt
 __  __     __        ______   __     __   __     __  __    
/\ \_\ \   /\ \      /\  == \ /\ \   /\ "-.\ \   /\ \/ /    
\ \  __ \  \ \ \     \ \  _-/ \ \ \  \ \ \-.  \  \ \  _"-.  
 \ \_\ \_\  \ \_\     \ \_\    \ \_\  \ \_\\"\_\  \ \_\ \_\ 
  \/_/\/_/   \/_/      \/_/     \/_/   \/_/ \/_/   \/_/\/_/ 
EOF

touch root.txt
cat<<EOF > root.txt
                                                          
▄▄▄   ▄▄▄ ▄▄▄▄▄   ▄▄▄▄▄▄▄     ▄▄▄▄▄     ▄▄▄▄▄   ▄▄▄▄▄▄▄▄▄ 
███   ███  ███    ███▀▀███▄ ▄███████▄ ▄███████▄ ▀▀▀███▀▀▀ 
█████████  ███    ███▄▄███▀ ███   ███ ███   ███    ███    
███▀▀▀███  ███    ███▀▀██▄  ███▄▄▄███ ███▄▄▄███    ███    
███   ███ ▄███▄   ███  ▀███  ▀█████▀   ▀█████▀     ███    
                                                          
> BEWARE you have entered SUPERUSER mode
EOF

cd .. # osboot/multi/etc
touch profile
cat<<EOF > profile
USR=\$(whoami)

clear
if [ "\$USR" = "blue" ]; then
        cat /etc/loginArt/blue.txt
elif [ "\$USR" = "green" ]; then
        cat /etc/loginArt/green.txt
elif [ "\$USR" = "red" ]; then
        cat /etc/loginArt/red.txt
elif [ "\$USR" = "pink" ]; then
        cat /etc/loginArt/pink.txt
elif [ "\$USR" = "root" ]; then
        cat /etc/loginArt/root.txt
fi
EOF

chmod 644 loginArt/blue.txt
chmod 644 loginArt/green.txt
chmod 644 loginArt/red.txt
chmod 644 loginArt/pink.txt
chmod 644 loginArt/root.txt
```

9. Kembali ke directory `osboot/multi` dan mengompresi menjadi file `gzip` untuk mendapatkan image `initramfs`.
```bash
cd .. # osboot/multi
find . | cpio -oHnewc | gzip > ../multi.gz
cd .. # osboot/
sudo rm -r multi
```

### Screenshot _(Screenshot)_
Masukkan screenshot hasil eksekusi program atau proses yang relevan.  
_Insert screenshots of program execution results or other relevant processes._

<img width="622" height="51" alt="image" src="https://github.com/user-attachments/assets/c2cab91c-ca5e-4147-a00a-b667e3c2cc0e" />

### Kode Penuh _(Full Code)_
Masukkan kode lengkap yang digunakan untuk menyelesaikan bagian ini.  
_Insert the full source code used to solve this section._

```bash
#!/bin/bash

cd osboot # osboot
mkdir -p multi/{bin,dev,proc,sys,etc,root,home/{blue,green,red,pink}/}

cp -a /dev/null multi/dev
cp -a /dev/tty* multi/dev
cp -a /dev/zero multi/dev
cp -a /dev/console multi/dev

cp /usr/bin/busybox multi/bin
cd multi/bin # osboot/multi/bin
./busybox --install .

cd .. # osboot/multi
cd etc # osboot/multi/etc

touch passwd
cat<<EOF > passwd
root:$(openssl passwd -1 root123):0:0:root:/root:/bin/sh
blue:$(openssl passwd -1 blue123):1000:67:userBlue:/home/blue:/bin/sh
green:$(openssl passwd -1 green123):1001:68:userGreen:/home/green:/bin/sh
red:$(openssl passwd -1 red123):1002:69:userRed:/home/red:/bin/sh
pink:$(openssl passwd -1 pink123):1003:76:userPink:/home/pink:/bin/sh
EOF

touch group
cat<<EOF > group    
root:x:0:
bin:x:1:root
sys:x:2:root
tty:x:5:root,blue,green,red,pink
disk:x:6:root
wheel:x:10:root,blue
blue:x:67:blue
green:x:68:blue,green
red:x:69:blue,green,red
pink:x:76:blue,green,red,pink
EOF

cd .. # osboot/multi
chown 1000:67 home/blue
chmod 750 home/blue
chown 1001:68 home/green
chmod 750 home/green
chown 1002:69 home/red
chmod 750 home/red
chown 1003:76 home/pink
chmod 750 home/pink

touch init
cat<<EOF > init
#!/bin/sh
/bin/mount -t proc none /proc
/bin/mount -t sysfs none /sys

while true
do
        /bin/getty -L tty1 115200 vt100
        sleep 1
done
EOF
chmod +x init

cd etc # osboot/multi/etc
mkdir loginArt
cd loginArt # osboot/multi/etc/loginArt

touch blue.txt
cat<<EOF > blue.txt
 __  __     __        ______     __         __  __     ______    
/\ \_\ \   /\ \      /\  == \   /\ \       /\ \/\ \   /\  ___\   
\ \  __ \  \ \ \     \ \  __<   \ \ \____  \ \ \_\ \  \ \  __\   
 \ \_\ \_\  \ \_\     \ \_____\  \ \_____\  \ \_____\  \ \_____\ 
  \/_/\/_/   \/_/      \/_____/   \/_____/   \/_____/   \/_____/ 
EOF

touch green.txt
cat<<EOF > green.txt
 __  __     __        ______     ______     ______     ______     __   __    
/\ \_\ \   /\ \      /\  ___\   /\  == \   /\  ___\   /\  ___\   /\ "-.\ \   
\ \  __ \  \ \ \     \ \ \__ \  \ \  __<   \ \  __\   \ \  __\   \ \ \-.  \  
 \ \_\ \_\  \ \_\     \ \_____\  \ \_\ \_\  \ \_____\  \ \_____\  \ \_\\"\_\ 
  \/_/\/_/   \/_/      \/_____/   \/_/ /_/   \/_____/   \/_____/   \/_/ \/_/ 
EOF

touch red.txt
cat<<EOF > red.txt
 __  __     __        ______     ______     _____    
/\ \_\ \   /\ \      /\  == \   /\  ___\   /\  __-.  
\ \  __ \  \ \ \     \ \  __<   \ \  __\   \ \ \/\ \ 
 \ \_\ \_\  \ \_\     \ \_\ \_\  \ \_____\  \ \____- 
  \/_/\/_/   \/_/      \/_/ /_/   \/_____/   \/____/ 
EOF

touch pink.txt
cat<<EOF > pink.txt
 __  __     __        ______   __     __   __     __  __    
/\ \_\ \   /\ \      /\  == \ /\ \   /\ "-.\ \   /\ \/ /    
\ \  __ \  \ \ \     \ \  _-/ \ \ \  \ \ \-.  \  \ \  _"-.  
 \ \_\ \_\  \ \_\     \ \_\    \ \_\  \ \_\\"\_\  \ \_\ \_\ 
  \/_/\/_/   \/_/      \/_/     \/_/   \/_/ \/_/   \/_/\/_/ 
EOF

touch root.txt
cat<<EOF > root.txt
                                                          
▄▄▄   ▄▄▄ ▄▄▄▄▄   ▄▄▄▄▄▄▄     ▄▄▄▄▄     ▄▄▄▄▄   ▄▄▄▄▄▄▄▄▄ 
███   ███  ███    ███▀▀███▄ ▄███████▄ ▄███████▄ ▀▀▀███▀▀▀ 
█████████  ███    ███▄▄███▀ ███   ███ ███   ███    ███    
███▀▀▀███  ███    ███▀▀██▄  ███▄▄▄███ ███▄▄▄███    ███    
███   ███ ▄███▄   ███  ▀███  ▀█████▀   ▀█████▀     ███    
                                                          
> BEWARE you have entered SUPERUSER mode
EOF

cd .. # osboot/multi/etc
touch profile
cat<<EOF > profile
USR=\$(whoami)

clear
if [ "\$USR" = "blue" ]; then
        cat /etc/loginArt/blue.txt
elif [ "\$USR" = "green" ]; then
        cat /etc/loginArt/green.txt
elif [ "\$USR" = "red" ]; then
        cat /etc/loginArt/red.txt
elif [ "\$USR" = "pink" ]; then
        cat /etc/loginArt/pink.txt
elif [ "\$USR" = "root" ]; then
        cat /etc/loginArt/root.txt
fi
EOF

chmod 644 loginArt/blue.txt
chmod 644 loginArt/green.txt
chmod 644 loginArt/red.txt
chmod 644 loginArt/pink.txt
chmod 644 loginArt/root.txt

cd .. # osboot/multi
find . | cpio -oHnewc | gzip > ../multi.gz
cd .. # osboot/
sudo rm -r multi
```

## D. Langkah-langkah & Potongan Kode, Screenshot, Kode Penuh  
_(Steps & Code Snippets, Screenshot, Full Code)_

### Langkah-langkah & Potongan Kode _(Steps & Code Snippets)_
Jelaskan langkah-langkah yang dilakukan dan berikan potongan kode dari langkah-langkah yang kalian jelaskan jika ada.  
_Explain the steps performed and include relevant code snippets from the steps you describe if applicable._

1. Membuat directory `mylinuxiso/root/grub` di dalam directory `osboot/` dan meng-copy file `bzImage`, `single.gz`, dan `multi.gz` kedalam `mylinux/boot`
```bash
#!/bin/bash

cd osboot # osboot
mkdir -p mylinuxiso/boot/grub

cp bzImage mylinuxiso/boot
cp single.gz mylinuxiso/boot
cp multi.gz mylinuxiso/boot
```

2. Lalu membuat file `grub.cfg` di dalam directory `grub/` yang akan berisi menu grub saat file `.iso` dijalankan
```bash
cd mylinuxiso/boot/grub # osboot/mylinuxiso/boot/grub
touch grub.cfg
cat<<EOF > grub.cfg
set timeout=5
set default=0

menuentry "single" {
    linux /boot/bzImage
    initrd /boot/single.gz
    }

menuentry "multi" {
    linux /boot/bzImage
    initrd /boot/multi.gz
    }
EOF
```

3. Terakhir, kembali ke directory `osboot/` untuk membuat file ISO bootable
```bash
cd .. # osboot/mylinuxiso/boot
cd .. # osboot/mylinux
cd .. # osboot
grub-mkrescue -o mylinux.iso mylinuxiso

rm -r mylinuxiso
```

### Screenshot _(Screenshot)_
Masukkan screenshot hasil eksekusi program atau proses yang relevan.  
_Insert screenshots of program execution results or other relevant processes._

<img width="713" height="267" alt="image" src="https://github.com/user-attachments/assets/b1e028a3-23f8-4e91-9b8c-852caed32d87" />

### Kode Penuh _(Full Code)_
Masukkan kode lengkap yang digunakan untuk menyelesaikan bagian ini.  
_Insert the full source code used to solve this section._

```bash
#!/bin/bash

cd osboot # osboot
mkdir -p mylinuxiso/boot/grub

cp bzImage mylinuxiso/boot
cp single.gz mylinuxiso/boot
cp multi.gz mylinuxiso/boot

cd mylinuxiso/boot/grub # osboot/mylinuxiso/boot/grub
touch grub.cfg
cat<<EOF > grub.cfg
set timeout=5
set default=0

menuentry "single" {
    linux /boot/bzImage
    initrd /boot/single.gz
    }

menuentry "multi" {
    linux /boot/bzImage
    initrd /boot/multi.gz
    }
EOF

cd .. # osboot/mylinuxiso/boot
cd .. # osboot/mylinux
cd .. # osboot
grub-mkrescue -o mylinux.iso mylinuxiso

rm -r mylinuxiso
```

## E. Langkah-langkah & Potongan Kode, Screenshot, Kode Penuh  
_(Steps & Code Snippets, Screenshot, Full Code)_

### Langkah-langkah & Potongan Kode _(Steps & Code Snippets)_
Jelaskan langkah-langkah yang dilakukan dan berikan potongan kode dari langkah-langkah yang kalian jelaskan jika ada.  
_Explain the steps performed and include relevant code snippets from the steps you describe if applicable._

1. Menggunakan if statement untuk membandingkan input argumen pertama dengan yang di inginkan soal, jika argumen pertama adalah `--single` maka akan menjalankan qemu untuk single user
```bash
#!/bin/bash

cd osboot # osboot
if [ "$1" = "--single" ]; then
    qemu-system-x86_64 \
    -smp 2 \
    -m 256 \
    -display curses \
    -vga std \
    -kernel bzImage \
    -initrd single.gz
```

2. Jika argumennya adalah `--multi` akan menjalankan qemu untuk multi user
```bash
elif [ "$1" = "--multi" ]; then
    qemu-system-x86_64 \
    -smp 2 \
    -m 256 \
    -display curses \
    -vga std \
    -kernel bzImage \
    -initrd multi.gz
```

3. Dan jika argumennya merupakan `-all` maka akan menjalankan qemu melalui ISO
```bash
elif [ "$1" = "--all" ]; then
    qemu-system-x86_64 \
    -smp 2 \
    -m 256 \
    -display curses \
    -vga std \
    -cdrom mylinux.iso
```

4. Lalu jika argumen tidak ada yang cocok akan mengeluarkan petunjuk seperti dibawah.
```bash
else
    echo "Usage of $0 must be followed by --[single/multi/all]"
    exit 1
fi
```

### Screenshot _(Screenshot)_
Masukkan screenshot hasil eksekusi program atau proses yang relevan.  
_Insert screenshots of program execution results or other relevant processes._

**Single User**
<img width="1435" height="632" alt="image" src="https://github.com/user-attachments/assets/6f8702be-3d97-415c-b7cc-56efef7d2726" />

**Multi User**
`root`
<img width="1214" height="395" alt="image" src="https://github.com/user-attachments/assets/362344fb-34ce-46c5-a36c-2b20117b1b92" />

`Blue`
<img width="1259" height="380" alt="image" src="https://github.com/user-attachments/assets/88b3fffd-a1e6-4f9b-8c9f-6d9a37fd2a23" />

`Green`
<img width="1272" height="388" alt="image" src="https://github.com/user-attachments/assets/cd7c25e1-d66f-4127-9307-ad1d785844e9" />

`Red`
<img width="1263" height="402" alt="image" src="https://github.com/user-attachments/assets/34c23218-1f9f-40d1-ab6a-3215ef44fa10" />

`Pink`
<img width="1155" height="346" alt="image" src="https://github.com/user-attachments/assets/14f8fa8b-f289-4e6b-8942-b01e1ed2ec66" />

**Melalui ISO**
<img width="1297" height="750" alt="image" src="https://github.com/user-attachments/assets/1125ee2f-d55f-4fe0-abb4-c7c81524bb6d" />

### Kode Penuh _(Full Code)_
Masukkan kode lengkap yang digunakan untuk menyelesaikan bagian ini.  
_Insert the full source code used to solve this section._

```bash
#!/bin/bash

cd osboot # osboot
if [ "$1" = "--single" ]; then
    qemu-system-x86_64 \
    -smp 2 \
    -m 256 \
    -display curses \
    -vga std \
    -kernel bzImage \
    -initrd single.gz
elif [ "$1" = "--multi" ]; then
    qemu-system-x86_64 \
    -smp 2 \
    -m 256 \
    -display curses \
    -vga std \
    -kernel bzImage \
    -initrd multi.gz
elif [ "$1" = "--all" ]; then
    qemu-system-x86_64 \
    -smp 2 \
    -m 256 \
    -display curses \
    -vga std \
    -cdrom mylinux.iso
else
    echo "Usage of $0 must be followed by --[single/multi/all]"
    exit 1
fi
```

## F. Langkah-langkah & Potongan Kode, Screenshot, Kode Penuh  
_(Steps & Code Snippets, Screenshot, Full Code)_

### Langkah-langkah & Potongan Kode _(Steps & Code Snippets)_
Jelaskan langkah-langkah yang dilakukan dan berikan potongan kode dari langkah-langkah yang kalian jelaskan jika ada.  
_Explain the steps performed and include relevant code snippets from the steps you describe if applicable._

1. Mengambil tanggal disaat script dijalankan dalam format DDMMYY_HHMMSS, kemudian melakukan zip secara rekursif kedalam directory `osboot/` dengan nama `backup_DDMMYY_HHMMSS`.
```bash
DT=$(date "+%d%m%Y_%H%M%S")

zip -r osboot/backup_"$DT".zip  osboot
```

### Screenshot _(Screenshot)_
Masukkan screenshot hasil eksekusi program atau proses yang relevan.  
_Insert screenshots of program execution results or other relevant processes._

<img width="620" height="123" alt="image" src="https://github.com/user-attachments/assets/656c2774-a78d-4b00-9c18-43e6c2e3fe06" />


### Kode Penuh _(Full Code)_
Masukkan kode lengkap yang digunakan untuk menyelesaikan bagian ini.  
_Insert the full source code used to solve this section._

```bash
#!/bin/bash

DT=$(date "+%d%m%Y_%H%M%S")

zip -r osboot/backup_"$DT".zip  osboot
```

---
