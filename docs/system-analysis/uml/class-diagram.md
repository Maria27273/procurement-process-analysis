# Class Diagram

```mermaid
classDiagram

    class User {
        - int id
        - string name
        - string email
        - string passwordHash
        - datetime createdAt
        + createRequest()
        + editRequest()
        + viewRequests()
    }

    class Role {
        - int id
        - string name
    }

    class Department {
        - int id
        - string name
    }

    class Request {
        - int id
        - string description
        - decimal amount
        - string justification
        - string status
        - datetime createdAt
        - datetime updatedAt
        - datetime completedAt
        + sendForApproval()
        + changeStatus()
        + getCurrentStep()
    }

    class PurchaseType {
        - int id
        - string name
        - string description
    }

    class Document {
        - int id
        - string name
        - string fileUrl
        - int size
        - string format
        - datetime uploadedAt
        + download()
        + preview()
    }

    class RequestHistory {
        - int id
        - string action
        - string oldStatus
        - string newStatus
        - string comment
        - datetime changedAt
    }

    class Route {
        - int id
        - string name
        - bool isActive
    }

    class ApprovalStep {
        - int id
        - int stepOrder
        - string approverRole
        - int deadlineHours
        - string status
    }

    class ApprovalDecision {
        - int id
        - string decision
        - string reason
        - string comment
        - datetime decidedAt
        + approve()
        + reject()
        + returnForRevision()
    }

    class Notification {
        - int id
        - string type
        - string message
        - bool isRead
        - datetime sentAt
        - datetime readAt
        + send()
        + markAsRead()
    }

    User "*" --> "1" Role : имеет
    User "*" --> "1" Department : работает в

    User "1" --> "*" Request : создаёт

    Request "*" --> "1" PurchaseType : имеет тип
    Request "1" --> "*" Document : содержит
    Request "1" --> "*" RequestHistory : имеет историю
    Request "*" --> "1" Route : использует маршрут
    Request "*" --> "1" Route

    PurchaseType "1" --> "*" Route : определяет маршрут
    Route "1" --> "*" ApprovalStep : состоит из

    ApprovalStep "1" --> "*" ApprovalDecision : содержит
    ApprovalDecision "*" --> "1" User : принято пользователем

    RequestHistory "*" --> "1" User : действие выполнил

    Request "1" --> "*" Notification : вызывает
    User "1" --> "*" Notification : получает
```

---

## Описание основных классов

### User

Пользователь системы. Может создавать и редактировать собственные заявки, а также просматривать доступные ему заявки.

### Role

Роль пользователя в системе, определяющая доступные ему действия.

### Department

Подразделение, к которому относится пользователь.

### Request

Основная сущность системы — закупочная заявка. Содержит информацию о закупке, её текущем статусе и датах обработки.

### PurchaseType

Тип закупки. Используется для определения маршрута согласования.

В проекте предусмотрены следующие типы:

- Простая;
- IT;
- Юридическая.

### Document

Документ, прикреплённый к закупочной заявке.

### RequestHistory

История действий с заявкой. Сохраняет информацию об изменениях статуса, выполненном действии, пользователе, комментарии и времени изменения.

### Route

Маршрут согласования, который определяется типом закупки и состоит из последовательности этапов.

### ApprovalStep

Отдельный этап маршрута согласования. Определяет порядок этапа, роль согласующего и установленный срок обработки.

### ApprovalDecision

Решение, принятое согласующим на конкретном этапе. Возможные решения: согласование, отклонение или возврат на доработку.

### Notification

Уведомление пользователя о событиях, связанных с заявкой.

---

