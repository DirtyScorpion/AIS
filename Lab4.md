
flowchart TD

    classDef startEnd fill:#c8e6c9,stroke:#2e7d32,stroke-width:2px;
    classDef process fill:#bbdefb,stroke:#1565c0,stroke-width:1px;
    classDef decision fill:#fff9c4,stroke:#fbc02d,stroke-width:1px;
    classDef document fill:#d1c4e9,stroke:#512da8,stroke-width:1px;
    classDef subprocess fill:#ffe0b2,stroke:#e65100,stroke-width:1px;
    classDef external fill:#e1f5fe,stroke:#01579b,stroke-width:1px;

    Start(("Начало<br>Идея игры")):::startEnd --> Idea

    subgraph PreProduction ["Этап 1: Концепция"]
        Idea["Концепция игры<br>Жанр, сюжет, механики"]:::process --> GDD["Написание GDD<br>(Game Design Document)"]:::document
        GDD --> Pitch["Представление команде / инвесторам"]:::process
        Pitch --> Decision{"Утверждение<br>концепции?"}:::decision
        Decision -->|Да| ConceptArt["Создание<br>концепт-артов"]:::process
        Decision -->|Нет| Idea
        ConceptArt --> Prototype["Создание<br>игрового прототипа"]:::process
    end

    Prototype --> Production

    subgraph Production ["Этап 2: Производство"]
        subgraph Prod_Art ["Арт-производство"]
            Art1["3D-моделирование<br>Blender"]:::process --> Art2["Анимация"]:::process
            Art2 --> Art3["Текстурная<br>запечка"]:::process
        end
        
        subgraph Prod_Code ["Программирование"]
            Code1["Разработка механик"]:::process --> Code2["Интеграция арта<br>и анимаций"]:::process
            Code2 --> Code3["UI/UX программирование"]:::process
        end
        
        subgraph Prod_Sound ["Звук"]
            Sound1["Запись / синтез звуков"]:::process --> Sound2["Музыкальное<br>оформление"]:::process
        end
        
        subgraph Prod_Level ["Левел-дизайн"]
            Level1["Сборка уровней<br>из ассетов"]:::process --> Level2["Настройка освещения<br>и пост-эффектов"]:::process
        end
        
        Art3 --> Code2
        Sound2 --> Level2
        Code3 --> Level2
        Level2 --> Build["Сборка билда"]:::process
    end


    Build --> QA

    subgraph QA_Phase ["Этап 3: Тестирование "]
        Testing["Функциональное тестирование<br>QA-инженеры"]:::process --> BugTrack["Заведение багов<br>в трекер"]:::process
        BugTrack --> BugFix["Исправление ошибок<br>Dev + Art"]:::process
        BugFix --> Retest["Повторное тестирование"]:::process
        Retest --> Balance["Балансировка геймплея"]:::process
        Balance --> ReleaseCandidate["Формирование<br>релиз-кандидата"]:::process
    end

    ReleaseCandidate --> Release

    subgraph Release_Phase ["Этап 4: Релиз"]
        Platform["Загрузка на платформы<br>RuStore, VK Play"]:::process --> Marketing["Маркетинг и продвижение"]:::process
        Marketing --> Sales["Продажи и монетизация"]:::process
        Sales --> Support["Техническая поддержка<br>и обновления"]:::process
        Support --> Analytics["Сбор аналитики<br>и отзывов"]:::process
    end


    Analytics --> Idea
    
    subgraph Supporting_Processes ["Сквозные процессы "]
        HR["HR и подбор<br>персонала"]:::subprocess
        Finance["Финансы и бухгалтерия"]:::subprocess
        IT["IT-инфраструктура"]:::subprocess
        SalesDept["Продажи"]:::subprocess
        Merch["Создание мерча"]:::subprocess
    end


    Supporting_Processes -.- Production
    Supporting_Processes -.- QA_Phase
    Supporting_Processes -.- Release_Phase

    Analytics --> End(("Прибыль /<br>Новый цикл")):::startEnd
