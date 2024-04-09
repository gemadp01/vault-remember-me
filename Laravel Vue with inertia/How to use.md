## Tips and trick
bagaimana jika v-model nya banyak?

Jika Anda memiliki banyak `v-model` yang ingin Anda gunakan dalam sebuah komponen Vue 3, Anda dapat mengelompokkannya dalam sebuah objek di dalam komponen parent dan mengirimkan objek tersebut sebagai prop ke komponen child. Di komponen child, Anda dapat mengakses nilai-nilai tersebut secara individu dan mengirimkan perubahan menggunakan event. Berikut adalah contoh bagaimana Anda dapat melakukannya:

<!-- ChildComponent.vue -->
```
<template>
  <div>
    <input :value="formData.name" @input="updateForm('name', $event.target.value)">
    <input :value="formData.email" @input="updateForm('email', $event.target.value)">
  </div>
</template>

<script>
export default {
  props: ['formData'],
  methods: {
    updateForm(key, value) {
      this.$emit('updateFormData', { ...this.formData, [key]: value });
    }
  }
};
</script>
```

<!-- ParentComponent.vue -->
```
<template>
  <div>
    <ChildComponent :formData="formData" @updateFormData="updateFormData" />
  </div>
</template>

<script>
import { ref } from 'vue';
import ChildComponent from './ChildComponent.vue';

export default {
  components: {
    ChildComponent
  },
  setup() {
    const formData = ref({
      name: '',
      email: ''
      // tambahkan properti lain sesuai kebutuhan
    });

    const updateFormData = (data) => {
      formData.value = data;
    };

    return {
      formData,
      updateFormData
    };
  }
};
</script>
```

Dengan cara ini, Anda dapat mengelola banyak nilai `v-model` dalam sebuah objek di dalam komponen parent dan mengirimkan objek tersebut sebagai prop ke komponen child. Di komponen child, Anda kemudian dapat mengakses dan memperbarui nilai-nilai tersebut secara individu dan mengirimkan perubahan kembali ke komponen parent melalui event.

## 2 ways to move your files from storage folder (private) to public (accessable) in controller
I use the update controller (cause there have delete guide)

kirimkan form input dengan tipe hidden beri value gambar pada db
`<input type="hidden" name="oldImage" :value="form.image" />`

`public function update_product(Product $product, Request $request) {`
        `$validatedData = $request->validate([`
            `'name' => 'required',`
            `'price' => 'required',`
            `'stock' => 'required',`
            `'description' => 'required',`
            `'image' => 'required|image|file|mimes:jpeg,png,jpg|max:2048',`
        `]);`

cek apakah ada request file image dan cek apakah ada request oldImage
jika ada request oldImage hapus value gambar pada storage
jika tidak ada, langsung store/buat folder 'products' dan masukkan $request->file('image') kedalam folder tersebut
        `if($request->file('image')) {`
            `if($request->oldImage) {`
                `Storage::delete($request->oldImage);`
            `}`
            `$validatedData['image'] = $request->file('image')->store('products');`
kemudian 
            `Storage::disk('local')->put('public/' . $validatedData['image'], file_get_contents($request->file('image')));`
        `}`
        `$product->update();`
        `return inertia('Products/Show', $product);`
    `}`

keduanya harus di php artisan storage:link
1. cara pertama

$validatedData['image'] = $request->file('image')->store('products');

nama dari file image otomatis acak

filesystems.php, nilai default local jika pada .env tidak ada variable dibawah
syarat ialah FILESYSTEM_DISK=local -> ubah value menjadi public

3. cara kedua (tanpa mengubah value dari filesystem_disk )
`$file = $request->file('image');`

`$fileNameWithoutSpacing = str_replace(' ', '', $request->name);`
        
`$validatedData['image'] = time() . '_' . $fileNameWithoutSpacing . '.' . $file->getClientOriginalExtension();`
        
`Storage::disk('local')->put('public/products/' . $validatedData['image'], file_get_contents($file));`