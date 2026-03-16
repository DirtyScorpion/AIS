```mermaid
flowchart TD
    %% Определение стилей
    classDef startEnd fill:#c8e6c9,stroke:#2e7d32,stroke-width:2px;
    classDef process fill:#bbdefb,stroke:#1565c0,stroke-width:1px;
    classDef decision fill:#fff9c4,stroke:#fbc02d,stroke-width:1px;
    classDef document fill:#d1c4e9,stroke:#512da8,stroke-width:1px;
    classDef external fill:#ffccbc,stroke:#bf360c,stroke-width:1px;
    classDef system fill:#e1f5fe,stroke:#01579b,stroke-width:1px;

    Start(("Поступление заявки<br>от заказчика")):::startEnd --> LeadCapture

    subgraph Stage1 ["Этап 1: Обработка запроса"]
        LeadCapture["Фиксация заявки"]:::process --> LeadQualify["Первичная оценка"]:::process
        LeadQualify --> IsFit{"Подходит под<br>компетенции студии?"}:::decision
    end

    IsFit -->|Нет| Reject["Отказ заказчику"]:::process
    Reject --> EndReject(("Конец")):::startEnd

    IsFit -->|Да| Agreement

    subgraph Stage2 ["Этап 2: Юридическое оформление"]
        Agreement["Согласование условий, написание ТЗ"]:::process --> Contract["Заключение договора"]:::process
        Contract --> Prepay["Получение предоплаты"]:::process
    end

    Prepay --> Production

    subgraph Stage3 ["Этап 3: Производство"]
        
        subgraph Prod_Internal ["Внутренняя постановка"]
            IP1["Создание задачи в<br>Яндекс Трекере / Eva Projects"]:::process --> IP2["Назначение ответственных:<br>— Арт-директор<br>— Ведущий геймдизайнер"]:::process
            IP2 --> IP3["Декомпозиция ТЗ<br>на спринты"]:::process
        end
        subgraph Prod_Assets ["Создание контента"]

            PA1["Blender / Krita"]:::process --> PA2["ShotGrid<br>(ревью версий)"]:::process
            PA2 --> PA3["GitLab<br>(фиксация артефактов)"]:::process
        end 
        subgraph Prod_Review ["Промежуточный контроль"]
            PR1["Демо заказчику"]:::process --> PR2["Сбор обратной связи"]:::process
            PR2 --> PR3{"Правки<br>в рамках ТЗ?"}:::decision
        end
        
        IP3 --> PA1
        PA3 --> PR1
        PR3 -->|Да| PA1
    end

    PR3 -->|Нет| Acceptance

    subgraph Stage4 ["Этап 4: Приемка и сдача"]
        FinalDemo["Финальная демонстрация<br>заказчику"]:::process --> SignAct["Подписание акта<br>сдачи-приемки"]:::process
        SignAct --> Invoice["Выставление финального счета<br>(1С:Бухгалтерия)"]:::process
    end

    Invoice --> Payment["Получение оплаты"]:::process
    Payment --> Feedback["Сбор отзывов<br>и кейса в портфолио"]:::process
    Feedback --> EndSuccess(("Проект закрыт")):::startEnd

    subgraph Support ["Поддержка"]
        S1["1С:ЗУП"]:::system --> S2["RuBackup<br>(архивация проекта)"]:::system
        S2 --> S3["Yandex Cloud<br>(хранение демо)"]:::system
    end

    Production -.- S1
    Acceptance -.- S2
    EndSuccess -.- S3

    Stage1 -.-> Sys1["<b>Системы:</b> Bitrix24"]:::system
    Stage2 -.-> Sys2["<b>Системы:</b>1С Бухгалтерия "]:::system
    Stage3 -.-> Sys3["<b>Системы:</b> Яндекс Трекер, GitLab, ShotGrid"]:::system
    Stage4 -.-> Sys4["<b>Системы:</b> 1С, Bitrix24"]:::system

    class Start,EndReject,EndSuccess startEnd;
    class LeadCapture,LeadQualify,Agreement,Contract,Prepay,IP1,IP2,IP3,PA1,PA2,PA3,PR1,PR2,FinalDemo,SignAct,Invoice,Payment,Feedback,Reject process;
    class IsFit,PR3 decision;
    class Support system;
```
