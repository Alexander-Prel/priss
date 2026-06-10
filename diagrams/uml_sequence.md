# UML-диаграмма последовательности

[Вернуться к мастер-документу](../docs/MLSD_Master_Document.md)

```mermaid
sequenceDiagram
    actor User
    participant SN as Social Network
    participant ING as Ingestion
    participant PRE as Preprocessing
    participant ML as ML Inference
    participant DEC as Moderation Decision
    participant UI as Moderator UI
    participant ST as Storage
    participant MON as Monitoring

    User->>SN: Отправляет комментарий
    SN->>ING: Передает событие
    ING->>PRE: Публикует комментарий
    PRE->>ML: Передает нормализованный текст
    ML->>DEC: Возвращает класс и confidence score
    alt Низкий риск
        DEC->>SN: Разрешает публикацию
    else Средний или высокий риск
        DEC->>UI: Создает задачу проверки
        UI->>DEC: Возвращает решение модератора
        DEC->>SN: Публикует, скрывает или блокирует
    end
    DEC->>ST: Сохраняет решение и признаки
    DEC->>MON: Отправляет технические и ML-метрики
    ST->>MON: Передает агрегаты качества
```

