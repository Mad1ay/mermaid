# UC-FR6.1.6: Управління бонусною системою

## Mermaid діаграма

```mermaid
flowchart TD
    Start([Початок]) --> Flow{Обрати потік}

    %% Потік 1: Автоматичне нарахування бонусів (UC-FR6.1.6.1)
    Flow --> |Нарахування| CheckoutSuccess[Тригер: Успішне закриття чеку]
    CheckoutSuccess --> CheckLoyalty{Клієнт - учасник програми?}
    CheckLoyalty --> |Ні| NoBonus[Бонуси не нараховуються]
    NoBonus --> EndAccrual([Завершено])

    CheckLoyalty --> |Так| FilterItems[Фільтрувати товари-винятки]
    FilterItems --> |Виключити акційні, тютюн тощо| CalcBase[Розрахувати базу нарахування]
    CalcBase --> GetClientLevel[Отримати рівень клієнта]
    GetClientLevel --> CalcBonus[Розрахувати бонуси]
    CalcBonus --> |Сума × % рівня| ActivationDelay{Відкладена активація?}

    ActivationDelay --> |Так| SetPending[Встановити статус: Очікують активації]
    ActivationDelay --> |Ні| SetActive[Встановити статус: Активні]

    SetPending --> |Наприклад, через 14 днів| AccrueBonuses[Нарахувати бонуси на рахунок]
    SetActive --> AccrueBonuses

    AccrueBonuses --> UpdateBalance1[Оновити баланс бонусів]
    UpdateBalance1 --> EndAccrual

    %% Потік 2: Використання бонусів для оплати (UC-FR6.1.6.2)
    Flow --> |Використання| PaymentStart[Процес оплати замовлення]
    PaymentStart --> ClientRequest[Клієнт бажає використати бонуси]
    ClientRequest --> CheckBonusBalance[Перевірити доступний баланс]
    CheckBonusBalance --> DisplayBalance[Показати активні бонуси]
    DisplayBalance --> EnterBonusAmount[Ввести суму бонусів до списання]

    EnterBonusAmount --> ValidateAmount{Перевірити обмеження}

    ValidateAmount --> |Перевірка 1| CheckSufficient{Достатньо бонусів?}
    CheckSufficient --> |Ні| InsufficientError[Помилка: Недостатньо бонусів]
    InsufficientError --> ShowMax[Показати максимальну доступну суму]
    ShowMax --> EnterBonusAmount

    CheckSufficient --> |Так| CheckItemRestrictions{Товари дозволені?}
    CheckItemRestrictions --> |Є заборонені товари| AdjustRestricted[Виключити заборонені товари з бази]
    CheckItemRestrictions --> |Всі дозволені| CheckPercentLimit
    AdjustRestricted --> CheckPercentLimit{Перевірити % від чеку}

    CheckPercentLimit --> |Перевищує ліміт| AutoAdjust[Автоматично скоригувати суму]
    CheckPercentLimit --> |В межах норми| DeductBonuses[Списати бонуси]
    AutoAdjust --> NotifyAdjustment[Повідомити адміністратора]
    NotifyAdjustment --> DeductBonuses

    DeductBonuses --> UpdateBalance2[Оновити баланс бонусів]
    UpdateBalance2 --> ReduceTotal[Зменшити суму до оплати]
    ReduceTotal --> PaymentComplete([Завершено: Бонуси застосовано])

    %% Стилізація
    style Start fill:#e1f5e1
    style EndAccrual fill:#ffe1e1
    style PaymentComplete fill:#ffe1e1
    style CheckoutSuccess fill:#fff4e1
    style FilterItems fill:#e1f0ff
    style CalcBonus fill:#fff4e1
    style AccrueBonuses fill:#e1f5e1
    style SetPending fill:#f0e1ff
    style SetActive fill:#e1f5e1
    style DeductBonuses fill:#fff4e1
    style InsufficientError fill:#ffe1e1
    style AutoAdjust fill:#ffe1f0
    style UpdateBalance1 fill:#e1f5e1
    style UpdateBalance2 fill:#e1f5e1
```

## Опис процесу

Повний цикл управління бонусною програмою лояльності, що включає автоматичне нарахування бонусів за покупки та їх використання для оплати з урахуванням обмежень та правил програми.

## Основні потоки

### 1. Автоматичне нарахування бонусів (UC-FR6.1.6.1)
- Перевірка участі клієнта в програмі
- Фільтрація товарів-винятків
- Розрахунок бонусів за рівнем клієнта
- Нарахування з відкладеною активацією (опціонально)

### 2. Використання бонусів для оплати (UC-FR6.1.6.2)
- Перевірка балансу активних бонусів
- Валідація обмежень (товари, % від чеку)
- Автоматичне коригування суми при порушеннях
- Списання бонусів та зменшення суми до оплати

## Результат

Бонуси нараховані/списані. Баланс бонусів оновлено. Сума до оплати зменшена (при використанні).
