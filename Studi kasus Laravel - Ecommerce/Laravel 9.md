https://laravel.com/docs/9.x


## Database
### Database Migration
Sebuah tempat dimana semua informasi aplikasi tersimpan didalamnya
bentuk umum dari database ialah database jenis sql, tiap data direpresentasikan sebagai table

didalam table terdapat kolom dan baris,
- kolom, atribut yang dimiliki oleh setiap data (id, nama, dll)
- baris, record data (data dari sebuah table)

umumnya untuk membuat sebuah table didalam database sql, harus menguasai query language sql

mySQL all in one packages 

#### Migration
https://laravel.com/docs/9.x/migrations#main-content
pada laravel terdapat fitur migration (database migration),
beberapa kasus ketika menggunakan database sql secara langsung
"Can i have the database configuration for your app?"
"Hey team, I need to add a new column in the table A, guess you should follow my new database structure."
"I want to create a table but I'm not familiar with SQL"

dengan laravel kita dapat menggunakan fitur migration (tool) untuk memudahkan dalam mendefine apa-apa saja konfigurasi database yang akan kita gunakan, 
atribut pada table dapat digambar dalam sebuah syntax php (id, nama dll)

#### Create Migration
backtick -> tilda

`php artisan make:migration create_students_table`
kata-kerja_nama-table_table
!nama table umumnya plural

`public function up()`
    `{`
        `Schema::create('students', function (Blueprint $table) {`
        atribut/kolom id dan timestamps default bawaan laravel,
        pembuatan atribut property $table->tipedata('nama-kolom');
            `$table->id();`
            `$table->string('name');`
            `$table->integer('score');`
            `$table->timestamps();`
        `});`
    `}`

pada file migration terdapat 2 method utama ialah up dan down
- up, apa yang akan kita lakukan pada saat menjalankan migrasi ini (membuat sebuah table baru dengan nama students, dan isi dari table ini berisi atribut id, name, score, dan timestamps)
- down, apa yang terjadi jika make:migration ini di reverse (table students akan di delete, jika direverse)

konfigurasi .env disesuaikan dengan management database yang kita gunakan, dan jika sudah membuat database nya, DB_DATABASE disesuaikan dengan nama database yang sudah ada
`DB_CONNECTION=mysql`
`DB_HOST=127.0.0.1`
`DB_PORT=3306`
`DB_DATABASE=laravel`
`DB_USERNAME=root`
`DB_PASSWORD=`

#### Menjalankan Migration
jika belum mempunyai database dan langsung menjalankan migration, maka laravel akan otomatis memberi saran untuk membuat database dengan nama yang sama dengan value DB_DATABASE pada .env
`php artisan migrate`

### Database Seeding
adalah sebuah aksi, kita akan mempopulate database dengan data yang sudah kita sediakan,
misal, pada saat initiate database kita dapat memasukkan data-data yang sudah kita sediakan sebelumnya

#### Create seeder
`php artisan make:seeder StudentSeeder`
konfensi penamaan seeder pada laravel,
singular dari nama table diikuti "Seeder"
dipisahkan dengan huruf kapital

didalam StudentSeeder terdapat satu method default ialah run(), yang dimana method tersebut akan dijalankan pada saat kita menjalankan StudentSeeder

`use Illuminate\Support\Facades\DB;`

`Class Student extend Seeder {`
`public function run()`
    `{`
    membuat object baru $data yang berisi data-data didalamnya (array of array)
    setiap array berisi record yang kita masukkan
        `$data = [`
        array[0] -> record pertama
            `[`
                `'id' => 1,`
                `'name' => 'Maryanto',`
                `'score' => 100`
            `],`
            array[1] -> record kedua
            `[`
                `'id' => 2,`
                `'name' => 'Ahmad',`
                `'score' => 90`
            `],`
            array[2] -> record ketiga
            `[`
                `'id' => 3,`
                `'name' => 'Budi',`
                `'score' => 95`
            `],`
        `];`
        Facades DB specified table 'Students', lalu passing data from $data (agar tidak terlalu panjang isi dari methodnya)
        `DB::table('students')->insert($data);`
    `}`
`}`

#### Menjalankan seeder
`php artisan db:seed`
menjalankan DatabaseSeeder.php yang mana isinya dapat memanggil kelas seeder yang lain

`php artisan db:seed --class=StudentSeeder`
menjalankan spesifik seeder

### Faker
https://fakerphp.github.io/
sebuah library php yang dapat men-generate data palsu seperti nama, angka, alamat, dll.

contoh penggunaan
`use Faker\Factory as Faker;`

`$faker = Faker::create('id_ID');`
objek $faker baru dari Facades Faker
specify data berdasarkan region 
jadi, data-data yang di generate faker name, dll akan disesuaikan dengan region nya

setiap looping (5 kali) insert data name dan score kedalam table students
        `for($i = 1; $i <= 5; $i++){`
            `DB::table('students')->insert([`
            id bisa di insert secara otomatis, 
            data baru (tanpa id) akan keeptrack id yang lama
                `'name' => $faker->name,`
                `'score' => $faker->numberBetween(0, 100),`
            `]);`
        `}`

terus jalankan seedernya
`php artisan db:seed`
!tanpa specify kelas, karena kita buat di DatabaseSeeder.php

## MVC
Laravel merupakan Framework MVC
### Model
Bagian dari Laravel yang berinteraksi secara langsung dengan database, sehingga setiap record yang ada pada database akan direpresentasikan dalam object php yang bernama **Model**.

jadi ketika ingin mengambil/membuat menggunakan data, karena semua data pada database sudah direpresentasikan sebagai model

`php artisan make:model Student`
!konfensi penamaan model singular dengan huruf pertama kapital

model dapat dilihat pada,
app -> Models -> yourModel.php

yang dapat dilakukan pada model,
https://laravel.com/docs/9.x/eloquent

kita dapat men-state table apa yang berasosiasi dengan model terkait
misal, Student.php 
`class Student extends Model`
	`{`
		`protected $table = 'students';`
		`protected $primaryKey = 'flight_id';`
		`public $incrementing = false;`
		`protected $keyType = 'string';`
		dll
	`}`
!karena kita sudah mengikuti konvensi sesuai aturan laravel cara diatas tidak perlu dilakukan, karena laravel akan secara otomatis mem-mapping model dengan nama table yang sesuai
### Routing
sebuah bagian yang mengatur endpoint apa-apa saja yang ada pada aplikasi web kita.
misal /register apa yang akan dilakukan ketika mengunjungi endpoint /register, dll

untuk melihat routing ada di,
routes -> web.php

Terdapat 5 (lima) http method,
get, digunakan untuk mem-retrieve data (mengambil data)
post, digunakan untuk mem-submit data
put, digunakan untuk mem-update keseluruhan data
patch, digunakan untuk mem-update sebagian data
delete, digunakan untuk mem-delete data
`Route::get('/greeting', function () {`
	argument pertama ialah endpoint
	argument kedua ialah callback function, apa yang dilakukan ketika user mengakses endpoint /greeting ini
    `return 'Hello World!';`
`});`
#### Route Parameters
passing sesuatu pada route yang dapat digunakan pada callback function

// passing variable {name}
`Route::get('/greeting/{name}', function () {`
passing {name} variabel name 
    `return 'Hello ' . $name;`
    value $name didapat dari route yang dipassing (akan berisi apapun yang kita kirim pada url )
`});`

#### Named Route (optional)
setiap route bisa kita beri nama, jadi mempermudah ketika kita mereference suatu route (baik itu pada controller maupun view),
!usahakan memberi nama pada route
`Route::get('/greeting/{name}', function () {`
    `return 'Hello ' . $name;`
`})->name('greeting_with_name');`
ini tidak akan berpengaruh apa-apa, tapi ketika ingin memanggil route dari komponen lain, kita cukup reference name dari route nya

!menjalankan aplikasi laravel,
`php artisan serve`

### Views
Mengatur tampilan dari aplikasi web.
mengatur apa yang akan dilihat user nantinya.

Laravel menggunakan Blade, 
merupakan templating engine dari laravel (html + php), seperti html murni yang didalamnya kita dapat menggunakan logic pada blade dengan syntax php

resources -> views -> yourView.blade.php

format penamaan, 
namaFile diikuti .blade.php
example.blade.php

`Route::get('/greeting/{name}', function () {`
    `return 'Hello ' . $name;`
`})->name('greeting_with_name');`
### Controller