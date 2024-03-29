---
title: "Проблема с отправкой сообщений на mail.ru из-за DMARC"
authors: 
 - glowingsword
tags:
 - DMARC
 - Ошибки отправки сообщений на почтовый ящик
date: 2020-04-05
---
# Проблема с отправкой сообщений на mail.ru из-за DMARC

Данная проблема возникает из-за того, что в качестве адреса отправителя для сайтов указан почтовый адрес, обслуживаемый почтовой службой mail.ru. 

С того времени, как почтовая службу mail.ru включила строгую проверку DMARC(в целях защиты получателей сообщений от получения спама с поддельным адресом отправителя), у владельцев сайтов, указавших в настройках php.ini что-то вроде

```bash
sendmail_path= "-t -i -f moy_login@mail.ru")  
```
и/или указавших такой адрес в настройках отправки сообщений в административной части своего сайта, сталкиваются с тем, что их сообщения не доходят до получателей, почтовые ящики которых также расположены на почтовой службе mail.ru. Так как SPF и DMARC-политики почтового сервиса mail.ru не разрешают отправлять сообщения со сторонних серверов от имени отправителей, распложенных на почтовом сервисе mail.ru, и тщательно проверяют полученные сообщение на нарушение политики DMARC, такие сообщения не отправляются успешно.  

В итоге, в случае, если в качестве адреса отправителя указан почтовый адрес, обслуживаемый mail.ru, но при этом сообщение отправлено не с почтовых серверов mail.ru, принимающий сообщения почтовый сервер mail.ru отклоняет полученное сообщение, а отправителю сообщения приходит сообщение-отбойник о невозможности доставить отправленное им сообщение. 

!!! info "На заметку..."
    На самом деле DMARC проверяют и некоторые другие почтовые службы, так что подобноая политика нулевой терпимости по отношению к нарушителям DMARC не является эксклюзивной "фишкой" Mail.ru. Просто в рунете из широко известных почтовых служб они первые внедрили данную политику в такой форме, но нужно всегда помнить о том, что и имея дело с другими почтовыми службами тоже можно столкнуться с DMARC, и споследствиями его нарушения.

Узнать больше о данной технологии вы можете на странице 

[`https://help.mail.ru/mail-help/postmaster/dmarc`](https://help.mail.ru/mail-help/postmaster/dmarc)

Для решения данной проблемы необходимо указать в настройках скриптов сайта, а также в файле php.ini php-обработчика, выполняющего скрипты сайта адрес отправителя,
обслуживаемого любой другой почтовой службой, не использующей DMARC для отбрасывания сообщений, отправленных с серверов отличных от почтовых серверов сервиса, предоставляющего данную услугу электронной почты.

Также, если настройки скрипта позволяют задействовать отправку по протоколу SMTP вместо использования функции mail(), для решения данной проблемы можно настроить отправку сообщений с сайта по протоколу SMTP с
использованием почтового сервера mail.ru, в таком случае сообщение будет отправлено с SMTP-сервера Mail.ru, а не с сервера, где расположен сайт, а следовательно и нарушения DMARC не возникнет, и письмо будет успешно доствлено до получателей, почтовые ящики которых также расположены на почтовом сервисе Mail.ru.
