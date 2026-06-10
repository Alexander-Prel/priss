# Архитектура production-системы

[Вернуться к мастер-документу](../docs/MLSD_Master_Document.md)

```mermaid
flowchart TB
    subgraph ONLINE[Online-контур модерации]
        direction LR
        SN[Social Network<br/>Backend] --> K[Kafka]
        K --> P[Preprocessing<br/>Service]
        P --> I[ML Inference<br/>Service]
        I --> D{Moderation<br/>Decision}
        D -->|safe| PUB[Публикация]
        D -->|review or block| UI[Moderation<br/>UI]
    end

    subgraph DATA[Контур хранения]
        direction LR
        PG[(PostgreSQL)]
        S3[(S3 Data Lake)]
        CH[(ClickHouse)]
        DM[(Feature Store<br/>and Data Mart)]
        CH --> DM
    end

    subgraph MLOPS[Контур обучения]
        direction LR
        T[Training<br/>Pipeline] --> MR[MLflow<br/>Model Registry]
    end

    M[Monitoring<br/>Service]

    UI --> PG
    I --> PG
    P --> S3
    I --> CH
    S3 --> T
    PG --> T
    DM --> T
    MR --> I
    ONLINE -. metrics .-> M
    DATA -. metrics .-> M
    M --> CH
```

Разделение online-контура, аналитического хранения и контура обучения позволяет независимо масштабировать обработку комментариев, аналитику и переобучение.
