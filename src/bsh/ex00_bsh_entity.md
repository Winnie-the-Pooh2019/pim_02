# Exercise 00 — Entity detection (Выявление сущностей)

Задача: 1 — Sign up for a haircut (prefix: `bsh`)

Ниже — кандидаты в сущности предметной области, с которыми работает система онлайн‑записи. Для каждой сущности указаны назначение и ключевые атрибуты.

| Сущность | Назначение | Ключевые атрибуты (примеры) |
|---|---|---|
| Филиал (BarbershopBranch) | Точка оказания услуг (где проходит запись) | `branchId`, `name`, `address`, `timezone`, `contacts`, `isActive` |
| Клиент (UserAccount) | Зарегистрированный пользователь системы | `userId`, `phone/email`, `fullName`, `createdAt`, `consents`, `preferredChannel`, `status` |
| Гость (GuestContact) | Контакт для записи без аккаунта | `guestId`, `phone`, `name`, `verificationStatus`, `createdAt` |
| Услуга (Service) | Справочник услуг и их параметров | `serviceId`, `name`, `category`, `durationMin`, `price`, `isActive` |
| Мастер (Master) | Исполнитель услуги | `masterId`, `fullName`, `branchId`, `skills/serviceIds`, `isActive` |
| Слот расписания (ScheduleSlot) | Интервал доступности мастера для записи | `slotId`, `masterId`, `startAt`, `endAt`, `status` (free/booked/blocked), `blockReason`, `updatedAt` |
| Запись (Appointment) | Факт бронирования слота под услугу | `appointmentId`, `clientRef` (userId/guestId), `serviceId`, `masterId`, `branchId`, `startAt/endAt`, `status`, `createdAt`, `comment` |
| Напоминание/уведомление (Notification) | Сообщение клиенту/мастеру о статусе/визите | `notificationId`, `appointmentId`, `recipient`, `channel`, `templateId`, `sendAt`, `deliveryStatus`, `providerMessageId` |
| Оплата (Payment) | Фиксация оплаты/начислений по записи (онлайн/офлайн) | `paymentId`, `appointmentId`, `amount`, `currency`, `method`, `status`, `transactionRef`, `paidAt` |
| Отзыв/оценка (Review) | Обратная связь после оказания услуги | `reviewId`, `appointmentId`, `rating`, `comment`, `createdAt`, `published` |

