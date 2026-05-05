# Лабораторная работа №4
## Разработка инфраструктуры MVP AI приложения

---

| Поле | Значение |
|---|---|
| **University** | [ITMO University](https://itmo.ru/ru/) |
| **Faculty** | [FTMI](https://ftmi.itmo.ru/) |
| **Course** | [Cloud platforms as the basis of technology entrepreneurship](https://itmo-ict-faculty.github.io/cloud-platforms-as-the-basis-of-technology-entrepreneurship/) |
| **Year** | 2025/2026 |
| **Group** | U4125 |
| **Author** | Мажукина Ирина |
| **Lab** | Lab4 |
| **Date of create** | 30.04.2026 |
| **Date of finished** | 30.04.2026 |

---

## Разработка инфраструктуры MVP AI приложения

---

## Описание

Это четвертая лабораторная работа "Разработка инфраструктуры MVP AI приложения."

---

## Цель работы

Создать прототип AI-приложения с базовой функциональностью.

---

## Ход работы

### 1. Формализация требований и нагрузок

· Начальное (MVP): Закрытый контур разработчика. Цель - доказать гипотезу за неделю с минимальным чеком.
· Тестирование (UAT): Доступ по invite-ссылкам для 50 пользователей. Сквозная аналитика использования AI.
· Прод: Публичный релиз. Всплески трафика перед праздниками (23 февраля, 8 марта, НГ). 

### 2. Выбор стека GCP 

AI-модель: MVP - Vertex AI API (Gemini 1.5 Flash), UAT - Vertex AI API + Controlled Generation, prod - Vertex AI или Cloud Run GPU (облегченная модель).
Прямой доступ к Gemini без аренды GPU. На старте платим только за токены (примерно $0.15/$0.60 за 1M токенов). Не нужно администрировать инстансы.

Фронтенд: MVP - Cloud Storage (Static website) + Load Balancer (External), UAT - Cloud Storage + Global External, prod - LB + Cloud CDN Cloud Storage + Global LB + CDN
SPA-приложение собирается в статику. Дешевле и быстрее Compute Engine. CDN бесплатен для Cloud Storage.

Бэкенд: MVP - Cloud Run (on demand), UAT - Cloud Run + Secrets Manager, prod - GKE Autopilot (или Cloud Run при стабильном трафике)
Cloud Run идеален для малого трафика (платим 0 за простой). GKE в проде нужен, если появится сложная маршрутизация или WebSockets.

База данных: MVP - Cloud SQL (Development, 1 vCPU), UAT - Cloud SQL (Enterprise/Standard), prod - Cloud SQL (High Availability, Read-replicas)
Связка «юзер-генерация». SQL удобен для отчетов партнеров. Cloud SQL проще в миграциях, чем NoSQL.

Кэш/Лимиты: MVP - Нет (или Memorystore), UAT - Memorystore (Redis), prod - Memorystore (Redis, High Availability)
Хранение сессий и Rate Limiting (защита от перерасхода токенов Vertex AI).

### 3. Архитектурная схема
<img width="953" height="525" alt="image" src="https://github.com/user-attachments/assets/9f18c554-1b95-44b8-a284-4c9411b6d123" />


### 4. Расчет экономической модели
Все расчеты примерные на момент 2026 года для региона us-central1.

MVP (0 руб. в простое, кроме БД)

- Cloud Run (Бэк): Бесплатно в рамках Free Tier (2 млн запросов в месяц). Тратим $0.
- Vertex AI (Gemini Flash): $0.15 за 1 млн входных токенов + $0.60 за выходные.
Допустим: 1000 генераций/мес по 500 токенов = 0.5 млн токенов.
Итого: $0.30.
- Cloud SQL: Самый маленький db-f1-micro (shared core). $9/мес с учетом HDD на 10 ГБ.
- Cloud Storage: 1 ГБ хранения $0.02.
Итого MVP: $10/мес.

UAT (Добавляем кэш и нормальную БД)

- Cloud Run: Выходим из фри-тира. Допустим, выделяем 1 vCPU, 1 GB RAM и лимит 10 инстансов.
Нагрузка: 100 RPM * 60 * 24 = 144 000 запросов/сутки. Время обработки запроса (генерация) - 3 сек.
Расчет: 144к * (3/60) * 24ч = 720 vCPU-часов/мес? Грубо: $15-25/мес за Cloud Run.
- Memorystore (Redis) Basic: $35/мес (M1 instance).
- Cloud SQL: Переход на выделенный db-standard-1 (1 vCPU, 3.75 GB), CP/Планы. $75/мес.
- Vertex AI: Нагрузка выросла в 30 раз.
Итого AI: ~$30-50/мес.
Итого UAT: ~$150-180/мес.

Prod

- GKE Autopilot или Cloud Run 24/7: Автоскейлинг. $100-200/мес.
- Cloud SQL HA: Две ноды в разных зонах. db-standard-2. $280/мес.
- Нагрузка на AI: Основные расходы ($500 - $2000+/мес).
Оптимизация: Настройка кэширования в Memorystore и семантическая фильтрация, чтобы не генерировать запросы дважды.
- Cloud CDN & LB: $20-50/мес за трафик.
Итого Prod: $800-$2500/мес (сильно зависит от креативности пользователей).
