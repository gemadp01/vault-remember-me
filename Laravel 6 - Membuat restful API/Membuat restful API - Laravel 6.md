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
- sudah membuat model
- design database
- sudah merealisasikannya
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

membuat kelas seed DataTableSeeder.php
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
Memb
### Browse API Endpoint
### Read API Endpoint
### Add API Endpoint
### Edit API Endpoint
### Delete API Endpoint
