# 🚀 Cheat Sheet: API Testing & Reconnaissance

> "APIs are the connective tissue of modern web apps. Finding flaws here often leads to the core of the system."

## 📌 Apa itu Pengujian API?

Pengujian API fokus pada menemukan kerentanan dalam *interface* yang memungkinkan komunikasi antar sistem. Karena API sering berinteraksi langsung dengan data sensitif, kerentanan di sini dapat merusak aspek **Confidentiality, Integrity, dan Availability (CIA)**.

---

## 🔍 Tahap 1: API Reconnaissance (Pengintaian)

Sebelum menyerang, kamu harus memetakan "medan perang" API tersebut.

### 1. Identifikasi Endpoints

Cari lokasi di mana API menerima permintaan.

* **Contoh:** `GET /api/v1/books` atau `POST /api/users/register`.
* **Tips:** Perhatikan pola URL dan jelajahi file JavaScript menggunakan **BApp JS Link Finder** untuk menemukan endpoint tersembunyi.

### 2. Menemukan Dokumentasi API

Dokumentasi (Swagger, OpenAPI, dsb.) adalah "buku panduan" bagi penyerang.

* **Path Umum:** * `/api`
  * `/swagger/index.html`
  * `/openapi.json`
  * `/v1/docs`
* **Teknik:** Jika kamu menemukan `/api/v1/users/123`, coba lakukan *directory brute force* pada jalur dasarnya: `/api/v1/` atau `/api/v1/users/`.

---

## 🛠️ Tahap 2: Interaksi & Manipulasi

Gunakan **Burp Repeater** untuk melihat bagaimana API merespons perubahan input.

### 1. Manipulasi Metode HTTP

Jangan hanya terpaku pada `GET` atau `POST`. Coba ganti metode pada endpoint yang sama untuk menemukan fungsionalitas tersembunyi:

* `GET /api/tasks` → Mengambil data.
* `POST /api/tasks` → Membuat data baru.
* `DELETE /api/tasks/1` → Mencoba menghapus data.
* **Tips:** Gunakan metode **OPTIONS** untuk melihat daftar *verb* HTTP yang diizinkan oleh server.

### 2. Manipulasi Content-Type

Ubah format data untuk memicu error atau melewati filter keamanan.

* **Contoh:** Ubah `application/json` menjadi `application/xml`.
* **Kenapa?** Terkadang server aman saat memproses JSON, namun rentan terhadap serangan lain (seperti XXE) saat memproses XML.

---

## ⚠️ Tahap 3: Exploiting Mass Assignment

**Mass Assignment** (atau Auto-binding) terjadi ketika aplikasi secara otomatis memetakan parameter input ke field di database tanpa filter yang memadai.

### Cara Menemukan & Eksploitasi

1. **Bandingkan:** Lihat hasil `GET /api/users/me`. Misal muncul: `{"id":1, "name":"Hype", "isAdmin":false}`.
2. **Coba Ubah:** Kirim permintaan `PATCH /api/users/update` dengan menambahkan parameter sensitif yang ditemukan tadi:

   ```json
   {
     "name": "Hype",
     "isAdmin": true
   }
   ```

3. **Verifikasi:** Cek kembali akun kamu untuk melihat apakah status akses telah berubah menjadi admin.

---

## 📊 Checklist Pengujian API

| Langkah | Deskripsi | Alat |
| :--- | :--- | :--- |
| **Discovery** | Cari dokumentasi & endpoint tersembunyi. | Burp Scanner, Ffuf |
| **Verb Tampering** | Coba metode HTTP lain (PUT, DELETE, PATCH). | Burp Intruder |
| **Param Discovery** | Cari parameter tersembunyi (misal: `?debug=true`). | Param Miner |
| **Mass Assignment** | Tambahkan field sensitif ke dalam request JSON. | Burp Repeater |

---

## 🛡️ Best Practices (Pencegahan)

* [ ] **Secure Docs:** Amankan dokumentasi API; jangan buka untuk publik jika tidak diperlukan.

* [ ] **Method Allow-list:** Terapkan daftar metode HTTP yang diizinkan secara ketat.
* [ ] **Generic Errors:** Gunakan pesan error generik untuk menghindari kebocoran detail teknis database.
* [ ] **Mass Assignment Fix:** Gunakan DTO (Data Transfer Objects) atau *allow-list* pada field yang boleh diperbarui oleh pengguna.

---

## 🔗 Referensi

* [PortSwigger: API Testing Academy](https://portswigger.net/web-security/api-testing)
* [OWASP API Security Top 10 2023](https://owasp.org/www-project-api-security/)

---
*Terakhir diperbarui: 25 April 2026*

---

### 💡 Tips Tambahan

* **Integrasi Swagger:** Jika menemukan file `swagger.json`, impor ke **Postman** atau **Insomnia** untuk visualisasi endpoint yang lebih mudah.
* **Server-Side Parameter Pollution (SSPP):** Perhatikan bagaimana parameter di API publik dapat memanipulasi permintaan ke API internal.
* **Targeting:** Fokus pada API sangat efektif untuk program di HackerOne atau Bugcrowd karena sering kali memiliki *attack surface* yang luas.
