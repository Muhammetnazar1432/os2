---
## Front matter
title: "Отчёт по лабораторной работе №11"
subtitle: "Управление загрузкой системы"
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

Получить навыки работы с загрузчиком системы GRUB2.

# Выполнение

1. Сначала был получен доступ с правами суперпользователя с помощью команды **su -**.  
   Затем был открыт файл конфигурации **/etc/default/grub** для редактирования в текстовом редакторе **nano**.  
   В нём изменён параметр времени отображения меню загрузчика —  
   `GRUB_TIMEOUT=10`.  

   ![Редактирование файла /etc/default/grub](Screenshot_1.png){ #fig:001 width=80% }

2. После сохранения изменений в файле были перезаписаны параметры загрузчика командой  
   **grub2-mkconfig -o /boot/grub2/grub.cfg**.  
   Затем выполнена перезагрузка системы для применения конфигурации.  
   При запуске появилось меню загрузчика **GRUB**, отображающее список доступных ядер системы Rocky Linux.  

   ![Меню загрузчика GRUB](Screenshot_2.png){ #fig:002 width=80% }

3. Для входа в режим восстановления (**rescue.target**) в меню GRUB была выбрана текущая версия ядра и нажата клавиша **e** для редактирования параметров загрузки.  
   В конце строки, начинающейся с **linux**, был добавлен параметр  
   `systemd.unit=rescue.target`, после чего загрузка продолжена комбинацией **Ctrl + X**.  

   ![Редактирование параметров загрузки (режим rescue)](Screenshot_3.png){ #fig:003 width=80% }

4. После ввода пароля пользователя **root** система была успешно загружена в однопользовательском режиме.  
   Для просмотра списка активных модулей (unit-файлов) была выполнена команда **systemctl list-units**.  
   В результате видно, что загружена минимальная системная среда, включая службы **udev**, **journal**, **swap**, **rescue.target** и другие.  

   ![Вывод команды systemctl list-units в режиме rescue](Screenshot_4.png){ #fig:004 width=80% }

5. Для проверки переменных окружения текущей оболочки использовалась команда **systemctl show-environment**.  
   Вывод показал стандартные переменные среды, включая **LANG**, **PATH** и **XDG_DATA_DIRS**.  

6. После перезагрузки системы снова было выбрано редактирование параметров загрузки, где в строку ядра был добавлен параметр  
   `systemd.unit=emergency.target`.  
   Это обеспечивает загрузку в аварийный режим (**emergency mode**).  

   ![Редактирование параметров загрузки (режим emergency)](Screenshot_5.png){ #fig:005 width=80% }

7. После входа в систему в аварийном режиме была проверена текущая конфигурация служб командой **systemctl list-units**.  
   На этот раз видно, что активировано минимальное количество модулей, включая **emergency.target**, **systemd-journald**, **system.slice** и другие.  

   ![Вывод systemctl list-units в аварийном режиме](Screenshot_6.png){ #fig:006 width=80% }

8. Для проверки сценария восстановления пароля **root** была выполнена загрузка с параметром `rd.break`.  
   Этот параметр останавливает процесс загрузки до монтирования корневой файловой системы и позволяет произвести её повторное монтирование в режиме **rw** для изменения пароля.  

   ![Загрузка с параметром rd.break](Screenshot_7.png){ #fig:007 width=80% }


# Контрольные вопросы

1. **Какой файл конфигурации следует изменить для применения общих изменений в GRUB2?**  
   /etc/default/grub  

2. **Как называется конфигурационный файл GRUB2, в котором вы применяете изменения для GRUB2?**  
   /boot/grub2/grub.cfg  

3. **После внесения изменений в конфигурацию GRUB2, какую команду вы должны выполнить, чтобы изменения сохранились и воспринялись при загрузке системы?**  
   grub2-mkconfig -o /boot/grub2/grub.cfg  

# Заключение

В ходе выполнения работы были изучены методы управления загрузчиком **GRUB2** в операционной системе Linux.  
Были рассмотрены способы изменения параметров конфигурации, включение и настройка отображения меню загрузки, а также использование различных режимов загрузки системы: **rescue.target**, **emergency.target** и загрузка с параметром **rd.break**.  
В процессе работы были освоены приёмы восстановления системы, перехода в однопользовательский и аварийный режимы, а также методика сброса пароля пользователя **root** через загрузку в минимальной среде.  
Полученные навыки позволяют эффективно администрировать процесс загрузки и устранять неполадки, связанные с инициализацией системы.  
