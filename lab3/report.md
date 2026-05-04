# Лабораторная работа №1
## «Исследование Cloud Storage»

---

| Поле | Значение |
|---|---|
| **University** | [ITMO University](https://itmo.ru/ru/) |
| **Faculty** | [FTMI](https://ftmi.itmo.ru/) |
| **Course** | [Cloud platforms as the basis of technology entrepreneurship](https://itmo-ict-faculty.github.io/cloud-platforms-as-the-basis-of-technology-entrepreneurship/) |
| **Year** | 2025/2026 |
| **Group** | U4125 |
| **Author** | Мажукина Ирина |
| **Lab** | Lab3 |
| **Date of create** | 30.04.2026 |
| **Date of finished** | 30.04.2026 |

---

## Исследование Cloud Storage

---

## Описание

Это третья лабораторная работа Исследование Cloud Storage 

---

## Цель работы

Ознакомиться с основными понятиями и принципами работы облачного хранилища, изучат различные модели хранения данных (блок, файл, объектное хранилище), познакомятся с основными сервисами и функционалом, предоставляемым облачными хранилищами.
---

## Ход работы

### 1. Создание бакета

В меню навигации слева нашла раздел Cloud Storage и выберите Buckets. Нажала кнопку Create.

Имя бакета: lab3-files-2026-ig.
Остальное без изменений - парметры по умолчанию.
<img width="1280" height="813" alt="3_1" src="https://github.com/user-attachments/assets/f87368f3-73f3-4b13-8fd2-b4c3d77dc78b" />


Сняла галочку Enforce public access prevention, иначе не смогу открыть файлы.
<img width="797" height="531" alt="3_2" src="https://github.com/user-attachments/assets/2f406c54-fecd-4ef9-95b1-a9995e2555b0" />


Нажала Create.

### 2. Загрузка изображений

Нажмите созданный бакет. Нажала Upload files. Выбрала на компьютере 4 изображения jpg.
Файлы появились в корне бакета.

<img width="1280" height="831" alt="3_3" src="https://github.com/user-attachments/assets/9761c97e-72ed-423d-a316-c693d38bddfc" />


### 3. Создание папки и перемещение файлов

Внутри бакета нажала кнопку Create folder. Назвала папку images. 

<img width="1243" height="849" alt="3_4" src="https://github.com/user-attachments/assets/0c3b0bb0-fb2b-4e58-a95c-b850dd44f8cb" />


Отметила галочками изображения, которые загрузила. Нажала на кнопку More actions и выбрала Move.
В поле Destination выбрала путь к папке.

<img width="1209" height="962" alt="3_5" src="https://github.com/user-attachments/assets/c3aee856-2217-4ac5-80f7-ab9de68b7076" />


Переместила файлы.

### 4. Настройка публичного доступа

Пока у бакета включен Uniform режим (оставила параметр по умолчанию) - открыть доступ к файлу не смогу. Переключила бакет в Fine-grained.

<img width="741" height="243" alt="3_7" src="https://github.com/user-attachments/assets/158ffb7d-b341-472c-9225-7294ab13c431" />


Внутри папки images нажала на три точки рядом с первым файлом. Выберите пункт Edit access.
Выбрала Add entry. В поле Entity выбрала allUsers. В поле Access выбрала Reader.

<img width="1220" height="903" alt="3_8" src="https://github.com/user-attachments/assets/40a9e5ac-e7c5-440d-8556-1e7b4968b823" />


### 5. Получение ссылки на файл

Нажмите на имя файла, который я сделала публичным, чтобы открыть страницу Object details.
<img width="1246" height="1170" alt="3_9" src="https://github.com/user-attachments/assets/7ede80f1-4947-43c5-bfac-6c4a9c79f8f5" />


Перешла по URL, картинка открылась.
<img width="1280" height="803" alt="3_10" src="https://github.com/user-attachments/assets/7a0fb2ab-595d-4801-87f7-e025cc10769d" />


Перешла по URL в другом браузере в режиме инкогнито, картинка открылась.
<img width="1280" height="802" alt="3_11" src="https://github.com/user-attachments/assets/a927e3f8-894f-4863-b7ab-8b2629404320" />


В завершении лабораторной удалила бакет.

