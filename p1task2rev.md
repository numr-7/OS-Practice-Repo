# Task 2 - Monyet Punch dan Ayam Wilson

## B. Langkah-langkah & Potongan Kode, Screenshot, Kode Penuh

### Langkah-langkah & Potongan Kode _(Steps & Code Snippets)_
Jelaskan langkah-langkah yang dilakukan dan berikan potongan kode dari langkah-langkah yang kalian jelaskan jika ada.  
_Explain the steps performed and include relevant code snippets from the steps you describe if applicable._

1. Program yang saya buat di dalam  menggunakan fungsi `awk`.  
   Sebelum memulai program, menggunakan `-F','` option untuk mengganti separatornya menjadi **(,)**. 
```bash
awk -F',' '
```
2. Statement `if` dengan kondisi _regexp operator_ yaitu pada tanggal **01 hingga 05 Maret 2026**.  
   Operator `~ /[condition]/` digunakan untuk mencari string yang memiliki sub-string sama dengan kondisinya.  
   Sedangkan operator `|` di dalam _regexp_ ibaratnya sama dengan **alternatif atau OR**.

```bash
if($4 ~ /01-03-2026|02-03-2026|03-03-2026|04-03-2026|05-03-2026/){
```
3. Kemudian, menggunakan fungsi `substr()` untuk mengambil **jam dan menit** di column 4.  
   Ekspresi `+0` memaksa `substr` untuk mengubah variabel menjadi `int`,  
   Lalu saya ubah menjadi menit dan di totalkan.
```bash
h=substr($4, 12, 2) +0;
m=substr($4, 15, 2) +0;
tm=(h*60) + m;
```
4. Statement `if` untuk menentukan  
   seberapa banyak `[Punch]` tidur siang **diatas jam 12** _(Diatas menit ke-720)_ dan  
   seberapa banyak `[Wilson]` makan **diantara jam 8 dan jam 19** _(diantara menit ke-480 dan ke-1140)_  
```bash
if($1=="Punch" && tm>=720 && $2 ~ /tidur/){
	ptcount++;}
else if($1=="Wilson" && tm>=480 && tm<=1140 && $2 ~ /makan/){
	wmcount++;}
```
5. Kemudian menggunakan fungsi `END` untuk melakukan `print` agar hanya di lakukan sekali di akhir fungsi `awk`
```bash
END{
	print("[Punch] tidur sebanyak " ptcount " kali di atas pukul 12.00 siang");
	print("[Wilson] makan sebanyak " wmcount " kali dari pukul 08.00 sampai 19.00");
}' losiento.csv
```  
### Screenshot _(Screenshot)_
Masukkan screenshot hasil eksekusi program atau proses yang relevan.  
_Insert screenshots of program execution results or other relevant processes._

 <img width="980" height="97" alt="image" src="https://github.com/user-attachments/assets/3a3f4eb1-15dd-4d9e-a79e-4423152b7918" />

### Kode Penuh _(Full Code)_
Masukkan kode lengkap yang digunakan untuk menyelesaikan bagian ini.  
_Insert the full source code used to solve this section._

[analisis_b.sh](https://github.com/user-attachments/files/26230720/analisis_b.sh)
```bash
#!/bin/bash

awk -F',' '
{
	if($4 ~ /01-03-2026|02-03-2026|03-03-2026|04-03-2026|05-03-2026/){
		h=substr($4, 12, 2) +0;
		m=substr($4, 15, 2) +0;
		tm=(h*60) + m;
		if($1=="Punch" && tm>=720 && $2 ~ /tidur/){
			ptcount++;}
		else if($1=="Wilson" && tm>=480 && tm<=1140 && $2 ~ /makan/){
			wmcount++;}
	}
}
END{
	print("[Punch] tidur sebanyak " ptcount " kali di atas pukul 12.00 siang");
	print("[Wilson] makan sebanyak " wmcount " kali dari pukul 08.00 sampai 19.00");
}' losiento.csv
```

## D. Langkah-langkah & Potongan Kode, Screenshot, Kode Penuh  
_(Steps & Code Snippets, Screenshot, Full Code)_

### Langkah-langkah & Potongan Kode _(Steps & Code Snippets)_
Jelaskan langkah-langkah yang dilakukan dan berikan potongan kode dari langkah-langkah yang kalian jelaskan jika ada.  
_Explain the steps performed and include relevant code snippets from the steps you describe if applicable._

1. Mengambil setiap file `.log` yang ada di directory menggunakan `ls *.log`  
   kemudian memotong menggunakan `cut -d` untuk mengekstrak hanya tanggalnya serta  
   mengurutkan menggunakan `sort -u` (`-u` untuk menghilangkan file duplikat). 
```bash
dates=$(ls *.log | cut -d'_' -f2 | cut -d'.' -f1 | sort -u);
```

2. Menginisiasi loop untuk setiap value/tanggal yang ada di `$dates`. Kemudian melakukan zip terhadap  
   file menggunakan `zip -q` (`-q` digunakan agar command zip tidak mengeluarkan output).  
   Kemudian print yang harus di print. Lalu menunjukan file yang telah di zip menggunakan `unzip -Z1`  
   (`-Z1` digunakan sebagai indikator kalau command `unzip` hanya menampil informasi/isi zip nya).
```bash
for date in $dates; do
        zip -q "backup_log_${date}.zip" *_${date}.log;

        printf "Log berhasil diarsipkan\n.";
        printf "Isi arsip:\n";
        unzip -Z1 "backup_log_${date}.zip";

        rm *_${date}.log;
done
```

### Screenshot _(Screenshot)_
Masukkan screenshot hasil eksekusi program atau proses yang relevan.  
_Insert screenshots of program execution results or other relevant processes._

<img width="1184" height="467" alt="image" src="https://github.com/user-attachments/assets/b85b339a-65b0-4554-af69-c10614a0b39b" />


### Kode Penuh _(Full Code)_
Masukkan kode lengkap yang digunakan untuk menyelesaikan bagian ini.  
_Insert the full source code used to solve this section._

```bash
#!/bin/bash

dates=$(ls *.log | cut -d'_' -f2 | cut -d'.' -f1 | sort -u);

for date in $dates; do
        zip -q "backup_log_${date}.zip" *_${date}.log;

        printf "Log berhasil diarsipkan\n.";
        printf "Isi arsip:\n";
        unzip -Z1 "backup_log_${date}.zip";

        rm *_${date}.log;
done
```

## E. Langkah-langkah & Potongan Kode, Screenshot, Kode Penuh  
_(Steps & Code Snippets, Screenshot, Full Code)_

### Langkah-langkah & Potongan Kode _(Steps & Code Snippets)_
Jelaskan langkah-langkah yang dilakukan dan berikan potongan kode dari langkah-langkah yang kalian jelaskan jika ada.  
_Explain the steps performed and include relevant code snippets from the steps you describe if applicable._

1. Pertama-tama, set directory dimana script ini akan berjalan menggunakan `cd`.
```bash
cd ~/Documents/sisop-modul-1-a10/task-2/revisi;
```

2. Sama seperti script `backup_log.sh`, saya menggunakan command cut untuk memotong dan memisahhkan tanggal dari  
   nama file zip yang ada. Kemudian di sort menggunakan `sort -u`.
```bash
tgl=$(ls *.zip | cut -d'_' -f3 | cut -d'.' -f1 | sort -u);
```

3. Kemudian memulai for loop untuk semua value yang ada di `$tgl`. Di dalam loop ini, saya mengambil substring dari  
   variabel `$tgl` dan memisahnya pada menjadi hari, bulan, serta tahun sendiri-sendiri.
```bash
for tanggal in $tgl; do
	dd=${tanggal:0:2};
	mm=${tanggal:2:2};
	yy=${tanggal:4:4};
```

4. Lalu saya mengkonversi menjadi detik sejah waktu _Epoch_ (yaitu 1970) dengan menggunakan `date -d [...] +%s`. Hal yang sama  
   saya lakukan dengan mengambil waktu hari ini script ini di jalankan dan mengkonversi menjadi _Epoch time._
```bash
	fsec=$(date -d "${yy}-${mm}-${dd}" +%s);
	tsec=$(date +%s);
```

5. Kemudian saya mencari perbedaan dari kedua waktu serta mengkonversi detik menjadi hari lagi. 
```bash
	diffday=$(( (tsec - fsec) / 86400 ));
```

6. Kemmudian menjalankan `if` statement untuk mencari yang menentukan zip folder mana yang akan di remove dengan fungsi `-rm`.
```bash
	if [ "$diffday" -gt 7 ]; then
		rm "backup_log_${tanggal}.zip";
	fi
```

7. Untuk konfigurasi cronjob akan menjalankan `backup_log.sh` setiap hari di jam 23:59 serta  
   `remove_archive.sh` setiap hari di jam 02:34.
```sh
59 23 * * * ~/Documents/sisop-modul-1-a10/task-2/revisi/backup_log.sh
34 02 * * * ~/Documents/sisop-modul-1-a10/task-2/revisi/remove_archive.sh
```

### Screenshot _(Screenshot)_
Masukkan screenshot hasil eksekusi program atau proses yang relevan.  
_Insert screenshots of program execution results or other relevant processes._

<img width="1172" height="176" alt="image" src="https://github.com/user-attachments/assets/ce48abec-f950-4cc8-a58d-c7b142f9f3d6" />


### Kode Penuh _(Full Code)_
Masukkan kode lengkap yang digunakan untuk menyelesaikan bagian ini.  
_Insert the full source code used to solve this section._

```bash
#!/bin/bash

cd ~/Documents/sisop-modul-1-a10/task-2/revisi;

tgl=$(ls *.zip | cut -d'_' -f3 | cut -d'.' -f1 | sort -u);

for tanggal in $tgl; do
	dd=${tanggal:0:2};
	mm=${tanggal:2:2};
	yy=${tanggal:4:4};

	fsec=$(date -d "${yy}-${mm}-${dd}" +%s);
	tsec=$(date +%s);
	diffday=$(( (tsec - fsec) / 86400 ));

	if [ "$diffday" -gt 7 ]; then
		rm "backup_log_${tanggal}.zip";
	fi
done
```

```sh
59 23 * * * ~/Documents/sisop-modul-1-a10/task-2/revisi/backup_log.sh
34 02 * * * ~/Documents/sisop-modul-1-a10/task-2/revisi/remove_archive.sh
```
