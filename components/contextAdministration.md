# Контекст решения
<!-- Окружение системы (роли, участники, внешние системы) и связи системы с ним. Диаграмма контекста C4 и текстовое описание. 
Подробнее: https://confluence.mts.ru/pages/viewpage.action?pageId=375783261
-->
```plantuml
@startuml exzample
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml



Person(customer4, "Администратор")


System_Boundary(alias, "label"){
    



Container(webOf, "BackOffice", "Web приложение, которое отвечает за администрирование")
Container(be, "BackEnd", "Приложение, которое отвечает за контент, управление расписанием, администрирование")

Container(metr, "Metrics", "Приложение, которое отвечает за  администрирование")
ContainerDb(db, "DB", "Приложение, которое отвечает за контент, управление расписанием, администрирование")



Rel(customer4, webOf, "Управление трансляцией")

Rel(webOf, be, "Доклады, логи, трансляции, ретинги, расписание")


Rel(be, db, "Загрузка докладов")
Rel(metr, db, "Сбор метрик на основе логов")
Rel(webOf, metr, "Сбор метрик на основе логов")


}


@enduml
```
