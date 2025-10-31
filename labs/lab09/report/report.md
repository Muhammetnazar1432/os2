---
## Front matter
title: "Отчёт по лабораторной работе №9"
subtitle: "Управление SELinux"
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

Получить навыки работы с контекстом безопасности и политиками SELinux.

# Выполнение

## Управление режимами SELinux

1. Сначала был выполнен переход в режим суперпользователя с помощью команды **su -**.  
   Далее просмотрено текущее состояние SELinux командой **sestatus -v**, которая показывает параметры политики и контексты безопасности.  

   ![Вывод команды sestatus -v](Screenshot_1.png){ #fig:001 width=80% }

   В результате видно:  
   - **SELinux status: enabled** — механизм безопасности включён;  
   - **Current mode: enforcing** — активен режим принудительного контроля;  
   - **Loaded policy name: targeted** — применяется политика *targeted*, защищающая основные службы;  
   - **Policy MLS status: enabled** — многоуровневая защита включена;  
   - ниже отображаются контексты процессов и файлов, например, *system_u:object_r:passwd_file_t:s0* для */etc/passwd*.

2. Для определения текущего режима SELinux использовалась команда **getenforce**.  
   По умолчанию система находилась в состоянии **Enforcing** — политика безопасности применялась ко всем процессам.  
   Затем командой **setenforce 0** режим был временно изменён на **Permissive**, при котором нарушения фиксируются, но не блокируются.  
   Повторная проверка через **getenforce** подтвердила изменение.  

   ![Переключение режима SELinux на Permissive](Screenshot_2.png){ #fig:002 width=80% }

3. Далее был открыт файл */etc/sysconfig/selinux* с помощью текстового редактора **nano**.  
   В параметре **SELINUX** установлено значение *disabled*, что полностью отключает механизм SELinux после перезагрузки.  

   ![Изменение параметра SELINUX=disabled](Screenshot_3.png){ #fig:003 width=80% }

4. После перезагрузки выполнена проверка текущего состояния.  
   **getenforce** показал значение **Disabled**, что подтверждает отключение SELinux.  
   Попытка активировать его командой **setenforce 1** завершилась сообщением *SELinux is disabled*, так как смена режима невозможна без перезапуска системы.  

   ![SELinux отключён — попытка включения невозможна](Screenshot_4.png){ #fig:004 width=80% }

5. Для повторного включения защиты в том же конфигурационном файле установлено значение *SELINUX=enforcing*.  
   Поле **SELINUXTYPE** оставлено как *targeted*.  

   ![Включение режима enforcing в конфигурационном файле](Screenshot_5.png){ #fig:005 width=80% }

6. При следующей загрузке системы появилось предупреждение о необходимости восстановления меток SELinux (relabeling).  
   Процесс выполнялся автоматически и мог занять продолжительное время в зависимости от объёма файловой системы.  

   ![Автоматическое восстановление меток SELinux при загрузке](Screenshot_6.png){ #fig:006 width=80% }

7. После загрузки команда **sestatus -v** вновь показала, что SELinux включён и работает в режиме **enforcing**, а политика — *targeted*.  

   ![SELinux снова включён, активен режим enforcing](Screenshot_7.png){ #fig:007 width=80% }

## Использование restorecon для восстановления контекста безопасности

1. Проверен текущий контекст безопасности файла */etc/hosts* командой **ls -Z /etc/hosts**.  
   Тип контекста — *net_conf_t*.  

2. Файл был скопирован в домашний каталог с помощью команды **cp /etc/hosts ~/**.  
   После этого контекст нового файла *~/hosts* изменился на *admin_home_t*, что характерно для пользовательских файлов.  

3. Файл из домашнего каталога был перемещён обратно в */etc*, после чего контекст остался *admin_home_t*, что не соответствует системным требованиям.  

4. Для восстановления корректного контекста безопасности использовалась команда **restorecon -v /etc/hosts**.  
   Утилита изменила метку на *net_conf_t*, что подтверждено повторной проверкой **ls -Z /etc/hosts**.  

   ![Использование restorecon для восстановления контекста файла hosts](Screenshot_8.png){ #fig:008 width=80% }

5. Для массового восстановления контекстов на всей файловой системе создан файл */.autorelabel* с помощью команды **touch /.autorelabel**.  
   После перезагрузки система автоматически перемаркировала все файлы, что сопровождалось сообщениями о выполнении relabel.  

   ![Автоматическое восстановление контекстов SELinux после создания /.autorelabel](Screenshot_9.png){ #fig:009 width=80% }

# Выполнение

## Настройка контекста безопасности для нестандартного расположения файлов веб-сервера

1. После получения полномочий администратора было установлено необходимое программное обеспечение для веб-сервера и текстового браузера: **httpd** и **lynx**.  
   Затем создан новый каталог для хранения веб-контента — */web*, в котором размещён файл *index.html* с тестовой строкой *Welcome to my web server*.  

   ![Создание каталога /web и файла index.html](Screenshot_11.png){ #fig:010 width=80% }

2. В конфигурационном файле */etc/httpd/conf/httpd.conf* была закомментирована стандартная строка  
   *DocumentRoot "/var/www/html"* и добавлена новая — *DocumentRoot "/web"*.  
   Также внесён соответствующий раздел **Directory** для нового каталога, разрешающий доступ к файлам.  

   ![Изменение параметров DocumentRoot и Directory в httpd.conf](Screenshot_10.png){ #fig:011 width=80% }

3. После внесения изменений запущена служба **httpd** и настроен её автоматический запуск при старте системы.  

   ![Запуск службы httpd и её автозагрузка](Screenshot_11.png){ #fig:012 width=80% }

4. При обращении к локальному веб-серверу через текстовый браузер **lynx** отобразилась стандартная страница Rocky Linux, что говорит о том, что SELinux не разрешил использовать новый каталог */web*.  

   ![Отображение стандартной страницы Rocky Linux](Screenshot_12.png){ #fig:013 width=80% }

5. Для решения проблемы был назначен корректный контекст безопасности для каталога */web* с помощью команды  
   **semanage fcontext -a -t httpd_sys_content_t "/web(/.*)?"**, а затем выполнено восстановление контекста **restorecon -R -v /web**.  
   В результате файлам и каталогу были присвоены метки безопасности, разрешающие доступ службе **httpd**.  

   ![Присвоение контекста httpd_sys_content_t каталогу /web](Screenshot_13.png){ #fig:014 width=80% }

6. Повторное обращение к серверу через **lynx http://localhost** показало корректную загрузку пользовательской страницы с текстом *Welcome to my web server*.  
   Это подтверждает, что настройка контекста SELinux для каталога /web выполнена успешно.  

   ![Отображение пользовательской страницы веб-сервера](Screenshot_14.png){ #fig:015 width=80% }

## Работа с переключателями SELinux

1. Был выполнен просмотр всех переключателей SELinux, связанных с FTP-сервисом, командой **getsebool -a | grep ftp**.  
   Из вывода видно, что параметр **ftpd_anon_write** по умолчанию имеет состояние *off*.  

2. Далее получен список переключателей для службы **ftpd_anon** с пояснениями с помощью команды **semanage boolean -l | grep ftpd_anon**.  
   Параметр **ftpd_anon_write** отвечает за разрешение анонимной записи в FTP.  

3. Переключатель **ftpd_anon_write** был временно активирован командой **setsebool ftpd_anon_write on**, после чего проверено его состояние — значение изменилось на *on*.  

4. Для сохранения параметра между перезагрузками он был включён постоянно с помощью команды **setsebool -P ftpd_anon_write on**.  
   Повторная проверка через **semanage boolean -l | grep ftpd_anon** показала, что оба состояния (*runtime* и *persistent*) установлены в *on*.  

   ![Просмотр и изменение состояния переключателя ftpd_anon_write](Screenshot_15.png){ #fig:016 width=80% }

# Контрольные вопросы

1. **Вы хотите временно поставить SELinux в разрешающем режиме. Какую команду вы используете?**  
   setenforce 0  

2. **Вам нужен список всех доступных переключателей SELinux. Какую команду вы используете?**  
   getsebool -a  

3. **Каково имя пакета, который требуется установить для получения легко читаемых сообщений журнала SELinux в журнале аудита?**  
   setroubleshoot  

4. **Какие команды вам нужно выполнить, чтобы применить тип контекста httpd_sys_content_t к каталогу /web?**  
   semanage fcontext -a -t httpd_sys_content_t "/web(/.*)?"  
   restorecon -R -v /web  

5. **Какой файл вам нужно изменить, если вы хотите полностью отключить SELinux?**  
   /etc/sysconfig/selinux  

6. **Где SELinux регистрирует все свои сообщения?**  
   /var/log/audit/audit.log  

7. **Вы не знаете, какие типы контекстов доступны для службы ftp. Какая команда позволяет получить более конкретную информацию?**  
   semanage fcontext -l | grep ftp  

8. **Ваш сервис работает не так, как ожидалось, и вы хотите узнать, связано ли это с SELinux или чем-то ещё. Какой самый простой способ узнать?**  
   setenforce 0  
   (Временное переключение в разрешающий режим для проверки влияния SELinux)

# Заключение

В ходе работы были изучены принципы управления системой безопасности **SELinux** в операционной системе Linux.  
Были рассмотрены режимы работы SELinux — **enforcing**, **permissive** и **disabled**, а также способы их временного и постоянного изменения.  
Проведена настройка контекста безопасности для нестандартного каталога веб-сервера */web*, что обеспечило корректный доступ службы **httpd** к его содержимому.  
С помощью инструментов **semanage** и **restorecon** освоены методы управления и восстановления меток безопасности.  
Также изучена работа с переключателями SELinux (**booleans**) на примере параметра **ftpd_anon_write**, который был изменён и закреплён на постоянной основе.  
