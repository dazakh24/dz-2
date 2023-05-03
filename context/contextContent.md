# Контекст решения
<!-- Окружение системы (роли, участники, внешние системы) и связи системы с ним. Диаграмма контекста C4 и текстовое описание. 
Подробнее: https://confluence.mts.ru/pages/viewpage.action?pageId=375783261
-->
```plantuml
@startuml exzample
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml



Person(customer1, "Выступающий")
Person(customer2, "Контент менеджер")


System_Boundary(alias, "label"){
    


Container(webUI, "Web UI", "Web приложение, которое отвечает за конференцию")
Container(webOf, "BackOffice", "Web приложение, которое отвечает за администрирование")
Container(be, "BackEnd", "Приложение, которое отвечает за контент, управление расписанием, администрирование")
Container(email, "Email", "Приложение, которое отвечает за управление контентом")
ContainerDb(db, "DB", "Приложение, которое отвечает за контент, управление расписанием, администрирование")
ContainerQueue(Queue, "Queue", "Приложение, которое отвечает за управление контентом")


Rel(customer1, webUI, "Загрузка докладов")
Rel(customer2, webOf, "Управление контентом")

Rel(webOf, be, "Доклады, логи, трансляции, ретинги, расписание")
Rel(email, be, "Загрузка докладов")
Rel(webOf, Queue, "Работа с докладами")
Rel(Queue, be, "Работа с докладами")
Rel(be, db, "Загрузка докладов")

Rel(webUI, email, "Загрузка доклада")

}


@enduml
```
