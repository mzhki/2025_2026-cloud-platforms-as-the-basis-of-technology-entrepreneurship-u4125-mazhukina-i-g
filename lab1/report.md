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

<img width="1280" height="732" alt="1 1" src="https://github.com/user-attachments/assets/41b22028-b44a-4672-9def-8c872ec24533" />

После ввода информации нажала CREATE AND CONTINUE.

Затем выбрала роль Storage Admin.

<img width="1280" height="764" alt="1 2" src="https://github.com/user-attachments/assets/0821cf5c-07e6-4c34-a442-a7fa9c488e72" />

Получила email imazhukina-sa-lab1@cloud-platforms-as-the-basis.iam.gserviceaccount.com.

### 3. Создание VM

Перешла в раздел Compute Engine - VM instances

<img width="1280" height="764" alt="1 3" src="https://github.com/user-attachments/assets/1c30fff0-8dcb-4aaa-8a08-d2bd2929bba3" />

Нажала CREATE INSTANCE

- Ввела имя - imazhukina-vm-lab1
- Выбрала регион, конфигурацию - серию и тип
- В разделе Advanced options - Availability policy, в VM provisioning model выбрала Spot.
- Изменила сервисный аккаунт на свой.

<img width="1280" height="758" alt="1 4" src="https://github.com/user-attachments/assets/4286d3e4-f826-4d28-8536-4167e6866789" />
<img width="1280" height="765" alt="1 5" src="https://github.com/user-attachments/assets/a8cdf23c-6d5d-43ef-8188-81a2784869ec" />
<img width="1280" height="763" alt="1 6" src="https://github.com/user-attachments/assets/148778db-b45c-4718-96df-66a9effd11de" />
<img width="846" height="236" alt="1 7" src="https://github.com/user-attachments/assets/276bc734-083b-4462-bd45-828c898cefbe" />


Нажала CREATE

VM создалась и включилась.

<img width="1161" height="323" alt="1 8" src="https://github.com/user-attachments/assets/fc97a09f-bdf0-4237-9e86-8ae659d95308" />

### 4. Работа с Cloud Storage и gsutil

Напротив моей ВМ нажала кнопку SSH (Открыть в браузере). Открылся терминал. 

Проверила какой аккаунт используется 

<img width="1280" height="238" alt="1 13" src="https://github.com/user-attachments/assets/7429fa0d-7ef3-41e6-affc-8045b03a8ef3" />

В терминале ВМ ввела команду, чтобы убедиться, что бакет существует и доступен:

```bash
gsutil ls gs://lab1-bucket-itmo
```
<img width="1280" height="439" alt="1 9" src="https://github.com/user-attachments/assets/cb9ad923-6817-4edd-b5ea-a7e4d41c0ce5" />

Создала папку на ВМ и скопировала туда 3 файла.

```bash
mkdir ~/lab-files
gsutil cp gs://lab1-bucket-itmo/* ~/lab-files/
```

<img width="1280" height="598" alt="1 10" src="https://github.com/user-attachments/assets/11df70f5-edaa-44b1-9d5d-35b0cf240fd8" />


Выполнила команду для списка файлов с размерами

```bash
ls -lah ~/lab-files/
```
<img width="1280" height="813" alt="1 11" src="https://github.com/user-attachments/assets/4ca166c7-3b93-480e-b6c3-53607e6cc893" />


### 5. Изменение прав доступа

Перешла в IAM & Admin - IAM

Нашла свой service account imazhukina-sa-lab1@cloud-platforms-as-the-basis.iam.gserviceaccount.com

Изменила роль: удалила роль Storage Admin и добавила роль Compute Viewer.

<img width="1256" height="1242" alt="1 12" src="https://github.com/user-attachments/assets/7e3c31d9-f8e7-4244-a35d-854f713a0528" />


Вернулась в SSH терминал.
Попыталась снова скопировать файлы с помощью команды:

```bash
gsutil cp gs://lab1-bucket-itmo/* ~/lab-files/
```

Получила ошибку
<img width="1280" height="167" alt="1 14" src="https://github.com/user-attachments/assets/7da4c69d-9e65-4921-9cd0-cfa5b89e002e" />


Это произошло в связи с тем, что у моего аккаунта больше нет прав Storage Admin, а роль Compute Viewer позволяет только смотреть ВМ, но не читать данные из Cloud Storage. Для доступа к хранилищу нужны специфичные роли Storage, а не Compute.

Удалила VM
<img width="880" height="479" alt="1 15" src="https://github.com/user-attachments/assets/c25b2ac6-8f49-4df3-a414-ca6d0fece33c" />


