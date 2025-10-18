---
## Front matter
lang: ru-RU
title: Лабораторная работа №7
subtitle: Управление журналами событий в системе
author:
  - Турсунов Мухамметназар
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 10 октября 2025

## i18n babel
babel-lang: russian
babel-otherlangs: english

## Formatting pdf
toc: false
slide_level: 2
aspectratio: 169
section-titles: true
theme: metropolis
header-includes:
 - \metroset{progressbar=frametitle,sectionpage=progressbar,numbering=fraction}
---

# Цель работы

## Основная цель

Получить навыки работы с системными журналами Linux, включая настройку служб **rsyslog** и **systemd-journald**, а также анализ событий при помощи утилиты **journalctl**.

# Ход выполнения работы

## Мониторинг системных событий

![Мониторинг системных событий через tail -f /var/log/messages](Screenshot_1.png){ #fig:001 width=70% }

## Ошибка авторизации su -

![Ошибка авторизации при попытке su -](Screenshot_2.png){ #fig:002 width=70% }

## Команда logger

![Результат работы команды logger hello](Screenshot_3.png){ #fig:003 width=70% }

## Просмотр журнала безопасности

![Вывод файла /var/log/secure](Screenshot_4.png){ #fig:004 width=70% }

## Настройка rsyslog для Apache

![Мониторинг журнала ошибок Apache](Screenshot_5.png){ #fig:005 width=70% }

## Перенаправление логов Apache в системный журнал

![Изменение конфигурации httpd.conf](Screenshot_6.png){ #fig:006 width=70% }

## Создание собственного файла логов Apache

![Создание правила для перенаправления логов Apache](Screenshot_7.png){ #fig:007 width=70% }

## Перезапуск служб rsyslog и httpd

![Перезапуск служб rsyslog и httpd](Screenshot_8.png){ #fig:008 width=70% }

## Запись отладочного сообщения

![Результат регистрации отладочного сообщения через logger](Screenshot_9.png){ #fig:009 width=70% }

# Работа с journalctl

## Общий просмотр журнала

![Просмотр системного журнала с момента загрузки](Screenshot_10.png){ #fig:010 width=70% }

## Просмотр журнала в реальном времени

![Просмотр журнала в реальном времени](Screenshot_11.png){ #fig:011 width=70% }

## Параметры фильтрации journalctl

![Отображение параметров фильтрации journalctl](Screenshot_12.png){ #fig:012 width=70% }

## Сообщения UID 0

![Вывод записей для UID 0](Screenshot_13.png){ #fig:013 width=70% }

## Последние строки журнала

![Отображение последних 20 строк журнала](Screenshot_14.png){ #fig:014 width=70% }

## Сообщения об ошибках

![Просмотр сообщений об ошибках](Screenshot_15.png){ #fig:015 width=70% }

## Просмотр по времени

![Просмотр журнала с фильтром по времени](Screenshot_16.png){ #fig:016 width=70% }

## Ошибки со вчерашнего дня

![Просмотр ошибок со вчерашнего дня](Screenshot_17.png){ #fig:017 width=70% }

## Подробный вывод (verbose)

![Режим подробного вывода verbose](Screenshot_18.png){ #fig:018 width=70% }

## Журнал SSHD

![Просмотр событий службы SSHD](Screenshot_19.png){ #fig:019 width=70% }

# Постоянный журнал journald

## Создание постоянного журнала

![Настройка постоянного журнала journald](Screenshot_20.png){ #fig:020 width=70% }

# Итоги работы

## Вывод

В ходе работы были изучены механизмы регистрации и хранения системных событий в Linux.  
Были освоены приёмы настройки **rsyslog**, перенаправления логов веб-службы Apache и фильтрации сообщений по уровням приоритета.  
Также исследованы возможности **journalctl** по просмотру, фильтрации и анализу событий, а система журналирования **journald** была настроена на постоянное хранение логов.
