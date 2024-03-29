---
title: "MySQL не понятно откуда берёт имя при коннекте к БД"
authors: 
 - glowingsword
tags:
 - MySQL
 - Ошибки MySQL
date: 2022-07-29
---

# MySQL не понятно откуда берёт имя существующего в системе пользователя при коннекте к БД

Первый раз этот баг заметил в скрипте PHP, где в одном из мест разработчик забыл указать реквизиты подключения к БД при mysql_connect.
В итоге скрипт, запущенный в консоли, возвращал 

```bash
ERROR 1045 (28000): Access denied for user 'someuser'@'localhost' (using password: NO)
```
где вместо someuser было имя реально существующего в системе пользователя, но другого, не того, от имени которого запускались скрипты при стрейсе и изучении проблемы.

Оказалось, что проблема легко воспроивзодится и без скрипта PHP:
1. логинимся под юзером, состоящим в группе sudo/wheel, к примеру в учётку admin;
2. с помощью ```su -``` или ```sudo -i``` получаем рута;
3. используя ```sudo -iu unprivileged_user1``` логинимся под другой, не привилегированной учёткой, от которой обычно выполняется интересующий нас скрипт;
4. командой ```mysql``` подключаемся к MySQL без указания пользователя, от имени которого происходит подключение;
5. получаем ошибку вида ```ERROR 1045 (28000): Access denied for user 'admin'@'localhost' (using password: NO)``` и недоумеваем, откуда в тексте ошибки пользователь admin, когда должен быть unprivileged_user1 ?!

Как видно, mysql при отсутствии указанного в параметрах подключения пользователя пытается осуществить подключение не от имени real uid пользователя, запускавщего mysql, а от имени effecive uid, что в итоге приводит к описанному выше появлению неожиданного имени пользователя в ошибке подключения.

Судя по тому, как проявляется данный баг, это баг из тикета https://bugs.mysql.com/bug.php?id=64522 
И он до сих пор проявляется на актуальных на лето 2022 года версиях MySQL.







