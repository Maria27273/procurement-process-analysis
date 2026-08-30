## Диаграмма классов

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
        - string description
        + getPermissions()
    }

    class Department {
        - int id
        - string name
        - string code
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
        + changeStatus(newStatus)
        + getCurrentStep()
    }

    class PurchaseType {
        - int id
        - string name
        - string description
        + getRoutes()
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
        + createHistoryRecord()
        + getHistoryByRequest()
    }

    class Route {
        - int id
        - string name
        - bool isActive
        - datetime createdAt
        + getSteps()
        + addStep(role, deadline)
        + removeStep(stepId)
    }

    class ApprovalStep {
        - int id
        - int stepOrder
        - string approverRole
        - int deadlineHours
        - string status
        - datetime startedAt
        - datetime completedAt
        + startStep()
        + completeStep(decision)
        + getRemainingTime()
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
    Request "*" --> "1" Route : следует по маршруту

    PurchaseType "1" --> "*" Route : определяет маршрут
    Route "1" --> "*" ApprovalStep : состоит из

    ApprovalStep "1" --> "1" User : назначен
    ApprovalStep "1" --> "*" ApprovalDecision : содержит решения

    ApprovalDecision "*" --> "1" User : принято пользователем
    ApprovalDecision "1" --> "1" RequestHistory : записано как событие

    RequestHistory "*" --> "1" User : действие выполнил

    Request "1" --> "*" Notification : генерирует
    User "1" --> "*" Notification : получает
