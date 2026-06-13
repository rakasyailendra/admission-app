# 🚀 Development Plan: Sistem Penerimaan Mahasiswa Baru (PMB) Prototype

**Author:** Muhammad Rakha Syailendra  
**Event:** Vibe Coding & Venture — SEVIMA  
**Project:** Admission App (Full Stack Prototype)  
**Tech Stack:** React 18 + Vite (Frontend) & Laravel 12 (Backend)  

---

## 1. 🎯 Executive Summary
Proyek ini adalah prototipe Sistem Penerimaan Mahasiswa Baru (PMB 2025) yang dirancang untuk mendemonstrasikan efisiensi pengembangan perangkat lunak modern. Dibangun 100% menggunakan *autonomous AI coding agent*, proyek ini tidak sekadar menghasilkan *working software*, tetapi juga membuktikan bagaimana *Prompt-Driven Development* dapat dikendalikan oleh *Engineering Mindset* untuk menghasilkan *codebase* yang bersih, aman, dan *scalable*—sesuai dengan standar industri EdTech.

---

## 2. 🏗️ Arsitektur Sistem & Alur Data
Mengadopsi pola *Decoupled Client-Server Architecture* (API-First Approach).

*   **Frontend (React 18 + Vite):** Bertindak sebagai *Single Page Application* (SPA) mandiri. Difokuskan pada *Client-Side Rendering* (CSR), manajemen *state* UI, dan UX yang responsif menggunakan Tailwind CSS. 
*   **Backend (Laravel 12):** Bertindak murni sebagai RESTful API Server. Menangani *Business Logic*, validasi *server-side*, autentikasi *stateless* via Sanctum, dan operasi I/O ke Database.
*   **Database (SQLite ➔ PostgreSQL):** Menggunakan SQLite untuk kecepatan iterasi selama fase *development/prototyping*, namun *schema migration* telah disiapkan secara penuh untuk transisi instan ke PostgreSQL di level *production*.

---

## 3. 🧠 Strategi "Advanced Prompt-Driven Development"
Penggunaan AI di sini tidak menggunakan prompt generik. AI diarahkan layaknya *Junior Developer* yang dibimbing oleh *Tech Lead*, menggunakan teknik **Context-Feeding**:

1.  **Context Injection:** Sebelum menulis kode, AI diberikan konteks menyeluruh melalui file `prd.md` (Product Requirements) dan `skill.md` (Technical Conventions).
2.  **Constraint Setting:** Membatasi halusinasi AI dengan instruksi ketat. *(Contoh: "Gunakan native Fetch API tanpa Axios", "Tulis styling murni dengan utility class Tailwind, hindari custom CSS").*
3.  **Iterative Generation:** Membangun sistem per lapisan (UI ➔ Logic ➔ API ➔ Integrasi), bukan sekaligus, untuk memastikan setiap komponen dapat diuji secara terisolasi.

**Contoh Prompt Arsitektural yang Digunakan:**
> *"Act as a Senior Laravel Architect. Baca file `prd.md` bagian fitur Admin. Buatkan file migration, model, dan controller untuk `Pendaftar`. Terapkan best practice: gunakan Form Request Validation terpisah (StorePendaftarRequest), dan pastikan endpoint dilindungi middleware `auth:sanctum`."*

---

## 4. 🛠️ Human-in-the-Loop: Mengatasi "AI Hallucination"
AI Agent mampu mempercepat penulisan *boilerplate*, namun tetap membutuhkan pengawasan teknis (*Engineering Overrides*) untuk menjamin performa perangkat lunak:

*   **Pencegahan N+1 Query Problem:** Pada saat AI men-generate endpoint `/api/statistik`, instruksi awal AI sering kali menghasilkan *query* yang berat di dalam *loop*. Intervensi dilakukan dengan memaksa AI menggunakan *Eager Loading* atau *Aggregate Functions* bawaan Laravel Eloquent (`count`, `groupBy`) agar komputasi dilakukan di level database, bukan di level aplikasi.
*   **Keamanan Token Auth:** Mengkoreksi AI yang mencoba menyimpan Sanctum Token di `localStorage` (rentan terhadap eksploitasi XSS). Intervensi dilakukan dengan memindahkan penyimpanan ke `sessionStorage` dengan alur *clear-on-logout* yang lebih aman untuk *dashboard* admin.

---

## 5. 🔐 Blueprint API & Spesifikasi Endpoint
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

## 6. 🚀 Scalability & Alignment dengan Visi SEVIMA (EdTech)
Meskipun berstatus prototipe *mini challenge*, *codebase* ini telah mengantisipasi kebutuhan skala kampus di dunia nyata:

1.  **Mobile-Ready Integration:** Arsitektur *API-First* memastikan bahwa jika kampus ingin berekspansi dengan menambahkan aplikasi *Mobile* mandiri (misalnya menggunakan *framework* seperti Flutter) bagi calon mahasiswa, *backend logic* tidak perlu dirombak sama sekali.
2.  **High-Performance Ready:** Pemisahan *Client-Server* memungkinkan *frontend* di-*deploy* di CDN (seperti Vercel atau Cloudflare Pages), sehingga beban *server backend* (Laravel) murni hanya untuk komputasi data dan transaksi database yang masif.
3.  **Data Portability:** Fitur *Export* CSV menggunakan *encoding* UTF-8 BOM yang telah disesuaikan agar 100% kompatibel dengan MS Excel, mempermudah tim tata usaha kampus (*Backoffice*) dalam mengolah data pelamar secara konvensional.