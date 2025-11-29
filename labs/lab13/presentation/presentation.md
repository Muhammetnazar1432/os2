---
## Front matter
lang: ru-RU
title: Лабораторная работа №13
subtitle: Настройка пакетного фильтра firewalld
author:
  - Турсунов Мухамметназар
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 07 ноября 2025

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

Получение навыков настройки пакетного фильтра Linux с использованием инструментов  
**firewall-cmd** и **firewall-config**.

# Ход выполнения работы

## Определение зоны брандмауэра

![Определение зоны](Screenshot_1.png){ width=70% }

## Просмотр доступных зон и служб

![Список зон и служб](Screenshot_1.png){ width=70% }

## Анализ конфигурации зоны

![Конфигурация зоны public](Screenshot_2.png){ width=70% }

## Добавление службы VNC (временное)

![Добавление vnc-server runtime](Screenshot_3.png){ width=70% }


## Добавление службы VNC (постоянное)

![Добавление vnc permanent](Screenshot_4.png){ width=70% }

## Добавление порта 2022/tcp

![Добавление порта 2022](Screenshot_5.png){ width=70% }

## Добавление сервисов в зоне public

![GUI — выбор сервисов](Screenshot_6.png){ width=70% }

## Добавление порта через GUI

![GUI — добавление порта](Screenshot_7.png){ width=65% }

## Применение изменений

![Перезагрузка и применение](Screenshot_8.png){ width=70% }

## Добавление служб telnet, imap, pop3, smtp

![Итоговая конфигурация](Screenshot_9.png){ width=70% }
# Итоги работы

## Вывод

В ходе работы:

- изучены режимы *runtime* и *permanent* в firewalld;
- освоены инструменты **firewall-cmd** и **firewall-config**;
- произведены операции добавления служб, портов и интерфейсов к зонам.

Получены навыки управления сетевой безопасностью в Linux.
