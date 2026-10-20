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

| Проект | Что это | Статус |
|---|---|---|
| [**MikoPBX_Prometheus**](https://github.com/kutyassistent-ops/MikoPBX_Prometheus)<br>![CI](https://github.com/kutyassistent-ops/MikoPBX_Prometheus/actions/workflows/ci.yml/badge.svg) | Экспортёр метрик MikoPBX в формате Prometheus. 8 групп метрик, REST-статус, bearer-токен, TLS, кэш ответа, 15 правил алертинга | v1.4.3 |
| [**MikoPBX_birix_notify**](https://github.com/kutyassistent-ops/MikoPBX_birix_notify)<br>![CI](https://github.com/kutyassistent-ops/MikoPBX_birix_notify/actions/workflows/ci.yml/badge.svg) | Уведомления о звонках в чат Битрикс24: пропущенные, входящие в реальном времени, произвольные сообщения через REST | v2.0.10 |

### Мониторинг

| Проект | Что это | Статус |
|---|---|---|
| [**grafana-dashboards**](https://github.com/kutyassistent-ops/grafana-dashboards)<br>![CI](https://github.com/kutyassistent-ops/grafana-dashboards/actions/workflows/ci.yml/badge.svg) | Дашборды Grafana для стенда: АТС (48 панелей), принтеры по SNMP, камеры и видеорегистраторы по ICMP. С документацией по каждому дашборду | 3 дашборда |
| [**grafana-bitrix-relay**](https://github.com/kutyassistent-ops/grafana-bitrix-relay)<br>![CI](https://github.com/kutyassistent-ops/grafana-bitrix-relay/actions/workflows/ci.yml/badge.svg) | Посредник Grafana → Битрикс24: принимает JSON от вебхука Grafana и отдаёт `im.notify`. Только стандартная библиотека Python, systemd-юнит, фильтр по IP | с юнит-тестами |

### Приложения и утилиты

| Проект | Что это | Статус |
|---|---|---|
| [**utility-billing**](https://github.com/kutyassistent-ops/utility-billing)<br>![CI](https://github.com/kutyassistent-ops/utility-billing/actions/workflows/ci.yml/badge.svg) | Учёт коммунальных платежей: роли арендодатель/арендатор, показания счётчиков, автоматический расчёт счетов, долг и статистика | Flask + SQLite |
| [**shopping-list-demo**](https://github.com/kutyassistent-ops/shopping-list-demo)<br>![CI](https://github.com/kutyassistent-ops/shopping-list-demo/actions/workflows/ci.yml/badge.svg) | Демо совместного списка покупок: REST API, синхронизация раз в 5 секунд, поиск по товарам и авторам | учебный пример |
| [**usb-watchdog**](https://github.com/kutyassistent-ops/usb-watchdog)<br>![CI](https://github.com/kutyassistent-ops/usb-watchdog/actions/workflows/ci.yml/badge.svg) | Управление аппаратным сторожевым таймером USB WatchDog Pro2 через COM-порт: протокол, драйвер CDC, автозапуск через Планировщик | Windows, PowerShell |

---

## Как это связано между собой

```
MikoPBX ──► MikoPBX_Prometheus ──► Prometheus ──► Grafana
                                                    │
                         grafana-bitrix-relay ◄─────┤  алерты
                                  │                 │
                                  ▼                 ▼
                               Bitrix24      grafana-dashboards

MikoPBX ──► MikoPBX_birix_notify ──────────────► Bitrix24   (звонки)
```

Модуль уведомлений (`MikoPBX_birix_notify`) работает по тому же методу
`im.notify`, что и relay: двумя путями доставляются и алерты Grafana, и
события телефонии.

---

## Принципы, которых придерживаюсь

- **Токены и пароли — не в коде.** Значения читаются из конфига или
  переменной окружения, в логи не попадают. В каждом репозитории CI
  проверяет, что секрет не закоммичен.
- **Данные — не в git.** Файлы баз данных с персональными данными
  исключены, в репозитории лежит только схема.
- **Каждое решение — в changelog.** Что чинили, почему и как проверяли.
- **Проверено и не проверено — раздельно.** В README есть раздел о том, что
  подтверждено на живой АТС, а что проверено только по исходникам.
- **Публикация проверяется автоматически.** В каждом репозитории есть CI:
  тесты, проверка синтаксиса, валидация архивов и дашбордов.

---

## Лицензии

- Модули для MikoPBX — **GPL-3.0-or-later**, исходники доступны.
- Остальные проекты — **MIT**.

Профиль оформлен как витрина: код и документация лежат в отдельных
репозиториях, здесь только указатель.
