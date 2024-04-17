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
https://laravel.com/docs/9.x/eloquent-relationships

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
`$table->foreign('student_id')->references('id')->on('students');`

// Shorter way
cara pendek jika kita mengikuti semua konvensi laravel
foreignId diikuti nama foreignKey yang akan kita gunakan -> constrained terhadap table yang mana ('students')
`$table->foreignId('student_id')->constrained('students');`

- [Introduction](https://laravel.com/docs/9.x/eloquent-relationships#introduction)
- [Defining Relationships](https://laravel.com/docs/9.x/eloquent-relationships#defining-relationships)
    - [One To One](https://laravel.com/docs/9.x/eloquent-relationships#one-to-one)
    - [One To Many](https://laravel.com/docs/9.x/eloquent-relationships#one-to-many)
    - [One To Many (Inverse) / Belongs To](https://laravel.com/docs/9.x/eloquent-relationships#one-to-many-inverse)
    - [Has One Of Many](https://laravel.com/docs/9.x/eloquent-relationships#has-one-of-many)
    - [Has One Through](https://laravel.com/docs/9.x/eloquent-relationships#has-one-through)
    - [Has Many Through](https://laravel.com/docs/9.x/eloquent-relationships#has-many-through)
- [Many To Many Relationships](https://laravel.com/docs/9.x/eloquent-relationships#many-to-many)
    - [Retrieving Intermediate Table Columns](https://laravel.com/docs/9.x/eloquent-relationships#retrieving-intermediate-table-columns)
    - [Filtering Queries Via Intermediate Table Columns](https://laravel.com/docs/9.x/eloquent-relationships#filtering-queries-via-intermediate-table-columns)
    - [Ordering Queries Via Intermediate Table Columns](https://laravel.com/docs/9.x/eloquent-relationships#ordering-queries-via-intermediate-table-columns)
    - [Defining Custom Intermediate Table Models](https://laravel.com/docs/9.x/eloquent-relationships#defining-custom-intermediate-table-models)
- [Polymorphic Relationships](https://laravel.com/docs/9.x/eloquent-relationships#polymorphic-relationships)
    - [One To One](https://laravel.com/docs/9.x/eloquent-relationships#one-to-one-polymorphic-relations)
    - [One To Many](https://laravel.com/docs/9.x/eloquent-relationships#one-to-many-polymorphic-relations)
    - [One Of Many](https://laravel.com/docs/9.x/eloquent-relationships#one-of-many-polymorphic-relations)
    - [Many To Many](https://laravel.com/docs/9.x/eloquent-relationships#many-to-many-polymorphic-relations)
    - [Custom Polymorphic Types](https://laravel.com/docs/9.x/eloquent-relationships#custom-polymorphic-types)
- [Dynamic Relationships](https://laravel.com/docs/9.x/eloquent-relationships#dynamic-relationships)
- [Querying Relations](https://laravel.com/docs/9.x/eloquent-relationships#querying-relations)
    - [Relationship Methods Vs. Dynamic Properties](https://laravel.com/docs/9.x/eloquent-relationships#relationship-methods-vs-dynamic-properties)
    - [Querying Relationship Existence](https://laravel.com/docs/9.x/eloquent-relationships#querying-relationship-existence)
    - [Querying Relationship Absence](https://laravel.com/docs/9.x/eloquent-relationships#querying-relationship-absence)
    - [Querying Morph To Relationships](https://laravel.com/docs/9.x/eloquent-relationships#querying-morph-to-relationships)
- [Aggregating Related Models](https://laravel.com/docs/9.x/eloquent-relationships#aggregating-related-models)
    - [Counting Related Models](https://laravel.com/docs/9.x/eloquent-relationships#counting-related-models)
    - [Other Aggregate Functions](https://laravel.com/docs/9.x/eloquent-relationships#other-aggregate-functions)
    - [Counting Related Models On Morph To Relationships](https://laravel.com/docs/9.x/eloquent-relationships#counting-related-models-on-morph-to-relationships)
- [Eager Loading](https://laravel.com/docs/9.x/eloquent-relationships#eager-loading)
    - [Constraining Eager Loads](https://laravel.com/docs/9.x/eloquent-relationships#constraining-eager-loads)
    - [Lazy Eager Loading](https://laravel.com/docs/9.x/eloquent-relationships#lazy-eager-loading)
    - [Preventing Lazy Loading](https://laravel.com/docs/9.x/eloquent-relationships#preventing-lazy-loading)
- [Inserting & Updating Related Models](https://laravel.com/docs/9.x/eloquent-relationships#inserting-and-updating-related-models)
    - [The `save` Method](https://laravel.com/docs/9.x/eloquent-relationships#the-save-method)
    - [The `create` Method](https://laravel.com/docs/9.x/eloquent-relationships#the-create-method)
    - [Belongs To Relationships](https://laravel.com/docs/9.x/eloquent-relationships#updating-belongs-to-relationships)
    - [Many To Many Relationships](https://laravel.com/docs/9.x/eloquent-relationships#updating-many-to-many-relationships)
- [Touching Parent Timestamps](https://laravel.com/docs/9.x/eloquent-relationships#touching-parent-timestamps)
### One to One
sebuah table dimana setiap table hanya bisa dimiliki salah satu record dari table lainnya (hanya salah satu)
![[Pasted image 20240416170158.png]]
satu student hanya memiliki satu contact, begitu juga dengan contact
satu contact hanya dimiliki satu student (tidak ada student yang memiliki contact yang sama persis)

contoh kasus
membuat migration baru untuk table contacts
`php artisan make:migration create_contacts_table`

inisialisasi atribut yang diperlukan pada **table contacts**
`public function up()`
    `{`
        `Schema::create('contacts', function (Blueprint $table) {`
            `$table->id();`
            `$table->foreignId('student_id')->constrained('students')->cascadeOnUpdate()->cascadeOnDelete();`
            `$table->string('phone');`
            `$table->string('email');`
            `$table->string('address');`
            `$table->timestamps();`
        `});`
    `}`

migrasi dari table yang belum terbuat kedalam database
`php artisan migrate`

**Defining Relationships**
selanjutnya kita definisikan relationship nya didalam model,
karena model representasi dari data pada database kita

`class Student extends Model`
`{`
    `use HasFactory;`
  
    `protected $fillable = [`
        `'name',`
        `'score',`
        `'teacher_id'`
    `];`

Satu student hanya boleh memiliki satu contact
    `public function contact() {`
        `return $this->hasOne(Contact::class);`
    `}`
`}`

**Defining The Inverse Of The Relationships**
`class Contact extends Model`
`{`
    `use HasFactory;`

    `public function student() {`
        `return $this->belongsTo(Student::class);`
    `}`
`}`

kita dapat mengakses contact dengan id yang sesuai dengan id student
`class StudentController extends Controller`
`{`
	`public function show($id) {`
		1. Temukan objek `Student` berdasarkan `id` yang diberikan.
		2. Panggil metode `contact` pada objek `Student` untuk mendapatkan objek `Contact`.
		3. Dari objek `Contact` yang diperoleh, dapatkan properti `address`, yang merupakan alamat siswa.
karena kita sudah define setiap student hasOne contact jadi hal dibawah tidak masalah, akan return satu address
		`$address = Student::find(id)->contact->address;`
		`return view('example', [`
			`'address' => $address`
		`]);`
	`}`
`}`

Kesimpulan:
kita dapat langsung mengakses data pada table contact pada table student yang sesuai dengan id yang ada pada data student.

Contact Table menyimpan atribut id sebagai foreignKey yang berasosiasi dengan atribut id pada Student Table,

Student hasOne Contact (karena student menitipkan id pada Contact sebagai foreignKey)
Contact belongsTo Student (karena contact menyimpan id milik Student sebagai foreignKey)

### One to Many
![[Pasted image 20240416210433.png]]
salah satu table memiliki banyak record dari table lainnya,
misal pada contoh diatas,
satu student hanya bisa memiliki satu wali kelas saja,
satu wali kelas bisa memiliki banyak student

Teacher table
`public function up()`
    `{`
        `Schema::create('teachers', function (Blueprint $table) {`
            `$table->id();`
            `$table->string('name');`
            `$table->timestamps();`
        `});`
    `}`

Student Table
`public function up()`
    `{`
        `Schema::create('students', function (Blueprint $table) {`
            `$table->id();`
            `$table->foreignId('teacher_id')->constrained('teachers')->cascadeOnUpdate()->cascadeOnDelete();`
            `$table->string('name');`
            `$table->integer('score');`
            `$table->timestamps();`
        `});`
    `}`

!ketika melakukan perintah "php artisan migrate" table yang sebelumnya sudah ter-migrate pada saat terjadi pengurangan ataupun pertambahan atribut pada table tersebut tidak akan terkait karena perintah tersebut berlaku hanya untuk table yang belum pernah sama sekali ter-migrate.

untuk mengatasi masalah tersebut kita pakai perintah,
`php artisan migrate:fresh`
dengan perintah migrate:fresh laravel akan drop semua table dan menambahkan ulang semua migration table yang ada

!urutan migration sangat berpengaruh ketika melakukan relationships (laravel melakukan migrate secara sequential)

dan

!bisa juga menggunakan outer table dengan migration (selain migrate:fresh)

**Definition**
`class Teacher extends Model`
`{`
    `use HasFactory;`

bedanya one to many dengan one to one ialah
pada one to many table yang menyimpan foreignKey ditulis dalam model dengan plural
    `public function students() {`
        `return $this->hasMany(Student::class);`
    `}`
`}`

**Definition Inverse**
`class Student extends Model`
`{`
    `use HasFactory;`
  
    `protected $fillable = [`
        `'name',`
        `'score',`
        `'teacher_id'`
    `];`

    `public function teacher() {`
        `return $this->belongsTo(Teacher::class);`
    `}`
`}`

Mengakses teacher dari salah satu murid
`class StudentController extends Controller`
`{`
	`public function show($id) {`
		`$name = Student::find(id)->teacher->name;`
		`return view('example', [`
			`'name' => $name`
		`]);`
	`}`
`}`

Mengakses Student dari salah satu teacher
`class StudentController extends Controller`
`{`
	`public function show($id) {`
		`$students = Teacher::find(id)->students;`
		`return view('example', [`
			`'students' => $students`
		`]);`
	`}`
`}`

!karena data Students memiliki hanya satu teacher, kita lakukan perulangan dengan foreach
### Many to Many
![[Pasted image 20240416225207.png]]
Diantara kedua tabel (Student dan Activity), masing-masing record itu bisa mempunyai hubungan ke record lain dalam jumlah banyak, 
misal, satu student bisa mengikuti banyak aktivitas disekolahnya
di aktivitas, dia (activity) bisa mempunyai banyak student untuk mengikutinya

!untuk mewujudkan many to many relationships, kita membutuhkan **pivot table**
pivot table berisi foreignKey dari kedua table Student dan Activity ini.

jadi, nama table pada pivot table harus sesuai dengan urutan abjad dari masing-masing table, karena "S"tudent dan "A"ctivity urutan abjad didahului oleh "A" maka ditulis "Activity_Student"

Activities Table
`public function up()`
    `{`
        `Schema::create('activities', function (Blueprint $table) {`
            `$table->id();`
            `$table->string('name');`
            `$table->timestamps();`
        `});`
    `}`

Pivot table
`public function up()`
    `{`
        `Schema::create('activity_student', function (Blueprint $table) {`
            `$table->id();`
            `$table->foreignId('activity_id')->constrained('activities')->cascadeOnUpdate()->cascadeOnDelete();`
            `$table->foreignId('student_id')->constrained('students')->cascadeOnUpdate()->cascadeOnDelete();`
            `$table->timestamps();`
        `});`
    `}`

Students table
`public function up()`
    `{`
        `Schema::create('students', function (Blueprint $table) {`
            `$table->id();`
            `$table->foreignId('teacher_id')->constrained('teachers')->cascadeOnUpdate()->cascadeOnDelete();`
            `$table->string('name');`
            `$table->integer('score');`
            `$table->timestamps();`
        `});`
    `}`

Table Structure
definition
setiap model belongsToMany model lainnya

untuk inverse
belongsToMany model lainnya

Activity model
`class Activity extends Model`
`{`
    `use HasFactory;`
nama method plural karena plural
    `public function students() {`
        `return $this->belongsToMany(Student::class);`
    `}`
`}`

Student model
`class Student extends Model`
`{`
    `use HasFactory;`

    `protected $fillable = [`
        `'name',`
        `'score',`
        `'teacher_id'`
    `];`
nama method plural karena many
    `public function activities() {`
        `return $this->belongsToMany(Activity::class);`
    `}`
`}`

Mengakses salah satu Activity yang mempunyai students didalamnya
`class StudentController extends Controller`
`{`
	`public function show($id) {`
		`$activity = Activity::find(id);`
		`$students = $activity->students;`
		`return view('example', [`
			`'activity' => $activity,`
			`'students' -> $students`
		`]);`
	`}`
`}`

Mengakses salah satu Student yang mempunyai activities didalamnya
`class StudentController extends Controller`
`{`
	`public function show($id) {`
		`$student = Student::find(id);`
		`$activities = $student->activities;`
		`return view('example', [`
			`'activities' => $activities,`
			`'student' -> $student`
		`]);`
	`}`
`}`
## Building Basic CRUD App