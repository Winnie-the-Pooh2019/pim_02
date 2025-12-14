# Exercise 00 — Entity detection (Выявление сущностей)

Задача: 2 — Delivery of orders (prefix: `del`)

Ниже — кандидаты в сущности предметной области, с которыми работает платформа доставки. Для каждой сущности указаны назначение и ключевые атрибуты.

| Сущность | Назначение | Ключевые атрибуты (примеры) |
|---|---|---|
| Поставщик (Supplier) | Источник заказов (магазин/кафе) | `supplierId`, `name`, `pickupPoints`, `contacts`, `integrationType`, `isActive` |
| Точка выдачи/получения (PickupPoint) | Место, где курьер забирает заказ | `pickupPointId`, `supplierId`, `address`, `geoLat/geoLon`, `workingHours`, `instructions` |
| Клиент (Customer) | Получатель заказа | `customerId`, `fullName`, `phone`, `notificationPrefs`, `createdAt` |
| Адрес доставки (DeliveryAddress) | Адрес/геоданные точки доставки | `addressId`, `rawAddress`, `normalizedAddress`, `geoLat/geoLon`, `entrance`, `floor`, `apartment`, `comment` |
| Заказ (Order) | Центральная сущность жизненного цикла доставки | `orderId`, `supplierId`, `customerId`, `pickupPointId`, `deliveryAddressId`, `deliveryWindowFrom/To`, `readyBy`, `status`, `createdAt`, `specialInstructions` |
| Позиция заказа (OrderItem) | Состав заказа (товары/позиции) | `orderItemId`, `orderId`, `sku`, `name`, `qty`, `unitPrice`, `lineTotal` |
| Курьер (Courier) | Исполнитель доставки (моб. приложение) | `courierId`, `fullName`, `phone`, `vehicleType`, `status`, `rating` |
| Назначение/бронь (OrderAssignment) | Связь “заказ ↔ курьер” и факт назначения | `assignmentId`, `orderId`, `courierId`, `assignedAt`, `acceptedAt`, `status`, `dispatchReason` |
| Событие статуса (OrderStatusEvent) | Таймлайн изменений заказа и действий ролей | `eventId`, `orderId`, `eventType`, `actorRole/actorId`, `createdAt`, `geoLat/geoLon`, `payload` |
| POD (ProofOfDelivery) | Подтверждение факта доставки | `podId`, `orderId`, `method` (code/photo/sign), `codeHash`, `photoUrl`, `signedBy`, `confirmedAt` |
| Инцидент (Incident) | Отклонение от сценария (опоздание/отказ/проблема) | `incidentId`, `orderId`, `category`, `description`, `openedAt`, `ownerRole/ownerId`, `status`, `resolution` |
| Начисление/выплата (CourierPayout) | Расчёт выплат курьеру за период | `payoutId`, `courierId`, `periodFrom/To`, `amount`, `status`, `externalRef`, `breakdown` |
| Уведомление (Notification) | Уведомления клиенту/курьеру и статусы доставки | `notificationId`, `orderId`, `recipientType`, `recipient`, `channel`, `message`, `deliveryStatus`, `providerMessageId` |
| Маршрут/ETA (RouteETA) | Результат расчёта маршрута и ETA (внешний геосервис) | `routeId`, `orderId`, `fromGeo`, `toGeo`, `distanceMeters`, `etaSeconds`, `provider`, `calculatedAt` |

