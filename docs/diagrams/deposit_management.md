# UC-FR6.1.5: Управління депозитним рахунком клієнта

## Mermaid діаграма

```mermaid
flowchart TD
    Start([Початок: Клієнт ідентифікований]) --> Operation{Обрати операцію}

    %% Шлях поповнення депозиту
    Operation --> |Поповнення| FindClient1[Знайти клієнта]
    FindClient1 --> InitiateTopup[Ініціювати поповнення депозиту]
    InitiateTopup --> EnterAmount[Ввести суму поповнення]
    EnterAmount --> SelectType{Обрати тип поповнення}

    SelectType --> |Фіскальне| FiscalTopup[Фіскальне поповнення]
    SelectType --> |Нефіскальне| NonFiscalTopup[Нефіскальне поповнення]

    FiscalTopup --> PaymentMethod1{Спосіб оплати}
    NonFiscalTopup --> PaymentMethod1

    PaymentMethod1 --> |Готівка| CashPayment[Прийняти готівку]
    PaymentMethod1 --> |Картка| CardPayment[Оплата карткою]

    CashPayment --> ProcessTopup[Обробити поповнення]
    CardPayment --> ProcessTopup

    ProcessTopup --> UpdateBalance1[Оновити баланс депозиту]
    UpdateBalance1 --> PrintReceipt{Тип чеку}

    PrintReceipt --> |Фіскальний| PrintFiscal[Друк фіскального чеку]
    PrintReceipt --> |Нефіскальний| PrintNonFiscal[Друк нефіскального чеку]

    PrintFiscal --> TopupComplete[Поповнення завершено]
    PrintNonFiscal --> TopupComplete
    TopupComplete --> End([Завершено])

    %% Шлях використання депозиту
    Operation --> |Використання для оплати| PaymentProcess[Процес оплати замовлення]
    PaymentProcess --> SelectDeposit[Обрати метод оплати: Депозит]
    SelectDeposit --> EnterWithdraw[Ввести суму до списання]
    EnterWithdraw --> CheckBalance{Перевірити баланс}

    CheckBalance --> |Достатньо коштів| CheckMinBalance{Перевірити незворотний залишок}
    CheckBalance --> |Недостатньо коштів| PartialPay[Списати доступну суму]

    CheckMinBalance --> |Дозволено| DeductAmount[Списати кошти з депозиту]
    CheckMinBalance --> |Заборонено| AdjustAmount[Скоригувати суму з урахуванням мін. залишку]

    AdjustAmount --> DeductAmount
    PartialPay --> DeductAmount

    DeductAmount --> UpdateBalance2[Оновити баланс депозиту]
    UpdateBalance2 --> ReducePayment[Зменшити суму до оплати]

    PartialPay --> OtherPayment[Запропонувати інший метод для решти]
    OtherPayment --> ReducePayment

    ReducePayment --> PaymentComplete[Оплату проведено]
    PaymentComplete --> End

    style Start fill:#e1f5e1
    style End fill:#ffe1e1
    style FiscalTopup fill:#fff4e1
    style NonFiscalTopup fill:#e1f0ff
    style UpdateBalance1 fill:#e1f5e1
    style UpdateBalance2 fill:#e1f5e1
    style PrintFiscal fill:#ffe1f0
    style PrintNonFiscal fill:#f0e1ff
    style DeductAmount fill:#fff4e1
    style PartialPay fill:#ffe1e1
    style TopupComplete fill:#e1f5e1
    style PaymentComplete fill:#e1f5e1
```

## Опис процесу

Управління депозитним рахунком клієнта, що включає поповнення депозиту (фіскальне/нефіскальне) та використання коштів для оплати послуг і товарів з урахуванням незворотного залишку.

## Основні операції

### 1. Поповнення депозиту
- Вибір типу поповнення (фіскальне/нефіскальне)
- Прийняття оплати (готівка/картка)
- Оновлення балансу
- Друк відповідного чеку

### 2. Використання депозиту для оплати
- Вибір депозиту як методу оплати
- Перевірка балансу та незворотного залишку
- Списання коштів
- Зменшення суми до оплати

## Результат

Баланс депозитного рахунку оновлено. Оплату проведено. Чек надруковано (при поповненні).
