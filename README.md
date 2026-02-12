flowchart TD
    subgraph SOURCES["🎥 FUENTES DE DATOS"]
        S1["🎮 Streamers\nRTMP / WebRTC"]
        S2["👥 Viewers\nChats & Interacciones"]
        S3["📱 Dispositivos\nMóvil / Consolas / Web"]
        S4["🔌 API Clients\nTerceros & Extensiones"]
    end

    subgraph INGESTION["⚡ INGESTA EN TIEMPO REAL"]
        I1["🌐 Ingest Edge Nodes\nRTMP Servers\n~1,000+ nodos globales\n💰 ~$8M–12M/año"]
        I2["💬 WebSocket Servers\nChat & PubSub System"]
        I3["📡 Event Collectors\nHTTP Endpoints\nClick, views, analytics"]
        I4["🔀 Apache Kafka\nMessage Broker\nMillones de eventos/seg\n💰 ~$3M–5M/año"]
    end

    subgraph PROCESSING["⚙️ PROCESAMIENTO & PIPELINES"]
        P1["🎬 Video Transcoding\nFFmpeg + GPU Clusters\n480p / 720p / 1080p / 4K\n💰 ~$15M–20M/año"]
        P2["⚡ Apache Flink\nStream Processing\nMétricas en tiempo real"]
        P3["🔥 Apache Spark\nBatch Processing\nReportes & análisis histórico"]
        P4["🤖 ML Pipeline\nRecomendaciones + Anti-spam\n💰 ~$5M–8M/año"]
        P5["🛡️ Trust & Safety\nDetección de contenido\nAbusos y bans automáticos"]
    end

    subgraph STORAGE["🗄️ ALMACENAMIENTO & BASES DE DATOS"]
        DB1[("🐘 PostgreSQL\nDatos de usuarios\nMetadata de canales")]
        DB2[("⚡ Redis Cluster\nSesiones & Cache\nLeaderboards en vivo\n💰 ~$2M/año")]
        DB3[("☁️ Amazon S3\nVODs, clips, assets\nPB de almacenamiento\n💰 ~$25M–35M/año")]
        DB4[("📊 Vertica / Druid\nAnalytics Warehouse\nConsultas OLAP")]
        DB5[("🔍 Elasticsearch\nSearch, logs\ny auditoría")]
        DB6[("🌍 CockroachDB\nSQL Distribuido\nAlta disponibilidad global")]
    end

    subgraph DELIVERY["🌐 ENTREGA DE CONTENIDO"]
        D1["🚀 CDN Global\nAkamai + AWS CloudFront\n💰 ~$100M–150M/año\n(Costo dominante)"]
        D2["📺 HLS / DASH\nAdaptive Bitrate\nLatencia ultra-baja ~3s"]
        D3["🔗 API Gateway\nREST & GraphQL\nAutenticación OAuth2"]
    end

    subgraph OBSERVABILITY["📊 OBSERVABILIDAD & MONITORING"]
        M1["📈 Prometheus + Grafana\nMétricas de infraestructura"]
        M2["🐕 Datadog\nAPM & Alertas\n💰 ~$5M–7M/año"]
        M3["🔭 Jaeger\nDistributed Tracing"]
    end

    S1 -->|"Stream RTMP"| I1
    S2 -->|"WS / HTTP"| I2
    S2 -->|"Eventos"| I3
    S3 -->|"Multi-protocolo"| I1
    S3 -->|"Interacciones"| I2
    S4 -->|"REST / EventSub"| I3

    I1 -->|"Video chunks"| I4
    I2 -->|"Mensajes"| I4
    I3 -->|"Eventos analítica"| I4

    I4 -->|"Video stream"| P1
    I4 -->|"Stream events"| P2
    I4 -->|"User events"| P4
    I4 -->|"Reportes"| P3

    P1 -->|"VODs y segmentos"| DB3
    P2 -->|"Métricas tiempo real"| DB4
    P3 -->|"Datos históricos"| DB4
    P4 -->|"Modelos & scores"| DB2
    P5 -->|"Logs de moderación"| DB5
    P2 -->|"Sesiones activas"| DB2

    DB3 -->|"Contenido de video"| D1
    DB1 -->|"Perfil & datos"| D3
    DB2 -->|"Cache de respuestas"| D3
    DB6 -->|"Consistencia global"| D3
    P1 -->|"Segmentos HLS"| D2
    D2 -->|"Manifiestos"| D1

    PROCESSING -.->|"métricas"| M1
    DELIVERY -.->|"métricas"| M2
    INGESTION -.->|"trazas"| M3

    classDef sources fill:#9b59b6,color:#fff,stroke:#7d3c98
    classDef ingestion fill:#2980b9,color:#fff,stroke:#1a5276
    classDef processing fill:#27ae60,color:#fff,stroke:#1e8449
    classDef storage fill:#e67e22,color:#fff,stroke:#ca6f1e
    classDef delivery fill:#e74c3c,color:#fff,stroke:#c0392b
    classDef monitoring fill:#7f8c8d,color:#fff,stroke:#566573

    class S1,S2,S3,S4 sources
    class I1,I2,I3,I4 ingestion
    class P1,P2,P3,P4,P5 processing
    class DB1,DB2,DB3,DB4,DB5,DB6 storage
    class D1,D2,D3 delivery
    class M1,M2,M3 monitoring
