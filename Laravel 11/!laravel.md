
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
	
Case study - Blog System (berbasis component)
- Tailwind
- alpinejs (bagian frontend dengan component tidak lagi menggunakan syntax pewarisan blade)
- starterpack untuk mengelola user termasuk registration, login, logout yaitu breeze

Prerequisite
- php dasar
- oop php
- php mvc (pemisahan kode menjadi 3 (tiga) bagian: model view controller)
- playlist laravel 8 hanya tonton intronya saja
- tailwindcss
- alpinejs (library/framework javascript yang digunakan untuk interaktivitas halaman web)

Requirement
- [PHP 8.2](php.net/downloads.php) (php nanti disimpan kedalam web server)
- Webserver: [Apache](https://httpd.apache.org/) / [nginx](https://nginx.org/en/download.html) 
- [MySQL](mysql.com/downloads) berbayar atau MariaDB gratis (optional) 
	karena kita nanti menggunakan sqlite (sequel lite) lebih sederhana (untuk mengetahui perbedaan dari MySQL dengan sqlite)
- [Composer (package manager pada php)](getcomposer.org/download)
- [NodeJS](nodejs.org/en/download)

- [Laragon](https://laragon.org/) (software all in one yang sudah menyediakan semua requirement diatas jadi tidak perlu download manual satu persatu dan melakukan config sendiri)
- Laravel HERD sama halnya Laragon tapi bedanya MySQL yang digunakan berbayar
- TABLEPLUS -> untuk berinteraksi atau mengelola database
- PHP IDE yaitu PHPSTORM paling nyaman ketika melakukan code dengan php tapi berbayar.
- VSCode (code editor) free

VSCode Extensions
- PHP Intelephense
- PHP Namespace Resolver
- Laravel Snippets
- Laravel Blade Snippets
- Laravel Blade Formatter
- Laravel Blade Spacer
- Laravel Extra Intellisense
- Laravel GoTo View
- Tailwind CSS Intellisense
- Alpine.js Intellisense
- Alpine.js Syntax Highlight


---

## Instalasi dan Konfigurasi
- XAMPP (all in one) software paket yang digunakan untuk membuat server web lokal di komputer ==*tapi sering bermasalah*== ketika melakukan konfigurasi
	- **X**: Cross-platform (bisa digunakan di Windows, macOS, dan Linux)
	- **A**: Apache (web server)
	- **M**: MySQL atau MariaDB (database server)
	- **P**: PHP (bahasa pemrograman server-side)
	- **P**: Perl (bahasa pemrograman lain yang disediakan)
		- XAMPP digunakan untuk:
			- Menjalankan website secara **offline** (localhost) tanpa harus hosting online.
			- Mengerjakan proyek berbasis PHP, Laravel, WordPress, dsb.
			- Belajar membuat aplikasi dengan **PHP + MySQL**.

- Laragon (pengganti XAMPP, lebih configurable) untuk windows
	- Blazing fast
	- pretty URLs
	- Easy & Flexible
	- Modern & Powerful
- Laravel HERD (laragon versi laravel tapi untuk versi gratis tidak bisa menggunakan MySQL, alternatif kita gunakan sqlite)
- MAMP & MAMP PRO untuk macos

### Instalasi Laragon (seperti biasanya) -> konfigurasi
- ==download PHP== (==**zip/versi binary-nya**==) Non-thread safe
	> materi mengenai php thread safe dan non-thread safe: [[php versi thread dan non-thread safe]]
	- extract dan simpan kedalam folder laragon/bin/php/here
	- pada ui laragonnya -> klik kanan -> php -> pilih versi php yang di inginkan -> cek pada terminal dengan mengetikkan perintah `php -v`
	- untuk mengganti web server, kita bisa masuk menu 'preferences' atau mengklik icon roda gigi dikanan pojok atas -> kebagian menu bar 'Services & ports' -> unchecklist 'Apache' -> checklist 'Nginx'
- ==download & install 'composer'== arahkan ke php yang ada di laragon -> untuk memastikan sudah terinstall atau belum -> buka terminal pada laragon (bebas bisa powershell atau cmd window) -> ketikkan perintah `composer` atau `composer -v`
- ==download & install 'nodejs'== ver ==**zip/binary**== nya agar kita bisa pasang dilaragon bukan di system operasi windows (tidak perlu karena sudah tersedia di laragon dan pastikan juga versi nya sesuai dengan yang di web resmi nodejs) -> mengecek versi nodejs `node -v`
- ==Menjalankan sebuah ekstensi agar aplikasi laragon bisa terkoneksi ke database sqlite== karena secara default didalam laragon ekstensi sqlite belum aktif karena laravel default database nya sqlite bukan mysql. 
	- masuk ke menu PHP dengan klik kanan pada tampilan menu laragon -> 'PHP' -> 'Extensions' -> aktifkan ekstensi =='pdo_sqlite==' dan '==sqlite3=='
- Menu Klik Kanan Laragon (System Tray):
	![[Pasted image 20250705123719.png]]![[Pasted image 20250705123753.png]]![[Pasted image 20250705123819.png]]
	1. **Start All / Stop All**
	    - Untuk memulai atau menghentikan semua layanan (Apache/Nginx, MySQL, dll).
	2. **Web**
	    - Buka halaman utama Laragon di browser (biasanya `http://localhost/`).
	    - Menampilkan daftar virtual host dan project kamu.
	3. **www**
	    - Buka folder `www` tempat semua project web kamu berada.
	4. **Database**
	    - Akses ke aplikasi database seperti **HeidiSQL**, **phpMyAdmin**, atau yang kamu pilih sebagai default DB client.
	5. **Terminal**
	    - Membuka terminal Laragon, biasanya mendukung Git Bash / CMD / PowerShell.
	6. **Root**
		- Membuka folder root 'www'
	7. **Notepad++ / Code Editor**
	    - Buka editor teks (jika sudah terdeteksi, seperti VS Code atau Notepad++).
	8. **Quick App**
	    - Install framework secara cepat (Laravel, WordPress, Symfony, dll) hanya dengan beberapa klik.
	9. **Menu Services**
	    - Start/Stop layanan tertentu secara individual:
	        - Apache / Nginx
	        - MySQL / MariaDB
	        - Redis / Memcached / PostgreSQL (jika tersedia)
	        - Mail Catcher (Mailhog)
	10. **Preferences / Settings**
	    - Mengatur pengaturan Laragon: port, direktori, auto start, service defaults, dll.
	11. **Tools**
	    - Beberapa utilitas tambahan:
	        - Hosts Editor
	        - SSL Generator
	        - Mail Catcher
	        - Auto Virtual Hosts
	12. **Help / About**
	    - Dokumentasi dan informasi versi Laragon.
	13. **Exit**
	    - Keluar dan menutup semua layanan Laragon.

### Instalasi laravel via composer
#### Cara manual
- menginstall laravel project via Composer's create-project -> ketik pada terminal laragon (otomatis diarahkan ke laragon/www) atau cmd windows atau git bash 
	`composer create-project laravel/laravel:^11.0 example-app` (tanpa :^11.0 maka akan otomatis yang terinstall Laravel versi terbaru)
	- laragon/www -> merupakan tempat menyimpan semua project laravel
- menginstall Laravel projects secara global dengan menginstall Laravel installer via composer
	- `composer global require laravel/installer`
	- `laravel new example-app`
		setelah project terbuat, jalankan local development laravel menggunakan perintah `artisan` (aplikasi didalam laravel yang bisa kita ketikkan didalam command line).
		> [[keyword 'artisan' pada Laravel]]
	
		- `cd example-app`
		- `php artisan serve` -> menjalankan server local pada [http://127.0.0.1:8000] yang akan memunculkan aplikasi laravel local page (==*server harus selalu berjalan diterminal*==)

#### Cara cepat (laragon)
