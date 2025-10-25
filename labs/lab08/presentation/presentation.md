---
## Front matter
lang: ru-RU
title: Лабораторная работа №8
subtitle: Планировщики событий
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

Получение навыков работы с планировщиками заданий **cron** и **at** в операционной системе Linux.

# Ход выполнения работы

## Проверка службы crond

![Проверка статуса службы crond](Screenshot_1.png){ #fig:001 width=70% }

## Конфигурация crontab

![Содержимое файла /etc/crontab](Screenshot_2.png){ #fig:002 width=70% }

## Просмотр расписания заданий

![Вывод команды crontab -l (пустое расписание)](Screenshot_3.png){ #fig:003 width=70% }

## Создание задания cron

![Создание задания cron через crontab -e](Screenshot_4.png){ #fig:004 width=70% }

## Проверка выполнения cron-заданий

![Проверка работы задания cron](Screenshot_5.png){ #fig:005 width=70% }

## Изменение расписания cron

![Изменённая запись crontab](Screenshot_6.png){ #fig:006 width=70% }

## Сценарий eachhour

![Создание сценария eachhour](Screenshot_7.png){ #fig:007 width=70% }

## Расписание /etc/cron.d

![Создание расписания в /etc/cron.d](Screenshot_8.png){ #fig:008 width=70% }

# Планирование с помощью at

## Проверка службы atd

![Проверка работы службы atd](Screenshot_1.png){ #fig:009 width=70% }

## Создание однократного задания

![Планирование и выполнение задачи с помощью at](Screenshot_8.png){ #fig:010 width=70% }

# Итоги работы

## Вывод

В ходе лабораторной работы были изучены средства планирования задач в Linux: **cron** для периодических и **at** для однократных запусков.  
Были освоены команды для редактирования расписаний, проверки их выполнения и управления службами **crond** и **atd**.  
Полученные навыки позволяют автоматизировать системные операции и повысить эффективность администрирования Linux.
