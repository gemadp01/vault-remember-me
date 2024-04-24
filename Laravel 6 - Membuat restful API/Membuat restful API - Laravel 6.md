## Intro
### Web Service dan REST API
Web service ialah sama halnya software yang kita letakkan didalam sebuah komputer, dimana berfungsi untuk mendengar/menunggu suatu request yang bekerja didalam network lalu dapat melayani sebuah web documents (HTML, JSON, XML, Images),

Web service merupakan aplikasi layanan berbasis web dimana dapat menyelesaikan suatu tugas secara spesifik melalui jaringan web atau menggunakan protocol http

singkatnya, web service merupakan layanan yang dapat membantu kita untuk bertukar data antar perangkat tanpa perlu khawatir kita harus menyamakan software/os/bahasa pemrograman yang digunakan karena web service mempunyai standar tipe yang sama karena kita hanya membutuhkan document yang diberikan dari web service

#### The Advantages of Web Service
- Usability
- Interoperability
- Reusability
- Deployability

kita dapat memanfaatkan resource yang kita perlu tanpa membuatnya secara manual/sendiri karena kita dapat memanfaatkan layanan yang dibuat dari orang lain tentunya mempermudah kita dalam membuat aplikasi

#### Considerations of web service
- Latency
	kecepatan dalam berinteraksi/bertukar data (web service mempercepat dalam pertukaran data), tidak cocok jika proses lamban/memakan waktu
- Security
	untuk menggunakan web service/bertukar data/membagikan suatu resource secara public kita perlu menentukan keamanannya.
	- Authentication & Authorization
		mengidentifikasi siapa saja yang melakukan request, user ini dapat request apa saja
		- Basic Auth (konsep security)
			seperti proses login
		- API Key (konsep security)
			berbasiskan token/string secara unique yang selalu dikirimkan pada saat melakukan request ke suatu web service.

			Jika pernah memanfaatkan layanan dari suatu web dan perlu untuk mengisikan token (API key ini tokennya), kita perlu membatasi resource dari web service ini karena kita tidak ingin kehabisan resource gara-gara ada yang melakukan spam/secara sengaja menghabiskan resource

#### REST
REST = Representational State Transfer
merupakan software arsitektural/gaya suatu arsitektural software dimana kita mendefinisikan suatu fungsi-fungsi tertentu (mempunyai scope masing-masing) dimana fungsi tersebut akan kita gunakan sebagai layanan berbasis web sebelumnya.

konsep desain untuk membuat layanan berbasis web/api, karena web service sendiri memiliki beberapa konsep bukan hanya rest, soap (simple object access protocol)

rest salah satu konsep dari web service
rest = web service
web service harus rest = tidak

perbedaan konsep rest dan web service lainnya,
##### Rest Principles
- API defined by URI (Uniform Resource Identifier)
- Operations: HTTP Verb (GET, POST, PUT, DELETE)
- Data Format: HTML, XML, JSON, Plain Text
- Stateless (tidak kebergantungan)

Laravel secara default menggunakan konsep rest

### Persiapan Project
- sudah membuat model ()
- design database
- sudah merealisasikannya (User, Post, Comment)
	- User boleh memiliki banyak post, post tersebut hanya terikat satu user
	- Post terikat pada user dan pada post boleh memiliki banyak comment
	- Comment hanya terdapat per post dan boleh banyak user
- seeder database

`git clone https://github.com/idstck/laravel-restapi/`

Composer 2.3.0 dropped support for PHP <7.2.5 and you are running 7.2.0, please upgrade PHP or use Composer 2.2 LTS via "composer self-update --2.2". Aborting.

`composer install`
`cp .env.example .env`
`php artisan key:generate`

!menyamakan progress sesuai commits, copy code hash kemudian "git checkout yourHashCopied"

pada .env
DB_DATABASE=laravel_restapi
DB_USERNAME dan DB_PASSWORD sesuaikan dengan dbms masing-masing

`php artisan migrate --seed`

pada database terdapat table users dan posts dan isi nya dihasilkan dari seeding
posts berelasi dengan user_id dan comments berelasi dengan user_id dan post_id

terdapat model User, Post, dan Comment yang sudah didefinisikan relasi tiap modelnya

terdapat database -> factories -> Comment, Post, UserFactory

`use Faker\Generator as Faker;`

CommentFactory
`use App\Comment;`

`$factory->define(Comment::class, function (Faker $faker) {`
    `return [`
        `'user_id' => rand(1, 10),`
        `'post_id' => rand(1, 20),`
        `'body' => $faker->paragraph(3)`
    `];`
`});`

PostFactory
`use App\Post;`

`$factory->define(Post::class, function (Faker $faker) {`
    `return [`
        `'user_id' => rand(1, 10),`
        `'title' => $faker->sentence(6),`
        `'body' => $faker->paragraph(4)`
    `];`
`});`

UserFactory
`use App\User;`

`$factory->define(User::class, function (Faker $faker) {`
    `return [`
        `'name' => $faker->name,`
        `'email' => $faker->unique()->safeEmail,`
        `'email_verified_at' => now(),`
        `'password' => '$2y$10$92IXUNpkjO0rOQ5byMi.Ye4oKoEa3Ro9llC/.og/at2.uheWG/igi', // password`
        `'api_token' => Str::random(80),`
        `'remember_token' => Str::random(10),`
    `];`
`});`

proses mengenerate data dummy

membuat syntax baru pada kelas seed DataTableSeeder.php (untuk memanggil )
`class DataTableSeeder extends Seeder`
`{`
    `public function run()`
    `{`
        `factory(App\User::class, 10)->create();`
        `factory(App\Post::class, 20)->create();`
        `factory(App\Comment::class, 40)->create();`
    `}`
`}`

dan memanggilnya pada DatabaseSeeder.php
`public function run()`
    `{`
        `// $this->call(UsersTableSeeder::class);`
        `$this->call(DataTableSeeder::class);`
    `}`
## Bread Api Endpoint
### Browse API Endpoint
Membuat web service dengan laravel

routes -> web.php, biasa digunakan untuk mendeklarasikan endpoint halaman

untuk layanan web service, laravel menyediakan satu file route khusus yaitu api.php

yang membedakan ialah penggunaan dari middleware web dan api pada app -> Http -> Kernel.php 

`protected $middlewareGroups = [`
        `'web' => [`
            `\App\Http\Middleware\EncryptCookies::class,`
            `\Illuminate\Cookie\Middleware\AddQueuedCookiesToResponse::class,`
            `\Illuminate\Session\Middleware\StartSession::class,`
            `// \Illuminate\Session\Middleware\AuthenticateSession::class,`
            `\Illuminate\View\Middleware\ShareErrorsFromSession::class,`
            `\App\Http\Middleware\VerifyCsrfToken::class,`
            `\Illuminate\Routing\Middleware\SubstituteBindings::class,`
        `],`
        pada middleware groups 'web' dan 'api' beda dalam memperlakukan keduanya

        `'api' => [`
            `'throttle:60,1',`
            `'bindings',`
        `],`
    `];`

jadi, pada saat membuat url kita gunakan file api.php pada saat mengarahkan pada suatu resource/endpoint yang akan kita gunakan.

hal pertama yang dilakukan
- menampilkan seluruh data pada tabel posts, memerlukan suatu controller dan method beserta url untuk memanggil method tersebut

`php artisan make:controller PostController`

pada PostController
class PostController extends Controller {
	public function index() 
	{
		$data = Post::all(); 
		mendapatkan semua data pada Post Table.

		return response()->json($data, 200);
selanjutnya, kita perlu menampilkan response nya (response/data yang dikirimkan web service dengan konsep rest ini bisa dalam format XML, JSON, plaintext, dll) kita akan tampilkan dalam bentuk JSON,
!pada umumnya response pada api sudah berbentuk tipe data json
	}
}

return dengan memanfaatkan helper response() (helper laravel) yang dimiliki laravel, lalu chaining dengan method json(p1-data yang ingin dimunculkan, p2-kode/status HTTP yang kita kirimkan)
200 yang berarti Response bisa kita jalankan, berhasil menampilkan data dari post tersebut dan response berhasil dijalankan

panggil helper response()->json($data, 200);

untuk melihat response api/endpoint dari api
- tambahkan syntax untuk endpoint pada api.php, agar dapat mengakses apa yang sudah di set pada controller sebelumnya
`Route::get('/post', 'PostController@index');`
- !perbedaan lagi web.php dan api.php, kita perlu tambahkan prefix /api/

`localhost:8000/api/post`
yang tampil ialah data berbentuk json dari Post table (karena sebelumnya kita set json()), yang berarti berhasil menampilkan data dari method pada PostController yaitu index().

didalam browser hanya menampilkan data json, kita tidak bisa dengan mengubah melihat response dari header maupun bagian payload lainnya melainkan kita perlu membuka inspect browser untuk melihat informasi tambahan, klik kanan -> inspect -> tab 'Network'
post -> Headers dan Response

Post -> Status 200 -> Type Document dll
double klik pada data Post untuk melihat request header, response header dll

mungkin akan sedikit kerepotan pada saat perlu mengirimkan suatu data dengan http method post dll, jika pada halaman browser kita harus membuat form dan karena didalam aplikasi berbasis web kita hanya membuat web service saja (tidak dengan view nya),
mengirimkan data untuk proses post, put, delete dengan menggunakan tool,
software postman, untuk mengetes api endpoint yang kita dibuat (tanpa membuat view)

tampilan sama halnya seperti browser,
tambahkan http method dan endpoint yang ingin dilakukan pengecekan
!tersedia tab params, Authorization, Headers, Body, Pre-request script, Test, Settings, ataupun beberapa payload lainnya,
dengan mudah tanpa perlu membuat halaman form (view)

HTTP Method GET - localhost:8000/api/post (url ini disebut api endpoint)
yang kita harapkan ialah response tipe json
pada headers, kita tentukan Key "Accept", dan value "application/json" (data yang diharapkan) -> Send request (untuk mengirimkan request pada api endpoint tersebut),
!hasilnya akan sama saja seperti pada pengecekan data langsung pada browser

setelah send request terdapat informasi lainnya,
tab "Body" didalamnya terdapat -> memiliki response dari data post
Status code, time, size (datanya)
dan memberikan format data yang ingin ditampilkan
dan informasi Headers (response Headers)

kesimpulan
membuat api endpoint pertama

### Read API Endpoint
Mengambil suatu data tertentu sebagai object yang ada didalam model post

- membuat endpoint baru hanya untuk menampilkan satu data/detail data dari kelas post, pada layanan rest api kita perlu menampilkan satu buah data saja dari sekumpulan data sebelumnya

menambah method baru pada PostController
`public function show($id)`
kita akan fungsikan method show, untuk menampilkan satu data saja berdasarkan parameter $id (primary key), akan kita gunakan parameter tersebut untuk menjadi data berdasarkan id yang kita pilih
    `{`
    memanfaatkan method find() dari eloquent ORM, berdasarkan id para parameter
        `$data = Post::find($id);`
        `return response()->json($data, 200);`
    `}`

- membuat uri untuk mengarahkan pada method show($id)
`Route::get('/post/{id}', 'PostController@show');`


- mengisikan nilai dari parameter /post/{id}
api endpoint pada postman
http://localhost/api/post/{$id}

tambahkan value dari id data pada table post kedalam parameter pada uri
HTTP Method GET -> localhost:8000/api/post/1
dan set request headers key "Accept" dan value "application/json"

!id disesuaikan dengan data yang dihasilkan pada json get 

http://localhost/api/post/4

maka data yang tampil akan otomatis sesuai dengan id yang tertulis pada uri

bedanya api endpoint pertama (get all data) dan api end point kedua (get detail data)
- pertama, menampilkan semua data post kedalam json (array dengan value objek)
- kedua, menampilkan satu data post (objek)

### Add API Endpoint
menambahkan suatu data kedalam model post sekaligus mempraktekkan HTTP method selain GET pada implementasi web service rest api 

- menyiapkan method untuk melakukan proses create, yang mana akan menyimpan data yang dikirimkan oleh client atau melalui apk postman dan data tersebut kita simpan kedalam database

`public function store(Request $request)`
untuk mempermudah data request/data yang dikirimkan oleh client,
kita inject Class Request dan kita masukkan kedalam variabel $request (agar dapat dipanggil objeknya)
    `{`
    mendapatkan seluruh data yang dikirimkan user
        `$data = $request->all();`
        `$response = Post::create(data);`
klasifikasi code dari response sesuai dengan status code
https://developer.mozilla.org/en-US/docs/Web/HTTP/Status
        `return response()->json($response, 201);`

    `}`

- membuat uri dengan HTTP method POST karena kita tidak memerlukan parameter apapun tetapi data yang kita kirimkan kita harus simpan pada **body payload** (bukan pada params uri)
`Route::post('/post', 'PostController@store');`

pada postman
1. HTTP Method POST - localhost:8000/api/post
2. Request Headers seperti biasa "Accept" n "application/json" dan juga "Content-Type" n application/json (content-type yang akan kita kirimpan ingin berupa apa?)
3. inputan kita simpan di Body payload dengan tipe raw dengan beautification nya JSON
4. untuk data yang diinputkan sesuaikan dengan struktur data pada tabel post
user_id - title - body
	- untuk user_id kita inputkan secara manual sesuai dengan user yang tersedia (untuk sementara)

`{`
    `"user_id": 1,`
    `"title": "This is the first post from API",`
    `"body": "This is the content of first post from API"`
`}`

maka akan tampil return response() berupa data $response yang di buat kedalam database dan status, time, size
### Edit API Endpoint
Mengubah data pada database menggunakan HTTP Method put (sebetulnya bisa patch juga untuk sebagian atribut saja yang diubah)

- Membuat method baru pada PostController
`public function update(Request $request, Post $post)`
menangkap data yang dikirim kan oleh client pada (**parameter pertama**),
untuk menentukan object/resource mana yang akan kita ubah, kita memanfaatkan fitur route model binding jadi pada saat kita akan mengirim kan primary key pada client maka didalam laravel akan diterjemahkan sebagai object (tanpa query) otomatis mendapatkan datanya (**parameter kedua**)
    `{`
        `return response()->json($post, 200);`
    `}`

api endpoint dan untuk parameter disesuaikan dengan nama model yang kita berikan pada parameter kedua (disamakan)
!itu merupakan syarat menggunakan route model binding
`Route::put('/post/{post}', 'PostController@update');`

pada postman
localhost:8000/api/post/{post}
HTTP Method put -> localhost:8000/api/post/2
!id 2 (karena kita butuhkan primary key dari field pada tabel post)
Headers seperti biasa "Accept" n "application/json"

data pada Body sementara dikosongkan terlebih dahulu, karena kita akan mencoba melihat proses route model binding -> Send Request

maka yang muncul akan sesuai dengan data pada id yang dikirimkan (yang mana kita tidak melakukan query apapun tetapi hanya return saja pada method)

=="Laravel automatically resolves Eloquent models defined in routes or controller actions whose type-hinted variable names match a route segment name."==
https://laravel.com/docs/11.x/routing#route-model-binding

`use App\Models\User;`
`Route::get('/users/{user}', function (User $user) {`
`return $user->email;`
`});`

=="Since the `$user` variable is type-hinted as the `App\Models\User` Eloquent model and the variable name matches the `{user}` URI segment, Laravel will automatically inject the model instance that has an ID matching the corresponding value from the request URI. If a matching model instance is not found in the database, a 404 HTTP response will automatically be generated."==

`public function update(Request $request, Post $post) {`
	`$post->update(request->all());`
	`return response()->json($post, 200);`
`}`

HTTP Method put -> localhost:8000/api/post/21
Content-Type "application/json" dan Accept "application/json"
payload body
`{`
    `"user_id": 1,`
    `"title": "This is the first update from API 1",`
    `"body": "This is the content of first update from API 1"`
`}`

kesimpulan
berhasil mengupdate suatu resource yang sudah ada dengan HTTP method PUT
dengan mengirimkan pada payload body
dan mengupdate data pada method didalam PostController dengan eloquent model dengan memanfaatkan route model biding

!bisa juga memanfaatkan route model binding pada method show tanpa melakukan query karena secara otomatis mendapatkan object sesuai dengan primary key yang dikirimkan pada api endpoint dengan syarat uri harus menggunakan nama yang sama dengan type hint yang diberikan pada parameter/closure pada method tersebut
### Delete API Endpoint
HTTP Method delete, berfungsi untuk menghapus sebuah resource


`public function destroy(Post $post)`
    `{`
        `$post->delete();`
untuk menghapus suatu resource didalam laravel pada object eloquent, dapat memanggil method delete();
        `return response()->json(null, 200);`
    `}`

api endpoint/resource nya
`Route::delete('/post/{post}', 'PostController@destroy');`

pada postman,
HTTP Method delete -> localhost:8000/api/post/21
Headers seperti biasa Accept n application json
body kosong
Send Request

mengembalikkan response kosong (karena return kita null)
dengan status 200 OK yang mana eksekusi berhasil

!jika kita paksakan send request pada id yang sama (yang sudah didelete) yang terjadi akan menampilkan json yang berbeda/property
key "Accept" n "application/json" -> hanya menerima response json saja (menerima data dengan tipe json)
Status 404 Not Found -> resource yang kita cari tidak ada

selanjutnya ialah mengubah response yang diberikan laravel agar lebih rapih (handle error untuk mudah dibaca)
## Error Handling and Exception
### Custom Response untuk Data tidak ditemukan
menampilkan response yang lebih sederhana/pesan error yang dapat digunakan oleh client.

`public function show($id)`
    `{`
        `data = Post::find(id);`

        `if (is_null($data)) {`
            `return response()->json(`
                `'message' => 'Resource not found!'`
            `], 404);`
        `}`
ketika kita mengakses data dengan id yang tidak ada, yang tampil ialah message dengan value 'Resource not found!' dengan status code 404 Not Found
        `return response()->json(new PostResource($data), 200);`
    `}`

dapat mengkondisikan suatu fungsi dimana contohnya ketika kita tidak menemukan resource yang kita cari, kita harus memberikan response yang informatif  salah satunya dengan membuat custom response
### Menambahkan Validasi beserta Responsenya
## Api Resource
### Membuat Custom Response
### Menggunakan Resource Collection
### Membuat Response Pagination
### Memuat Data Berelasi di API Resource
## Authentication
### Menggunakan API Authentication
didalam konsep rest api atau membuat web service terdapat API Authentication yang berfungsi untuk menjaga resource kita tetap aman, kita juga dapat mengidentifikasi siapa saja yang melakukan request dan kita juga bisa menghemat resource atau menentukan scope apa saja yang dapat diakses suatu client/user

didalam laravel sudah menyediakan konsep authentication didalam web service-nya, kita akan menggunakan token base/API authentication

**step by step,**
- menambahkan kolom baru didalam table user (satu kolom), bertipe data string yang bernama api_token sudah ditentukan untuk menyimpan/acuan string unique yang akan kita gunakan (harus menggunakan kolom yang sama)
- ketika sudah mengenerate dari API token ini, kita dapat mengamankan suatu api endpoint dengan menggunakan middleware auth:api (laravel 6) !pada laravel versi selanjutnya menggunakan sanctum

laravel 6
`Route::middleware('auth:api')->get('/user', function (Request $request) {`
    `return $request->user();`
`});`

next laravel
`Route::middleware('auth:sanctum')->get('/user', function (Request $request) {`
    `return $request->user();`
`});`

memproteksi sebuah routes dengan middleware auth:api/auth:sanctum, atau middleware tersebut bisa kita tambahkan kedalam sebuah controller yang ingin dikenakan securitynya

teknis,
- membuat column baru api_token kedalam table users
`php artisan make:migration add_api_token_column_to_users_table`
*membuat kelas dengan skema yang sudah ditentukan untuk membuat kolom baru didalam migrationnya, ini dilakukan oleh laravel pada saat mengecek terdapat nama dengan format api_token*

copy paste sesuai database preparation pada laravel 
`public function up()`
    `{`
        `Schema::table('users', function (Blueprint $table) {`
            `$table->string('api_token', 80)->after('password')`
                        `->unique()`
                        `->nullable()`
                        `->default(null);`
        `});`
    `}`

ketika melakukan rollback kita hapus juga column api_token
`public function down()`
    `{`
        `Schema::table('users', function (Blueprint $table) {`
            `$table->dropColumn('api_token');`
        `});`
    `}`

- mengubah factory untuk mengenerate tokennya agar bisa langsung digunakan
menambah index baru pada UserFactory.php
`'api_token' => Str::random(80),`
sesuai dengan panjang yang ada pada migration
dan jika membuat halaman register tambahkan juga index array untuk api_token seperti diatas

kemudian php artisan migrate:fresh --seed

**Bagaimana cara proteksinya?**
kita inginkan pada saat mengakses kedalam api dari PostController yaitu harus user yang sudah ter-registrasi atau sudah memiliki token baru dapat mengakses api endpoint PostController

- panggil middleware kedalam __construct (method yang akan dieksesuki ketika kita memanggil kelas PostController)
`public function __construct() {`
middleware auth dengan jenis api
	`$this->middleware('auth:api');`
jadi pada saat akan memanggil api endpoint apapun pada method didalam kelas (PostController) harus menyertakan api_token
`}`

`public function __construct() {`
	`$this->middleware('auth:sanctum');`
`}`

pengetesan pada postman
url http://localhost:8000/api/post dengan http method get,
pada headers tambahkan key: Accept dengan Value: application/json kemudian send,
yang terjadi ialah error
`{`
    `"message": "Unauthenticated."`
    "Client yang melakukan request didalam api endpoint tersebut sudah mengimplementasikan middleware api auth harus menyertakan api_token"
`}`

dalam postman kita dapat memanfaatkan tab "Authorization" dan pilih type nya ialah "Bearer Token", pada kolom Token isi string/kode unique yang kita generate disimpan dikolom tersebut, untuk bisa mengakses api endpoint yang menggunakan middleware auth:api,

token didapatkan dari dengan memanfaat dari table user pada database pada kolom api_token copy dan paste pada kolom Token (Bearer Token)

kita dapat langsung cek, Send maka data akan didapatkan/diakses karena sudah menyertakan token,
hal ini dapat membantu kita pada saat melakukan create data post tanpa perlu menyertakan id dari user_id nya, karena jika kita lihat didalam database pada table posts kita perlu menyertakan kolom user_id pada saat membuat data posts,

karena sudah menggunakan token, token tersebut sudah mewakili data usernya.