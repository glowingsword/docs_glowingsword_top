---
title: 'Для сайта не работает обработчик php в режиме "как модуль Apache"'
authors: 
 - glowingsword
tags:
 - Nginx
 - Apache
 - Проблемы Apache
 - настройка Apache
date: 2020-04-21
---
# Для сайта не работает обработчик php в режиме "как модуль Apache"

Столкнулся с ситуацией, когда на сервере не работал обработчик php в режиме "как модуль Apache". 
При этом модуль php был установлен, файл libphp5.so также присутствовал на сервере и имел корректные права.
``` bash
ls -lha /etc/httpd/modules/libphp5.so
```

Файл `/etc/httpd/conf.d/php.conf` и конфиг `apache` для `WWW-домена` также
выглядели абсолютно корректно. 

В логах никаких ошибок, strace тоже не помог обнаружить проблему. 
Оказалось, что проблема возникала из-за параметра `open_basedir`
``` bash
php_admin_value open_basedir "/var/www/user/data:."
```

корректируем его в конфиге(добавляем нужные пути), или удаляем. 
Перезапускаем апачик.