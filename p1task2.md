[analisis_b.sh](https://github.com/user-attachments/files/26230704/analisis_b.sh)# Task 2 - Monyet Punch dan Ayam Wilson

## A. Langkah-langkah & Potongan Kode, Screenshot, Kode Penuh  
_(Steps & Code Snippets, Screenshot, Full Code)_

### Langkah-langkah & Potongan Kode _(Steps & Code Snippets)_
Jelaskan langkah-langkah yang dilakukan dan berikan potongan kode dari langkah-langkah yang kalian jelaskan jika ada.  
_Explain the steps performed and include relevant code snippets from the steps you describe if applicable._

File analisi_a.sh di isi dengan command awk,
Menggunakan `-F','` option untuk mengganti separator data menjadi **(,)**  
```bash
awk -F','
``` 

Menggunakan _if_ function untuk mengambil data yang dibutuhkan, yaitu **durasi_detik diatas 7199** dan **tanggal 07-03-2026**
```bash
if($3 > 7199 && $4 ~ /07-03-2026/)
```

Menggunakan fungsi _built-in_ dari awk, yaitu `gsub` untuk mengganti (_) dengan ( )
```bash
gsub(/_/, " ", $2)
```

Lalu print 
```bash
printf($1 " " $2 " selama " $3 " detik pada 07032026 \n")
```  
### Screenshot _(Screenshot)_
Masukkan screenshot hasil eksekusi program atau proses yang relevan.  
_Insert screenshots of program execution results or other relevant processes._

<img width="948" height="228" alt="image" src="https://github.com/user-attachments/assets/20de3a49-01c4-46bb-8c7e-c29509ab56b5" />


### Kode Penuh _(Full Code)_
Masukkan kode lengkap yang digunakan untuk menyelesaikan bagian ini.  
_Insert the full source code used to solve this section._

```bash
awk -F',' '{if($3 > 7199 && $4 ~ /07-03-2026/) {gsub(/_/, " ", $2);  printf($1 " " $2 " selama " $3 " detik pada 07032026 \n");}}' losiento.csv 
```
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
   seberapa banyak `[Wilson]` makan **diantara jam 9 dan jam 19** _(diantara menit ke-540 dan ke-1140)_  
```bash
if($1=="Punch" && tm>=720 && $2 ~ /tidur/){
	ptcount++;}
else if($1=="Wilson" && tm>=540 && tm<=1140 && $2 ~ /makan/){
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
		else if($1=="Wilson" && tm>=540 && tm<=1140 && $2 ~ /makan/){
			wmcount++;}
	}
}
END{
	print("[Punch] tidur sebanyak " ptcount " kali di atas pukul 12.00 siang");
	print("[Wilson] makan sebanyak " wmcount " kali dari pukul 08.00 sampai 19.00");
}' losiento.csv
```



## C. Langkah-langkah & Potongan Kode, Screenshot, Kode Penuh

### Langkah-langkah & Potongan Kode _(Steps & Code Snippets)_
Jelaskan langkah-langkah yang dilakukan dan berikan potongan kode dari langkah-langkah yang kalian jelaskan jika ada.  
_Explain the steps performed and include relevant code snippets from the steps you describe if applicable._

1. Membaca `input` dari user menggunakan fungsi `read -p` agar bisa menampilkan pesan sebelum input dimmulai.
```bash
read -p "Nama : " nama;
read -p "Aktivitas : " akt;
read -p "Durasi : " dur;
read -p "Waktu mulai : " wak;
```

2. Memasukkan hasil `input` kedalam file `losiento.csv`.
```bash
echo "$nama,$akt,$dur,$wak" >> losiento.csv;
```

3. Memisah **hari, bulan dan tahun** dari `input` waktu menggunakan _string extraction_  
   yaitu `${[string]:[offset]:[length]}` yang kemudian digabungkan di variable `tm`.
```bash
dd=${wak:0:2};
mm=${wak:3:2};
yy=${wak:6:4};
tm="$dd$mm$yy";
```

4. Menggabungkan variable `nama` dan `tm` menjadi satu variable yang kemudian akan menjadi nama file.
```bash
fname="${nama}_${tm}.log";
```

5. Terakhir ada _statement_ `if` untuk memisah jenis output
   menggunakan _condition_ `[ $dur -gt 21600 ]` (-gt adalah _greater than_)  
   yang kemudian di tulis di file `$fname`.
```bash
if [ $dur -gt 21600 ]; then
 echo "[ALERT] $wak : $nama $akt selama $dur detik" > "$fname";
else
 echo "[ACCEPTED] $wak : $nama $akt selama $dur detik" > "$fname";
fi
```

### Screenshot _(Screenshot)_
Masukkan screenshot hasil eksekusi program atau proses yang relevan.  
_Insert screenshots of program execution results or other relevant processes._

<img width="690" height="161" alt="image" src="https://github.com/user-attachments/assets/ad7f90fd-2288-4562-9716-901aad8b884b" />
<img width="688" height="162" alt="Screenshot from 2026-03-25 20-19-28" src="https://github.com/user-attachments/assets/6bf27005-791e-4fba-8539-56b68e08aa87" />


### Kode Penuh _(Full Code)_
Masukkan kode lengkap yang digunakan untuk menyelesaikan bagian ini.  
_Insert the full source code used to solve this section._

[input_aktivitas.sh](https://github.com/user-attachments/files/26243911/input_aktivitas.sh)
```bash
#!/bin/bash

read -p "Nama : " nama;
read -p "Aktivitas : " akt;
read -p "Durasi : " dur;
read -p "Waktu mulai : " wak;

echo "$nama,$akt,$dur,$wak" >> losiento.csv;

dd=${wak:0:2};
mm=${wak:3:2};
yy=${wak:6:4};
tm="$dd$mm$yy";

fname="${nama}_${tm}.log";

if [ $dur -gt 21600 ]; then
 echo "[ALERT] $wak : $nama $akt selama $dur detik" > "$fname";
else
 echo "[ACCEPTED] $wak : $nama $akt selama $dur detik" > "$fname";
fi
```

## D. Langkah-langkah & Potongan Kode, Screenshot, Kode Penuh

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
