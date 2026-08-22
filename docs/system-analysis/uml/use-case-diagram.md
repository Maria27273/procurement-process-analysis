# Диаграмма вариантов использования

```mermaid
graph LR

    Employee["Сотрудник"]
    Manager["Руководитель"]
    IT["IT-специалист"]
    Finance["Финансовый специалист"]
    Legal["Юрист"]
    Purchasing["Сотрудник отдела закупок"]
    Admin["Администратор"]

    subgraph System["Система управления закупочными заявками"]

        UC1["UC-01. Создать заявку"]
        UC2["UC-02. Отправить заявку на согласование"]
        UC3["UC-03. Редактировать заявку"]
        UC4["UC-04. Принять решение по заявке"]
        UC5["UC-05. Настроить маршрут согласования"]
        UC6["UC-06. Добавить нестандартный этап"]
        UC7["UC-07. Искать и просматривать заявки"]
        UC8["UC-08. Принять заявку в отдел закупок"]
        UC9["UC-09. Просмотреть историю заявки"]
        UC10["UC-10. Получать уведомления"]
        UC11["UC-11. Просматривать и скачивать документы"]

    end

    Employee --> UC1
    Employee --> UC2
    Employee --> UC3
    Employee --> UC7
    Employee --> UC9
    Employee --> UC10
    Employee --> UC11

    Manager --> UC4
    Manager --> UC7
    Manager --> UC9
    Manager --> UC10
    Manager --> UC11

    IT --> UC4
    IT --> UC7
    IT --> UC9
    IT --> UC10
    IT --> UC11

    Finance --> UC4
    Finance --> UC7
    Finance --> UC9
    Finance --> UC10
    Finance --> UC11

    Legal --> UC4
    Legal --> UC7
    Legal --> UC9
    Legal --> UC10
    Legal --> UC11

    Purchasing --> UC8
    Purchasing --> UC7
    Purchasing --> UC9
    Purchasing --> UC10
    Purchasing --> UC11

    Admin --> UC5
    Admin --> UC6
```
