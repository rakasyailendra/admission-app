# 🚀 Development Plan: Sistem Penerimaan Mahasiswa Baru (PMB) Prototype

**Author:** Muhammad Rakha Syailendra  
**Event:** Vibe Coding & Venture — SEVIMA  
**Project:** Admission App (Full Stack Prototype)  
**Tech Stack:** React 18 + Vite (Frontend) & Laravel 12 (Backend)  

---

## 1. 🎯 Executive Summary
Proyek ini adalah prototipe Sistem Penerimaan Mahasiswa Baru (PMB 2025) yang dirancang untuk mendemonstrasikan efisiensi pengembangan perangkat lunak modern. Dibangun 100% menggunakan *autonomous AI coding agent*, proyek ini tidak sekadar menghasilkan *working software*, tetapi juga membuktikan bagaimana *Prompt-Driven Development* dapat dikendalikan oleh *Engineering Mindset* untuk menghasilkan *codebase* yang bersih, aman, dan *scalable*—sesuai dengan standar industri EdTech.

---

## 2. 👥 Target Pengguna (User Persona)
Sistem ini dirancang secara spesifik untuk memfasilitasi dua aktor utama dengan kebutuhan yang berbeda:
1. **Calon Mahasiswa (Public):** Membutuhkan antarmuka yang cepat, responsif di perangkat *mobile*, dan alur pendaftaran yang jelas.
2. **Admin PMB / Tata Usaha (Internal):** Membutuhkan keamanan data (via *Sanctum Auth*), visualisasi statistik pendaftar yang *real-time*, dan kemudahan rekapitulasi data (via fitur *Export CSV*).

---

## 3. 🏗️ Arsitektur Sistem & Alur Data
Mengadopsi pola *Decoupled Client-Server Architecture* (API-First Approach).

* **Frontend (React 18 + Vite):** Bertindak sebagai *Single Page Application* (SPA) mandiri. Difokuskan pada *Client-Side Rendering* (CSR), manajemen *state* UI, dan UX yang responsif menggunakan Tailwind CSS. 
* **Backend (Laravel 12):** Bertindak murni sebagai RESTful API Server. Menangani *Business Logic*, validasi *server-side*, autentikasi *stateless* via Sanctum, dan operasi I/O ke Database.
* **Database (SQLite ➔ PostgreSQL):** Menggunakan SQLite untuk kecepatan iterasi selama fase *development/prototyping*, namun *schema migration* telah disiapkan secara penuh untuk transisi instan ke PostgreSQL di level *production*.

---

## 4. ⏱️ Timeline & Estimasi Pengerjaan
Penggunaan AI Agent memangkas waktu *development* secara drastis. Berikut adalah rincian *roadmap* penyelesaian proyek yang dieksekusi secara iteratif:

| Fase Pengembangan | Tanggal Eksekusi | Waktu (AI-Assisted) | Fokus Pengerjaan |
| :--- | :--- | :---: | :--- |
| **Fase 1: Frontend Core** | 11 Juni 2026 | ~2 Jam | Setup React & Vite, komponen *Tailwind UI*, form pendaftaran, dan validasi sisi klien. |
| **Fase 2: Backend API** | 12 Juni 2026 | ~2 Jam | Inisialisasi Laravel 12, skema *Database* (SQLite), pembuatan model, controller, dan CRUD *Endpoints*. |
| **Fase 3: Integration & Security** | 13 Juni 2026 | ~1.5 Jam | Implementasi *Sanctum Auth*, perbaikan performa API (N+1 Query), integrasi *Fetch API*, dan *Export* CSV. |

---

## 5. 🧪 Skenario Pengujian (Testing Strategy)
Untuk memastikan sistem berjalan tanpa cacat sebelum dirilis, dilakukan tiga lapis pengujian:
1. **Unit Testing (Prompt-Level):** Memastikan fungsi spesifik (seperti generator nomor pendaftaran PMB-2025-XXXX) berjalan sesuai parameter saat di-generate AI.
2. **API Testing:** Simulasi *HTTP Request* (GET, POST, PATCH) menggunakan **Postman / Thunder Client** ke sisi *Backend* secara terisolasi sebelum dihubungkan ke *Frontend*.
3. **End-to-End (E2E) Testing:** Pengujian aliran data nyata di *browser*, mulai dari pengisian form oleh calon mahasiswa hingga data tersebut divalidasi dan muncul di *dashboard* admin.

---

## 6. 🧠 Strategi "Advanced Prompt-Driven Development"
Penggunaan AI di sini tidak menggunakan prompt generik. AI diarahkan layaknya *Junior Developer* yang dibimbing oleh *Tech Lead*, menggunakan teknik **Context-Feeding**:

1.  **Context Injection:** Sebelum menulis kode, AI diberikan konteks menyeluruh melalui file `prd.md` (Product Requirements) dan `skill.md` (Technical Conventions).
2.  **Constraint Setting:** Membatasi halusinasi AI dengan instruksi ketat. *(Contoh: "Gunakan native Fetch API tanpa Axios", "Tulis styling murni dengan utility class Tailwind, hindari custom CSS").*
3.  **Iterative Generation:** Membangun sistem per lapisan (UI ➔ Logic ➔ API ➔ Integrasi), bukan sekaligus, untuk memastikan setiap komponen dapat diuji secara terisolasi.

**Contoh Prompt Arsitektural yang Digunakan:**
> *"Act as a Senior Laravel Architect. Baca file `prd.md` bagian fitur Admin. Buatkan file migration, model, dan controller untuk `Pendaftar`. Terapkan best practice: gunakan Form Request Validation terpisah (StorePendaftarRequest), dan pastikan endpoint dilindungi middleware `auth:sanctum`."*

---

## 7. 🛠️ Human-in-the-Loop: Mengatasi "AI Hallucination"
AI Agent mampu mempercepat penulisan *boilerplate*, namun tetap membutuhkan pengawasan teknis (*Engineering Overrides*) untuk menjamin performa perangkat lunak:

* **Pencegahan N+1 Query Problem:** Pada saat AI men-generate endpoint `/api/statistik`, instruksi awal AI sering kali menghasilkan *query* yang berat di dalam *loop*. Intervensi dilakukan dengan memaksa AI menggunakan *Eager Loading* atau *Aggregate Functions* bawaan Laravel Eloquent (`count`, `groupBy`) agar komputasi dilakukan di level database, bukan di level aplikasi.
* **Keamanan Token Auth:** Mengkoreksi AI yang mencoba menyimpan Sanctum Token di `localStorage` (rentan terhadap eksploitasi XSS). Intervensi dilakukan dengan memindahkan penyimpanan ke `sessionStorage` dengan alur *clear-on-logout* yang lebih aman untuk *dashboard* admin.

---

## 8. 🔐 Blueprint API & Spesifikasi Endpoint
Semua endpoint komunikasi mengembalikan respons berformat JSON dan dilindungi oleh aturan CORS (`config/cors.php`) yang ketat (hanya mengizinkan origin `http://localhost:5173`).

| Method | Endpoint | Auth | Fungsi Utama |
| :--- | :--- | :---: | :--- |
| `POST` | `/api/auth/login` | ✗ | Validasi kredensial dan penerbitan *Personal Access Token* Sanctum. |
| `POST` | `/api/auth/logout` | ✓ | *Revoke* token saat ini (Logout admin). |
| `POST` | `/api/pendaftar` | ✗ | *Data Entry* calon mahasiswa baru (Public). |
| `GET` | `/api/pendaftar` | ✓ | Menarik seluruh data pelamar untuk tabel admin (Protected). |
| `PATCH`| `/api/pendaftar/{id}/status`| ✓ | Mutasi status pelamar (Menunggu ➔ Lolos/Tidak Lolos). |
| `POST` | `/api/pendaftar/{no}/heregistrasi`| ✗ | Mutasi data *enrollment confirmation* bagi yang berstatus Lolos. |
| `GET` | `/api/statistik` | ✓ | Agregasi data pendaftar (Total, Prodi, Jalur Masuk). |
| `GET` | `/api/pendaftar/export/csv` | ✓ | *Export* rekapitulasi data pendaftar ke file CSV. |

---

## 9. 🚀 Scalability & Alignment dengan Visi SEVIMA (EdTech)
Meskipun berstatus prototipe *mini challenge*, *codebase* ini telah mengantisipasi kebutuhan skala kampus di dunia nyata:

1.  **Mobile-Ready Integration:** Arsitektur *API-First* memastikan bahwa jika kampus ingin berekspansi dengan menambahkan aplikasi *Mobile* mandiri (misalnya menggunakan *framework* seperti Flutter) bagi calon mahasiswa, *backend logic* tidak perlu dirombak sama sekali.
2.  **High-Performance Ready:** Pemisahan *Client-Server* memungkinkan *frontend* di-*deploy* di CDN (seperti Vercel atau Cloudflare Pages), sehingga beban *server backend* (Laravel) murni hanya untuk komputasi data dan transaksi database yang masif.
3.  **Data Portability:** Fitur *Export* CSV menggunakan *encoding* UTF-8 BOM yang telah disesuaikan agar 100% kompatibel dengan MS Excel, mempermudah tim tata usaha kampus (*Backoffice*) dalam mengolah data pelamar secara konvensional.