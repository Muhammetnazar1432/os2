---
## Front matter
lang: ru-RU
title: Лабораторная работа №9
subtitle: Управление SELinux
author:
  - Турсунов Мухамметназар
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 17 октября 2025

## Formatting
toc: false
slide_level: 2
aspectratio: 169
theme: metropolis
section-titles: true
header-includes:
 - \metroset{progressbar=frametitle,sectionpage=progressbar,numbering=fraction}
---

# Цель работы

## Основная цель

Изучение принципов работы системы безопасности **SELinux**, освоение режимов работы, восстановления контекстов и настройки политик безопасности для служб Linux.

# Управление режимами SELinux

## Проверка состояния SELinux

![Вывод команды sestatus -v](Screenshot_1.png){ #fig:001 width=70% }

## Переключение режима работы

![Изменение режима на Permissive](Screenshot_2.png){ #fig:002 width=70% }

## Отключение SELinux

![Изменение параметра SELINUX=disabled](Screenshot_3.png){ #fig:003 width=70% }

## Проверка отключённого состояния

![Попытка включения SELinux при disabled](Screenshot_4.png){ #fig:004 width=70% }

## Повторное включение enforcing

![Включение SELINUX=enforcing](Screenshot_5.png){ #fig:005 width=70% }

## Восстановление меток при загрузке

![Автоматическое восстановление меток SELinux](Screenshot_6.png){ #fig:006 width=70% }

## Проверка статуса после восстановления

![SELinux снова включён, режим enforcing](Screenshot_7.png){ #fig:007 width=70% }

# Восстановление контекста безопасности

## Использование restorecon

![Восстановление контекста файла hosts](Screenshot_8.png){ #fig:008 width=70% }

## Массовое восстановление контекстов

![Автоматическое перемаркирование файловой системы](Screenshot_9.png){ #fig:009 width=70% }

# Настройка контекста для веб-сервера

## Новый каталог и файл index.html

![Создание каталога /web и файла index.html](Screenshot_11.png){ #fig:010 width=70% }

## Изменение DocumentRoot в httpd.conf

![Редактирование конфигурации Apache](Screenshot_10.png){ #fig:011 width=70% }

## Старт службы httpd

![Запуск службы httpd](Screenshot_11.png){ #fig:012 width=70% }

## Проверка результата в браузере lynx

![Отображение стандартной страницы Rocky Linux](Screenshot_12.png){ #fig:013 width=70% }

## Присвоение контекста httpd_sys_content_t

![Настройка контекста каталога /web](Screenshot_13.png){ #fig:014 width=70% }

## Корректное отображение веб-страницы

![Отображение пользовательской страницы веб-сервера](Screenshot_14.png){ #fig:015 width=70% }

# Работа с переключателями SELinux

## Проверка состояния переключателей

![Просмотр и изменение переключателя ftpd_anon_write](Screenshot_15.png){ #fig:016 width=70% }

# Итоги работы

## Вывод

В ходе работы были изучены режимы функционирования **SELinux**:  
**Enforcing**, **Permissive** и **Disabled**.  
Отработаны навыки восстановления контекстов безопасности, применения политик доступа и настройки правил для служб Linux.  
Освоено использование инструментов **semanage**, **restorecon** и **setsebool**.  
SELinux был успешно возвращён в активный режим **enforcing** с корректной политикой безопасности.
