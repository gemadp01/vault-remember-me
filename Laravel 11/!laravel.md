
## Intro
"Laravel is a **web application framework** with expressive, elegant syntax."

### apa itu Web Application Framework?
- Sebuah 'kerangka' yang didesain untuk mendukung pembangunan aplikasi web.
- Menyediakan standar / cara untuk membangun aplikasi web.
- Bertujuan untuk mengotomasi hal-hal umum yang biasanya dilakukan saat membangun aplikasi web.
- Contohnya: database library, templating engine, session management, authentication, security, dll.

### Laravel
- dibuat oleh Taylor Otwell
- Juni 2011
- sebagai alternatif framework untuk Codeigniter
- Versi terbarunya versi 8 (sekarang 12 tapi untuk materi sementara yaitu laravel 11)

### Filosofi Laravel
"We believe development must be an enjoyable, creative experience to be truly fulfilling." - Taylor Otwell

### Tujuan Laravel
"Laravel aims to make the development process a pleasing one for the developer without sacrificing application functionality, **Happy developers make the best code.**" - Taylor Otwell

### Fitur utama pada Laravel
- MVC sebuah paradigma yang memisahkan antara tampilan, proses dan juga data (Model untuk mengolah data, View mengolah UI, Controller untuk mengolah process-nya)
- Templating Engine
- Artisan Console, kita bisa melakukan modifikasi / konfigurasi terhadap framework-nya hanya dengan mengetikkan baris-baris perintah di consolenya, console 'artisan' sudah ada didalam laravelnya.
- Eloquent ORM (Object Relational Mapping) mempermudah ketika nantinya kita ingin berinteraksi dengan database relational seperti mySQL.
- Authentication & Authorization
- Testing
- Packaging System
- Multiple File System
- Task Schedulling
- Websocker Programming

### Ekosistem Laravel - **kumpulan tools dan layanan** yang dibuat untuk **mempermudah dan mempercepat pengembangan aplikasi web** dengan Laravel.

|Komponen|Fungsi Singkat|
|---|---|
|**Laravel**|Framework inti PHP untuk membuat aplikasi web.|
|**Eloquent**|ORM untuk mengelola database dengan sintaks OOP.|
|**Artisan**|Command line tool untuk automasi tugas (buat controller, migrate, dll).|
|**Blade**|Templating engine untuk membuat tampilan HTML.|
|**Laravel Mix**|Untuk mengelola dan compile asset front-end (CSS/JS).|
|**Laravel Breeze / Jetstream / Fortify**|Paket starter autentikasi.|
|**Laravel Sanctum / Passport**|Untuk autentikasi API (token).|
|**Laravel Livewire**|Membuat komponen UI reaktif tanpa JavaScript.|
|**Laravel Nova**|Admin panel premium.|
|**Laravel Forge**|Layanan untuk deploy server Laravel.|
|**Laravel Vapor**|Platform Laravel di serverless AWS.|
|**Laravel Sail**|Lingkungan pengembangan dengan Docker.|
|**Laravel Envoyer**|Untuk deployment tanpa downtime.|

➡️ **Intinya**: Ekosistem Laravel mencakup alat bantu backend, frontend, autentikasi, hingga deployment — semuanya saling terintegrasi untuk mempercepat proses pengembangan aplikasi.

### Website dibuat dengan Laravel
[Website dibuat dengan Laravel](awwwards.com/websites/laravel)

---

## Laravel 11
Info:
- Rilis 12 Maret 2024
- Tampilan welcome yang lebih fresh
- Struktur folder yang lebih ringkas
- Health routing (melihat kesehatan website)
- New artisan commands (artisan merupakan sebuah aplikasi yang ada didalam laravel yang berbentuk cli yang memudahkan kita ketika menggunakan Laravel, misal ketika ingin membuat file controller, model, migration, dll).
	> mengenai artisan: [[keyword 'artisan' pada Laravel]]
	
Case study - Blog System