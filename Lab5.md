
graph TD
    %% Определение стилей
    classDef business fill:#e1f5fe,stroke:#01579b,stroke-width:2px;
    classDef critical fill:#ffebee,stroke:#b71c1c,stroke-width:2px;
    classDef high fill:#fff3e0,stroke:#e65100,stroke-width:2px;
    classDef medium fill:#f3e5f5,stroke:#4a148c,stroke-width:2px;
    classDef low fill:#e8f5e8,stroke:#1b5e20,stroke-width:2px;
    classDef infrastructure fill:#e8eaf6,stroke:#283593,stroke-width:1px;

    %% Бизнес-процессы (верхний уровень)
    subgraph Business_Processes ["БИЗНЕС-ПРОЦЕССЫ"]
        direction TB
        BP1["Разработка игр"]:::business
        BP2["Аутсорсинг цифрового контента"]:::business
        BP3["Издание и техподдержка игр"]:::business
        BP4["Управление и финансы"]:::business
    end

    %% Уровень приложений (ИС)
    subgraph Application_Layer ["ПРИКЛАДНОЕ ПО"]
        direction TB
        
        subgraph Prod_Systems ["Производственные системы"]
            direction LR
            A1["GitFlic<br>Контроль версий"]:::critical
            A2["Nau Engine / Godot<br>Игровые движки"]:::high
            A3["Blender / Krita / Audacity<br>Создание контента"]:::high
            A4["ShotGrid<br>Управление ассетами"]:::high
        end
        
        subgraph Mgmt_Systems ["Системы управления"]
            direction LR
            B1["Яндекс Трекер<br>Управление задачами"]:::critical
            B2["Eva Projects / Directum<br>Портфели проектов"]:::high
            B3["Obsidian<br>База знаний (GDD)"]:::medium
            B4["ADVANTA<br>Стратег. планирование"]:::medium
        end
        
        subgraph Business_Systems ["Бизнес-системы"]
            direction LR
            C1["1С:Бухгалтерия + ЗУП"]:::critical
            C2["Bitrix24<br>CRM и коммуникации"]:::high
            C3["Яндекс 360<br>Почта, документы"]:::critical
        end
        
        subgraph Distribution ["Системы дистрибуции"]
            D1["RuStore / VK Play"]:::high
            D2["Yandex Cloud"]:::high
        end
    end

    %% Уровень инфраструктуры
    subgraph Infrastructure_Layer ["ИНФРАСТРУКТУРА"]
        direction TB
        
        subgraph Compute ["Вычисления"]
            I1["Серверы виртуализации<br>(Astra Linux)"]:::infrastructure
            I2["Рабочие станции<br>(Alt Linux)"]:::infrastructure
        end
        
        subgraph Storage ["Хранение"]
            I3["СХД (SAN/NAS)<br>для артов и кода"]:::infrastructure
            I4["RuBackup<br>Резервное копирование"]:::critical
        end
        
        subgraph Network ["Сеть и безопасность"]
            I5["UserGate / Kaspersky"]:::critical
            I6["OpenVPN<br>Удаленный доступ"]:::high
            I7["Zabbix<br>Мониторинг"]:::high
        end
    end

    %% Связи между уровнями
    Business_Processes --> Application_Layer
    Application_Layer --> Infrastructure_Layer

    %% Режимы функционирования (легенда)
    subgraph Legend ["РЕЖИМЫ ФУНКЦИОНИРОВАНИЯ"]
        L1["Критический (24/7, отказоустойчивость, SLA 99.9%)"]:::critical
        L2["Высокий (Рабочее время, допустим простой до 4ч)"]:::high
        L3["Средний (Терпим простой до 24ч)"]:::medium
        L4["Низкий (Без жестких требований)"]:::low
    end

    %% Применение классов
    class A1,B1,C1,C3,I4,I5 critical;
    class A2,A3,A4,B2,C2,D1,D2,I6,I7 high;
    class B3,B4 medium;
    class I1,I2,I3 infrastructure;
