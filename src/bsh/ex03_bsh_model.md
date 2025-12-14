# Exercise 03 — Building a logical data model (Построение логической модели данных)

Задача: 1 — Sign up for a haircut (prefix: `bsh`)

Формат: Mermaid ER diagram (логическая модель данных).

## ER-диаграмма

```mermaid
erDiagram
    BARBERSHOP_BRANCH ||--o{ MASTER : "нанимает/закрепляет"
    BARBERSHOP_BRANCH ||--o{ APPOINTMENT : "принимает"

    MASTER ||--o{ SCHEDULE_SLOT : "имеет слоты"
    MASTER ||--o{ APPOINTMENT : "выполняет по записи"

    SERVICE ||--o{ APPOINTMENT : "заказывается в записи"

    USER_ACCOUNT ||--o{ APPOINTMENT : "создаёт (как клиент)"
    GUEST_CONTACT ||--o{ APPOINTMENT : "создаёт (как гость)"

    APPOINTMENT ||--o{ NOTIFICATION : "инициирует уведомления"
    APPOINTMENT ||--o{ PAYMENT : "оплачивается"
    APPOINTMENT ||--o{ REVIEW : "получает отзыв"

    MASTER ||--o{ REVIEW : "получает отзывы"

    MASTER ||--o{ MASTER_SERVICE : "оказывает"
    SERVICE ||--o{ MASTER_SERVICE : "входит в навыки"

    BARBERSHOP_BRANCH {
      uuid branch_id PK
      string name
      string address
      string timezone
      bool is_active
    }

    USER_ACCOUNT {
      uuid user_id PK
      string phone
      string email
      string full_name
      json consents
      string preferred_channel
      string status
      timestamp created_at
    }

    GUEST_CONTACT {
      uuid guest_id PK
      string phone
      string name
      string verification_status
      timestamp created_at
    }

    SERVICE {
      uuid service_id PK
      string name
      string category
      int duration_min
      decimal price
      bool is_active
    }

    MASTER {
      uuid master_id PK
      uuid branch_id FK
      string full_name
      bool is_active
    }

    MASTER_SERVICE {
      uuid master_id FK
      uuid service_id FK
    }

    SCHEDULE_SLOT {
      uuid slot_id PK
      uuid master_id FK
      timestamp start_at
      timestamp end_at
      string status
      string block_reason
      timestamp updated_at
    }

    APPOINTMENT {
      uuid appointment_id PK
      uuid branch_id FK
      uuid master_id FK
      uuid service_id FK
      uuid user_id FK
      uuid guest_id FK
      timestamp start_at
      timestamp end_at
      string status
      timestamp created_at
      string comment
    }

    NOTIFICATION {
      uuid notification_id PK
      uuid appointment_id FK
      string recipient
      string channel
      timestamp send_at
      string delivery_status
      string provider_message_id
    }

    PAYMENT {
      uuid payment_id PK
      uuid appointment_id FK
      decimal amount
      string currency
      string method
      string status
      string transaction_ref
      timestamp paid_at
    }

    REVIEW {
      uuid review_id PK
      uuid appointment_id FK
      int rating
      string comment
      bool published
      timestamp created_at
    }
```

## Допущения/гипотезы (для согласования)

- `APPOINTMENT` содержит **либо** `user_id` (для зарегистрированного клиента), **либо** `guest_id` (для гостя); оба одновременно не заполняются.
- Справочники статусов (`status`) и каналов (`channel`) показаны как строки/enum (отдельные таблицы можно добавить при необходимости).

