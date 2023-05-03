# Контекст решения
<!-- Окружение системы (роли, участники, внешние системы) и связи системы с ним. Диаграмма контекста C4 и текстовое описание. 
Подробнее: https://confluence.mts.ru/pages/viewpage.action?pageId=375783261
-->
```plantuml
@startuml exzample
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml


Person(customer3, "Фасилитатор/Администратор/Руководитель проекта")


System_Boundary(alias, "label"){
    



Container(webOf, "BackOffice", "Web приложение, которое отвечает за администрирование")
Container(be, "BackEnd", "Приложение, которое отвечает за контент, управление расписанием, администрирование")
ContainerDb(db, "DB", "Приложение, которое отвечает за контент, управление расписанием, администрирование")

Rel(customer3, webOf, "Управление расписанием, обратная связь")
Rel(webOf, be, "Доклады, логи, трансляции, ретинги, расписание")
Rel(be, db, "Загрузка докладов")



}


@enduml
```
