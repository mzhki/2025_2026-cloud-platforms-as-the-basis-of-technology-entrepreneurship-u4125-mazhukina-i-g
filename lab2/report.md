# Лабораторная работа №2
## «Исследование Cloud Run»

---

| Поле | Значение |
|---|---|
| **University** | [ITMO University](https://itmo.ru/ru/) |
| **Faculty** | [FTMI](https://ftmi.itmo.ru/) |
| **Course** | [Cloud platforms as the basis of technology entrepreneurship](https://itmo-ict-faculty.github.io/cloud-platforms-as-the-basis-of-technology-entrepreneurship/) |
| **Year** | 2025/2026 |
| **Group** | U4125 |
| **Author** | Мажукина Ирина |
| **Lab** | Lab2 |
| **Date of create** | 30.04.2026 |
| **Date of finished** | 30.04.2026 |

---

## Исследование Cloud Run

---

## Описание

Это вторая лабораторная работа "Исследование Cloud Run" 

---

## Цель работы

Ознакомиться с работой Cloud Run.
---

## Ход работы

### 1. Подготовка среды

С помощью Google Shell подключила API.
- Cloud Run API
- Cloud Logging API
- Cloud Monitoring API

```bash
gcloud services enable run.googleapis.com logging.googleapis.com monitoring.googleapis.com
```

![Скрин](../2_1.jpg)

Указала Google Cloud CLI, с каким проектом работать по умолчанию.

```bash
gcloud config set project cloud-platforms-as-the-basis
```
![Скрин](../2_2.jpg)

### 2. Создание Cloud Run сервиса

Перешла в раздел Cloud Run - Create Service. Выбрала Deploy one revision from an existing container image. В поле Container image URL указала:
gcr.io/cloudrun/hello. Назвала сервис.
Регион: us-central1 (или ближайший).
В разделе Container задала:
CPU: 1
Memory: 256 MiB

![Скрин](../2_3.jpg)
![Скрин](../2_4.jpg)
![Скрин](../2_5.jpg)

Нажала Create.

Сервис создан.

![Скрин](../2_6.jpg)

### 3. Тестирование сервиса

Перешла по полученной ссылке в браузере. Убедиться, что сервис работает.

![Скрин](../2_7.jpg)

### 4. Логи и метрики

Перешла в сервисе во вкладку LOGS.
Посмотреть логи запроса, который ввыполнила.
Нашла там HTTP GET и статус 200.

![Скрин](../2_8.jpg)

Во вкладке METRICS посмотрела графики:

Request count - сервис получает очень мало трафика (тестовые запросы), все запросы успешные (нет ошибок 4xx/5xx), сервис работает корректно
Request latency - показывает распределение времени ответа сервиса:
50% запросов (медиана): около 5 мс
95% запросов: около 10-11 мс*
99% запросов: около 13-14 мс*
Container instance count - показывает количество активных и простаивающих экземпляров сервиса. В период с 6:00 до 12:00 значение равно 0 для обоих типов, что указывает на отсутствие трафика.

![Скрин](../2_9.jpg)
![Скрин](../2_10.jpg)

### 5. Изменение порта

В сервисе нажала Edit & Deploy New Revision.Раскрыла Container - Container port - указала 8090 вместо 8080.
Остально оставила как было.
Нажала Deploy.

![Скрин](../2_11.jpg)

URL все равно не перестал отвечать, ошибки не появилось, проверила в логах, ошибки нет.

![Скрин](../2_12.jpg)

Образ gcr.io/cloudrun/hello поддерживает переменную PORT. Когда я указала порт 8090 в настройках Cloud Run:
- Cloud Run передал внутрь контейнера переменную PORT=8090
- приложение внутри прочитало эту переменную и запустилось на порту 8090
- Cloud Run направил трафик на этот порт 
Ошибка возникает только если образ не поддерживает переменную PORT и слушает на жестко заданном порту (например, 8080).

При переключении трафика между версиями очевидно также все работало.

![Скрин](../2_13.jpg)

В завершении лабораторной удалила сервис.

