
---
title: "Выводим top-20 наиболее долго отдававшихся веб-сервером страниц"
authors: 
 - glowingsword
tags:
 - Linux
 - Полезные команды
date: 2021-01-26
---

#  Выводим top-20 наиболее долго отдававшихся веб-сервером страниц

К примеру, на сервере есть сайт domain.example, страницы которого отдаются быстро, но отдельные страницы и разделы подтормаживают.

Чтобы быстро обнаружить проблемные URL, достаточно натравить на лог(если у вас ведётся общий лог сервера для разных доменов с указанием времени загрузки в формате ==$request_time-$upstream_response_time==) вот такую вот команду

```bash
grep domain.example /var/log/nginx/access.log|perl -nle 'print "$1 $2 $3" if /.*\s-\s\[.*\]\s(\S+)\s\S+\s(\S+).*"\s(\S+)/g'|sort -k 3 -V |tail -20
```
В результате мы получим набор из строк вида

```bash
domain.example /some_page 445.084-0.063
```
где:

* 445.084 – значение из переменной nginx request_time
* 0.063 – upstream_response_time


