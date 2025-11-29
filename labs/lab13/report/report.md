---
## Front matter
title: "Отчёт по лабораторной работе №13"
subtitle: "Фильтр пакетов"
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

Получить навыки настройки пакетного фильтра в Linux.

# Выполнение

## Управление брандмауэром с помощью firewall-cmd

1. Сначала были получены права суперпользователя командой **su -**.

2. Для определения зоны брандмауэра, используемой по умолчанию, была выполнена команда `firewall-cmd --get-default-zone`.  
   В результате отображена зона **public**.

   ![Определение зоны по умолчанию](Screenshot_1.png){ #fig:fw1 width=80% }

3. Затем был получен список доступных зон (`firewall-cmd --get-zones`).  
   Брандмауэр поддерживает несколько зон, среди которых *public, home, work, trusted* и другие.

4. Для просмотра всех доступных системных служб использовалась команда `firewall-cmd --get-services`.  
   На экране был выведен длинный перечень известных сервисов.

   ![Список доступных служб](Screenshot_1.png){ #fig:fw2 width=80% }

5. Далее был определён список активных служб, разрешённых в текущей зоне (`firewall-cmd --list-services`).  
   Отображены службы:
   - cockpit
   - dhcpv6-client
   - ssh

6. Для сравнения выводов была просмотрена конфигурация текущей зоны:
   - `firewall-cmd --list-all`
   - `firewall-cmd --list-all --zone=public`

   Оба вывода совпали, так как текущей зоной стоит **public**.

   ![Сравнение конфигурации зоны](Screenshot_2.png){ #fig:fw3 width=80% }

7. В конфигурацию была добавлена служба **vnc-server** (временная конфигурация времени выполнения).

8. Повторный вывод конфигурации (`firewall-cmd --list-all`) показал, что vnc-server добавлен.

   ![Добавление vnc-server во время выполнения](Screenshot_3.png){ #fig:fw4 width=80% }

9. После перезапуска службы firewalld (`systemctl restart firewalld`) служба исчезла из вывода.

10. Это произошло потому, что добавление было выполнено *во время выполнения* (runtime), а не записано в постоянную конфигурацию на диск. После перезапуска runtime-настройки сбрасываются.

11. Затем vnc-server был добавлен как постоянная конфигурация (`--permanent`).

12. Повторный вывод конфигурации пока не показал изменений — постоянные настройки не применяются автоматически.

13. После перезагрузки конфигурации (`firewall-cmd --reload`) vnc-server появился в списке активных.

   ![Постоянное добавление vnc-server](Screenshot_4.png){ #fig:fw5 width=80% }

### Добавление порта 2022 (TCP)

1. В конфигурацию был добавлен порт **2022/tcp** как постоянный.

2. После перезагрузки конфигурации (`firewall-cmd --reload`) порт появился в списке.

   ![Порт 2022 добавлен](Screenshot_5.png){ #fig:fw6 width=80% }

## Управление брандмауэром с помощью firewall-config (GUI)

1. Была запущена утилита **firewall-config**.

2. В меню *Configuration* выбрано значение **Permanent**, чтобы изменения сохранялись на диск.

3. В зоне **public** были включены службы:
   - http
   - https
   - ftp

   ![Добавление сервисов GUI](Screenshot_6.png){ #fig:fw7 width=80% }

4. На вкладке **Ports** был добавлен порт **2022/udp**.

   ![Добавление порта GUI](Screenshot_7.png){ #fig:fw8 width=80% }

5. После закрытия интерфейса и проверки в терминале изменения ещё не были применены — они записаны как постоянные.

6. После выполнения `firewall-cmd --reload` изменения вступили в силу.

   ![Изменения применены](Screenshot_8.png){ #fig:fw9 width=80% }

## Самостоятельная работа

1. Служба **telnet** была добавлена в постоянную конфигурацию через командную строку.
2. Службы **imap**, **pop3**, **smtp** были разрешены через GUI firewall-config.
3. После перезагрузки конфигурации firewall все службы присутствуют в списке.

   ![Итоговая конфигурация](Screenshot_9.png){ #fig:fw10 width=80% }

# Контрольные вопросы

1. **Какая служба должна быть запущена перед началом работы с менеджером конфигурации брандмауэра firewall-config?**  
   firewalld.service

2. **Какая команда позволяет добавить UDP-порт 2355 в конфигурацию брандмауэра в зоне по умолчанию?**  
   firewall-cmd --add-port=2355/udp --permanent

3. **Какая команда позволяет показать всю конфигурацию брандмауэра во всех зонах?**  
   firewall-cmd --list-all-zones

4. **Какая команда позволяет удалить службу vnc-server из текущей конфигурации брандмауэра?**  
   firewall-cmd --remove-service=vnc-server

5. **Какая команда firewall-cmd позволяет активировать новую конфигурацию, добавленную опцией --permanent?**  
   firewall-cmd --reload

6. **Какой параметр firewall-cmd позволяет проверить, что новая конфигурация была добавлена в текущую зону и теперь активна?**  
   firewall-cmd --list-all

7. **Какая команда позволяет добавить интерфейс eno1 в зону public?**  
   firewall-cmd --zone=public --change-interface=eno1

8. **Если добавить новый интерфейс в конфигурацию брандмауэра, пока не указана зона, в какую зону он будет добавлен?**  
   В зону по умолчанию (обычно — public).

# Заключение

В выполненной работе были освоены основные методы администрирования брандмауэра в Linux с использованием системы **firewalld** и инструмента **firewall-cmd**, а также графического интерфейса **firewall-config**.  
Были изучены зоны безопасности, просмотр доступных служб и портов, добавление и удаление сервисов, отличие временной конфигурации (runtime) от постоянной (permanent), а также применение изменений через перезагрузку конфигурации.  
Работа позволила на практике понять, как управлять сетевым доступом, открывать и блокировать порты, назначать службы и интерфейсы сетевым зонам, что является важной частью обеспечения безопасности операционной системы.
