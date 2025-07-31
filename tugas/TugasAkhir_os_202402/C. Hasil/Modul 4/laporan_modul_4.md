# 📝 Laporan Tugas Akhir

**Mata Kuliah**: Sistem Operasi

**Semester**: Genap / Tahun Ajaran 2024–2025

**Nama**: `<Handika Dwi Ardiyanto>`

**NIM**: `<240202863>`

**Modul yang Dikerjakan**:
`Modul 4 – Subsistem Kernel Alternatif (xv6-public)`

---

## 📌 Deskripsi Singkat Tugas

Pada modul ini saya mengimplementasikan dua fitur baru ke dalam kernel xv6:

* Menambahkan system call baru `chmod(path, mode)` untuk mengatur mode file menjadi **read-only** atau **read-write** secara sederhana.
* Menambahkan driver device baru `/dev/random` sebagai **pseudo-device** yang menghasilkan byte acak ketika dibaca oleh proses user.

---

## 🛠️ Rincian Implementasi

Langkah-langkah implementasi terbagi menjadi dua bagian utama:

### A. System Call `chmod(path, mode)`

* Menambahkan field `short mode` pada `struct inode` di `fs.h` (hanya digunakan di memori).
* Menambahkan syscall `chmod`:

  * `syscall.h`: definisi `SYS_chmod`
  * `user.h`: deklarasi prototipe
  * `usys.S`: entry syscall
  * `syscall.c`: registrasi syscall
  * `sysfile.c`: implementasi `sys_chmod()`
* Memodifikasi `filewrite()` di `file.c` untuk mengecek jika inode dalam mode **read-only**, maka `write()` akan gagal.
* Menambahkan program uji `chmodtest.c` untuk menguji fungsi `chmod`.

### B. Device `/dev/random`

* Membuat file baru `random.c` berisi fungsi `randomread()` untuk menghasilkan byte acak.
* Mendaftarkan device ke dalam `devsw[]` di `file.c` dengan `major = 3`.
* Menambahkan baris `mknod("/dev/random", 1, 3)` pada `init.c` untuk membuat device node saat boot.
* Menambahkan program uji `randomtest.c` untuk membaca byte acak dari `/dev/random`.

---

## ✅ Uji Fungsionalitas

Program uji yang digunakan:

* `chmodtest`: menguji bahwa file yang telah diubah menjadi read-only tidak dapat ditulis kembali.
* `randomtest`: menguji bahwa `/dev/random` menghasilkan byte acak ketika dibaca.

---

## 📷 Hasil Uji
<img width="960" height="540" alt="Screenshot 2025-07-31 123420" src="https://github.com/user-attachments/assets/ebcf8870-d11c-4a88-824d-3643c842812d" />


### 📍 Output `chmodtest`:

```
Write blocked as expected
```

### 📍 Output `randomtest` (acak):

```
83 47 202 19 124 88 53 17
```



## ⚠️ Kendala yang Dihadapi

* Lupa menambahkan `chmod` ke dalam `usys.S` menyebabkan syscall tidak dikenali.
* Salah registrasi index `devsw[]` menyebabkan `/dev/random` tidak berfungsi.
* Field `mode` di `inode` sebenarnya tidak tersimpan di disk layout xv6, sehingga mode akan hilang saat reboot.

---

## 📚 Referensi

* Mencari referensi dari Ai atau Chat GPT
* Diskusi praktikum dan forum internal
---
