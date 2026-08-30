# Sequence Diagram

## Логика построения

Для диаграммы выбран сценарий **«Отправка заявки на согласование»**, так как он отражает один из основных процессов системы.


## Диаграмма последовательности

```mermaid
sequenceDiagram
    actor Employee as Сотрудник
    participant System as Система
    participant Notification as Сервис уведомлений
    actor Manager as Руководитель

    Employee->>System: Нажимает «Отправить на согласование»

    System->>System: Проверяет обязательные поля

    alt Обязательные поля не заполнены
        System-->>Employee: Показывает ошибки заполнения
        Employee->>System: Исправляет данные
    else Заявка заполнена корректно

        System->>System: Определяет тип закупки
        System->>System: Определяет маршрут согласования
        System->>System: Определяет первого согласующего

        System->>System: Создаёт этап согласования
        System->>System: Изменяет статус на «На проверке»

        System->>Notification: Передаёт данные для уведомления
        Notification->>Manager: Отправляет уведомление о новой заявке

        System-->>Employee: «Заявка отправлена на согласование»
    end
```

---

