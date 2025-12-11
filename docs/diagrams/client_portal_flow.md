# UC-FR6.1.10: Онлайн-особистий кабінет клієнта

## Mermaid діаграма

```mermaid
flowchart TD
    Start([Початок: Клієнт відкриває портал]) --> HasAccount{Має обліковий запис?}

    %% Реєстрація нового користувача
    HasAccount --> |Ні| Registration[Реєстрація]
    Registration --> FillRegForm[Заповнити форму реєстрації]
    FillRegForm --> |Ім'я, телефон/email, пароль| SendCode[Система надсилає код підтвердження]
    SendCode --> |SMS/Email| EnterCode[Ввести код підтвердження]
    EnterCode --> ValidateCode{Код вірний?}
    ValidateCode --> |Ні| CodeError[Помилка: Невірний код]
    CodeError --> EnterCode
    ValidateCode --> |Так| CreateProfile[Система створює профіль]
    CreateProfile --> LoginSuccess[Успішна авторизація]

    %% Авторизація існуючого користувача
    HasAccount --> |Так| Login[Авторизація]
    Login --> EnterCredentials[Ввести логін та пароль]
    EnterCredentials --> |Телефон/Email + Пароль| ValidateLogin{Дані вірні?}
    ValidateLogin --> |Ні| LoginError[Помилка: Невірний пароль]
    LoginError --> ForgotPassword{Відновити пароль?}
    ForgotPassword --> |Так| PasswordRecovery[Процедура відновлення]
    ForgotPassword --> |Ні| EnterCredentials
    PasswordRecovery --> EnterCredentials
    ValidateLogin --> |Так| LoginSuccess

    %% Використання кабінету
    LoginSuccess --> Dashboard[Головна сторінка кабінету]
    Dashboard --> SelectSection{Обрати розділ}

    %% Розділ: Профіль
    SelectSection --> |Профіль| ViewProfile[Переглянути персональні дані]
    ViewProfile --> EditProfile{Редагувати?}
    EditProfile --> |Так| ModifyProfile[Внести зміни]
    ModifyProfile --> VerifyChange{Зміна телефону?}
    VerifyChange --> |Так| SMSVerification[SMS-верифікація]
    VerifyChange --> |Ні| SaveProfile[Зберегти зміни]
    SMSVerification --> SaveProfile
    SaveProfile --> ProfileUpdated[Профіль оновлено]
    ProfileUpdated --> Dashboard
    EditProfile --> |Ні| Dashboard

    %% Розділ: Історія
    SelectSection --> |Історія| ViewHistory[Переглянути історію візитів]
    ViewHistory --> DisplayVisits[Відобразити список візитів та покупок]
    DisplayVisits --> Dashboard

    %% Розділ: Баланси
    SelectSection --> |Баланси| ViewBalances[Переглянути баланси]
    ViewBalances --> DisplayDeposit[Баланс депозиту]
    ViewBalances --> DisplayBonuses[Баланси бонусів]
    DisplayBonuses --> ShowActive[Активні бонуси]
    DisplayBonuses --> ShowPending[Неактивні бонуси]
    DisplayDeposit --> Dashboard
    ShowActive --> Dashboard
    ShowPending --> Dashboard

    %% Розділ: Бронювання
    SelectSection --> |Бронювання| ViewBookings[Переглянути бронювання]
    ViewBookings --> DisplayFuture[Майбутні візити]
    ViewBookings --> DisplayPast[Минулі візити]
    DisplayFuture --> Dashboard
    DisplayPast --> Dashboard

    %% Розділ: Фіскальні чеки
    SelectSection --> |Чеки| ViewReceipts[Переглянути фіскальні чеки]
    ViewReceipts --> SelectReceipt[Обрати чек]
    SelectReceipt --> DownloadReceipt[Завантажити чек]
    DownloadReceipt --> Dashboard

    %% Вихід
    SelectSection --> |Вийти| Logout[Вийти з кабінету]
    Logout --> End([Завершено])

    %% Стилізація
    style Start fill:#e1f5e1
    style End fill:#ffe1e1
    style LoginSuccess fill:#e1f5e1
    style Dashboard fill:#fff4e1
    style CreateProfile fill:#e1f0ff
    style ProfileUpdated fill:#e1f5e1
    style LoginError fill:#ffe1e1
    style CodeError fill:#ffe1e1
    style ViewProfile fill:#e1f0ff
    style ViewHistory fill:#e1f0ff
    style ViewBalances fill:#e1f0ff
    style ViewBookings fill:#e1f0ff
    style ViewReceipts fill:#e1f0ff
```

## Опис процесу

Онлайн-особистий кабінет надає клієнтам самостійний доступ до їх профілю, історії візитів, балансів депозиту та бонусів, бронювань та фіскальних чеків через веб-сайт або мобільний додаток.

## Основні функції

### Реєстрація та авторизація
- Реєстрація з верифікацією через SMS/Email
- Авторизація за логіном та паролем
- Відновлення пароля

### Розділи кабінету

1. **Профіль** - Перегляд та редагування персональних даних
2. **Історія** - Перегляд історії візитів та покупок
3. **Баланси** - Перевірка депозиту та бонусів
4. **Бронювання** - Перегляд майбутніх та минулих візитів
5. **Чеки** - Перегляд та завантаження фіскальних чеків

## Результат

Клієнт отримав доступ до інформації. Профіль оновлено (у разі редагування).
