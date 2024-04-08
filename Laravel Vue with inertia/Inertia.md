https://inertiajs.com/
https://www.youtube.com/playlist?list=PLdXLsjL7A9k3HRt6baScpwggCgQYTRZrx

#### Links and Routing
```
<Link :href="/route"/>
```
or use ziggy library
```
<link :href="route('your.route.name')"/>
```

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
#### Form Validation
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
```

#### Flash Messages
in your controller after create or update data, add with('your-var', "your-message");
```
return redirect()->route('index_product')->with("message", "Product Created Successfully");
```

!we cannot using session() method from laravel (cause we write on vue)

to catch message from controller we go to middleware HandleInertiaRequests,php

share any globals variabel from laravel to vue.js ke inersia
```
'flash' => [
                'message' => $request->session()->get('message')
]
```

shortly
//'message' => session('message')

and will available as page property flash.message
$page -> global
props -> global
{{ $page.props.flash.message }}
#### Delete Record
we'll use the manual configuration (without helper inertia) cause so many data on the page we must delete one by one with event @click

```
<button
                    type="button"
                    @click="destroy(product.id)"
                    class="py-2 px-4 bg-red-600 hover:bg-red-700 focus:ring-red-500 focus:ring-offset-indigo-200 text-white w-full transition ease-in duration-200 text-center text-base font-semibold shadow-md focus:outline-none focus:ring-2 focus:ring-offset-2 rounded-lg"
                >
                    Delete
</button>
```

!note
import { Head, Link } from "@inertiajs/vue3"; -> adaptor of vuejs
import { Inertia } from "@inertiajs/inertia"; -> not a vuejs spesific but inertia global

```
setup() {
        const destroy = (id) => {
            if (confirm("Are you sure?")) {
                Inertia.delete(route("delete_product", id));
            }
        };
        
        return {
            destroy,
        };
    },
```