---
## Front matter
title: "Отчёт по лабораторной работе №10"
subtitle: "Основы работы с модулями ядра операционной системы"
author: "Турсунов Мухамметназар"

## Generic otions
lang: ru-RU
toc-title: "Содержание"

## Bibliography
bibliography: bib/cite.bib
csl: pandoc/csl/gost-r-7-0-5-2008-numeric.csl

## Pdf output format
toc: true
toc-depth: 2
lof: true
lot: true
fontsize: 12pt
linestretch: 1.5
papersize: a4
documentclass: scrreprt
## I18n polyglossia
polyglossia-lang:
  name: russian
  options:
    - spelling=modern
    - babelshorthands=true
polyglossia-otherlangs:
  name: english
## I18n babel
babel-lang: russian
babel-otherlangs: english
## Fonts
mainfont: IBM Plex Serif
romanfont: IBM Plex Serif
sansfont: IBM Plex Sans
monofont: IBM Plex Mono
mathfont: STIX Two Math
mainfontoptions: Ligatures=Common,Ligatures=TeX,Scale=0.94
romanfontoptions: Ligatures=Common,Ligatures=TeX,Scale=0.94
sansfontoptions: Ligatures=Common,Ligatures=TeX,Scale=MatchLowercase,Scale=0.94
monofontoptions: Scale=MatchLowercase,Scale=0.94,FakeStretch=0.9
mathfontoptions:
## Biblatex
biblatex: true
biblio-style: "gost-numeric"
biblatexoptions:
  - parentracker=true
  - backend=biber
  - hyperref=auto
  - language=auto
  - autolang=other*
  - citestyle=gost-numeric
## Pandoc-crossref LaTeX customization
figureTitle: "Рис."
tableTitle: "Таблица"
listingTitle: "Листинг"
lofTitle: "Список иллюстраций"
lotTitle: "Список таблиц"
lolTitle: "Листинги"
## Misc options
indent: true
header-includes:
  - \usepackage{indentfirst}
  - \usepackage{float}
  - \floatplacement{figure}{H}
---

# Цель работы

Получить навыки работы с утилитами управления модулями ядра операционной системы.

# Выполнение

## Управление модулями ядра из командной строки

1. Сначала был запущен терминал и получены права суперпользователя с помощью команды **su -**.  
   Далее была выполнена команда **lspci -k**, отображающая список устройств и соответствующих им модулей ядра.  

   Среди устройств можно отметить:  
   - **VGA контроллер** — VMware SVGA II Adapter (модуль *vmwgfx*)  
   - **Ethernet контроллер** — Intel PRO/1000 MT Desktop Adapter (модуль *e1000*)  
   - **Система виртуализации** — VirtualBox Guest Service (модуль *vboxguest*)  
   - **Аудио контроллер** — Intel 82801AA AC’97 Audio Controller (модуль *snd_intel8x0*)  
   - **SATA контроллер** — Intel 82801HM/HEM (модуль *ahci*)  

   ![Список устройств и соответствующих им модулей ядра](Screenshot_1.png){ #fig:001 width=80% }

2. Для просмотра загруженных модулей ядра была выполнена команда **lsmod | sort**.  
   В результате получен отсортированный список модулей, включая драйверы *ahci*, *e1000*, *vmwgfx*, *snd_intel8x0*, *vboxguest* и другие.  

   ![Список загруженных модулей ядра](Screenshot_2.png){ #fig:002 width=80% }

3. Проверка загрузки модуля **ext4** выполнена с помощью команды **lsmod | grep ext4**.  
   После загрузки модуля через **modprobe ext4** было подтверждено его присутствие в системе.  

   ![Загрузка и просмотр модуля ext4](Screenshot_3.png){ #fig:003 width=80% }

4. Для получения подробной информации о модуле **ext4** использовалась команда **modinfo ext4**.  
   Вывод показал, что модуль распространяется под лицензией **GPL**, относится к **Fourth Extended Filesystem**, не имеет параметров и зависит от модулей *jbd2* и *mbcache*.  

5. Попытка выгрузить модуль **ext4** командой **modprobe -r ext4** показала, что иногда требуется несколько повторов команды.  
   При попытке выгрузить модуль **xfs** система выдала сообщение об ошибке, так как он используется в данный момент.  

   ![Попытка выгрузки модулей ext4 и xfs](Screenshot_4.png){ #fig:004 width=80% }

## Загрузка модулей ядра с параметрами

1. Проверка наличия модуля **bluetooth** в системе выполнена командой **lsmod | grep bluetooth**.  
   После загрузки модуля с помощью **modprobe bluetooth** в списке модулей отобразились *bluetooth* и *rfkill*.  

   ![Проверка и загрузка модуля bluetooth](Screenshot_5.png){ #fig:005 width=80% }

2. Команда **modinfo bluetooth** показала, что модуль распространяется по лицензии **GPL**, имеет версию **2.22**, а также зависит от модуля *rfkill*.  
   Среди параметров модуля доступны опции:  
   - `disable_esco` — отключение eSCO соединений  
   - `disable_ertm` — отключение режима расширенной повторной передачи  
   - `enable_ecred` — включение режима enhanced credit flow control  

3. Модуль **bluetooth** был успешно выгружен с помощью команды **modprobe -r bluetooth**, без ошибок.  

   ![Выгрузка модуля bluetooth](Screenshot_6.png){ #fig:006 width=80% }

## Обновление ядра системы

1. Для определения текущей версии ядра использовалась команда **uname -r**, показавшая версию *6.12.0-55.37.1.el10_0.x86_64*.  
   Затем команда **dnf list kernel** вывела список установленных и доступных версий ядра.  

   ![Просмотр версии и списка пакетов ядра](Screenshot_7.png){ #fig:007 width=80% }

2. Команды **dnf update kernel**, **dnf update** и **dnf upgrade --refresh** подтвердили, что система обновлена и все пакеты находятся в актуальном состоянии.  

   ![Обновление ядра и системы](Screenshot_8.png){ #fig:008 width=80% }

3. После перезагрузки системы команда **hostnamectl** показала обновлённую информацию об операционной системе **Rocky Linux 10.0**, ядре *6.12.0-55.39.1.el10_0.x86_64* и параметрах виртуальной машины VirtualBox.  

   ![Информация о системе после обновления ядра](Screenshot_9.png){ #fig:009 width=80% }

# Контрольные вопросы

1. **Какая команда показывает текущую версию ядра, которая используется на вашей системе?**  
   uname -r  

2. **Как можно посмотреть более подробную информацию о текущей версии ядра операционной системы?**  
   hostnamectl  

3. **Какая команда показывает список загруженных модулей ядра?**  
   lsmod  

4. **Какая команда позволяет вам определять параметры модуля ядра?**  
   modinfo module_name  

5. **Как выгрузить модуль ядра?**  
   modprobe -r module_name  

6. **Что вы можете сделать, если получите сообщение об ошибке при попытке выгрузить модуль ядра?**  
   Проверить, используется ли модуль в данный момент, и остановить процесс, его использующий,  
   либо выгрузить зависимые модули перед повторной попыткой.  

7. **Как определить, какие параметры модуля ядра поддерживаются?**  
   modinfo module_name — в выводе команды указаны доступные параметры (parm).  

8. **Как установить новую версию ядра?**  
   dnf update kernel  
   или  
   dnf upgrade --refresh  


# Заключение

В ходе лабораторной работы были изучены основные команды для управления модулями ядра в операционной системе **Linux**.  
Были выполнены действия по загрузке, выгрузке и просмотру параметров модулей ядра с использованием утилиты **modprobe**.  
Исследованы сведения о модулях **ext4** и **bluetooth**, включая их зависимости, лицензии и поддерживаемые параметры.  
Также было выполнено обновление ядра операционной системы с применением менеджера пакетов **dnf**, проверена версия ядра и подтверждена корректность его установки.  
В результате работы были закреплены навыки администрирования системы на уровне ядра и управления его компонентами.  
