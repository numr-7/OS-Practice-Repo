### Laporan Resmi Praktikum Modul 2 _(Module 2 Lab Work Report)_

Tulis laporan di template berikut!

_Write your lab work on the template here!_

---

# Task 1 - Tim Biru
_(Blue Team)_

## A. Langkah-langkah & Potongan Kode, Screenshot, Kode Penuh  
_(Steps & Code Snippets, Screenshot, Full Code)_

### Langkah-langkah & Potongan Kode _(Steps & Code Snippets)_
Jelaskan langkah-langkah yang dilakukan dan berikan potongan kode dari langkah-langkah yang kalian jelaskan jika ada.  
_Explain the steps performed and include relevant code snippets from the steps you describe if applicable._

1. Menginisialisasi fungsi `copyToDir` agar fungsi dapat di panggil di dalam fungsi `copyZIP`.
```C
int copyToDir(zip_t *zipFile, zip_int64_t idx, const char *destDir);
```
2. Fungsi `copyZIP` berfungsi untuk membuka serta menyalin isi zip `evidence.zip` lalu, setiap iterasi/penyalinan file  
   akan memanggil fungsi `copyToDir` yang bertugas untuk menempelkan hasil salinan dari `copyZIP`.
```C
void copyZIP(const char *zipPath){
    const char *destDir = "logs_dump"; // directory dimana isi zip akan di tempelkan
    int errCode; // Error code
    zip_t *zipFile = zip_open(zipPath, ZIP_RDONLY, &errCode);
    if(!zipFile){
        fprintf(stderr, "Failed to open ZIP! Error code -> %d\n", errCode);
        return; // memberhentikan fungsi jika terjadi error (cnth. file zip tidak ada)
    }
    
    zip_int64_t i = 0;
    while(1){ // loop untuk menyalis setiap file di dalam zip
        const char *fname = zip_get_name(zipFile, i , 0);
        if(!fname) break;
        if(fname[strlen(fname) - 1] == '/') {
            i++;
            continue;
        }

        // Mengambil nama file saja (tanpa nama subdirektori)
        const char *nameFile = strrchr(fname, '/');
        nameFile = nameFile ? nameFile + 1 : fname;

        // Menulis file path dan disimpan di dalam destPath
        char destPath[1024];
        snprintf(destPath, sizeof(destPath), "%s/%s", destDir, nameFile);
        
        copyToDir(zipFile, i, destPath);
        i++;
    } 
    zip_close (zipFile);
    remove(zipPath);
}
```
3. Kemudian, fungsi `copyToDir` menempelkan hasil salinan dari `copyZIP`
```C
int copyToDir(zip_t *zipFile, zip_int64_t idx, const char *destDir){
    zip_file_t *zipF = zip_fopen_index(zipFile, idx, 0);
    if(!zipF) return -1; // jika zip tidak bisa dibuka, maka akan berhenti (fail)

    // Membuat file di direktori tujuan 
    FILE *outp = fopen(destDir, "wb");
    if(!outp) {
        zip_fclose(zipF);
        return -1; // jika gagal dalam membuat file output -1
    }

    
    char buff[8192]; // buffer
    zip_int64_t byteRead; // variabel untuk menyimpan banyaknya bit
    while((byteRead = zip_fread(zipF, buff, sizeof(buff))) > 0){ // membaca data dari zip 
        fwrite(buff, 1, byteRead, outp); // Menulis datanya
    }

    // Menutup file dan zip karena sudah selesai
    fclose(outp);
    zip_fclose(zipF);
    return 0;
}
```
4. Fungsi utama yang di berjalan mengambil satu argumen, yaitu file zip yang akan di extrak.
```C
int main(int argc, char *argv[]){ // Argumen count dan argument vector
    if(argc < 2){  // Mengecek jika banyaknya argumen kurang dari dua
        return 1; // non-zero exit code (terjadi error)
    }

    // Membuat direktori logs_dump
    mkdir("logs_dump", 0755);
    copyZIP(argv[1]); // memanggil fungsi copyZIP dan memulai ekstraksi
    return 0;
}
```

### Screenshot _(Screenshot)_
Masukkan screenshot hasil eksekusi program atau proses yang relevan.  
_Insert screenshots of program execution results or other relevant processes._
<p align="center"><img width="683" height="39" alt="image" src="https://github.com/user-attachments/assets/9205e7cc-accf-48e2-95c3-09d2fb980998" />
<img width="1094" height="879" alt="image" src="https://github.com/user-attachments/assets/bf3b3d6e-1286-48f2-997b-17c43ca5584d" /></p>

### Kode Penuh _(Full Code)_
Masukkan kode lengkap yang digunakan untuk menyelesaikan bagian ini.  
_Insert the full source code used to solve this section._

```C
#include <stdio.h>
#include <zip.h>
#include <string.h>
#include <sys/stat.h>
#include <unistd.h>

int copyToDir(zip_t *zipFile, zip_int64_t idx, const char *destDir);

void copyZIP(const char *zipPath){
    const char *destDir = "logs_dump";
    int errCode;
    zip_t *zipFile = zip_open(zipPath, ZIP_RDONLY, &errCode);
    if(!zipFile){
        fprintf(stderr, "Failed to open ZIP! Error code -> %d\n", errCode);
        return;
    }
    
    zip_int64_t i = 0;
    while(1){
        const char *fname = zip_get_name(zipFile, i , 0);
        if(!fname) break;
        if(fname[strlen(fname) - 1] == '/') {
            i++;
            continue;
        }

        const char *nameFile = strrchr(fname, '/');
        nameFile = nameFile ? nameFile + 1 : fname;
        
        char destPath[1024];
        snprintf(destPath, sizeof(destPath), "%s/%s", destDir, nameFile);
        
        copyToDir(zipFile, i, destPath);
        i++;
    } 
    zip_close (zipFile);
    remove(zipPath);
}

int copyToDir(zip_t *zipFile, zip_int64_t idx, const char *destDir){
    zip_file_t *zipF = zip_fopen_index(zipFile, idx, 0);
    if(!zipF) return -1;
    
    FILE *outp = fopen(destDir, "wb");
    if(!outp) {
        zip_fclose(zipF);
        return -1;
    }

    char buff[8192];
    zip_int64_t byteRead;
    while((byteRead = zip_fread(zipF, buff, sizeof(buff))) > 0){
        fwrite(buff, 1, byteRead, outp);
    }

    fclose(outp);
    zip_fclose(zipF);
    return 0;
}

int main(int argc, char *argv[]){
    if(argc < 2){
        return 1;
    }
    
    mkdir("logs_dump", 0755);
    copyZIP(argv[1]);
    return 0;
}
```

## B. Langkah-langkah & Potongan Kode, Screenshot, Kode Penuh  
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

---
