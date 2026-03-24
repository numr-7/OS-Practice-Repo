# Task 2 - Monyet Punch dan Ayam Wilson

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
```
if($3 > 7199 && $4 ~ /07-03-2026/)
```

Menggunakan fungsi _built-in_ dari awk, yaitu `gsub` untuk mengganti (_) dengan ( )
```
gsub(/_/, " ", $2)
```

Lalu print 
```
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

- 

### Screenshot _(Screenshot)_
Masukkan screenshot hasil eksekusi program atau proses yang relevan.  
_Insert screenshots of program execution results or other relevant processes._

- 

### Kode Penuh _(Full Code)_
Masukkan kode lengkap yang digunakan untuk menyelesaikan bagian ini.  
_Insert the full source code used to solve this section._

-

## C. Langkah-langkah & Potongan Kode, Screenshot, Kode Penuh

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
