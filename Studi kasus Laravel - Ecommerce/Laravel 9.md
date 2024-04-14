https://laravel.com/docs/9.x

## Database
Sebuah tempat dimana semua informasi aplikasi tersimpan didalamnya
bentuk umum dari database ialah database jenis sql, tiap data direpresentasikan sebagai table

didalam table terdapat kolom dan baris,
- kolom, atribut yang dimiliki oleh setiap data (id, nama, dll)
- baris, record data (data dari sebuah table)

umumnya untuk membuat sebuah table didalam database sql, harus menguasai query language sql

mySQL all in one packages 

### Migration
https://laravel.com/docs/9.x/migrations#main-content
pada laravel terdapat fitur migration (database migration),
beberapa kasus ketika menggunakan database sql secara langsung
"Can i have the database configuration for your app?"
"Hey team, I need to add a new column in the table A, guess you should follow my new database structure."
"I want to create a table but I'm not familiar with SQL"

dengan laravel kita dapat menggunakan fitur migration (tool) untuk memudahkan dalam mendefine apa-apa saja konfigurasi database yang akan kita gunakan, 
atribut pada table dapat digambar dalam sebuah syntax php (id, nama dll)

### Create Migration
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

### Menjalankan Migration
jika belum mempunyai database dan langsung menjalankan migration, maka laravel akan otomatis memberi saran untuk membuat database dengan nama yang sama dengan value DB_DATABASE pada .env
`php artisan migrate`

## Database Seeding
adalah sebuah aksi, kita akan mempopulate database dengan data yang sudah kita sediakan,
misal, pada saat initiate database kita dapat memasukkan data-data yang sudah kita sediakan sebelumnya

### Create seeder
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

### Menjalankan seeder
`php artisan db:seed`
menjalankan DatabaseSeeder.php yang mana isinya dapat memanggil kelas seeder yang lain

`php artisan db:seed --class=StudentSeeder`
menjalankan spesifik seeder

## Faker
https://fakerphp.github.io/
sebuah library php yang dapat mengenerate data palsu seperti nama, angka, alamat, dll.
