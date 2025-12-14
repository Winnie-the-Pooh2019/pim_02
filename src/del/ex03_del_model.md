# Exercise 03 — Building a logical data model (Построение логической модели данных)

Задача: 2 — Delivery of orders (prefix: `del`)

Формат: Mermaid ER diagram (логическая модель данных).

## ER-диаграмма

```mermaid
erDiagram
    SUPPLIER ||--o{ PICKUP_POINT : "имеет точки выдачи"
    SUPPLIER ||--o{ ORDER : "создаёт/передаёт заказы"

    CUSTOMER ||--o{ DELIVERY_ADDRESS : "имеет адреса"
    CUSTOMER ||--o{ ORDER : "является получателем"

    PICKUP_POINT ||--o{ ORDER : "используется для получения"
    DELIVERY_ADDRESS ||--o{ ORDER : "используется для доставки"

    ORDER ||--|{ ORDER_ITEM : "содержит позиции"
    ORDER ||--o{ ORDER_EVENT : "имеет таймлайн"
    ORDER ||--o{ INCIDENT : "имеет инциденты"
    ORDER ||--o{ NOTIFICATION : "порождает уведомления"
    ORDER ||--o{ ROUTE_ETA : "имеет расчёты ETA"
    ORDER ||--o| POD : "закрывается POD"

    COURIER ||--o{ ASSIGNMENT : "получает назначения"
    ORDER ||--o{ ASSIGNMENT : "назначается курьеру"

    COURIER ||--o{ PAYOUT : "получает выплаты"
    PAYOUT ||--o{ PAYOUT_ORDER : "содержит строки выплат"
    ORDER ||--o{ PAYOUT_ORDER : "участвует в начислении"

    SUPPLIER {
      uuid supplier_id PK
      string name
      json contacts
      string integration_type
      bool is_active
    }

    PICKUP_POINT {
      uuid pickup_point_id PK
      uuid supplier_id FK
      string address
      decimal geo_lat
      decimal geo_lon
      json working_hours
      string instructions
    }

    CUSTOMER {
      uuid customer_id PK
      string full_name
      string phone
      json notification_prefs
      timestamp created_at
    }

    DELIVERY_ADDRESS {
      uuid address_id PK
      uuid customer_id FK
      string raw_address
      string normalized_address
      decimal geo_lat
      decimal geo_lon
      string entrance
      string floor
      string apartment
      string comment
    }

    ORDER {
      uuid order_id PK
      uuid supplier_id FK
      uuid customer_id FK
      uuid pickup_point_id FK
      uuid delivery_address_id FK
      timestamp delivery_window_from
      timestamp delivery_window_to
      timestamp ready_by
      string status
      timestamp created_at
      string special_instructions
    }

    ORDER_ITEM {
      uuid order_item_id PK
      uuid order_id FK
      string sku
      string name
      int qty
      decimal unit_price
      decimal line_total
    }

    COURIER {
      uuid courier_id PK
      string full_name
      string phone
      string vehicle_type
      string status
      decimal rating
    }

    ASSIGNMENT {
      uuid assignment_id PK
      uuid order_id FK
      uuid courier_id FK
      timestamp assigned_at
      timestamp accepted_at
      string status
      string dispatch_reason
    }

    ORDER_EVENT {
      uuid event_id PK
      uuid order_id FK
      string event_type
      string actor_role
      string actor_id
      timestamp created_at
      decimal geo_lat
      decimal geo_lon
      json payload
    }

    POD {
      uuid pod_id PK
      uuid order_id FK
      string method
      string code_hash
      string photo_url
      string signed_by
      timestamp confirmed_at
    }

    INCIDENT {
      uuid incident_id PK
      uuid order_id FK
      string category
      string description
      timestamp opened_at
      string owner_role
      string owner_id
      string status
      string resolution
    }

    PAYOUT {
      uuid payout_id PK
      uuid courier_id FK
      timestamp period_from
      timestamp period_to
      decimal amount
      string status
      string external_ref
      json breakdown
    }

    PAYOUT_ORDER {
      uuid payout_id FK
      uuid order_id FK
      decimal amount
    }

    NOTIFICATION {
      uuid notification_id PK
      uuid order_id FK
      string recipient_type
      string recipient
      string channel
      string message
      string delivery_status
      string provider_message_id
    }

    ROUTE_ETA {
      uuid route_id PK
      uuid order_id FK
      json from_geo
      json to_geo
      int distance_meters
      int eta_seconds
      string provider
      timestamp calculated_at
    }
```

## Допущения/гипотезы (для согласования)

- `DELIVERY_ADDRESS.customer_id` задан как связь “адрес принадлежит клиенту”; если адреса не хранятся в профиле, поле можно сделать необязательным.
- Для выплат добавлена развязочная таблица `PAYOUT_ORDER` (связь M:M между `PAYOUT` и `ORDER`), т.к. выплата обычно агрегирует несколько заказов.
- Справочники статусов (`status`, `vehicle_type`, `channel`) представлены как строки/enum (отдельные таблицы можно добавить при необходимости).

