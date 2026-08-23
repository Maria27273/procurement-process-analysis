# Use Case Diagram

@startuml

left to right direction
skinparam packageStyle rectangle
skinparam actorStyle awesome
skinparam usecaseBorderColor #2C3E50
skinparam usecaseBackgroundColor #ECF0F1

actor "Сотрудник" as Employee
actor "Руководитель" as Manager
actor "Администратор" as Admin
actor "Отдел закупок" as Purchasing

rectangle "Система управления закупочными заявками" {
  

  usecase "UC-01 Создать заявку" as UC1
  usecase "UC-02 Редактировать заявку" as UC2
  usecase "UC-03 Отправить на согласование" as UC3
  

  usecase "UC-04 Принять решение по заявке" as UC4
  usecase "UC-05 Оставить комментарий" as UC5
  

  usecase "UC-06 Настроить маршрут согласования" as UC6
  usecase "UC-07 Добавить нестандартный этап" as UC7
  

  usecase "UC-08 Принять заявку в отдел закупок" as UC8
  

  usecase "UC-09 Искать и просматривать заявки" as UC9
  usecase "UC-10 Просмотреть историю заявки" as UC10
  usecase "UC-11 Просматривать и скачивать документы" as UC11
}


Employee --> UC1
Employee --> UC2
Employee --> UC3
Employee --> UC9
Employee --> UC10
Employee --> UC11


Manager --> UC4
Manager --> UC5
Manager --> UC9
Manager --> UC10
Manager --> UC11


Admin --> UC6
Admin --> UC7
Admin --> UC9
Admin --> UC10


Purchasing --> UC8
Purchasing --> UC9
Purchasing --> UC10
Purchasing --> UC11


UC4 ..> UC10 : <<include>>

@enduml

