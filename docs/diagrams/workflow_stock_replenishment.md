# WP-FR6.9.3: Робочий процес поповнення запасів

## Mermaid діаграма

```mermaid
flowchart TD
    Start([Початок: Моніторинг запасів]) --> LowStock{Низькі залишки?}
    LowStock --> |Так| Notification[Система генерує сповіщення]
    LowStock --> |Ні| Monitor[Продовження моніторингу]
    Monitor --> LowStock
    Notification --> |UC-FR6.4.11| ReviewAlert[Товарознавець переглядає сповіщення]
    ReviewAlert --> AnalyzeNeeds[Аналіз потреби в поповненні]
    AnalyzeNeeds --> CreatePO[Створення Замовлення на закупівлю]
    CreatePO --> SendToSupplier[Відправка замовлення постачальнику]
    SendToSupplier --> WaitDelivery[Очікування доставки]
    WaitDelivery --> Delivery[Постачальник доставляє товари]
    Delivery --> Receiving[Приймання товарів товарознавцем]
    Receiving --> ScannerCheck{Використання сканера?}
    ScannerCheck --> |Так| ScanItems[Сканування штрих-кодів]
    ScannerCheck --> |Ні| ManualEntry[Ручне введення]
    ScanItems --> |UC-FR6.4.13| CreateReceipt[Створення документа Надходження]
    ManualEntry --> CreateReceipt
    CreateReceipt --> |UC-FR6.4.2| UpdateStock[Оновлення залишків]
    UpdateStock --> RecalculateCost[Перерахунок собівартості]
    RecalculateCost --> StockUpdated[Запаси оновлені в системі]
    StockUpdated --> End([Завершено: Запаси поповнені])

    style Start fill:#e1f5e1
    style End fill:#ffe1e1
    style Notification fill:#ffe1e1
    style ReviewAlert fill:#fff4e1
    style AnalyzeNeeds fill:#e1f0ff
    style CreatePO fill:#fff4e1
    style SendToSupplier fill:#e1f0ff
    style WaitDelivery fill:#f0f0f0
    style Delivery fill:#fff4e1
    style Receiving fill:#fff4e1
    style CreateReceipt fill:#fff4e1
    style UpdateStock fill:#e1f0ff
    style RecalculateCost fill:#e1f0ff
    style StockUpdated fill:#e1f5e1
```

## Опис процесу

Повний цикл поповнення запасів від автоматичного виявлення низьких залишків до оприбуткування товару та оновлення собівартості в системі.

## Основні кроки

1. **Виявлення потреби** - Система автоматично генерує сповіщення про низькі залишки
2. **Створення замовлення** - Товарознавець створює Замовлення на закупівлю постачальнику
3. **Доставка** - Постачальник доставляє товари
4. **Приймання** - Товарознавець приймає товари, можливо зі сканером
5. **Створення Надходження** - Оформлення документа надходження товарів
6. **Оновлення системи** - Автоматичне оновлення залишків та перерахунок собівартості

## Результат

Запаси поповнені, залишки та собівартість оновлені в системі.
