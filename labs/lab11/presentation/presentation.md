---
## Front matter
lang: ru-RU
title: Лабораторная работа №11
subtitle: Управление загрузкой системы
author:
  - Турсунов Мухамметназар
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 27 октября 2025

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

Получение навыков работы с загрузчиком **GRUB2** в операционной системе Linux.

# Ход выполнения работы

## Изменение конфигурации GRUB2

![Редактирование файла /etc/default/grub](Screenshot_1.png){ #fig:001 width=70% }

## Применение изменений в GRUB2

![Меню загрузчика GRUB](Screenshot_2.png){ #fig:002 width=70% }

## Вход в режим восстановления (rescue)

![Редактирование параметров загрузки (режим rescue)](Screenshot_3.png){ #fig:003 width=70% }

## Работа в режиме rescue.target

![Вывод systemctl list-units в режиме rescue](Screenshot_4.png){ #fig:004 width=70% }

## Проверка переменных среды

![Переменные среды systemctl show-environment](Screenshot_4.png){ #fig:005 width=70% }

## Переход в аварийный режим (emergency)

![Редактирование параметров загрузки (режим emergency)](Screenshot_5.png){ #fig:006 width=70% }

## Работа в emergency.target

![Вывод systemctl list-units в аварийном режиме](Screenshot_6.png){ #fig:007 width=70% }

## Восстановление пароля root

![Загрузка с параметром rd.break](Screenshot_7.png){ #fig:008 width=70% }

# Итоги работы

## Вывод

В ходе лабораторной работы были изучены принципы управления загрузчиком **GRUB2**.  
Рассмотрены методы изменения конфигурации, настройка параметров загрузки, переход в режимы **rescue.target** и **emergency.target**, а также процесс восстановления пароля **root**.  
Полученные знания позволяют администратору эффективно управлять процессом загрузки и устранять ошибки при инициализации системы.
