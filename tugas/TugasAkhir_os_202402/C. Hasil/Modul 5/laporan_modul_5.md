# 📝 Laporan Tugas Akhir

**Mata Kuliah**: Sistem Operasi

**Semester**: Genap / Tahun Ajaran 2024–2025

**Nama**: `<Handika Dwi Ardiyanto>`

**NIM**: `<240202863>`

**Modul yang Dikerjakan**:
`Modul 5 – Audit dan Keamanan Sistem (xv6-public)`

---

## 📌 Deskripsi Singkat Tugas

* **Modul 5 – Audit dan Keamanan Sistem**:
  Membangun sistem audit log sederhana di dalam kernel xv6 untuk mencatat setiap system call yang dipanggil oleh proses, termasuk PID pemanggil, nomor syscall, dan waktu (tick). Audit log ini hanya bisa diakses oleh proses dengan PID 1 (init).

---

## 🛠️ Rincian Implementasi

* Menambahkan struktur `audit_entry` dan array `audit_log[]` di `syscall.c`
* Memodifikasi fungsi `syscall()` untuk mencatat semua system call yang valid
* Menambahkan syscall baru `get_audit_log()`:

  * Didefinisikan di `sysproc.c`
  * Didaftarkan di `syscall.c`, `syscall.h`, `user.h`, `usys.S`
* Membatasi akses audit log hanya untuk proses dengan `pid == 1`
* Menambahkan program uji `audit.c` untuk membaca dan mencetak isi audit log
* Mendaftarkan program `audit` ke dalam `Makefile`

---

## ✅ Uji Fungsionalitas

Program uji yang digunakan:

* `audit`: membaca dan mencetak seluruh isi log system call.

  * Jika dijalankan oleh proses non-init: akan ditolak.
  * Jika dijalankan sebagai PID 1 (misalnya mengganti `init`), maka dapat mengakses log dengan benar.

---

## 📷 Hasil Uji
<img width="960" height="540" alt="Screenshot 2025-07-31 125520" src="https://github.com/user-attachments/assets/f201fec9-3750-4551-b343-27e9cf57dff5" />


### 📍 Output `audit` jika dijalankan bukan oleh PID 1:

```
Access denied or error.
```

### 📍 Output `audit` jika dijalankan sebagai proses `init` (PID 1):

```
=== Audit Log ===
[0] PID=1 SYSCALL=5 TICK=12
[1] PID=1 SYSCALL=6 TICK=13
[2] PID=2 SYSCALL=11 TICK=14
...
```

> Output mencetak log system call dari berbagai proses, lengkap dengan PID dan waktu.

---

## ⚠️ Kendala yang Dihadapi

* Lupa membatasi akses `get_audit_log()` hanya untuk `PID 1`, menyebabkan semua proses bisa membaca log.
* Salah offset saat `memmove()` menyebabkan data log tidak lengkap.
* Audit log bersifat volatile (di RAM), sehingga akan hilang setelah reboot.
* Belum ada mekanisme sinkronisasi (lock), meskipun tidak terjadi race condition pada praktikum.

---

## 📚 Referensi

* Mencari dari sumber Ai dan chat GPT
* Diskusi praktikum dan forum
* Stack Overflow dan dokumentasi C (khusus penggunaan `memmove`, `argptr`, dan `argint`)

---
