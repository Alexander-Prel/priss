# UML-диаграмма компонентов

[Вернуться к мастер-документу](../docs/MLSD_Master_Document.md)

```mermaid
flowchart TB
    subgraph ONLINE[Online-компоненты]
        direction LR
        ING[Ingestion] --> PRE[Preprocessing]
        PRE --> INF[Inference]
        INF --> MQ[Moderation Queue]
        MQ --> HR[Human Review]
    end

    subgraph LEARNING[Контур улучшения модели]
        direction LR
        FB[Feedback] --> TR[Training]
    end

    MON[Monitoring]

    HR --> FB
    TR --> INF
    ONLINE -.-> MON
    LEARNING -.-> MON
```

| Компонент | Ответственность |
|---|---|
| Ingestion Component | Прием комментариев и публикация событий |
| Preprocessing Component | Нормализация, дедупликация и определение языка |
| Inference Component | Классификация и расчет confidence score |
| Moderation Queue Component | Приоритизация спорных и опасных комментариев |
| Human Review Component | Рабочее место модератора и фиксация решения |
| Feedback Component | Сбор исправлений и оценка качества |
| Monitoring Component | Технические и ML-метрики, алерты |
| Training Component | Подготовка выборки, обучение и регистрация модели |
