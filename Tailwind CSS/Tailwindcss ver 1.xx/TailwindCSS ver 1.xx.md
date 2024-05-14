## Concept (Utility First)
Tailwind berbeda dengan framework css pada umumnya.
"A utility-first CSS framework for ==rapidly building custom designs=="

Tailwind sekumpulan kelas kecil/utility class.
untuk membuat sebuah component, kita dapat menggabungkan utility class yang nantinya akan tercipta visual yang kita inginkan.

```html
<div class="md:flex">
  <div class="md:flex-shrink-0">
    <img class="rounded-lg md:w-56" src="https://images.unsplash.com/photo-1556740738-b6a63e27c4df?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=448&q=80" alt="Woman paying for a purchase">
  </div>
  <div class="mt-4 md:mt-0 md:ml-6">
    <div class="uppercase tracking-wide text-sm text-indigo-600 font-bold">Marketing</div>
    <a href="#" class="block mt-1 text-lg leading-tight font-semibold text-gray-900 hover:underline">Finding customers for your new business</a>
    <p class="mt-2 text-gray-600">Getting a new business off the ground is a lot of hard work. Here are five ideas you can use to find your first customers.</p>
  </div>
</div>
```

beda halnya dengan bootstrap (walaupun dibootstrap juga dapat menggabungkan beberapa kelas) tapi class card sudah mewakili component dari card itu sendiri.
dan juga bootstrap main concept yang berbeda ialah **UI Kit Framework** (lebih fokus ke UI nya, components) dan tidak banyak orang tau kita juga dapat melakukan apa yang dilakukan tailwind tapi kembali lagi ke main concept awal.

Tailwind css tidak menyediakan components bawaan, kita harus rangkai class dan terbuat sebuah component.

Keuntungan tailwindcss, lebih bervariasi.
## Installations (Node -> npm (include))
install node yang mana didalamnya sudah otomatis include npm
https://nodejs.org/en/download

init project npm nya,
```
npm init -y
```
- untuk menulis package.json yang dimana untuk mencatat semua dependencies yang kita install, salah satunya tailwindcss.
- -y yang berarti flag yang memberitahu `npm init` untuk menerima semua nilai default yang biasanya ditanyakan selama proses inisialisasi. Nilai-nilai default ini termasuk nama proyek, versi, deskripsi, titik masuk (entry point), skrip pengujian, repositori git, kata kunci, penulis, dan lisensi. Semua nilai ini diambil dari konfigurasi default npm atau dari file `.npmrc` jika ada.
- tanpa -y kita akan ditanya satu persatu

Using npm (install tailwindcss)
```
npm install tailwindcss@^1
```
menginstall packages yang diperlukan untuk menjalankan tailwindcss.
untuk mengecek -> ls node_modules -> akan tersedia dependencies/packages yang diperlukan.

!.html sudah diserve oleh browser, tidak memerlukan programming language maupun cgi


struktur html -> import tailwindcss nya (bisa dengan CDN maupun cli nya) ->

kekurangan CDN,
Before using the CDN build, please note that many of the features that make Tailwind CSS great are not available without incorporating Tailwind into your build process.

- You can't customize Tailwind's default theme
- You can't use any [directives](https://v1.tailwindcss.com/docs/functions-and-directives) like `@apply`, `@variants`, etc.
- You can't enable features like [`group-hover`](https://v1.tailwindcss.com/docs/pseudo-class-variants#group-hover)
- You can't install third-party plugins
- You can't tree-shake unused styles

```
<link href="https://unpkg.com/tailwindcss@^1.0/dist/tailwind.min.css" rel="stylesheet">
```

Membuat file tailwind.css
- jika menggunakan CDN kita tidak perlu menambahkan file tailwind.css yang didalamnya terdapat directives tailwindnya
```
@tailwind base;
@tailwind components; 
@tailwind utilities;
```
kita tidak perlu membuat file tailwind, karena tailwind sudah bisa men-generate file yang didalamnya sudah banyak class didalamnya.

{name}.css berisi @tailwind base; @tailwind components; @tailwind utilities;
file tailwind.css -> berisi directives -> file tersebut dicompile (sama halnya cara kerja sass, less, stylus, postcss karena tailwind menggunakan postcss untuk memproses kode css nya)
- 
## Costumization
## Tree Shake atau Purging
Tree shaking is ==a term commonly used within a JavaScript context to describe the removal of dead code==.

"Menghapus sisa kelas yang tidak digunakan agar lebih ringan dan mengurangi size pada saat production"