Install
https://github.com/cretueusebiu/valet-windows

composer global require cretueusebiu/valet-windows
valet install

You have to run `valet uninstall` first, then composer global remove
composer global remove cretueusebiu/valet-windows

valet park

Manually Configure
https://mayakron.altervista.org/support/acrylic/Windows10Configuration.htm


!Valet will automatically start its daemon each time your machine boots. There is no need to run `valet start` or `valet install` ever again once the initial Valet installation is complete.

Uninstall
composer global remove laravel/valet


```php
valet uninstall --force
```
