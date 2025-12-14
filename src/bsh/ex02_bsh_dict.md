# Exercise 02 — Building a data dictionary (Построение словаря данных)

Задача: 1 — Sign up for a haircut (prefix: `bsh`)

Словарь данных построен по сущностям из `src/bsh/ex00_bsh_entity.md`.  
Формат: **Сущность**, **мнемоника поля**, **описание**, **тип**, **обязательность**.

Обозначения типов:
- `uuid` — идентификатор
- `string` — строка
- `int` — целое число
- `decimal` — число с точкой (деньги)
- `bool` — логическое
- `timestamp` — дата/время
- `enum(...)` — перечисление
- `json` — структурированные данные

## Филиал (BarbershopBranch) — `BRANCH`

| Сущность | Мнемоника | Описание | Тип | Обяз. |
|---|---|---|---|:---:|
| BRANCH | branch_id | Идентификатор филиала | uuid | + |
| BRANCH | name | Название филиала | string | + |
| BRANCH | address | Адрес филиала | string | + |
| BRANCH | timezone | Часовой пояс филиала | string | + |
| BRANCH | contacts | Контакты филиала (телефон/почта) | json | - |
| BRANCH | is_active | Признак активности филиала | bool | + |

## Клиент (UserAccount) — `USER`

| Сущность | Мнемоника | Описание | Тип | Обяз. |
|---|---|---|---|:---:|
| USER | user_id | Идентификатор пользователя | uuid | + |
| USER | phone | Номер телефона | string | - |
| USER | email | Email | string | - |
| USER | full_name | Имя/ФИО | string | - |
| USER | created_at | Дата регистрации | timestamp | + |
| USER | consents | Согласия (ПДн/уведомления) | json | + |
| USER | preferred_channel | Предпочитаемый канал уведомлений | enum(telegram,whatsapp,vk,sms,email,push) | - |
| USER | status | Статус аккаунта | enum(active,blocked,deleted) | + |

## Гость (GuestContact) — `GUEST`

| Сущность | Мнемоника | Описание | Тип | Обяз. |
|---|---|---|---|:---:|
| GUEST | guest_id | Идентификатор гостя | uuid | + |
| GUEST | phone | Телефон для связи | string | + |
| GUEST | name | Имя (если указано) | string | - |
| GUEST | verification_status | Статус подтверждения контакта | enum(pending,verified,failed) | + |
| GUEST | created_at | Дата создания контакта | timestamp | + |

## Услуга (Service) — `SERVICE`

| Сущность | Мнемоника | Описание | Тип | Обяз. |
|---|---|---|---|:---:|
| SERVICE | service_id | Идентификатор услуги | uuid | + |
| SERVICE | name | Название услуги | string | + |
| SERVICE | category | Категория (парикмахерская/косметология) | string | + |
| SERVICE | duration_min | Длительность (мин.) | int | + |
| SERVICE | price | Цена | decimal | + |
| SERVICE | is_active | Доступность услуги | bool | + |

## Мастер (Master) — `MASTER`

| Сущность | Мнемоника | Описание | Тип | Обяз. |
|---|---|---|---|:---:|
| MASTER | master_id | Идентификатор мастера | uuid | + |
| MASTER | full_name | ФИО мастера | string | + |
| MASTER | branch_id | Филиал, где работает мастер | uuid | + |
| MASTER | service_ids | Список услуг, которые выполняет | json | + |
| MASTER | is_active | Активен/не активен | bool | + |

## Слот расписания (ScheduleSlot) — `SLOT`

| Сущность | Мнемоника | Описание | Тип | Обяз. |
|---|---|---|---|:---:|
| SLOT | slot_id | Идентификатор слота | uuid | + |
| SLOT | master_id | Мастер | uuid | + |
| SLOT | start_at | Начало интервала | timestamp | + |
| SLOT | end_at | Конец интервала | timestamp | + |
| SLOT | status | Статус слота | enum(free,booked,blocked) | + |
| SLOT | block_reason | Причина блокировки | string | - |
| SLOT | updated_at | Дата/время изменения | timestamp | + |

## Запись (Appointment) — `APPT`

| Сущность | Мнемоника | Описание | Тип | Обяз. |
|---|---|---|---|:---:|
| APPT | appointment_id | Идентификатор записи | uuid | + |
| APPT | client_ref | Ссылка на клиента (`user_id` или `guest_id`) | string | + |
| APPT | service_id | Услуга | uuid | + |
| APPT | master_id | Мастер | uuid | + |
| APPT | branch_id | Филиал | uuid | + |
| APPT | start_at | Начало визита | timestamp | + |
| APPT | end_at | Конец визита | timestamp | + |
| APPT | status | Статус записи | enum(created,confirmed,cancelled,rescheduled,done,no_show) | + |
| APPT | created_at | Дата создания | timestamp | + |
| APPT | comment | Комментарий клиента/менеджера | string | - |

## Напоминание/уведомление (Notification) — `NOTIF`

| Сущность | Мнемоника | Описание | Тип | Обяз. |
|---|---|---|---|:---:|
| NOTIF | notification_id | Идентификатор уведомления | uuid | + |
| NOTIF | appointment_id | Запись, к которой относится уведомление | uuid | + |
| NOTIF | recipient | Получатель (телефон/идентификатор в мессенджере/email) | string | + |
| NOTIF | channel | Канал отправки | enum(telegram,whatsapp,vk,sms,email,push) | + |
| NOTIF | template_id | Шаблон сообщения | string | - |
| NOTIF | send_at | Плановое время отправки | timestamp | + |
| NOTIF | delivery_status | Статус доставки | enum(planned,sent,delivered,failed) | + |
| NOTIF | provider_message_id | Идентификатор у провайдера | string | - |

## Оплата (Payment) — `PAY`

| Сущность | Мнемоника | Описание | Тип | Обяз. |
|---|---|---|---|:---:|
| PAY | payment_id | Идентификатор платежа | uuid | + |
| PAY | appointment_id | Запись, за которую оплачено | uuid | + |
| PAY | amount | Сумма | decimal | + |
| PAY | currency | Валюта | string | + |
| PAY | method | Способ оплаты | enum(cash,card,online) | + |
| PAY | status | Статус платежа | enum(pending,paid,failed,refunded) | + |
| PAY | transaction_ref | Идентификатор транзакции провайдера | string | - |
| PAY | paid_at | Дата/время оплаты | timestamp | - |

## Отзыв/оценка (Review) — `REVIEW`

| Сущность | Мнемоника | Описание | Тип | Обяз. |
|---|---|---|---|:---:|
| REVIEW | review_id | Идентификатор отзыва | uuid | + |
| REVIEW | appointment_id | Связь с записью | uuid | + |
| REVIEW | rating | Оценка (например, 1..5) | int | + |
| REVIEW | comment | Текст отзыва | string | - |
| REVIEW | created_at | Дата/время создания | timestamp | + |
| REVIEW | published | Признак публикации | bool | + |

