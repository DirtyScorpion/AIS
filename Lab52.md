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

    %% Внешний канал
    subgraph INET [Внешний канал связи]
        ISP1[Основной провайдер<br>1 Гбит/с]:::net
        ISP2[Резервный провайдер<br>500 Мбит/с]:::net
    end

    %% Сетевое ядро
    subgraph NETWORK [Сетевое оборудование]
        FW[UserGate C150<br>Аппаратный межсетевой экран]:::sec
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

        subgraph ENV_MONITOR [Мониторинг среды серверной]
            TEMP[Датчик температуры]:::env
            HUM[Датчик влажности воздуха]:::env
            LEAK[Датчик протечки воды]:::env
            SMOKE[Датчик дыма]:::env
            Zabbix[Zabbix Мониторинг]:::env
        end

        subgraph POWER_SYS [Электропитание]
            UPS1[ИБП основной<br>РУБИН NS-20kVA<br>20 кВА / 18 кВт]:::power
            UPS2[ИБП резервный<br>РУБИН NS-10kVA<br>10 кВА / 9 кВт]:::power
            PDU1[PDU управляемый 1]:::power
            PDU2[PDU управляемый 2]:::power
        end
    end

    %% Рабочие станции
    subgraph WS_ART [Арт-отдел и Аниматоры - 40 мест]
        ART_PC[Рабочая станция<br>iRU Опал i9-14900K<br>GPU: NVIDIA RTX 4070 Ti<br>RAM: 64 ГБ DDR5<br>SSD: 2 ТБ NVMe<br>Монитор: 32 дюйма 4K IPS]:::ws
    end

    subgraph WS_DEV [Разработчики и Геймдизайн - 70 мест]
        DEV_PC[Рабочая станция<br>iRU Опал i7-14700K<br>GPU: NVIDIA RTX 4060 Ti<br>RAM: 32 ГБ DDR5<br>SSD: 1 ТБ NVMe<br>Монитор: 27 дюймов 2K IPS]:::ws
    end

    subgraph WS_OFF [Администрация и Продажи - 41 место]
        OFF_PC[Рабочая станция<br>iRU Опал i5-14400<br>GPU: Intel UHD 730<br>RAM: 16 ГБ DDR5<br>SSD: 512 ГБ NVMe<br>Монитор: 24 дюйма FHD IPS]:::ws
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

    %% === СВЯЗИ ===

    %% Интернет -> Firewall
    ISP1 --> FW
    ISP2 --> FW

    %% Firewall -> Ядро сети
    FW --> CORE_SW

    %% Ядро -> Коммутаторы доступа
    CORE_SW -->|10GbE| ACC_SW1
    CORE_SW -->|10GbE| ACC_SW2
    CORE_SW -->|10GbE| ACC_SW3
    CORE_SW -->|10GbE| ACC_SW4

    %% Коммутаторы -> Wi-Fi
    ACC_SW1 -->|PoE| WIFI1
    ACC_SW2 -->|PoE| WIFI2
    ACC_SW3 -->|PoE| WIFI3
    ACC_SW4 -->|PoE| WIFI4

    %% Ядро -> Серверы
    CORE_SW -->|10GbE| SRV1
    CORE_SW -->|10GbE| SRV2
    CORE_SW -->|10GbE| SRV3

    %% Серверы -> СХД
    SRV1 -->|NVMe-oF| NAS1
    SRV2 -->|NVMe-oF| NAS1
    SRV3 -->|iSCSI| NAS2
    NAS1 -->|Репликация| NAS2
    NAS2 -->|Бэкап| BK_SRV

    %% Связи мониторинга среды
    TEMP --> Zabbix
    HUM --> Zabbix
    LEAK --> Zabbix
    SMOKE --> Zabbix
    CORE_SW --> Zabbix
    SRV1 --> Zabbix
    SRV2 --> Zabbix
    SRV3 --> Zabbix

    %% Питание
    UPS1 --> PDU1
    UPS2 --> PDU2
    PDU1 --> SRV1
    PDU1 --> SRV2
    PDU2 --> SRV3
    PDU2 --> NAS1

    %% Коммутаторы -> Рабочие станции
    ACC_SW1 -->|1GbE| ART_PC
    ACC_SW2 -->|1GbE| DEV_PC
    ACC_SW3 -->|1GbE| DEV_PC
    ACC_SW4 -->|1GbE| OFF_PC

    %% Периферия
    ACC_SW4 -->|Сеть| PRINT1
    ACC_SW4 -->|Сеть| PRINT2
    ART_PC --- GRAPH_TAB

    %% Облако
    FW -->|VPN-туннель| YC_VM
    YC_VM --> YC_S3
    YC_VM --> YC_DNS
