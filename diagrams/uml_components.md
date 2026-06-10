# UML-диаграмма компонентов

[Вернуться к мастер-документу](../docs/MLSD_Master_Document.md)

```mermaid
flowchart LR
    ING[Ingestion Component] --> PRE[Preprocessing Component]
    PRE --> INF[Inference Component]
    INF --> MQ[Moderation Queue Component]
    MQ --> HR[Human Review Component]
    HR --> FB[Feedback Component]
    FB --> TR[Training Component]
    TR --> INF

    ING --> MON[Monitoring Component]
    PRE --> MON
    INF --> MON
    MQ --> MON
    HR --> MON
    FB --> MON
    TR --> MON
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

