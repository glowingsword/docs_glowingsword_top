---
title: 'Не корректно отображается дата на сайтах на базе CMS SMF'
authors: 
 - glowingsword
tags:
 - CMS
 - Wordpress
date: 2020-04-18
---
# Не корректно отображается дата на сайтах на базе CMS SMF

Проблема возникает из-за не корректно указанной локали в файле index.russian.php темы оформления сайта.

К примеру, в попавшемся недавно случае необходимо было в файле
`./Themes/default/languages/index.russian.php` заменить строку вида

``` php
$txt['lang_locale'] = 'ru_RU';
```
на аналогичную с указанием используемой на сайте клиента кодировки CP1251

``` php
$txt['lang_locale'] = 'ru_RU.CP1251';
```

после чего даты на сайте стали отображаться корректно.

Решение подсмотрел на <https://www.simplemachines.ru/index.php?topic=10369.0>