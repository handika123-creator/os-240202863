# 📝 Laporan Tugas Akhir

**Mata Kuliah**: Sistem Operasi

**Semester**: Genap / Tahun Ajaran 2024–2025

**Nama**: `<Handika Dwi Ardiyanto>`

**NIM**: `<240202863>`

**Modul yang Dikerjakan**:
`Modul 2 – Penjadwalan CPU Lanjutan (Priority Scheduling Non-Preemptive)`

---

## 📌 Deskripsi Singkat Tugas

Pada modul ini, sistem penjadwalan proses diubah dari algoritma default **Round Robin** menjadi **Non-Preemptive Priority Scheduling**. Perubahan ini memungkinkan proses dengan **prioritas lebih tinggi (angka lebih kecil)** dijalankan lebih dulu, dan proses lain menunggu walaupun sudah dalam keadaan RUNNABLE.

---

## 🛠️ Rincian Implementasi

Langkah-langkah yang saya lakukan dalam implementasi modul ini:

* Menambahkan field `int priority` ke dalam `struct proc` di `proc.h`
* Inisialisasi nilai default `priority = 60` pada fungsi `allocproc()` di `proc.c`
* Membuat system call baru `set_priority(int)`:

  * Menambahkan `#define SYS_set_priority 24` di `syscall.h`
  * Menambahkan deklarasi di `user.h` dan `usys.S`
  * Mendaftarkan syscall ke tabel `syscalls[]` di `syscall.c`
  * Mengimplementasikan fungsi `sys_set_priority()` di `sysproc.c`
* Memodifikasi fungsi `scheduler()` di `proc.c` agar memilih proses RUNNABLE dengan `priority` terendah (paling tinggi)
* Menambahkan program uji `ptest.c` untuk mengamati perilaku scheduler dengan dua proses anak yang memiliki prioritas berbeda
* Mendaftarkan `ptest.c` ke bagian `UPROGS` di `Makefile`

---

## ✅ Uji Fungsionalitas

Program uji yang digunakan:

* `ptest`: untuk menguji bahwa proses dengan prioritas lebih tinggi (angka lebih kecil) dijalankan terlebih dahulu, meskipun proses lain sudah RUNNABLE.

Contoh perintah uji:

```bash
$ ptest
Child 2 selesai
Child 1 selesai
Parent selesai
```

> Di mana `Child 2` memiliki prioritas lebih tinggi (misal: 10) dibanding `Child 1` (misal: 90)

---

## 📷 Hasil Uji
<img width="960" height="540" alt="Screenshot 2025-07-28 171448" src="https://github.com/user-attachments/assets/db284e06-413a-492f-9ee2-b29d0472c56e" />


### 📍 Output `ptest`:

```
Child 2 selesai
Child 1 selesai
Parent selesai
---

## ⚠️ Kendala yang Dihadapi

* Salah membandingkan prioritas (`>` bukan `<`) menyebabkan proses prioritas rendah berjalan lebih dulu
* Lupa mengatur nilai default priority di `allocproc()` menyebabkan proses tidak diprioritaskan dengan benar
* Tidak menambahkan `set_priority()` di `usys.S` → menyebabkan error saat link user program

---

## 📚 Referensi

* Mencari dan meminta pengarahan melalui ai atau chat GPT
