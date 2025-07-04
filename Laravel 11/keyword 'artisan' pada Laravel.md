Artisan adalah **command-line interface (CLI)** bawaan Laravel yang sangat powerful, digunakan untuk membantu developer dalam mengelola dan membangun aplikasi Laravel secara efisien.

### 🔧 Fungsi Artisan

Artisan bisa digunakan untuk berbagai hal, seperti:

- Membuat file **controller**, **model**, **migration**, **seeder**, dll
    
- Menjalankan **migrasi database**
    
- Membersihkan cache
    
- Menjalankan **scheduler**
    
- Men-serve aplikasi secara lokal
    
- Membuat custom command sendiri
    

---

### 📌 Contoh Perintah Artisan

Berikut beberapa contoh perintah yang sering digunakan:

|Tujuan|Perintah|
|---|---|
|Menampilkan daftar semua perintah|`php artisan list`|
|Melihat bantuan perintah|`php artisan help migrate`|
|Menjalankan server lokal|`php artisan serve`|
|Membuat controller|`php artisan make:controller NamaController`|
|Membuat model|`php artisan make:model NamaModel`|
|Membuat migration|`php artisan make:migration create_users_table`|
|Menjalankan migration|`php artisan migrate`|

---

### 🧠 Analogi Sederhana

Bayangkan Laravel seperti dapur profesional, Artisan adalah **asisten dapur yang bisa kamu suruh**:

> “Tolong bikinin resep baru (controller),” atau “Yuk mulai masak (jalankan server)”.
