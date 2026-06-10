# Архитектура production-системы

[Вернуться к мастер-документу](../docs/MLSD_Master_Document.md)

```mermaid
flowchart LR
    SN[Social Network Backend] -->|comments| K[Kafka]
    K --> P[Preprocessing Service]
    P --> I[ML Inference Service]
    I --> D{Moderation Decision}
    D -->|safe| SN
    D -->|review or block| UI[Moderation UI]
    UI --> PG[(PostgreSQL)]
    I --> PG
    P --> S3[(S3 Data Lake)]
    I --> CH[(ClickHouse)]
    CH --> DM[(Feature Store and Data Mart)]
    UI --> K

    MR[MLflow Model Registry] --> I
    S3 --> T[Training Pipeline]
    PG --> T
    DM --> T
    T --> MR

    P --> M[Monitoring]
    I --> M
    K --> M
    PG --> M
    M --> CH
```

Разделение online-контура, аналитического хранения и контура обучения позволяет независимо масштабировать обработку комментариев, аналитику и переобучение.
