https://inertiajs.com/
https://www.youtube.com/playlist?list=PLdXLsjL7A9k3HRt6baScpwggCgQYTRZrx

pada controller kita bisa render dengan facades bawaan inertia ataupun helpernya

return Inertia::render('Users/Post', [data => 'value'])
return inertia('Users/Post', compact('data')))

#### Links and Routing
```
<Link :href="/route"/>
```
or use ziggy library
```
<link :href="route('your.route.name')"/>
```

#### Progress Indicator (latest inertia)
`createInertiaApp({`
add progress property
`progress: {`
        `color: "#dc2626",`
    `},`
`});`

#### Main menu and Layout
Resources -> js -> Layouts -> AppLayout.vue (main layout)

I'll use the main layout to the index of product view
<Head title="Product List" />

  

```
    <AppLayout>
        <Link :href="route('create_product')">Add new Product</Link>
    </AppLayout>
```
    
```
<script>
import { Head, Link } from "@inertiajs/vue3";
import AppLayout from "../../Layouts/App.vue";
export default {

    components: {
        Head,
        Link,
        AppLayout,
    },
};
</script>
```

#### Head and Title components inertia/vue3
```
<Head> 
	<title>test</title>
</Head>
```
or, shortly
```
<Head title="Your title here"/>
```

or if you have global variable, you can add that to the app.js
```
title: (title) => `${title} - Yourtitleglobal`,
```

#### Multiple Layouts
Login and Register to Layouts -> Auth.vue

#### Form Submit (with useForm helper)
`public function store_product(StoreProductRequest $request) {`
        `$validatedData = $request->validated();`

        `$file = $request->file('image');`

        `$fileNameWithoutSpacing = str_replace(' ', '', $request->name);`

        `$validatedData['image'] = time() . '_' . $fileNameWithoutSpacing . '.' . $file->getClientOriginalExtension();`

        `Storage::disk('local')->put('public/products/' . $validatedData['image'], file_get_contents($file));`

        `Product::create($validatedData);`
        
        `return redirect()->route('index_product')->with("message", "Product created Successfully");`
    `}`

```
<form @submit.prevent="form.post(route('store_product'))">
                    <label for="name">Nama</label>
                    <input
                        type="text"
                        name="name"
                        id="name"
                        v-model="form.name"
                    />
                    <label for="description">Description</label>
                    <input
                        type="text"
                        name="description"
                        id="description"
                        v-model="form.description"
                    />
                    <label for="stock">Stock</label>
                    <input
                        type="number"
                        name="stock"
                        id="stock"
                        v-model="form.stock"
                    />
                    <label for="price">Price</label>
                    <input
                        type="number"
                        name="price"
                        id="price"
                        v-model="form.price"
                    />
                    <label for="image">Image</label>
                    <input
                        type="file"
                        @input="form.image = $event.target.files[0]"
                    />
                    <progress
                        v-if="form.progress"
                        :value="form.progress.percentage"
                        max="100"
                    >
                        {{ form.progress.percentage }}%
                    </progress>
                    <button type="submit">Submit</button>
                </form>
```

@submit.prevent="form.post(route('store_product'))"
v-model="form.stock" -> pengganti value dan name
@input="form.image = $event.target.files[0]" -> jika form input file

```
<script>
export default {
setup() {
        const form = useForm({
            name: "",
            stock: "",
            price: "",
            description: "",
            image: "",
        });

        return { form };
    },
}
</script>
```

value dari v-model disesuaikan dengan variable form didalam script diatas
#### Form Validation
before 
`$validatedData = $request->validate([`
            `'name' => 'required',`
            `'stock' => 'required',`
            `'price' => 'required',`
            `'description' => 'required',`
            `'image' => 'required|image|file|mimes:jpeg,png,jpg|max:2048',`
        `]);`

after
split the validation into validation class
php artisan make:request Store"yourcontrollername"Request

php artisan make:request StoreProductRequest

set authorize return to true;

`public function authorize(): bool`
    `{`
        `return true;`
    `}`

```
and provide the validation into rules() method
`public function rules(): array`
    `{`
        `return [`
            `'name' => 'required',`
            `'stock' => 'required',`
            `'price' => 'required',`
            `'description' => 'required',`
            `'image' => 'required|image|file|mimes:jpeg,png,jpg|max:2048',`
        `];`
    `}`

`public function store_product(StoreProductRequest $request) {`
        `Product::create($request->validated());`
        `return redirect()->back();`
    `}`

if you want have validation error message, try this below
only write the props of
props: {
	errors: object;
}

```
<template>
	<div v-if="errors.description" class="text-red-600">
                                {{ errors.description }}
        </div>
</template>


#### Flash Messages
in your controller after create or update data, add with('your-var', "your-message");

`return redirect()->route('index_product')->with("message", "Product Created Successfully");`


!we cannot using session() method from laravel (cause we write on vue)

to catch message from controller we go to middleware HandleInertiaRequests,php

share any globals variabel from laravel to vue.js ke inersia

`'flash' => [`
                `'message' => $request->session()->get('message')`
`]`


shortly
`'flash' => [`
                `'message' => session('message')`
`]`


and will available as page property flash.message
$page -> global
props -> global
{{ $page.props.flash.message }}

#### Delete Record
`public function delete_product(Product $product) {`
        `if($product->image) {`
            `Storage::disk('local')->delete('public/products/' . $product->image);`
        `}`
        `$product->delete();`
        `return redirect()->route('index_product')->with("message", "Product deleted successfully");`
    `}`

we'll use the manual configuration (without helper inertia) cause so many data on the page we must delete one by one with event @click


`<button`
                    `type="button"`
                    `@click="destroy(product.id)"`
                    `class="py-2 px-4 bg-red-600 hover:bg-red-700 focus:ring-red-500 focus:ring-offset-indigo-200 text-white w-full transition ease-in duration-200 text-center text-base font-semibold shadow-md focus:outline-none focus:ring-2 focus:ring-offset-2 rounded-lg"`
                `>`
                    `Delete`
                    
`</button>`

!note
`import { Head, Link } from "@inertiajs/vue3";` -> adaptor of vuejs
`import { Inertia } from "@inertiajs/inertia";` -> not a vuejs spesific but inertia global

`setup() {`
        `const destroy = (id) => {`
            `if (confirm("Are you sure?")) {`
                `Inertia.delete(route("delete_product", id));`
            `}`
        `};`
        
        `return {`
            `destroy,`
        `};`
    `},`


or if the guide above not works, you can try this below 

`<Link`

                                `:href="route('delete_product', product.id)"`
                                `method="DELETE"`
                                `class="py-2 px-4 bg-red-600 hover:bg-red-700 focus:ring-red-500 focus:ring-offset-indigo-200 text-white w-full transition ease-in duration-200 text-center text-base font-semibold shadow-md focus:outline-none focus:ring-2 focus:ring-offset-2 rounded-lg">`
         `Delete`
`</Link>`
#### Edit Form
