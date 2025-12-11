# WP-FR6.9.1: Робочий процес виконання замовлення

## Mermaid діаграма

```mermaid
flowchart TD
    Start([Початок: Клієнт готовий зробити замовлення]) --> CreateOrder[Створення замовлення]
    CreateOrder --> |UC-FR6.3.1| PrintKitchen[Друк на кухню/бар]
    PrintKitchen --> |UC-FR6.3.15| ApplyLoyalty{Застосувати програму лояльності?}
    ApplyLoyalty --> |Так| ProcessLoyalty[Застосування знижок та бонусів]
    ApplyLoyalty --> |Ні| ProcessPayment[Обробка оплати]
    ProcessLoyalty --> |UC-FR6.3.2| ProcessPayment
    ProcessPayment --> |UC-FR6.3.3| Fiscalization[Фіскалізація через РРО]
    Fiscalization --> |UC-FR6.3.5| CloseVisit[Закриття візиту]
    CloseVisit --> |UC-FR6.3.7| StockWriteOff[Списання запасів]
    StockWriteOff --> |UC-FR6.4.8| LinkLegalEntity[Прив'язка до юр. особи]
    LinkLegalEntity --> |UC-FR6.3.14| Integration1C[Інтеграція з 1С]
    Integration1C --> End([Завершено: Замовлення виконано])

    style Start fill:#e1f5e1
    style End fill:#ffe1e1
    style CreateOrder fill:#fff4e1
    style PrintKitchen fill:#fff4e1
    style ProcessLoyalty fill:#e1f0ff
    style ProcessPayment fill:#fff4e1
    style Fiscalization fill:#ffe1f0
    style CloseVisit fill:#fff4e1
    style StockWriteOff fill:#e1f0ff
    style LinkLegalEntity fill:#e1f0ff
    style Integration1C fill:#f0e1ff
```

## Опис процесу

Повний наскрізний процес обробки замовлення клієнта від створення до закриття візиту та інтеграції з 1С.

## Основні кроки

1. **Створення замовлення** - Адміністратор рецепції створює замовлення в POS
2. **Друк на кухню/бар** - Система надсилає замовлення на принтери
3. **Застосування лояльності** - Система/Адміністратор застосовує знижки та бонуси
4. **Оплата** - Обробка платежу різними методами
5. **Фіскалізація** - Друк фіскального чека через РРО
6. **Закриття візиту** - Система закриває візит
7. **Списання запасів** - Автоматичне списання використаних запасів
8. **Прив'язка до юр. особи** - Транзакція прив'язується до правильної юр. особи
9. **Інтеграція з 1С** - Передача даних в 1С

## Результат

Замовлення повністю виконано, оплачено, фіскалізовано; запаси списані.
