# 🧠 Cheat Sheet: Business Logic Vulnerabilities

> "Logic flaws are often invisible to those not looking for them, as they won't be exposed by normal use of the application."

## 📌 Deskripsi Singkat

Vulnerabilitas logika bisnis adalah celah dalam desain dan implementasi aplikasi yang memungkinkan penyerang memanipulasi fungsionalitas sah untuk mencapai tujuan berbahaya. Celah ini muncul karena kegagalan pengembang dalam mengantisipasi perilaku pengguna yang tidak konvensional.

---

## ⚡ Mindset & Pemahaman Utama

- [ ] **Berpikir "Out of the Box":** Jangan ikuti alur yang diinginkan pengembang.
- [ ] **Abaikan Scanner:** Celah ini memerlukan intuisi manusia; otomatisasi seringkali gagal di sini.
- [ ] **Identifikasi Asumsi:** Temukan apa yang diasumsikan aman oleh pengembang, lalu patahkan asumsi tersebut.
- [ ] **Pahami Domain:** Pelajari bagaimana bisnis tersebut menghasilkan uang atau memproses data sensitif.

---

## 🛠️ Teknik Eksploitasi & Checklist

### 1. Client-Side Controls Bypass

Aplikasi seringkali hanya melakukan validasi di sisi browser.

- **Target:** Harga barang, biaya pengiriman, ID pengguna, diskon.
- **Cara:** Gunakan Burp Proxy untuk `intercept` dan ubah nilai parameter saat data dikirim ke server.

### 2. Unconventional Input (Input Tidak Wajar)

Menguji batas logika pada field input numerik atau teks.

- **Negatif:** Masukkan nilai minus (contoh: `-1` pada jumlah pesanan).
- **Extreme Values:** Masukkan angka yang sangat besar untuk memicu *overflow*.
- **Format:** Kirim array atau objek jika sistem mengharapkan string tunggal.

### 3. Workflow Bypassing (Pelanggaran Alur)

Melewati langkah-langkah wajib dalam sebuah proses.

- **Contoh:** Langsung menuju `/checkout/confirmation` tanpa melakukan pembayaran.
- **2FA Bypass:** Mencoba mengakses halaman dashboard langsung setelah login tanpa memasukkan kode OTP.

### 4. Parameter Tampering & Removal

Menghapus atau mengubah parameter untuk melihat reaksi server.

- **Teknik:** Hapus parameter satu per satu untuk menemukan jalur kode (*code path*) yang tersembunyi.
- **Dual-use Endpoints:** Cek apakah satu endpoint melakukan dua tugas berbeda tergantung parameter yang ada.

### 5. Domain-Specific Flaws

Celah yang unik hanya pada jenis bisnis tertentu.

- **E-commerce:** Menambah barang agar dapat diskon, lalu menghapusnya kembali sebelum *checkout*.
- **Social Media:** Memanipulasi jumlah pengikut atau status verifikasi.

---

## 📊 Ringkasan Strategi

| Area Fokus | Tindakan Utama |
| :--- | :--- |
| **Integritas Data** | Selalu validasi ulang di sisi server (Server-side validation). |
| **Urutan Kejadian** | Uji akses halaman secara acak (Forced Browsing). |
| **Trust Boundary** | Jangan percaya data dari klien meskipun sudah dienkripsi (cek Encryption Oracle). |

---

## 🔗 Referensi & Latihan

- [PortSwigger Academy: Business Logic Vulnerabilities](https://portswigger.net/web-security/logic-vulnerabilities)
- [OWASP: Business Logic Errors](https://owasp.org/www-community/vulnerabilities/Business_logic_vulnerability)

---
*Terakhir diperbarui: 24 April 2026*
