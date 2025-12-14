# Exercise 02 — Building a data dictionary (Построение словаря данных)

Задача: 2 — Delivery of orders (prefix: `del`)

Словарь данных построен по сущностям из `src/del/ex00_del_entity.md`.  
Формат: **Сущность**, **мнемоника поля**, **описание**, **тип**, **обязательность**.

Обозначения типов: `uuid`, `string`, `int`, `decimal`, `bool`, `timestamp`, `enum(...)`, `json`.

## Поставщик (Supplier) — `SUPPLIER`

| Сущность | Мнемоника | Описание | Тип | Обяз. |
|---|---|---|---|:---:|
| SUPPLIER | supplier_id | Идентификатор поставщика | uuid | + |
| SUPPLIER | name | Название поставщика | string | + |
| SUPPLIER | contacts | Контакты поставщика | json | - |
| SUPPLIER | integration_type | Тип интеграции/канал приёма | enum(manual,api,file,email) | + |
| SUPPLIER | is_active | Активность поставщика | bool | + |

## Точка выдачи/получения (PickupPoint) — `PICKUP_POINT`

| Сущность | Мнемоника | Описание | Тип | Обяз. |
|---|---|---|---|:---:|
| PICKUP_POINT | pickup_point_id | Идентификатор точки выдачи | uuid | + |
| PICKUP_POINT | supplier_id | Поставщик | uuid | + |
| PICKUP_POINT | address | Адрес точки выдачи | string | + |
| PICKUP_POINT | geo_lat | Широта | decimal | - |
| PICKUP_POINT | geo_lon | Долгота | decimal | - |
| PICKUP_POINT | working_hours | Режим работы | json | - |
| PICKUP_POINT | instructions | Инструкции для курьера | string | - |

## Клиент (Customer) — `CUSTOMER`

| Сущность | Мнемоника | Описание | Тип | Обяз. |
|---|---|---|---|:---:|
| CUSTOMER | customer_id | Идентификатор клиента | uuid | + |
| CUSTOMER | full_name | Имя/ФИО | string | - |
| CUSTOMER | phone | Телефон | string | + |
| CUSTOMER | notification_prefs | Настройки уведомлений | json | - |
| CUSTOMER | created_at | Дата создания | timestamp | + |

## Адрес доставки (DeliveryAddress) — `ADDRESS`

| Сущность | Мнемоника | Описание | Тип | Обяз. |
|---|---|---|---|:---:|
| ADDRESS | address_id | Идентификатор адреса | uuid | + |
| ADDRESS | raw_address | Адрес “как ввёл источник” | string | + |
| ADDRESS | normalized_address | Нормализованный адрес | string | - |
| ADDRESS | geo_lat | Широта | decimal | - |
| ADDRESS | geo_lon | Долгота | decimal | - |
| ADDRESS | entrance | Подъезд | string | - |
| ADDRESS | floor | Этаж | string | - |
| ADDRESS | apartment | Квартира/офис | string | - |
| ADDRESS | comment | Комментарий к доставке | string | - |

## Заказ (Order) — `ORDER`

| Сущность | Мнемоника | Описание | Тип | Обяз. |
|---|---|---|---|:---:|
| ORDER | order_id | Идентификатор заказа | uuid | + |
| ORDER | supplier_id | Поставщик | uuid | + |
| ORDER | customer_id | Клиент | uuid | + |
| ORDER | pickup_point_id | Точка выдачи | uuid | + |
| ORDER | delivery_address_id | Адрес доставки | uuid | + |
| ORDER | delivery_window_from | Начало окна доставки | timestamp | - |
| ORDER | delivery_window_to | Конец окна доставки | timestamp | - |
| ORDER | ready_by | Срок готовности заказа | timestamp | - |
| ORDER | status | Статус заказа | enum(created,accepted,ready,assigned,picked_up,in_transit,delivered,failed,cancelled) | + |
| ORDER | created_at | Дата/время создания | timestamp | + |
| ORDER | special_instructions | Доп. инструкции | string | - |

## Позиция заказа (OrderItem) — `ORDER_ITEM`

| Сущность | Мнемоника | Описание | Тип | Обяз. |
|---|---|---|---|:---:|
| ORDER_ITEM | order_item_id | Идентификатор позиции | uuid | + |
| ORDER_ITEM | order_id | Заказ | uuid | + |
| ORDER_ITEM | sku | Артикул/SKU | string | - |
| ORDER_ITEM | name | Наименование | string | + |
| ORDER_ITEM | qty | Количество | int | + |
| ORDER_ITEM | unit_price | Цена за единицу | decimal | - |
| ORDER_ITEM | line_total | Сумма по строке | decimal | - |

## Курьер (Courier) — `COURIER`

| Сущность | Мнемоника | Описание | Тип | Обяз. |
|---|---|---|---|:---:|
| COURIER | courier_id | Идентификатор курьера | uuid | + |
| COURIER | full_name | ФИО | string | - |
| COURIER | phone | Телефон | string | + |
| COURIER | vehicle_type | Тип транспорта | enum(walk,bike,car,other) | + |
| COURIER | status | Статус курьера | enum(active,inactive,blocked) | + |
| COURIER | rating | Рейтинг | decimal | - |

## Назначение/бронь (OrderAssignment) — `ASSIGNMENT`

| Сущность | Мнемоника | Описание | Тип | Обяз. |
|---|---|---|---|:---:|
| ASSIGNMENT | assignment_id | Идентификатор назначения | uuid | + |
| ASSIGNMENT | order_id | Заказ | uuid | + |
| ASSIGNMENT | courier_id | Курьер | uuid | + |
| ASSIGNMENT | assigned_at | Время назначения | timestamp | + |
| ASSIGNMENT | accepted_at | Время принятия курьером | timestamp | - |
| ASSIGNMENT | status | Статус назначения | enum(assigned,accepted,rejected,cancelled,completed) | + |
| ASSIGNMENT | dispatch_reason | Причина назначения/переназначения | string | - |

## Событие статуса (OrderStatusEvent) — `ORDER_EVENT`

| Сущность | Мнемоника | Описание | Тип | Обяз. |
|---|---|---|---|:---:|
| ORDER_EVENT | event_id | Идентификатор события | uuid | + |
| ORDER_EVENT | order_id | Заказ | uuid | + |
| ORDER_EVENT | event_type | Тип события/статуса | string | + |
| ORDER_EVENT | actor_role | Роль инициатора | enum(operator,courier,dispatcher,system,customer,supplier,support,admin) | + |
| ORDER_EVENT | actor_id | Идентификатор инициатора (если применимо) | string | - |
| ORDER_EVENT | created_at | Время события | timestamp | + |
| ORDER_EVENT | geo_lat | Координата (если применимо) | decimal | - |
| ORDER_EVENT | geo_lon | Координата (если применимо) | decimal | - |
| ORDER_EVENT | payload | Доп. данные события | json | - |

## POD (ProofOfDelivery) — `POD`

| Сущность | Мнемоника | Описание | Тип | Обяз. |
|---|---|---|---|:---:|
| POD | pod_id | Идентификатор POD | uuid | + |
| POD | order_id | Заказ | uuid | + |
| POD | method | Метод подтверждения | enum(code,photo,sign) | + |
| POD | code_hash | Хэш кода (если method=code) | string | - |
| POD | photo_url | Ссылка на фото (если method=photo) | string | - |
| POD | signed_by | Подписал (если method=sign) | string | - |
| POD | confirmed_at | Время подтверждения | timestamp | + |

## Инцидент (Incident) — `INCIDENT`

| Сущность | Мнемоника | Описание | Тип | Обяз. |
|---|---|---|---|:---:|
| INCIDENT | incident_id | Идентификатор инцидента | uuid | + |
| INCIDENT | order_id | Заказ | uuid | + |
| INCIDENT | category | Категория | enum(late,customer_unreachable,address_issue,damage,cancelled,other) | + |
| INCIDENT | description | Описание | string | - |
| INCIDENT | opened_at | Время открытия | timestamp | + |
| INCIDENT | owner_role | Кто обрабатывает | enum(dispatcher,support,operator) | + |
| INCIDENT | owner_id | Идентификатор обработчика | string | - |
| INCIDENT | status | Статус | enum(open,in_progress,resolved,rejected) | + |
| INCIDENT | resolution | Итог/решение | string | - |

## Начисление/выплата (CourierPayout) — `PAYOUT`

| Сущность | Мнемоника | Описание | Тип | Обяз. |
|---|---|---|---|:---:|
| PAYOUT | payout_id | Идентификатор выплаты | uuid | + |
| PAYOUT | courier_id | Курьер | uuid | + |
| PAYOUT | period_from | Начало периода | timestamp | + |
| PAYOUT | period_to | Конец периода | timestamp | + |
| PAYOUT | amount | Сумма к выплате | decimal | + |
| PAYOUT | status | Статус выплаты | enum(planned,paid,held,failed) | + |
| PAYOUT | external_ref | Ссылка на выплату в бух. системе | string | - |
| PAYOUT | breakdown | Детализация (заказы/штрафы/доплаты) | json | - |

## Уведомление (Notification) — `NOTIF`

| Сущность | Мнемоника | Описание | Тип | Обяз. |
|---|---|---|---|:---:|
| NOTIF | notification_id | Идентификатор уведомления | uuid | + |
| NOTIF | order_id | Заказ | uuid | + |
| NOTIF | recipient_type | Тип получателя | enum(customer,courier) | + |
| NOTIF | recipient | Адресат (телефон/email/deviceToken/мессенджер) | string | + |
| NOTIF | channel | Канал | enum(sms,push,email,telegram,whatsapp,vk) | + |
| NOTIF | message | Текст/параметры сообщения | string | + |
| NOTIF | delivery_status | Статус доставки | enum(planned,sent,delivered,failed) | + |
| NOTIF | provider_message_id | ID провайдера | string | - |

## Маршрут/ETA (RouteETA) — `ROUTE`

| Сущность | Мнемоника | Описание | Тип | Обяз. |
|---|---|---|---|:---:|
| ROUTE | route_id | Идентификатор расчёта | uuid | + |
| ROUTE | order_id | Заказ | uuid | + |
| ROUTE | from_geo | Координаты отправления | json | + |
| ROUTE | to_geo | Координаты назначения | json | + |
| ROUTE | distance_meters | Дистанция (м) | int | - |
| ROUTE | eta_seconds | ETA (сек.) | int | - |
| ROUTE | provider | Провайдер геосервиса | string | + |
| ROUTE | calculated_at | Время расчёта | timestamp | + |

