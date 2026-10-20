# KOUT

Системный администратор / инженер эксплуатации. Занимаюсь телефонией на
MikoPBX, мониторингом на Prometheus + Grafana, доставкой алертов в чат
Битрикс24 и внутренними веб-приложениями.

**Стек:** PHP 8.4 (Phalcon 5) · Python 3 · Flask · SQLite · MySQL · Asterisk /
MikoPBX · Prometheus · Grafana · SNMP · ICMP (blackbox_exporter) · PowerShell ·
Linux (systemd) · Docker

---

## Проекты

### Модули для MikoPBX

| | |
|---|---|
| [**MikoPBX_Prometheus**](https://github.com/kutyassistent-ops/MikoPBX_Prometheus) | Экспортёр метрик MikoPBX в формате Prometheus. 8 групп метрик, REST-статус, bearer-токен, TLS, кэш ответа, 15 правил алертинга. Версия 1.4.3 |
| [**MikoPBX_birix_notify**](https://github.com/kutyassistent-ops/MikoPBX_birix_notify) | Уведомления о звонках в чат Битрикс24: пропущенные, входящие в реальном времени, произвольные сообщения через REST. Версия 2.0.10 |

### Мониторинг

| | |
|---|---|
| [**grafana-dashboards**](https://github.com/kutyassistent-ops/grafana-dashboards) | Дашборды Grafana для стенда: АТС (48 панелей), принтеры по SNMP, камеры и видеорегистраторы по ICMP. С документацией по каждому дашборду |
| [**grafana-bitrix-relay**](https://github.com/kutyassistent-ops/grafana-bitrix-relay) | Посредник Grafana → Битрикс24: принимает JSON от вебхука Grafana и отдаёт `im.notify`. Только стандартная библиотека Python, systemd-юнит, фильтр по IP |

### Приложения и утилиты

| | |
|---|---|
| [**utility-billing**](https://github.com/kutyassistent-ops/utility-billing) | Учёт коммунальных платежей: роли арендодатель/арендатор, показания счётчиков, автоматический расчёт счетов, долг и статистика. Flask + SQLite |
| [**shopping-list-demo**](https://github.com/kutyassistent-ops/shopping-list-demo) | Демо совместного списка покупок: REST API, синхронизация раз в 5 секунд, поиск по товарам и авторам |
| [**usb-watchdog**](https://github.com/kutyassistent-ops/usb-watchdog) | Управление аппаратным сторожевым таймером USB WatchDog Pro2 через COM-порт: протокол, примеры PowerShell, разбор особенностей |

---

## Что между собой связано

```
MikoPBX ──► ModulePrometheus ──► Prometheus ──► Grafana
                                                     │
                            grafana-bitrix-relay ◄──┤  (alerts)
                                     │               │
                                     ▼               ▼
                                  Bitrix24      grafana-dashboards
```

Модуль уведомлений (`MikoPBX_birix_notify`) работает по тому же методу
`im.notify`, что и relay, — двумя путями доставляются и алерты Grafana, и
события телефонии.

---

## Принципы, которых придерживаюсь

- **Токены и пароли — не в коде.** Значения читаются из конфига или
  переменной окружения, в логи не попадают.
- **Данные — не в git.** Файлы баз данных с персональными данными
  исключены, в репозитории лежит только схема.
- **Каждое решение — в changelog.** Что чинили, почему и как проверяли.
- **Проверено и не проверено — раздельно.** В README есть раздел о том, что
  подтверждено на живой АТС, а что проверено только по исходникам.

---

## Лицензии

Модули для MikoPBX распространяются под **GPL-3.0-or-later** — исходники
доступны в репозиториях.
