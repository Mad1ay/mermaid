# UC-FR6.1.1: Управління профілем клієнта

## Mermaid діаграма

```mermaid
flowchart TD
    Start([Початок: Актор авторизований]) --> Action{Обрати дію}

    Action --> |Створення нового| CreateClient[Ініціювати створення клієнта]
    Action --> |Пошук існуючого| SearchClient[Ініціювати пошук клієнта]

    %% Шлях пошуку
    SearchClient --> EnterCriteria[Ввести критерії пошуку]
    EnterCriteria --> |ПІБ/телефон/номер картки| Search[Система шукає профілі]
    Search --> Found{Клієнт знайдений?}
    Found --> |Ні| NotFound[Система повідомляє: не знайдено]
    NotFound --> Action
    Found --> |Так| DisplayResults[Відобразити знайдені профілі]
    DisplayResults --> SelectProfile[Обрати профіль]
    SelectProfile --> ViewOrEdit{Обрати операцію}

    %% Перегляд
    ViewOrEdit --> |Перегляд| ViewMode[Режим тільки для читання]
    ViewMode --> DisplayData[Відобразити дані профілю]
    DisplayData --> End([Завершено])

    %% Редагування
    ViewOrEdit --> |Редагування| EditMode[Режим редагування]
    EditMode --> ModifyFields[Внести зміни у дозволені поля]
    ModifyFields --> ValidateEdit[Перевірити унікальність даних]
    ValidateEdit --> DuplicateCheck{Дані унікальні?}
    DuplicateCheck --> |Ні| DuplicateError[Помилка: Дублікат картки/телефону]
    DuplicateError --> EditMode
    DuplicateCheck --> |Так| SaveChanges[Зберегти зміни]
    SaveChanges --> UpdateDB[Оновити профіль у БД]
    UpdateDB --> End

    %% Шлях створення
    CreateClient --> FillForm[Заповнити форму даних клієнта]
    FillForm --> |ПІБ, контакти, дата народження| ValidateData[Перевірити формат даних]
    ValidateData --> FormatValid{Дані коректні?}
    FormatValid --> |Ні| FormatError[Помилка: Неправильний формат]
    FormatError --> |Email/телефон| FillForm
    FormatValid --> |Так| CheckUnique[Перевірити унікальність]
    CheckUnique --> UniqueCheck{Картка/телефон унікальні?}
    UniqueCheck --> |Ні| UniqueError[Система блокує: Дублікат]
    UniqueError --> FillForm
    UniqueCheck --> |Так| CreateProfile[Створити профіль у БД]
    CreateProfile --> End

    style Start fill:#e1f5e1
    style End fill:#ffe1e1
    style CreateClient fill:#fff4e1
    style SearchClient fill:#fff4e1
    style DisplayResults fill:#e1f0ff
    style ViewMode fill:#e1f5e1
    style EditMode fill:#fff4e1
    style SaveChanges fill:#e1f0ff
    style CreateProfile fill:#e1f0ff
    style FormatError fill:#ffe1e1
    style DuplicateError fill:#ffe1e1
    style UniqueError fill:#ffe1e1
    style NotFound fill:#ffe1e1
```

## Опис процесу

Управління профілями клієнтів, що включає створення нових профілів, пошук існуючих, перегляд та редагування даних клієнтів з перевіркою унікальності та валідацією даних.

## Основні операції

1. **Створення нового клієнта** - Заповнення форми з персональними даними та валідацією
2. **Пошук клієнта** - Швидкий пошук за ПІБ, номером телефону або картки
3. **Перегляд профілю** - Відображення даних у режимі "тільки для читання"
4. **Редагування профілю** - Внесення змін з перевіркою унікальності

## Результат

Профіль клієнта успішно створено або оновлено в системі з дотриманням унікальності та коректності даних.
