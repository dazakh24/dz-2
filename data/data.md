# Модель предметной области
<!-- Логическая модель, содержащая бизнес-сущности предметной области, атрибуты и связи между ними. 
Подробнее: https://confluence.mts.ru/pages/viewpage.action?pageId=375782602

Используется диаграмма классов UML. Документация: https://plantuml.com/class-diagram 
-->

```plantuml


@startuml
class Reports {
id : string
topic : string
estimation : int
dataCreate : datatime
id: contentManager
}

class Speakers {
id : string
FIO : string
topic : string
dataCreate : datatime
id:Reports
id:contentManager
}

Speakers "1" *-- "many" Reports

class contentManager {
id : string
FIO : string
}

contentManager "1" *-- "many" Reports
contentManager "1" *-- "many" Speakers

class schedule {
id : string
dateOfPerformance : datatime
id:Speakers
id:Reports
}

schedule "1" *-- "1" Speakers
schedule "1" *-- "1" Reports


@enduml

```