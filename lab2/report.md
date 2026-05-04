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

<img width="1280" height="119" alt="2_1" src="https://github.com/user-attachments/assets/485d34c5-0ce5-436e-abe3-e045fd88644c" />


Указала Google Cloud CLI, с каким проектом работать по умолчанию.

```bash
gcloud config set project cloud-platforms-as-the-basis
```
<img width="1280" height="71" alt="2_2" src="https://github.com/user-attachments/assets/0261b700-6ebd-437a-b6bf-d223c94452f7" />


### 2. Создание Cloud Run сервиса

Перешла в раздел Cloud Run - Create Service. Выбрала Deploy one revision from an existing container image. В поле Container image URL указала:
gcr.io/cloudrun/hello. Назвала сервис.
Регион: us-central1 (или ближайший).
В разделе Container задала:
CPU: 1
Memory: 256 MiB

<img width="1280" height="763" alt="2_3" src="https://github.com/user-attachments/assets/24cca3c6-8171-4c31-8c28-529e0051e601" />
<img width="1280" height="1059" alt="2_4" src="https://github.com/user-attachments/assets/ee5f3ded-7105-41d4-8901-14ad90523777" />
<img width="1195" height="1025" alt="2_5" src="https://github.com/user-attachments/assets/c94afc2e-5844-4a36-b790-5c47c62ecc18" />


Нажала Create.

Сервис создан.

<img width="1280" height="106" alt="2_6" src="https://github.com/user-attachments/assets/77b48525-fac0-45b2-81a1-0837313645bd" />


### 3. Тестирование сервиса

Перешла по полученной ссылке в браузере. Убедиться, что сервис работает.

<img width="1280" height="806" alt="2_7" src="https://github.com/user-attachments/assets/d4a2c5d7-9603-4cdf-bd4e-e77c30537b62" />


### 4. Логи и метрики

Перешла в сервисе во вкладку LOGS.
Посмотреть логи запроса, который ввыполнила.
Нашла там HTTP GET и статус 200.

<img width="1280" height="585" alt="2_8" src="https://github.com/user-attachments/assets/c784a45c-7602-451b-abc4-4bc1e41c50c6" />


Во вкладке METRICS посмотрела графики:

Request count - сервис получает очень мало трафика (тестовые запросы), все запросы успешные (нет ошибок 4xx/5xx), сервис работает корректно
Request latency - показывает распределение времени ответа сервиса:
50% запросов (медиана): около 5 мс
95% запросов: около 10-11 мс*
99% запросов: около 13-14 мс*
Container instance count - показывает количество активных и простаивающих экземпляров сервиса. В период с 6:00 до 12:00 значение равно 0 для обоих типов, что указывает на отсутствие трафика.

<img width="1280" height="584" alt="2_9" src="https://github.com/user-attachments/assets/f4e9419a-52bc-4bf9-bd20-4179cdc6f94b" />
<img width="1280" height="383" alt="2_10" src="https://github.com/user-attachments/assets/a9eaa047-7949-442b-b482-6a39779739e0" />


### 5. Изменение порта

В сервисе нажала Edit & Deploy New Revision.Раскрыла Container - Container port - указала 8090 вместо 8080.
Остально оставила как было.
Нажала Deploy.

<img width="1220" height="1173" alt="2_11" src="https://github.com/user-attachments/assets/8738a139-e29a-4d01-b76f-5588bd7f76d3" />


URL все равно не перестал отвечать, ошибки не появилось, проверила в логах, ошибки нет.

<img width="1280" height="282" alt="2_12" src="https://github.com/user-attachments/assets/4089f0e3-c463-4c25-be7d-83771299d8cf" />


Образ gcr.io/cloudrun/hello поддерживает переменную PORT. Когда я указала порт 8090 в настройках Cloud Run:
- Cloud Run передал внутрь контейнера переменную PORT=8090
- приложение внутри прочитало эту переменную и запустилось на порту 8090
- Cloud Run направил трафик на этот порт 
Ошибка возникает только если образ не поддерживает переменную PORT и слушает на жестко заданном порту (например, 8080).

При переключении трафика между версиями очевидно также все работало.

<img width="1280" height="530" alt="2_13" src="https://github.com/user-attachments/assets/401f4918-9a05-46b6-a0c2-21568d341333" />


В завершении лабораторной удалила сервис.

