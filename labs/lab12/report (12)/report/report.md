---
## Front matter
title: "Отчёт по лабораторной работе №12"
subtitle: "Настройки сети в Linux"
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

Получить навыки настройки сетевых параметров системы.

# Выполнение

## Проверка конфигурации сети

1. Сначала были получены права суперпользователя с помощью команды **su -**.  
   Далее выполнена команда **ip -s link**, которая вывела статистику работы сетевых интерфейсов.

   ![Информация об интерфейсах и статистика пакетов](Screenshot_8.png){ #fig:001 width=80% }

   Интерфейс `enp0s3` имеет состояние **UP**, что означает его активность.  
   Отображены счётчики принятых (RX) и переданных (TX) пакетов, а также ошибки и количество коллизий.  
   Адрес интерфейса — `08:00:27:d3:be:a9`.

2. Командой **ip route show** были выведены текущие маршруты.

   Основной маршрут по умолчанию направлен через шлюз **10.0.2.2** с интерфейсом **enp0s3**.  
   В таблице также присутствует маршрут подсети **10.0.2.0/24**, соответствующий локальной сети устройства.  
   Текущий IP-адрес устройства — **10.0.2.15**.

3. Для проверки подключения к интернету выполнена команда **ping -c 4 8.8.8.8**.

   ![Проверка доступности узла 8.8.8.8](Screenshot_9.png){ #fig:002 width=80% }

   Ответы от узла 8.8.8.8 подтверждают успешное сетевое соединение. Потеря пакетов отсутствует.

4. К интерфейсу **enp0s3** был добавлен дополнительный адрес командой  
   **ip addr add 10.0.0.10/24 dev enp0s3**.  
   После этого с помощью **ip addr show** проверено, что новый адрес успешно добавлен.

   ![Добавление дополнительного IP-адреса](Screenshot_10.png){ #fig:003 width=80% }

   В выводе видно, что интерфейс **enp0s3** теперь имеет два IPv4-адреса:  
   `10.0.2.15/24` (основной) и `10.0.0.10/24` (дополнительный).

5. Для сравнения вывода утилит **ip** и **ifconfig** использована команда **ifconfig**.

   ![Информация об интерфейсах с помощью ifconfig](Screenshot_11.png){ #fig:004 width=80% }

   Результат аналогичен выводу **ip addr show**, однако **ifconfig** отображает меньше подробных параметров,  
   таких как время жизни адресов и параметры маршрутизации.

6. Для просмотра всех открытых TCP и UDP портов применена команда **ss -tul**.

   ![Список прослушиваемых портов TCP и UDP](Screenshot_12.png){ #fig:005 width=80% }

   Отображены сервисы, ожидающие входящие подключения, включая `ssh`, `http`, `ipp` и `mdns`.

## Управление сетевыми подключениями с помощью nmcli

1. После получения прав суперпользователя была выполнена команда **nmcli connection show**,  
   чтобы просмотреть текущие соединения.

   ![Список соединений NetworkManager](Screenshot_13.png){ #fig:006 width=80% }

   Отображены соединения: `enp0s3`, `lo`, а также созданные позже `dhcp` и `static`.

2. Создано новое соединение с именем **dhcp** для интерфейса **enp0s3**:  
   **nmcli connection add con-name "dhcp" type ethernet ifname enp0s3**.  
   Далее добавлено статическое соединение **static** с IP-адресом **10.0.0.10/24** и шлюзом **10.0.0.1**:  
   **nmcli connection add con-name "static" ifname enp0s3 autoconnect no type ethernet ip4 10.0.0.10/24 gw4 10.0.0.1**.

3. После активации соединения **static** командой  
   **nmcli connection up static** — соединение успешно активировалось.  

   Проверка через **nmcli connection show** и **ip addr** подтвердила,  
   что интерфейсу присвоен статический адрес **10.0.0.10/24**.

   ![Активация статического соединения](Screenshot_14.png){ #fig:007 width=80% }

4. Затем выполнено переключение обратно на соединение **dhcp**:  
   **nmcli connection up dhcp**.  
   После переключения интерфейс снова получил динамический адрес **10.0.2.15/24**.

   ![Переключение на соединение DHCP](Screenshot_15.png){ #fig:008 width=80% }

   Оба соединения функционируют корректно, что подтверждает успешную работу конфигурации сети.

## Изменение параметров соединения с помощью nmcli

1. Отключено автоподключение статического соединения с помощью команды  
   **nmcli connection modify "static" connection.autoconnect no**.

2. Добавлен первый DNS-сервер к соединению **static**:  
   **nmcli connection modify "static" ipv4.dns 10.0.0.10**.  
   Затем добавлен второй DNS-сервер с использованием оператора `+`:  
   **nmcli connection modify "static" +ipv4.dns 8.8.8.8**.

3. Изменён основной IP-адрес для статического соединения:  
   **nmcli connection modify "static" ipv4.addresses 10.0.0.20/24**.  
   После этого добавлен дополнительный адрес:  
   **nmcli connection modify "static" +ipv4.addresses 10.20.30.40/16**.

   ![Изменение параметров соединения static через nmcli](Screenshot_16.png){ #fig:009 width=80% }

   После активации соединения командой **nmcli connection up static**  
   проверено, что изменения применены. В выводе **ip addr** отображаются оба адреса:  
   `10.0.0.20/24` и `10.20.30.40/16`.

4. Для проверки параметров соединений была использована команда **nmtui**.  
   В интерфейсе **nmtui** в профиле `static` указаны:  
   - два IP-адреса (`10.0.0.20/24`, `10.20.30.40/16`);  
   - шлюз `10.0.0.1`;  
   - DNS-серверы `10.0.0.10` и `8.8.8.8`.  

   ![Настройки соединения static в nmtui](Screenshot_17.png){ #fig:010 width=80% }

   В профиле `dhcp` указано автоматическое получение параметров IPv4 и IPv6.  

   ![Настройки соединения dhcp в nmtui](Screenshot_18.png){ #fig:011 width=80% }

   Интерфейс `enp0s3` также настроен на автоматическое подключение.  

   ![Параметры интерфейса enp0s3 в nmtui](Screenshot_19.png){ #fig:019 width=80% }

5. Проверка конфигурации через графический интерфейс системы показала аналогичные настройки.  
   Для профиля `static` активен ручной режим IPv4 (Manual), указаны два адреса и два DNS-сервера.

   ![Параметры соединения static в графическом интерфейсе](Screenshot_20.png){ #fig:020 width=80% }

   Для профиля `dhcp` выбран автоматический режим (Automatic DHCP),  
   DNS и маршруты получаются динамически от сервера.

   ![Параметры соединения dhcp в графическом интерфейсе](Screenshot_21.png){ #fig:021 width=80% }

6. Для возврата к исходному сетевому соединению выполнена команда  
   **nmcli connection up "enp0s3"**, после чего интерфейс вновь получил параметры через DHCP.  
   Проверка с помощью **nmcli connection show** и **ip addr** подтвердила успешное переключение.


# Контрольные вопросы

1. **Какая команда отображает только статус соединения, но не IP-адрес?**  
   nmcli device status  

2. **Какая служба управляет сетью в ОС типа RHEL?**  
   NetworkManager  

3. **Какой файл содержит имя узла (устройства) в ОС типа RHEL?**  
   /etc/hostname  

4. **Какая команда позволяет вам задать имя узла (устройства)?**  
   hostnamectl set-hostname <имя_узла>  

5. **Какой конфигурационный файл можно изменить для включения разрешения имён для конкретного IP-адреса?**  
   /etc/hosts  

6. **Какая команда показывает текущую конфигурацию маршрутизации?**  
   ip route show  

7. **Как проверить текущий статус службы NetworkManager?**  
   systemctl status NetworkManager  

8. **Какая команда позволяет вам изменить текущий IP-адрес и шлюз по умолчанию для вашего сетевого соединения?**  
   nmcli connection modify <имя_соединения> ipv4.addresses <IP-адрес/маска> ipv4.gateway <шлюз>

# Заключение

В ходе работы были освоены основные приёмы конфигурирования сетевых интерфейсов в операционных системах семейства RHEL.  
Были изучены команды **ip**, **ifconfig**, **ping**, **ss**, а также инструменты управления сетевыми подключениями **nmcli** и **nmtui**.  
Проведено добавление и удаление IP-адресов, настройка маршрутов, DNS-серверов и параметров шлюза.  