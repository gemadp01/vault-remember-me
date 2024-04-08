```
composer create-project laravel/laravel:^11.0 example-app
```
or
**if you already installed the global laravel,** 
```
laravel new example-app
```

The Basic - Asset Bundling (Vite)
https://laravel.com/docs/11.x/vite

copy config of @vitejs/plugin-vue to vite.config.js and before that install first the plugin-vue (on laravel web docs)
```
npm i vue
```
and 
```
npm install --save-dev @vitejs/plugin-vue

```
next config server side inertia 
composer require inertiajs/inertia-laravel

create main root template app.blade.php (on resources -> views)
```
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0" />
    @vite('resources/js/app.js')
    @inertiaHead
</head>
<body>
    @inertia
</body>
</html>
```

```
php artisan inertia:middleware
```

register the middleware on middleware group in kernel.php

next after install server side is client side
```
npm install @inertiajs/vue3
```

copy code below to resources -> js -> app.js (this code from inertia laravel web)
```
import { createApp, h } from "vue";
import { createInertiaApp } from "@inertiajs/vue3";
import { resolvePageComponent } from "laravel-vite-plugin/inertia-helpers";
createInertiaApp({
    resolve: (name) =>
        resolvePageComponent(
            ./Pages/${name}.vue,
            import.meta.glob("./Pages/**/*.vue")
        ),
    setup({ el, App, props, plugin }) {
        return createApp({ render: () => h(App, props) })
            .use(plugin)
            .mount(el);
    },
});
```

install progress loading bar (optional)
```
npm install @inertiajs/progress
```

import this code below on app.js 
```
import { InertiaProgress } from "@inertiajs/progress";

InertiaProgress.init();
```

try on web routes 
```
Route::get('/', function () {
    return Inertia::render('Users/Index', [
        "name" => "Gema"
    ]);
});
```

and create Resources -> js -> Pages (folder) -> Users (folder) -> Index.vue
```
<template>Test!</template>
<script>
export default {};
</script>
```


next "npm i && npm run dev"

