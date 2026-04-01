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
else if($1=="Wilson" && tm>=4800 && tm<=1140 && $2 ~ /makan/){
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

## C. Langkah-langkah & Potongan Kode, Screenshot, Kode Penuh  
_(Steps & Code Snippets, Screenshot, Full Code)_

### Langkah-langkah & Potongan Kode _(Steps & Code Snippets)_
Jelaskan langkah-langkah yang dilakukan dan berikan potongan kode dari langkah-langkah yang kalian jelaskan jika ada.  
_Explain the steps performed and include relevant code snippets from the steps you describe if applicable._

- 

### Screenshot _(Screenshot)_
Masukkan screenshot hasil eksekusi program atau proses yang relevan.  
_Insert screenshots of program execution results or other relevant processes._

<img width="1184" height="467" alt="image" src="https://github.com/user-attachments/assets/b85b339a-65b0-4554-af69-c10614a0b39b" />


### Kode Penuh _(Full Code)_
Masukkan kode lengkap yang digunakan untuk menyelesaikan bagian ini.  
_Insert the full source code used to solve this section._

- 
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


# Task [] - []

## []. Langkah-langkah & Potongan Kode, Screenshot, Kode Penuh  
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

...
