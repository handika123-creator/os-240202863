# 📝 Laporan Tugas Akhir

**Mata Kuliah**: Sistem Operasi

**Semester**: Genap / Tahun Ajaran 2024–2025

**Nama**: `<Handika Dwi Ardiyanto>`

**NIM**: `<240202863>`

**Modul yang Dikerjakan**:
`Modul 3 – Manajemen Memori Tingkat Lanjut`

---

## 📌 Deskripsi Singkat Tugas

* **Modul 3 – Manajemen Memori Tingkat Lanjut**:
  Mengimplementasikan dua fitur penting manajemen memori pada kernel xv6, yaitu:

  * **Copy-on-Write Fork (CoW)** untuk efisiensi `fork()` melalui penundaan salin memori.
  * **Shared Memory System V-style** antar proses menggunakan key dan reference counting.

---

## 🛠️ Rincian Implementasi

Langkah-langkah implementasi pada modul ini meliputi:

### A. Copy-on-Write Fork (CoW)

* Menambahkan array `ref_count[]` global di `vm.c` untuk setiap halaman fisik.
* Membuat fungsi `incref()` dan `decref()` untuk manajemen reference count.
* Menambahkan flag baru `PTE_COW` di `mmu.h`.
* Membuat fungsi baru `cowuvm()` sebagai pengganti `copyuvm()` saat `fork()`.
* Mengubah `fork()` di `proc.c` agar menggunakan `cowuvm()`.
* Menangani `page fault` write di `trap.c` untuk melakukan salin halaman (copy-on-write).

### B. Shared Memory ala System V

* Menambahkan struktur `shmtab[]` di `vm.c` untuk menyimpan informasi shared memory.
* Menambahkan syscall `shmget(int key)` untuk alokasi dan pemetaan halaman bersama.
* Menambahkan syscall `shmrelease(int key)` untuk melepaskan shared memory.
* Registrasi syscall ke `syscall.h`, `syscall.c`, `user.h`, dan `usys.S`.

---

## ✅ Uji Fungsionalitas

Program uji yang digunakan:

* `cowtest`: untuk menguji fork dengan Copy-on-Write
* `shmtest`: untuk menguji fungsi `shmget()` dan `shmrelease()`

---

## 📷 Hasil Uji
<img width="960" height="540" alt="Screenshot 2025-07-31 121200" src="https://github.com/user-attachments/assets/9e991e2a-661e-4486-9f1e-2aca20720a6d" />
<img width="960" height="540" alt="Screenshot 2025-07-31 121506" src="https://github.com/user-attachments/assets/b590ce99-6f5d-4cf1-855c-3e0d8e92a53c" />



### 📍 Contoh Output `cowtest`:

```
Child sees: Y
Parent sees: X
```

> Menunjukkan bahwa CoW berhasil: halaman awal dibagi, lalu disalin saat child menulis.

### 📍 Contoh Output `shmtest`:

```
Child reads: A
Parent reads: B
```

> Menunjukkan bahwa child dan parent berbagi satu halaman shared memory.



---

## ⚠️ Kendala yang Dihadapi

* Salah pengaturan flag `PTE_COW` menyebabkan page fault tidak ditangani.
* Lupa memanggil `incref()` saat `kalloc()` pada CoW → menyebabkan double-free.
* Salah pemetaan alamat shared memory di user space (USERTOP) → menyebabkan crash.
* Potensi race condition pada `shmtab[]` karena belum menggunakan locking.

---

## 📚 Referensi

* Mencari referensi dari Ai dan chat GPT
* Diskusi kelas dan praktikum

---
