📝 Laporan Tugas Akhir

**Mata Kuliah**: Sistem Operasi
**Semester**: Genap / Tahun Ajaran 2024–2025
**Nama**: `Handika Dwi Ardiyanto`
**NIM**: `<240202863>`
**Modul yang Dikerjakan**:
`Modul 1 – System Call dan Instrumentasi Kernel`

---

## 📌 Deskripsi Singkat Tugas

Pada modul ini, saya mengimplementasikan dua system call baru ke dalam kernel xv6-public. System call tersebut bertujuan untuk:

* Mengambil informasi proses aktif melalui `getpinfo()`
* Menghitung jumlah total pemanggilan fungsi `read()` sejak sistem boot dengan `getreadcount()`

---

## 🛠️ Rincian Implementasi

Langkah-langkah implementasi yang dilakukan adalah sebagai berikut:

* Menambahkan struktur `struct pinfo` ke dalam file `proc.h`
* Menambahkan global counter `readcount` di `sysproc.c`
* Menambahkan nomor syscall baru di `syscall.h`
* Mendaftarkan syscall ke tabel di `syscall.c`
* Menambahkan deklarasi syscall baru di `user.h` dan `usys.S`
* Mengimplementasikan `sys_getpinfo()` dan `sys_getreadcount()` di `sysproc.c`
* Menambahkan counter `readcount++` di awal fungsi `sys_read()` di `sysfile.c`
* Membuat dua program uji:

  * `ptest.c` → untuk menguji `getpinfo()`
  * `rtest.c` → untuk menguji `getreadcount()`
* Menambahkan file uji ke bagian `UPROGS` di `Makefile`

---

## ✅ Uji Fungsionalitas

Program uji yang digunakan:

* ✅ `ptest` — menguji system call `getpinfo()`
* ✅ `rtest` — menguji system call `getreadcount()`

Contoh perintah pengujian di shell xv6:

```bash
$ ptest
PID	MEM	NAME
1	4096	init
2	2048	sh
3	2048	ptest
...

$ rtest
Read Count Sebelum: 4
hello
Read Count Setelah: 5
```

---

## 📷 Hasil Uji
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/46a8e8b3-0872-4ba0-a817-dbeec83c567d" />


### 📍 Output `ptest`:

```
PID	MEM	NAME
1	4096	init
2	2048	sh
3	2048	ptest
```

### 📍 Output `rtest`:

```
Read Count Sebelum: 4
hello
Read Count Setelah: 5
```

## ⚠️ Kendala yang Dihadapi

* Awalnya salah menggunakan pointer `ptable` pada `sys_getpinfo`, sehingga data tidak muncul di user-level
* Lupa menambahkan forward declaration `struct pinfo` di `user.h` menyebabkan error saat kompilasi
* Lupa mendaftarkan program `ptest` dan `rtest` di `Makefile`, menyebabkan program tidak tersedia di shell xv6

---

## 📚 Referensi

* Mencari pemahaman dan pengarahan dari ai ataupun chat GPT
* Diskusi praktikum dan dokumentasi kernel
* Stack Overflow untuk error debugging umum di `xv6`

---
