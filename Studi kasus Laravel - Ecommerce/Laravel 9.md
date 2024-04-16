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

- [Generating Model Classes](https://laravel.com/docs/9.x/eloquent#generating-model-classes)
- [Eloquent Model Conventions](https://laravel.com/docs/9.x/eloquent#eloquent-model-conventions)
    - [Table Names](https://laravel.com/docs/9.x/eloquent#table-names)
    - [Primary Keys](https://laravel.com/docs/9.x/eloquent#primary-keys)
    - [UUID & ULID Keys](https://laravel.com/docs/9.x/eloquent#uuid-and-ulid-keys)
    - [Timestamps](https://laravel.com/docs/9.x/eloquent#timestamps)
    - [Database Connections](https://laravel.com/docs/9.x/eloquent#database-connections)
    - [Default Attribute Values](https://laravel.com/docs/9.x/eloquent#default-attribute-values)
    - [Configuring Eloquent Strictness](https://laravel.com/docs/9.x/eloquent#configuring-eloquent-strictness)
- [Retrieving Models](https://laravel.com/docs/9.x/eloquent#retrieving-models)
    - [Collections](https://laravel.com/docs/9.x/eloquent#collections)
    - [Chunking Results](https://laravel.com/docs/9.x/eloquent#chunking-results)
    - [Chunk Using Lazy Collections](https://laravel.com/docs/9.x/eloquent#chunking-using-lazy-collections)
    - [Cursors](https://laravel.com/docs/9.x/eloquent#cursors)
    - [Advanced Subqueries](https://laravel.com/docs/9.x/eloquent#advanced-subqueries)
- [Retrieving Single Models / Aggregates](https://laravel.com/docs/9.x/eloquent#retrieving-single-models)
    - [Retrieving Or Creating Models](https://laravel.com/docs/9.x/eloquent#retrieving-or-creating-models)
    - [Retrieving Aggregates](https://laravel.com/docs/9.x/eloquent#retrieving-aggregates)
- [Inserting & Updating Models](https://laravel.com/docs/9.x/eloquent#inserting-and-updating-models)
    - [Inserts](https://laravel.com/docs/9.x/eloquent#inserts)
    - [Updates](https://laravel.com/docs/9.x/eloquent#updates)
    - [Mass Assignment](https://laravel.com/docs/9.x/eloquent#mass-assignment)
    - [Upserts](https://laravel.com/docs/9.x/eloquent#upserts)
- [Deleting Models](https://laravel.com/docs/9.x/eloquent#deleting-models)
    - [Soft Deleting](https://laravel.com/docs/9.x/eloquent#soft-deleting)
    - [Querying Soft Deleted Models](https://laravel.com/docs/9.x/eloquent#querying-soft-deleted-models)
- [Pruning Models](https://laravel.com/docs/9.x/eloquent#pruning-models)
- [Replicating Models](https://laravel.com/docs/9.x/eloquent#replicating-models)
- [Query Scopes](https://laravel.com/docs/9.x/eloquent#query-scopes)
    - [Global Scopes](https://laravel.com/docs/9.x/eloquent#global-scopes)
    - [Local Scopes](https://laravel.com/docs/9.x/eloquent#local-scopes)
- [Comparing Models](https://laravel.com/docs/9.x/eloquent#comparing-models)
- [Events](https://laravel.com/docs/9.x/eloquent#events)
    - [Using Closures](https://laravel.com/docs/9.x/eloquent#events-using-closures)
    - [Observers](https://laravel.com/docs/9.x/eloquent#observers)
    - [Muting Events](https://laravel.com/docs/9.x/eloquent#muting-events)
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

https://laravel.com/docs/9.x/blade#main-content
Laravel menggunakan Blade, 
merupakan templating engine dari laravel (html + php), seperti html murni yang didalamnya kita dapat menggunakan logic pada blade dengan syntax php

resources -> views -> yourView.blade.php

format penamaan, 
namaFile diikuti .blade.php
example.blade.php

`Route::get('/greeting/{name}', function () {`
		
view('namaFile');
method view() relative terhadap resources -> views -> namaFile
jika blade view didalam folder bisa ditambahkan "." untuk pemisah
view('layouts.example');

    `return view('example', [
	    'name' => $name
    ]);`
    
  
`})->name('greeting_with_name');`

You may display the contents of the `name` variable like so (in Views of blade):
Hello, {{ $name }}.

Blade's `{{ }}` echo statements are automatically sent through PHP's `htmlspecialchars` function to prevent XSS attacks.

ibarat ini codingan php {{}}

- [Introduction](https://laravel.com/docs/9.x/blade#introduction)
    - [Supercharging Blade With Livewire](https://laravel.com/docs/9.x/blade#supercharging-blade-with-livewire)
- [Displaying Data](https://laravel.com/docs/9.x/blade#displaying-data)
    - [HTML Entity Encoding](https://laravel.com/docs/9.x/blade#html-entity-encoding)
    - [Blade & JavaScript Frameworks](https://laravel.com/docs/9.x/blade#blade-and-javascript-frameworks)
- [Blade Directives](https://laravel.com/docs/9.x/blade#blade-directives)
    - [If Statements](https://laravel.com/docs/9.x/blade#if-statements)
    - [Switch Statements](https://laravel.com/docs/9.x/blade#switch-statements)
    - [Loops](https://laravel.com/docs/9.x/blade#loops)
    - [The Loop Variable](https://laravel.com/docs/9.x/blade#the-loop-variable)
    - [Conditional Classes](https://laravel.com/docs/9.x/blade#conditional-classes)
    - [Additional Attributes](https://laravel.com/docs/9.x/blade#additional-attributes)
    - [Including Subviews](https://laravel.com/docs/9.x/blade#including-subviews)
    - [The `@once` Directive](https://laravel.com/docs/9.x/blade#the-once-directive)
    - [Raw PHP](https://laravel.com/docs/9.x/blade#raw-php)
    - [Comments](https://laravel.com/docs/9.x/blade#comments)
- [Components](https://laravel.com/docs/9.x/blade#components)
    - [Rendering Components](https://laravel.com/docs/9.x/blade#rendering-components)
    - [Passing Data To Components](https://laravel.com/docs/9.x/blade#passing-data-to-components)
    - [Component Attributes](https://laravel.com/docs/9.x/blade#component-attributes)
    - [Reserved Keywords](https://laravel.com/docs/9.x/blade#reserved-keywords)
    - [Slots](https://laravel.com/docs/9.x/blade#slots)
    - [Inline Component Views](https://laravel.com/docs/9.x/blade#inline-component-views)
    - [Dynamic Components](https://laravel.com/docs/9.x/blade#dynamic-components)
    - [Manually Registering Components](https://laravel.com/docs/9.x/blade#manually-registering-components)
- [Anonymous Components](https://laravel.com/docs/9.x/blade#anonymous-components)
    - [Anonymous Index Components](https://laravel.com/docs/9.x/blade#anonymous-index-components)
    - [Data Properties / Attributes](https://laravel.com/docs/9.x/blade#data-properties-attributes)
    - [Accessing Parent Data](https://laravel.com/docs/9.x/blade#accessing-parent-data)
    - [Anonymous Components Paths](https://laravel.com/docs/9.x/blade#anonymous-component-paths)
- [Building Layouts](https://laravel.com/docs/9.x/blade#building-layouts)
    - [Layouts Using Components](https://laravel.com/docs/9.x/blade#layouts-using-components)
    - [Layouts Using Template Inheritance](https://laravel.com/docs/9.x/blade#layouts-using-template-inheritance)
- [Forms](https://laravel.com/docs/9.x/blade#forms)
    - [CSRF Field](https://laravel.com/docs/9.x/blade#csrf-field)
    - [Method Field](https://laravel.com/docs/9.x/blade#method-field)
    - [Validation Errors](https://laravel.com/docs/9.x/blade#validation-errors)
- [Stacks](https://laravel.com/docs/9.x/blade#stacks)
- [Service Injection](https://laravel.com/docs/9.x/blade#service-injection)
- [Rendering Inline Blade Templates](https://laravel.com/docs/9.x/blade#rendering-inline-blade-templates)
- [Rendering Blade Fragments](https://laravel.com/docs/9.x/blade#rendering-blade-fragments)
- [Extending Blade](https://laravel.com/docs/9.x/blade#extending-blade)
    - [Custom Echo Handlers](https://laravel.com/docs/9.x/blade#custom-echo-handlers)
    - [Custom If Statements](https://laravel.com/docs/9.x/blade#custom-if-statements)
### Controller
https://laravel.com/docs/9.x/controllers#main-content
bagian dari MVC yang mengatur logic dari aplikasi laravel.

kita dapat memasukkan logic di routing, tapi sesuai dengan framework MVC semua logic seharusnya ditempatkan pada controller

php artisan make:controller ExampleController
konvensi penamaan controller pada laravel ialah Nama diikuti keyword 'Controller'

App -> Http -> Controllers -> YourController.php

`use App\Models\Student;`

`class StudentController extends Controller {`
`public function show($id)`
    `{`
    melakukan query untuk mencari Student dengan id yang kita kirim lewat routing, kemudian kita akses atribut 'name' nya lalu kita passing ke view show.blade.php dengan nama variable 'student'
        `$student = Student::find(id)->name;`
        setiap model memiliki syntaxs eloquent dalam mempermudah melakukan query (jadi kita tidak perlu melakukan raw query), Model::find(); dll
        `return view(`
            `'show',`
                `'student' => $student`
            `]`
        `);`
    `}`
`}`

pada web.php
`Route::get('/show/{id}', App\Http\Controllers\StudentController::class, 'show')->name('show');`
jadi kita passing variable $id kedalam controller yang nantinya ditangkap pada parameter method dapat digunakan pada controller

- [Introduction](https://laravel.com/docs/9.x/controllers#introduction)
- [Writing Controllers](https://laravel.com/docs/9.x/controllers#writing-controllers)
    - [Basic Controllers](https://laravel.com/docs/9.x/controllers#basic-controllers)
    - [Single Action Controllers](https://laravel.com/docs/9.x/controllers#single-action-controllers)
- [Controller Middleware](https://laravel.com/docs/9.x/controllers#controller-middleware)
- [Resource Controllers](https://laravel.com/docs/9.x/controllers#resource-controllers)
    - [Partial Resource Routes](https://laravel.com/docs/9.x/controllers#restful-partial-resource-routes)
    - [Nested Resources](https://laravel.com/docs/9.x/controllers#restful-nested-resources)
    - [Naming Resource Routes](https://laravel.com/docs/9.x/controllers#restful-naming-resource-routes)
    - [Naming Resource Route Parameters](https://laravel.com/docs/9.x/controllers#restful-naming-resource-route-parameters)
    - [Scoping Resource Routes](https://laravel.com/docs/9.x/controllers#restful-scoping-resource-routes)
    - [Localizing Resource URIs](https://laravel.com/docs/9.x/controllers#restful-localizing-resource-uris)
    - [Supplementing Resource Controllers](https://laravel.com/docs/9.x/controllers#restful-supplementing-resource-controllers)
    - [Singleton Resource Controllers](https://laravel.com/docs/9.x/controllers#singleton-resource-controllers)
- [Dependency Injection & Controllers](https://laravel.com/docs/9.x/controllers#dependency-injection-and-controllers)



## Relations
Database Relationship ialah sebuah hubungan antar table.

Bagaimana sebuah table dapat berhubungan dengan table lain?
biasanya table tersebut menyimpan atribut dari table lainnya.
- Primary key (Atribut unik), Unique identifier of every records in table.
	id bersifat unique, karena tiap records mempunya id yang berbeda
- Foreign key, A database key that is used to link two tables together
	Misal table contact yang dapat dimiliki oleh student, jadi pada table contact menyimpan primary key dari student

**Adding Keys**
membuat atribut baru, (id pada laravel menggunakan unsignedBigInteger)
dengan nama 'student_id' (nama bebas)
$table->unsignedBigInteger('student_id');

menggunakan foreign agar atribut 'student_id' menjadi foreign key -> references ke atribut apa ('id') -> pada table apa ('students')
$table->foreign('student_id')->references('id')->on('students');

// Shorter way
cara pendek jika kita mengikuti semua konvensi laravel
foreignId diikuti nama foreignKey yang akan kita gunakan -> constrained terhadap table yang mana ('students')
$table->foreignId('student_id')->constrained('students');

**ERD (Entity Relationship Diagram)**
Diagram yang menggambarkan relasi antar entitas / objek yang akan disimpan di database dan diolah dengan aplikasi / sistem

Entitas -> unit/objek yang mempunyai kepentingan/menjadi bagian dari jalannya sistem

Atribut -> karakteristik/identitas entitas
### One to One
sebuah table dimana setiap table hanya bisa dimiliki salah satu record lainnya

### One to Many
### Many to Many
