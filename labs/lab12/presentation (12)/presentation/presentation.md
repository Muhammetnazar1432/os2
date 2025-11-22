---
## Front matter
lang: ru-RU
title: Лабораторная работа №12
subtitle: Настройки сети в Linux
author:
  - Турсунов Мухамметназар
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 2 ноября 2025

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

Получить навыки настройки сетевых параметров системы Linux с использованием инструментов **ip**, **ifconfig**, **nmcli** и **nmtui**.

# Ход выполнения работы

## Проверка конфигурации сети

![Информация об интерфейсах и статистика пакетов](Screenshot_8.png){ #fig:001 width=70% }

## Проверка подключения к интернету

![Проверка доступности узла 8.8.8.8](Screenshot_9.png){ #fig:003 width=70% }

## Добавление IP-адреса

![Добавление дополнительного IP-адреса](Screenshot_10.png){ #fig:004 width=70% }

## Сравнение ip и ifconfig

![Информация об интерфейсах с помощью ifconfig](Screenshot_11.png){ #fig:005 width=70% }

## Просмотр открытых портов

![Список прослушиваемых портов TCP и UDP](Screenshot_12.png){ #fig:006 width=70% }

## Управление подключениями через nmcli

![Список соединений NetworkManager](Screenshot_13.png){ #fig:007 width=70% }

## Создание соединений dhcp и static

![Активация статического соединения](Screenshot_14.png){ #fig:008 width=70% }

## Переключение между профилями

![Переключение на соединение DHCP](Screenshot_15.png){ #fig:009 width=70% }

## Изменение параметров static

![Изменение параметров соединения static через nmcli](Screenshot_16.png){ #fig:010 width=70% }

## Просмотр параметров через nmtui

![Настройки соединения static в nmtui](Screenshot_17.png){ #fig:011 width=70% }

## Просмотр параметров DHCP-профиля

![Настройки соединения dhcp в nmtui](Screenshot_18.png){ #fig:012 width=70% }

## Графический интерфейс

![Параметры соединения static в графическом интерфейсе](Screenshot_20.png){ #fig:013 width=70% }

## Итоговая проверка

![Параметры соединения dhcp в графическом интерфейсе](Screenshot_21.png){ #fig:014 width=70% }

# Итоги работы

## Вывод

В результате лабораторной работы были изучены методы конфигурирования сетевых интерфейсов в Linux.  
Освоены утилиты **ip**, **ifconfig**, **ping**, **ss**, а также инструменты **nmcli** и **nmtui**.  
Были выполнены операции добавления IP-адресов, настройки шлюза, DNS-серверов и маршрутов.  
Все поставленные задачи выполнены успешно.
