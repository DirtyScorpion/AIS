graph TD
    %% Стили
    classDef net fill:#e1f5fe,stroke:#01579b,stroke-width:2px,color:#000
    classDef srv fill:#ffebee,stroke:#b71c1c,stroke-width:2px,color:#000
    classDef ws fill:#e8f5e9,stroke:#1b5e20,stroke-width:2px,color:#000
    classDef stor fill:#fff3e0,stroke:#e65100,stroke-width:2px,color:#000
    classDef sec fill:#fce4ec,stroke:#880e4f,stroke-width:2px,color:#000
    classDef power fill:#f3e5f5,stroke:#4a148c,stroke-width:2px,color:#000
    classDef cloud fill:#e0f7fa,stroke:#006064,stroke-width:2px,color:#000
    classDef periph fill:#f1f8e9,stroke:#33691e,stroke-width:2px,color:#000
    classDef env fill:#fff9c4,stroke:#f57f17,stroke-width:2px,color:#000
    classDef climate fill:#e0f2f1,stroke:#006064,stroke-width:2px,color:#000
    classDef provider fill:#ffe0b2,stroke:#bf360c,stroke-width:2px,color:#000

    %% ==================== ВНЕШНИЕ ПРОВАЙДЕРЫ ====================
    subgraph PROVIDERS ["ПРОВАЙДЕРЫ СВЯЗИ"]
        direction TB
        
        subgraph PROVIDER1 ["Провайдер №1 — Ростелеком"]
            direction TB
            RT_INET[<b>Основной канал</b><br>Ростелеком<br>Технология: GPON / Ethernet<br>Скорость: 1 Гбит/с<br>Магистраль: Северная <br>Точка входа: ул. Сударева, 6<br>Резервирование: 2 независимых волокна]:::provider
            RT_BGP[<b>BGP AS 12389</b><br>Автономная система<br>Пиринг: М9, М10]:::provider
        end
        
        subgraph PROVIDER2 ["Провайдер №2 — МТС / ТрансТелеКом"]
            direction TB
            MT_INET[<b>Резервный канал</b><br>МТС / ТТК<br>Технология: xPON / Ethernet<br>Скорость: 500 Мбит/с<br>Магистраль: Южная <br>Точка входа: ул. Сударева, 6<br>Физически разные кабельные трассы]:::provider
            MT_BGP[<b>BGP AS 15774</b><br>Автономная система<br>Пиринг: М9, М10]:::provider
        end
        
        subgraph BACKUP_CHANNEL ["Резервный канал (3-й уровень)"]
            LTE[<b>4G/LTE резерв</b><br>Yota / Tele2<br>Скорость: до 100 Мбит/с<br>Автоматическое переключение<br>При отказе обоих оптоволоконных каналов]:::provider
        end
    end

    %% Внешнее электропитание
    DGU[Дизель-генератор<br>Автозапуск при отключении сети<br>30 кВт]:::power

    %% Сетевое ядро
    subgraph NETWORK [Сетевое оборудование]
        FW[UserGate C150<br>Аппаратный межсетевой экран<br>Поддержка BGP / ECMP<br>Балансировка нагрузки]:::sec
        CORE_SW[Коммутатор ядра L3<br>Eltex MES3124F<br>24x 10GbE SFP+]:::net
        ACC_SW1[Коммутатор доступа 1<br>Eltex MES2348P<br>48x 1GbE PoE]:::net
        ACC_SW2[Коммутатор доступа 2<br>Eltex MES2348P<br>48x 1GbE PoE]:::net
        ACC_SW3[Коммутатор доступа 3<br>Eltex MES2348P<br>48x 1GbE PoE]:::net
        ACC_SW4[Коммутатор доступа 4<br>Eltex MES2324<br>24x 1GbE]:::net
        WIFI1[Wi-Fi точка доступа 1<br>Eltex WEP-3ax]:::net
        WIFI2[Wi-Fi точка доступа 2<br>Eltex WEP-3ax]:::net
        WIFI3[Wi-Fi точка доступа 3<br>Eltex WEP-3ax]:::net
        WIFI4[Wi-Fi точка доступа 4<br>Eltex WEP-3ax]:::net
    end

    %% Серверная комната
    subgraph SERVER_ROOM [Серверная комната - Стойка 42U]
        subgraph VIRT [Серверы виртуализации - zVirt]
            SRV1[Сервер 1<br>Аквариус T50 D212<br>2x AMD EPYC 9254<br>256 ГБ RAM<br>2x SSD 1.92 ТБ]:::srv
            SRV2[Сервер 2<br>Аквариус T50 D212<br>2x AMD EPYC 9254<br>256 ГБ RAM<br>2x SSD 1.92 ТБ]:::srv
            SRV3[Сервер 3<br>Аквариус T50 D212<br>2x AMD EPYC 9254<br>256 ГБ RAM<br>2x SSD 1.92 ТБ]:::srv
        end

        subgraph STORAGE [Системы хранения данных]
            NAS1[Основная СХД<br>Аэродиск Engine N4<br>12x SSD NVMe 7.68 ТБ<br>Всего: 92 ТБ RAW]:::stor
            NAS2[Архивная СХД<br>Аэродиск Engine N2<br>12x HDD 18 ТБ<br>Всего: 216 ТБ RAW]:::stor
        end

        subgraph BACKUP_SRV [Сервер резервного копирования]
            BK_SRV[Сервер бэкапа<br>Аквариус T40 S212<br>AMD EPYC 9124<br>128 ГБ RAM<br>8x HDD 18 ТБ]:::stor
        end

        subgraph CLIMATE [Климатическое оборудование]
            direction TB
            AC1[<b>Кондиционер 1 </b><br>Ballu BSEI-24HN1<br>Мощность: 7.1 кВт<br>Тип: сплит-система<br>Управление: RS-485<br>Питание: 380В<br>N+1 резервирование]:::climate
            AC2[<b>Кондиционер 2 </b><br>Ballu BSEI-24HN1<br>Мощность: 7.1 кВт<br>Тип: сплит-система<br>Управление: RS-485<br>Питание: 380В<br>Автозапуск при сбое AC1]:::climate
        end

        subgraph ENV_MONITOR [Мониторинг среды серверной]
            direction TB
            TEMP[<b>Датчик температуры</b><br>Модель: Ruvinco RTS-10<br>Диапазон: -40°C … +85°C<br>Точность: ±0.5°C<br>Интерфейс: RS-485 Modbus]:::env
            HUM[<b>Датчик влажности воздуха</b><br>Модель: Ruvinco RHS-20<br>Диапазон: 0…100% RH<br>Точность: ±2% RH<br>Интерфейс: RS-485 Modbus]:::env
            LEAK[<b>Датчик протечки воды</b><br>Модель: Ruvinco RLD-100<br>Тип: позиционный<br>Длина кабеля: 10 м<br>Сработка: 100 кОм ± 5%]:::env
            SMOKE[<b>Датчик дыма</b><br>Модель: Болид ИП 212-141<br>Тип: оптико-электронный<br>Чувствительность: 0.05…0.2 дБ/м<br>Питание: 10…30 В]:::env
            Zabbix[<b>Система мониторинга</b><br>Zabbix 6.4 LTS<br>Сбор данных: SNMP / Modbus / IPMI<br>Оповещения: Telegram / Email / SMS]:::env
        end

        subgraph POWER_SYS [Электропитание]
            UPS1[ИБП основной<br>РУБИН NS-20kVA<br>20 кВА / 18 кВт]:::power
            UPS2[ИБП резервный<br>РУБИН NS-10kVA<br>10 кВА / 9 кВт]:::power
            PDU1[PDU управляемый 1<br>Атлант PDU-32-20A<br>8x IEC C13<br>Управление: SNMP]:::power
            PDU2[PDU управляемый 2<br>Атлант PDU-32-20A<br>8x IEC C13<br>Управление: SNMP]:::power
        end
    end

    %% РАБОЧИЕ СТАНЦИИ
    subgraph WS_ART [Арт-отдел и Аниматоры - 40 мест]
        ART_PC[<b>Рабочая станция iRU Опал</b><br>Процессор: Intel Core i9 / i7<br>GPU: NVIDIA RTX 4070 Ti / 4090<br>RAM: 64-128 ГБ DDR4<br>SSD: 2 ТБ NVMe<br>Монитор: 32 дюйма 4K IPS]:::ws
    end

    subgraph WS_DEV [Разработчики и Геймдизайн - 70 мест]
        DEV_PC[<b>Рабочая станция iRU Опал</b><br>Процессор: Intel Core i7 / i5<br>GPU: NVIDIA RTX 4060 Ti<br>RAM: 32-64 ГБ DDR4<br>SSD: 1 ТБ NVMe<br>Монитор: 27 дюймов 2K IPS]:::ws
    end

    subgraph WS_OFF [Администрация и Продажи - 41 место]
        OFF_PC[<b>Рабочая станция iRU Опал </b><br>Процессор: Intel Core i5 / i3<br>GPU: Встроенная Intel UHD<br>RAM: 16-32 ГБ DDR4<br>SSD: 512 ГБ NVMe<br>Монитор: 24 дюйма FHD IPS]:::ws
    end

    %% Периферия
    subgraph PERIPH [Периферийное оборудование]
        PRINT1[МФУ сетевое<br>Катюша M250<br>А4 цветное]:::periph
        PRINT2[МФУ сетевое<br>Катюша M150<br>А4 ч/б]:::periph
        GRAPH_TAB[Графические планшеты<br>XP-Pen Artist 24 Pro<br>40 шт]:::periph
        HEADSET[Гарнитуры<br>151 шт]:::periph
        WEBCAM[Веб-камеры<br>80 шт]:::periph
    end

    %% Облако
    subgraph CLOUD [Облачные ресурсы - Yandex Cloud]
        YC_VM[Виртуальные машины<br>Compute Cloud]:::cloud
        YC_S3[Объектное хранилище<br>Object Storage]:::cloud
        YC_DNS[Управление DNS<br>Cloud DNS]:::cloud
    end

    %% ==================== СВЯЗИ ====================
    
    %% Интернет — провайдеры к Firewall
    RT_INET -->|1 Гбит/с<br>BGP пиринг| FW
    MT_INET -->|500 Мбит/с<br>BGP пиринг| FW
    LTE -->|4G резерв<br>Автоматическое переключение| FW
    
    %% Питание от генератора
    DGU -->|Резервное питание| UPS1
    DGU -->|Резервное питание| UPS2
    
    %% Firewall к ядру сети
    FW -->|10GbE<br>ECMP балансировка| CORE_SW
    
    %% Ядро к коммутаторам доступа
    CORE_SW -->|10GbE| ACC_SW1
    CORE_SW -->|10GbE| ACC_SW2
    CORE_SW -->|10GbE| ACC_SW3
    CORE_SW -->|10GbE| ACC_SW4
    
    %% Wi-Fi
    ACC_SW1 -->|PoE| WIFI1
    ACC_SW2 -->|PoE| WIFI2
    ACC_SW3 -->|PoE| WIFI3
    ACC_SW4 -->|PoE| WIFI4
    
    %% Серверы
    CORE_SW -->|10GbE| SRV1
    CORE_SW -->|10GbE| SRV2
    CORE_SW -->|10GbE| SRV3
    
    %% Хранилища
    SRV1 -->|NVMe-oF| NAS1
    SRV2 -->|NVMe-oF| NAS1
    SRV3 -->|iSCSI| NAS2
    NAS1 -->|Репликация| NAS2
    NAS2 -->|Бэкап| BK_SRV
    
    %% Мониторинг среды
    TEMP -->|Modbus| Zabbix
    HUM -->|Modbus| Zabbix
    LEAK -->|Дискретный сигнал| Zabbix
    SMOKE -->|RS-485| Zabbix
    
    %% Мониторинг климата
    AC1 -->|RS-485 / Modbus| Zabbix
    AC2 -->|RS-485 / Modbus| Zabbix
    
    %% Мониторинг сети
    FW -->|SNMP / NetFlow| Zabbix
    CORE_SW -->|SNMP / sFlow| Zabbix
    SRV1 -->|IPMI / SNMP| Zabbix
    SRV2 -->|IPMI / SNMP| Zabbix
    SRV3 -->|IPMI / SNMP| Zabbix
    
    %% Мониторинг питания
    UPS1 -->|SNMP| Zabbix
    UPS2 -->|SNMP| Zabbix
    PDU1 -->|SNMP| Zabbix
    PDU2 -->|SNMP| Zabbix
    
    %% Распределение питания
    UPS1 --> PDU1
    UPS2 --> PDU2
    PDU1 --> SRV1
    PDU1 --> SRV2
    PDU1 --> AC1
    PDU2 --> SRV3
    PDU2 --> NAS1
    PDU2 --> AC2
    
    %% Рабочие станции
    ACC_SW1 -->|1GbE| ART_PC
    ACC_SW2 -->|1GbE| DEV_PC
    ACC_SW3 -->|1GbE| DEV_PC
    ACC_SW4 -->|1GbE| OFF_PC
    
    %% Периферия
    ACC_SW4 -->|Сеть| PRINT1
    ACC_SW4 -->|Сеть| PRINT2
    ART_PC --- GRAPH_TAB
    
    %% Облако
    FW -->|VPN-туннель<br>Резервирование по двум каналам| YC_VM
    YC_VM --> YC_S3
    YC_VM --> YC_DNS
