# WP-FR6.9.2: Робочий процес планування записів

## Mermaid діаграма

```mermaid
flowchart TD
    Start([Початок: Потреба в бронюванні]) --> InitBooking[Запит на бронювання]
    InitBooking --> Actor{Хто ініціює?}
    Actor --> |Клієнт| ClientSelect[Клієнт обирає послугу]
    Actor --> |Адміністратор| AdminSelect[Адміністратор обирає послугу]
    ClientSelect --> SelectService[Вибір послуги, дати та часу]
    AdminSelect --> SelectService
    SelectService --> CheckAvailability{Перевірка доступності ресурсів}
    CheckAvailability --> |UC-FR6.6.2| Available{Ресурси доступні?}
    Available --> |Так| CreateBooking[Створення запису в шахматці]
    Available --> |Ні| SelectService
    CreateBooking --> |UC-FR6.6.1, UC-FR6.6.3| BookingConfirmed[Бронювання підтверджено]
    BookingConfirmed --> WaitArrival[Очікування прибуття клієнта]
    WaitArrival --> ClientArrives[Клієнт прибуває]
    ClientArrives --> |UC-FR6.7.1| AccessControl[СКД фіксує вхід]
    AccessControl --> UpdateStatus[Оновлення статусу візиту]
    UpdateStatus --> |UC-FR6.6.8| StatusUpdated[Статус: Клієнт прибув]
    StatusUpdated --> End([Завершено: Запис активовано])

    style Start fill:#e1f5e1
    style End fill:#ffe1e1
    style InitBooking fill:#fff4e1
    style SelectService fill:#fff4e1
    style CheckAvailability fill:#e1f0ff
    style CreateBooking fill:#fff4e1
    style BookingConfirmed fill:#e1f5e1
    style WaitArrival fill:#f0f0f0
    style ClientArrives fill:#fff4e1
    style AccessControl fill:#e1f0ff
    style UpdateStatus fill:#fff4e1
    style StatusUpdated fill:#e1f5e1
```

## Опис процесу

Процес створення та управління попереднім записом клієнта на послуги оздоровчого комплексу. Включає бронювання, перевірку доступності ресурсів та автоматичне оновлення статусу при прибутті клієнта.

## Основні кроки

1. **Запит на бронювання** - Клієнт або Адміністратор ініціює процес бронювання
2. **Вибір послуги та часу** - Визначення бажаної послуги, дати та часу відвідування
3. **Перевірка доступності** - Система перевіряє наявність вільних ресурсів
4. **Створення запису** - Система створює бронювання в "шахматці"
5. **Прибуття клієнта** - СКД (Система Контролю Доступу) фіксує вхід
6. **Оновлення статусу** - Статус візиту змінюється на "Клієнт прибув"

## Результат

Бронювання створено, відображено в системі, статус візиту автоматично оновлюється при прибутті клієнта.
