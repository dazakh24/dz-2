# Контекст решения
<!-- Окружение системы (роли, участники, внешние системы) и связи системы с ним. Диаграмма контекста C4 и текстовое описание. 
Подробнее: https://confluence.mts.ru/pages/viewpage.action?pageId=375783261
-->
```plantuml
@startuml exzample
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml


Person(customer0, "Партнер/Слушатель/Выступающий")


System_Boundary(alias, "label"){
    


Container(webUI, "Web UI", "Web приложение, которое отвечает за конференцию")

Container(be, "BackEnd", "Приложение, которое отвечает за контент, управление расписанием, администрирование")
ContainerDb(db, "DB", "Приложение, которое отвечает за контент, управление расписанием, администрирование")
Rel(customer0, webUI, "Вход, регистрация,трансляции")

Rel(webUI, be, "Вход, регистрация, трансляция")
Rel(be, db, "Загрузка докладов")

}


@enduml
```
