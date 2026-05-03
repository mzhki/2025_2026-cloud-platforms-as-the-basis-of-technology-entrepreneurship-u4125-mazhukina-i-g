# Лабораторная работа №1
## «Обзор Google Cloud и исследование основных сервисов»

---

| Поле | Значение |
|---|---|
| **University** | [ITMO University](https://itmo.ru/ru/) |
| **Faculty** | [FTMI](https://ftmi.itmo.ru/) |
| **Course** | [Cloud platforms as the basis of technology entrepreneurship](https://itmo-ict-faculty.github.io/cloud-platforms-as-the-basis-of-technology-entrepreneurship/) |
| **Year** | 2025/2026 |
| **Group** | U4125 |
| **Author** | Мажукина Ирина |
| **Lab** | Lab1 |
| **Date of create** | 30.04.2026 |
| **Date of finished** | 30.04.2026 |

---

## Обзор Google Cloud и исследование основных сервисов

---

## Описание

Это первая лабораторная работа "Обзор Google Cloud и исследование основных сервисов." 

---

## Цель работы

Ознакомиться с основными возможностями и преимуществами облачной платформы Google Cloud.
---

## Ход работы

### 1. Получение доступа

Перед началом курсом заполнила гугл-форму, где указала свою почту gmail, чтобы преподаватель мог выдать мне доступ к платформе.

### 2. Создание Service account

Мне нужна учетная запись для программного доступа к ресурсам, а не личный Google аккаунт. В поиске сверху ввела IAM и перешла в раздел IAM & Admin - Service Accounts. Нажмите + СОЗДАТЬ СЕРВИСНЫЙ АККАУНТ.

![Скрин](../1_1.jpg)

После ввода информации нажала CREATE AND CONTINUE.

Затем выбрала роль Storage Admin.

![Скрин](../1_2.jpg)

Получила email imazhukina-sa-lab1@cloud-platforms-as-the-basis.iam.gserviceaccount.com.

### 3. Создание VM

Перешла в раздел Compute Engine - VM instances

![Скрин](../1_3.jpg)

Нажала CREATE INSTANCE

- Ввела имя - imazhukina-vm-lab1
- Выбрала регион, конфигурацию - серию и тип
- В разделе Advanced options - Availability policy, в VM provisioning model выбрала Spot.
- Изменила сервисный аккаунт на свой.

![Скрин](../1_4.jpg)
![Скрин](../1_5.jpg)
![Скрин](../1_6.jpg)
![Скрин](../1_7.jpg)

Нажала CREATE

VM создалась и включилась.

![Скрин](../1_8.jpg)

### 4. Работа с Cloud Storage и gsutil

Напротив моей ВМ нажала кнопку SSH (Открыть в браузере). Открылся терминал. 

Проверила какой аккаунт используется 

![Скрин](../1_13.jpg)

В терминале ВМ ввела команду, чтобы убедиться, что бакет существует и доступен:

```bash
gsutil ls gs://lab1-bucket-itmo
```
![Скрин](../1_9.jpg)

Создала папку на ВМ и скопировала туда 3 файла.

```bash
mkdir ~/lab-files
gsutil cp gs://lab1-bucket-itmo/* ~/lab-files/
```

![Скрин](../1_10.jpg)

Выполнила команду для списка файлов с размерами

```bash
ls -lah ~/lab-files/
```
![Скрин](../1_11.jpg)

### 5. Изменение прав доступа

Перешла в IAM & Admin - IAM

Нашла свой service account imazhukina-sa-lab1@cloud-platforms-as-the-basis.iam.gserviceaccount.com

Изменила роль: удалила роль Storage Admin и добавила роль Compute Viewer.

![Скрин](../1_12.jpg)

Вернулась в SSH терминал.
Попыталась снова скопировать файлы с помощью команды:

```bash
gsutil cp gs://lab1-bucket-itmo/* ~/lab-files/
```

Получила ошибку
![Скрин](../1_14.jpg)

Это произошло в связи с тем, что у моего аккаунта больше нет прав Storage Admin, а роль Compute Viewer позволяет только смотреть ВМ, но не читать данные из Cloud Storage. Для доступа к хранилищу нужны специфичные роли Storage, а не Compute.

Удалила VM
![Скрин](../1_15.jpg)

