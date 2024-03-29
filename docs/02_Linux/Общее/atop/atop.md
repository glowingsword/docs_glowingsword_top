---
title: 'Системный монитор atop'
authors: 
 - glowingsword
tags:
 - atop
date: 2021-03-22q
---

# Системный монитор atop

## Установка
В зависимости от дистрибутива ставится командой

### Centos
 
```bash
yum install atop
```

### Debian/Ubuntu

```bash
apt install atop
```

## Arch Linux

```bash
pacman -Sy atop
```

## Запуск

Запускаем в дефолтном режиме

```bash
atop
```

для того, чтобы видеть текущее состояние системы.

## Запуск с параметрами

| Команда       | Описание                                                                      | 
| ------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- | 
| atop -1       | Запускается со средними в секунду значениями                                                                                                                |
| atop -a       | Отображает только активные процессы                                                                                                                         |
| atop -c	    | Отображает строку команды с параметрами запуска для каждого процесса                                                                                        |
| atop -d       | Отображает информацию по нагрузке на дисковую подсистему для каждого процесса                                                                               | 
| atop -m       | Отображает информацию о потребляемой процессами памяти                                                                                                      |
| atop -n	    | Отображает информацию о использовании сети разными процессами                                                                                               |
| atop -s       | Отображает информацию планировщика для каждого процесса                                                                                                     | 
| atop -v       | Отображает разнообразную информацию о процессах, в том числе PPID, VPID, CTID(для OpenVZ), EUID, SUID, STDATE, STTIME, ENDATE и ENTIME для каждого процесса |
| atop -y	    | Отображает информацию об индивидуальных тредах для каждого процесса                                                                                         |


## Изменение отображаемой информации в уже запущенном atop

Если вы уже запустили atop, но хотите получить больше информации о процессах, чем отображает стандартный режим General, вы можете переключать отображение в atop с помощью горячих клавиш.

| Hotkey | Описание                                                                      | 
| ------ | --------------------------------------------------------------------------------------------------------------------------------------------------|
| 1      | Отображает средние значения в секунду вместо текущих                                                                                              |
| a      | Показывает только активные процессы                                                                                                               |
| c	     | Отображает команду с параметрами запуска для каждого процесса                                                                                     |
| d      | Данные по нагрузке на дисковую подсистему для каждого процесса                                                                                    |
| g      | Переключает отображение в режим General, что отобрбажается по умолчанию при запуске atop без параметров                                           |
| j      | Сортирует данные по процессам, группируя их по контейнерам Docker                                                                                 |
| m      | Информация о памяти, потребляемой процессами                                                                                                      |
| n	     | Информация о использовании сети для каждого процесса                                                                                              |
| p      | Сортирует данные, группируя их по наименованию программы                                                                                          |
| s      | Данные от планировщика по каждому процессу                                                                                                        | 
| u      | Сортирует данные, группируя их по имени пользователя                                                                                              | 
| v      | Разнообразная информация о процессах, в том числе PPID, VPID, CTID(для OpenVZ), EUID, SUID, STDATE, STTIME, ENDATE и ENTIME для каждого процесса  |
| y	     | Информация об индивидуальных тредах для каждого процесса                                                                                          |
| C      | Сортирует текущий список процессов по нагрузке на CPU.  Назвнаие предпоследней колонки в списке изменяется на **CPU**                             |
| M      | Сортирует текущий список процессов по размеру потребляемой резидентной памяти, название предпоследней колонки изменяется на **MEM**               |
| D      | Сортирует текущий список процессо по нагрузке на дисковую подсистему, название предпоследеней колонки изменяется на **DSK**                       |
| N      | Сортирует текущий список процессов по нагрузке на сетевую подсистему(полученным и переданным данным), предпосленяя колонка изменяется на **NET**  |
| A      | Сортирует текущий список процессов автоматически, в порядке отображения наиболее занимающих системые ресурсы процессов, предпоследний столбец отображает одно из следующих значений: **ACPU**, **AMEM**, **ADSK** или **ANET** в зависимости от того, какой ресурс в дефеците у системы в настоящий момент                    |

!!! info "Небольшой лайфхак"
    Режимы можно комбинировать! К примеру, вас интересует нагрузка на дисковую подсистему на OpenVZ-хосте со стороны процессов конкретного контейнера. Вы запускаете atop, нажимаете на d, чтобы процессы отсортировались по нагрузке на диск и отобразилась информация о нагрузке на диск для процессов, затем нажимаете на v, и получаете расширенную информацию о процессах, в том числе о CTID контейнера OpenVZ внутри которого запущены процессы, наиболее активно использующие диск. Красота!


## Parseable Output

У atop есть такая замечательная возмиожность, как **parseable output**, которую используют в случае, когда вывод atop необходимо распарсить скриптами.

К примеру, команда
``` bash
atop -PPRG -r atop_20160903 -b 14:00 -e 14:05
```
выведет информацию в формате **General** по всем процессам, выполнявшимся с 14:00 по 14:05.

Варианты отображения информации в формате **parseable output**

Первая часть вывода лога(6 полей) всегда содержит следующие поля, при любой используемой label

```yaml
label host epoch date time interval
```
где:

* label – метка, указывающая на используемый режим, к примеру PRG(1-е поле в записи);
* host  – hostname сервера, на котором писался лог(2-е поле в записи);
* epoch – Unix timestamp aka количество секунд, прошедших с 1-1-1970 до настоящий момент(3-е поле в записи);
* date  – дата(4-е поле в записи);
* time  – время(5-е поле в записи);
* interval – интервал, с которым atop пишет данные в лог(по умолчанию это 600 секунд или 1 раз в 10 минут), 6-е поле в записи.


### PRG
Отображает оставшуюся часть лога в формате **General**: 

 * PID – PID процесса, 7-е поле в записи лога
 * name – наименование процеса, указывается в круглых скобках(atop), 8-е поле в записи лога
 * state – состояние процесса, к примеру R, S или D, 9-е поле в записи лога
 * real uid – UID пользователя, от имени которого выполнялся процесс, 10-е поле в записи лога
 * real gid – GID группы, от имени которой выполнялся процесс, 11-е поле в записи лога
 * TGID – то же что и PID процесса, 12-е поле в записи лога
 * total number of threads – итоговое количество потоков процесса, 13-е поле в записи лога
 * exit code – код выхода процесса, 14-е поле в записи лога
 * start time – время запуска процесса, таймкод в формате epoch, 15-е поле в записи лога
 * full command line – команда запуска процесса с параметрами, указывается в круглых скобках, 16-е поле в записи лога
 * PPID – PPID aka Parent PID(PID родительского процесса, породившего данный процесс), 17-е поле в записи лога
 * number of threads in state 'running' (R) – количество тредов процесса в состоянии R(выполняющихся), 18-е поле в записи лога
 * number of threads in state 'interruptible sleeping' (S) – количество тредов в состоянии S(спящих тредов с возможностью прерывания), 19-е поле
 * number of threads in state 'uninterruptible sleeping' (D) – количество тредов в состоянии D(беспробудно спящих, без возможности прерывания), 20-е поле
 * effective uid – эффективный UID пользователя, от имени которого выполнялся процесс, 21-е поле
 * effective gid – эффективный GID группы, от имени которой выполнялся процесс, 22-е поле
 * saved uid – сохранённый UID пользователя, от имени которого выполнялся процесс, 23-е поле
 * saved gid – сохранённый GID группы, от имени которой выполнялся процесс, 24-е поле
 * filesystem uid – fsuid пользователя, от имени которого выполнялся процесс, 25-е поле
 * filesystem gid – fsgid группы, от имени которой выполнялся процесс, 26-е поле
 * elapsed time (hertz) – пройденое время, в Герцах, 27-е поле.

### PRC
Отображает оставшуюся часть лога в формате **CPU**:

 * PID – PID процесса, 7-е поле в записи лога
 * name – наименование процеса, указывается в круглых скобках(atop), 8-е поле в записи лога
 * state – состояние процесса, к примеру R, S или D, 9-е поле в записи лога
 * total number of clock-ticks per second for this machine – полное количество clockticks в секунду для этой машины, потреблённых процессом, 10-е поле в записи лога
 * CPU-consumption in user mode (clockticks) – потребление CPU в user mode, указанное в clockticks, 11-е поле
 * CPU-consumption in system mode (clockticks) – потребление CPU процессом в system mode, указаное в clockticks, 12-е поле
 * nice value – значение nice процесса, 13-е поле
 * priority – Linux system priority, числовое значение от 0 до 139, realtime от 0 до 99 и от 100 до 139 для пользовательских процессов, 14-е поле
 * realtime priority – Linux realtime priority, значение rtprio для процесса, 15-е поле
 * scheduling policy – политика планирования планировщика, 16-е поле
 * current CPU – текущий CPU(номер ядра) на котором выполняется процесс, 17-е поле
 * sleep average – значение sleep average, 18-е поле

### PRM
Отображает оставшуюся часть лога в формате **Memory**:

 * PID – PID процесса, 7-е поле в записи лога
 * name – наименование процеса, указывается в круглых скобках(atop), 8-е поле в записи лога
 * state – состояние процесса, к примеру R, S или D, 9-е поле в записи лога
 * page size for this machine – значение page size для процесса, в байтах, 10-е поле лога
 * virtual memory size – объём занимаемой виртуальной памяти, в килобайтах, 11-е поле лога
 * resident memory size – объём занимаемой процессом резидентной памяти, в килобайтах, 12-е поле лога
 * shared text memory size – объём разделяемой текстовой памяти, в килобайтах, 13-е поле лога
 * virtual memory growth – размер роста виртуальной памяти, в килобайтах, 14-е поле лога
 * resident memory growth – размер роста резидентной памяти, в килобайтах, 15-е поле лога
 * number of minor page faults – количество минимальных page faults( https://scoutapm.com/blog/understanding-page-faults-and-memory-swap-in-outs-when-should-you-worry), 16-е поле лога
 * number of major page faults – количество максимальных page faults процесса, 17-е поле лога

### PRD
Отображает оставшуюся часть лога в формате **Disk**:

 * PID – PID процесса, 7-е поле в записи лога
 * name – наименование процеса, указывается в круглых скобках(atop), 8-е поле в записи лога
 * state – состояние процесса, к примеру R, S или D, 9-е поле в записи лога
 * kernel-patch installed – установлен ли патч ядра, позволяющий собирать статистику по нагрузке на дисковую подсистему ('y' или 'n')
 * standard io statistics –  стандартная статистика по I/O, в формате ('y' или 'n')
 * number of reads on disk – количество чтений с диска
 * cumulative number of sectors read – кумулятивное количество прочитанных секторов
 * number of writes on disk – количество записей на диск
 * cumulative number of sectors written – куммулятивное количество записанных секторов 
 * cancelled number of written sectors 

Если патч ядра не установлен, и standard I/O statistics (>= 2.6.20) не используется, значения счётчиков I/O по процессам в записях лога не будет релевантно. 

Когда патч ядра установлен, значения счётчика 'cancelled number of written sectors' не является релеватным. Когда используется только стандартная статистика I/O, счётчики 'number of reads on disk' и 'number of writes on disk' отображают не релеватные значения.

### PRN
Отображает оставшуюся часть лога в формате **Network**:

 * PID – PID процесса, 7-е поле в записи лога
 * name – наименование процеса, указывается в круглых скобках(atop), 8-е поле в записи лога
 * state – состояние процесса, к примеру R, S или D, 9-е поле в записи лога
 * kernel-patch installed – установлен ли патч ядра('y' или 'n'), 10-е поле
 * number of TCP-packets transmitted – количество переданных TCP-пакетов, 11-е поле
 * cumulative size of TCP-packets transmitted – кумуллятивное количество переданных TCP-пакетов, 12-е поле
 * number of TCP-packets received – количество полученных пакетов TCP, 13-е поле
 * cumulative size of TCP-packets received – кумулятивное количество полученных TCP-пакетов, 14-е поле
 * number of UDP-packets transmitted – количество переданных UDP-пакетов, 15-е поле
 * cumulative size of UDP-packets transmitted – кумуллятивное количество переданных UDP-пакетов, 16-е поле
 * number of UDP-packets received – количество полученных пакетов UDP, 17-е поле
 * cumulative size of UDP-packets received – кумулятивное количество полученных UDP-пакетов, 18-е поле
 * number of raw packets transmitted – количество переданных сырых пакетов, 19-е поле
 * number of raw packets received  – количесво полученных сырых пакетов, 20-е поле

### ALL

С лейблом **ALL** отображается статистика уровня system- и process-уровня, друг-за-другом будут идти записи в форматах PRG, PRC, PRM, PRN, PRD и PRE(стата по потреблению ресурсов видюхи, нам не интересна).



## Лайфхак с перемещением по длинному логу atop

Если файл лога не ротировался несколько дней, и перемещаться в нём очень не удобно - можно перемещаться в файле с помощью команды b до конца первых суток. 
А затем попытаться двинуться дальше с помощью t, прокрутить немного, и опять с помощью b прыгнуть на несколько часов вперёд.




