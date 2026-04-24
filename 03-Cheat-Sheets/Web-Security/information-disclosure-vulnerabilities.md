# 📂 Cheat Sheet: Information Disclosure (Information Leakage)

> "Information disclosure is the art of finding the missing pieces of a puzzle to construct a high-severity attack."

## 📌 Apa itu Information Disclosure?

Information Disclosure terjadi ketika sebuah situs web secara tidak sengaja membocorkan informasi sensitif kepada penggunanya. Bocoran ini bisa berupa data pengguna lain, data bisnis rahasia, hingga detail teknis infrastruktur server.

---

## 🧠 Mindset Bug Hunter: Menghubungkan Titik (Connecting the Dots)

- **Informasi adalah Amunisi:** Detail teknis yang terlihat "sepele" (seperti versi framework) bisa menjadi kunci untuk menemukan exploit publik yang relevan.
- **Elicit, Don't Just Browse:** Jangan hanya melihat yang terlihat. Berinteraksilah dengan cara yang tidak terduga untuk memaksa server mengeluarkan pesan error atau log.
- **Demonstrasikan Dampak:** Saat membuat laporan, jangan hanya bilang "ada versi server". Jelaskan: "Karena versi server ini tua, saya bisa menggunakan CVE-XXXX untuk melakukan serangan."

---

## 🛠️ Teknik Eksploitasi & Checklist

### 1. File & Direktori Tersembunyi

Seringkali pengembang lupa menghapus file konfigurasi atau backup.

- **Target:** `/robots.txt`, `/.git/`, `/.env`, `/.DS_Store`, atau file backup seperti `config.php.bak`.
- **Checklist:** Gunakan alat seperti `ffuf`, `dirsearch`, atau `gobuster` dengan wordlist yang kuat.

### 2. Pesan Error yang Terlalu Deskriptif (Verbose Errors)

Pesan error yang detail adalah "peta" bagi penyerang.

- **Target:** Error database (MySQL/PostgreSQL), Stack traces, atau pesan debug.
- **Info yang dicari:** Nama tabel, nama kolom, path file internal (Local Path Disclosure), atau versi bahasa pemrograman.

### 3. Komentar Pengembang (Developer Comments)

Komentar di dalam kode HTML/JavaScript yang lupa dihapus saat naik ke produksi.

- **Checklist:** Cek `View Source` (Ctrl+U) untuk mencari:
  - Kredensial hardcoded (API Keys, password testing).
  - Alamat IP internal atau nama server.
  - Endpoint API rahasia yang masih dalam tahap pengembangan.

### 4. Perbedaan Respon (Subtle Differences)

Teknik untuk melakukan enumerasi data (seperti username).

- **Cara:** Perhatikan perbedaan kecil dalam respon server (waktu respon, pesan error "User tidak ditemukan" vs "Password salah", atau kode status HTTP).
- **Dampak:** Membantu penyerang memvalidasi daftar username untuk serangan Brute Force.

### 5. Konfigurasi Insecure & Fitur Debug

Fitur yang seharusnya hanya ada di tahap development tapi terbawa ke production.

- **Target:** `/phpinfo.php`, Django Debug Toolbar, Werkzeug Debugger, atau file `web.config` / `htaccess`.

---

## 📊 Kategori Data yang Bocor

| Kategori | Contoh Informasi | Potensi Dampak |
| :--- | :--- | :--- |
| **Teknis** | Versi Framework, Path Internal, IP Private | Mempermudah penargetan exploit spesifik (RCE). |
| **Bisnis** | Dokumen internal, Strategi harga, API Keys | Kerugian finansial atau akses ilegal ke layanan pihak ketiga. |
| **Pengguna** | Username, Email, Detail Kartu Kredit | Pelanggaran privasi (GDPR) dan Account Takeover. |

---

## 🛡️ Strategi Pencegahan (Untuk Developer)

- [ ] Gunakan pesan error generik (misal: "An error occurred, please contact admin").
- [ ] Nonaktifkan fitur debugging di lingkungan *Production*.
- [ ] Gunakan `.gitignore` dengan benar agar file sensitif tidak masuk ke repositori.
- [ ] Audit kode secara berkala untuk menghapus komentar internal.
- [ ] Pahami konfigurasi default dari teknologi pihak ketiga yang digunakan.

---

## 🔗 Referensi Praktik

- [PortSwigger Academy: Information Disclosure](https://portswigger.net/web-security/information-disclosure)
- [OWASP: Sensitive Data Exposure](https://owasp.org/www-project-top-ten/2017/A3_2017-Sensitive_Data_Exposure)

---
*Terakhir diperbarui: 24 April 2026*
