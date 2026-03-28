graph TD
    %% Определение стилей для режимов функционирования
    classDef mode24x7 fill:#ffcccc,stroke:#b30000,stroke-width:2px; 
    classDef mode12x5 fill:#ffebcc,stroke:#e68a00,stroke-width:2px; 
    classDef mode8x5 fill:#ffffcc,stroke:#cccc00,stroke-width:2px; 
    classDef mode10x6 fill:#ccffcc,stroke:#006600,stroke-width:2px;
    classDef modeScheduled fill:#cce5ff,stroke:#004d99,stroke-width:2px; 
    
    %% Уровни инфраструктуры
    subgraph LAN [Рабочие станции: ALT Linux]
        WS[Рабочие станции сотрудников<br/>Режим: 08:00-20:00]:::mode12x5
    end

    subgraph SEC [Безопасность]
        UG[UserGate: Firewall<br/>Режим: 24/7]:::mode24x7
        KS[Kaspersky: Endpoint Security<br/>Режим: 24/7]:::mode24x7
        OVPN[OpenVPN: Удаленный доступ<br/>Режим: 24/7]:::mode24x7
    end

    subgraph MGMT [Управление проектами и документацией]
        Eva[Eva Projects: Баг-трекинг<br/>Режим: 08:00-20:00]:::mode12x5
        Dir[Directum: Документооборот<br/>Режим: 08:00-20:00]:::mode12x5
        Track[Яндекс Трекер: Канбан<br/>Режим: 08:00-20:00]:::mode12x5
        Obs[Obsidian: Документация<br/>Режим: 08:00-20:00]:::mode12x5
    end

    subgraph CREATIVE [Разработка и контент]
        Eng[Nau Engine / Godot: Движки<br/>Режим: 08:00-20:00]:::mode12x5
        Bl[Blender: 3D/Анимация<br/>Режим: 08:00-20:00]:::mode12x5
        GK[GIMP / Krita: 2D-арт<br/>Режим: 08:00-20:00]:::mode12x5
        Au[Audacity: Звук<br/>Режим: 08:00-20:00]:::mode12x5
        SG[ShotGrid: Управление ассетами<br/>Режим: 08:00-20:00]:::mode12x5
    end

    subgraph BIZ [Бизнес и коммуникации]
        Y360[Яндекс 360: Почта/Доки<br/>Режим: 08:00-20:00]:::mode12x5
        B24[Битрикс24: CRM/Коммуникации<br/>Режим: 09:00-19:00]:::mode10x6
        1C_Ac[1С:Бухгалтерия<br/>Режим: 09:00-18:00]:::mode8x5
        1C_HR[1С:ЗУП<br/>Режим: 09:00-18:00]:::mode8x5
        Adv[ADVANTA: Управление портфелем<br/>Режим: 08:00-20:00]:::mode12x5
    end

    subgraph CLOUD_INFRA [Серверы и облако]
        YC[Yandex Cloud<br/>Режим: 24/7]:::mode24x7
        AS[Альт Сервер<br/>Режим: 24/7]:::mode24x7
        Dist[RuStore / VK Play: Дистрибуция<br/>Режим: 24/7]:::mode24x7
    end

    subgraph OPS [DevOps и сборка]
        GF[GitFlic: Репозиторий<br/>Режим: 24/7]:::mode24x7
        GFCI[GitFlic CI: Сборка<br/>Режим: 24/7]:::mode24x7
        TSF[Test Server Farm: Тестирование<br/>Режим: 08:00-20:00]:::mode12x5
        ArtRepo[Artifact Repository: Хранилище сборок<br/>Режим: 24/7]:::mode24x7
        Zab[Zabbix: Мониторинг<br/>Режим: 24/7]:::mode24x7
        Back[RuBackup: Бэкап<br/>Режим: ночь]:::modeScheduled
    end

    %% Связи
    WS <--> UG
    UG <--> SEC
    UG <--> LAN
    LAN <--> MGMT
    LAN <--> CREATIVE
    LAN <--> BIZ
    LAN <--> OPS
    OPS <--> CLOUD_INFRA
    
    %% Легенда режимов
    subgraph LEGEND [Легенда режимов функционирования]
        L1[24/7 - Критичные системы]:::mode24x7
        L2[08:00-20:00 - Основная работа]:::mode12x5
        L3[09:00-19:00 - Продажи]:::mode10x6
        L4[09:00-18:00 - Администрация]:::mode8x5
        L5[По расписанию - Фоновые процессы]:::modeScheduled
    end
