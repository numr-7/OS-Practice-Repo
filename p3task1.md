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

3. 
```bash
cp /usr/bin/busybox single/bin
cd single/bin # osboot/single/bin
./busybox --install .
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

- 

### Screenshot _(Screenshot)_
Masukkan screenshot hasil eksekusi program atau proses yang relevan.  
_Insert screenshots of program execution results or other relevant processes._

- 

### Kode Penuh _(Full Code)_
Masukkan kode lengkap yang digunakan untuk menyelesaikan bagian ini.  
_Insert the full source code used to solve this section._

- 

## D. Langkah-langkah & Potongan Kode, Screenshot, Kode Penuh  
_(Steps & Code Snippets, Screenshot, Full Code)_

### Langkah-langkah & Potongan Kode _(Steps & Code Snippets)_
Jelaskan langkah-langkah yang dilakukan dan berikan potongan kode dari langkah-langkah yang kalian jelaskan jika ada.  
_Explain the steps performed and include relevant code snippets from the steps you describe if applicable._

- 

### Screenshot _(Screenshot)_
Masukkan screenshot hasil eksekusi program atau proses yang relevan.  
_Insert screenshots of program execution results or other relevant processes._

- 

### Kode Penuh _(Full Code)_
Masukkan kode lengkap yang digunakan untuk menyelesaikan bagian ini.  
_Insert the full source code used to solve this section._

- 

## E. Langkah-langkah & Potongan Kode, Screenshot, Kode Penuh  
_(Steps & Code Snippets, Screenshot, Full Code)_

### Langkah-langkah & Potongan Kode _(Steps & Code Snippets)_
Jelaskan langkah-langkah yang dilakukan dan berikan potongan kode dari langkah-langkah yang kalian jelaskan jika ada.  
_Explain the steps performed and include relevant code snippets from the steps you describe if applicable._

- 

### Screenshot _(Screenshot)_
Masukkan screenshot hasil eksekusi program atau proses yang relevan.  
_Insert screenshots of program execution results or other relevant processes._

- 

### Kode Penuh _(Full Code)_
Masukkan kode lengkap yang digunakan untuk menyelesaikan bagian ini.  
_Insert the full source code used to solve this section._

- 

## F. Langkah-langkah & Potongan Kode, Screenshot, Kode Penuh  
_(Steps & Code Snippets, Screenshot, Full Code)_

### Langkah-langkah & Potongan Kode _(Steps & Code Snippets)_
Jelaskan langkah-langkah yang dilakukan dan berikan potongan kode dari langkah-langkah yang kalian jelaskan jika ada.  
_Explain the steps performed and include relevant code snippets from the steps you describe if applicable._

- 

### Screenshot _(Screenshot)_
Masukkan screenshot hasil eksekusi program atau proses yang relevan.  
_Insert screenshots of program execution results or other relevant processes._

- 

### Kode Penuh _(Full Code)_
Masukkan kode lengkap yang digunakan untuk menyelesaikan bagian ini.  
_Insert the full source code used to solve this section._

- 

---
