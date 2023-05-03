# Описание требований и архитектуры

## Введение
<!-- Общее краткое описание создаваемой системы -->
В рамках курса осуществляется проектирование решения на основе [постановки задачи от "заказчика"](../../task.md).

- [Описание требований и архитектуры](#описание-требований-и-архитектуры)
  - [Введение](#введение)
  - [Заинтересованные стороны](#заинтересованные-стороны)
  - [Бизнес-контекст (бизнес-требования)](#бизнес-контекст-бизнес-требования)
  - [Глоссарий](#глоссарий)
  - [Модель предметной области](#модель-предметной-области)
  - [Требования к системе](#требования-к-системе)
    - [Сценарии использования (Use case)](#сценарии-использования-use-case)
    - [Функциональные требования](#функциональные-требования)
    - [Нефункциональные требования/Требования к атрибутам качества](#нефункциональные-требованиятребования-к-атрибутам-качества)
    - [Ограничения](#ограничения)
  - [Архитектура](#архитектура)
    - [Журнал архитектурных решений](#журнал-архитектурных-решений)
  
## Заинтересованные стороны
<!-- Перечень заинтересованных сторон и их интересов по отношению к создаваемой системе. 
Подробнее: https://confluence.mts.ru/pages/viewpage.action?pageId=399975538 
-->
| Заинтересованная сторона | Интересы           |
|:-------------------------|:-------------------|
| Выступающий               | Заявить о своем продукте |
| Контент менеджер               | Модерация контента в рамках планируемой конференции |
| Администратор              | Технический контроль конференции |
| Фасилитатор              | Управление расписанием, сбор обратной связи |
| Партнер               | Просмотр конференции, получение ответов на интересующие вопросы |
| Слушатель               | Просмотр конференции, получение ответов на интересующие вопросы |
| Руководитель проекта               | Общий контроль |

## Бизнес-контекст (бизнес-требования)
<!-- Общее описание бизнес-контекста создаваемой системы (автоматизируемой деятельности), список бизнес-целей заинтересованных сторон 
Подробнее: https://confluence.mts.ru/pages/viewpage.action?pageId=399973845
-->
| Бизнес-требования | 
|:-------------------------|
| BR-INV-01: Для увеличения стоимости бренда, необходимо регулярно привлекать новых партнеров и высококвалифицированных сотрудников с помощью конференций|
| BR-INV-02: Тема конференций должна определяться исходя из текущих рыночных трендов и внутренних продуктов |

## Глоссарий
<!-- Содержит основные понятия и термины предметной области  
Подробнее: https://confluence.mts.ru/pages/viewpage.action?pageId=375782595
-->
| Понятие                        | Сокращение                         | Определение                       |
|:-------------------------------|:-----------------------------------|:----------------------------------|
| Веб-страница | Web UI | Предоставляет возможность ознакомиться с конференцией, отправить заявку, зарегистрироваться и входить в ЛК, смотреть трансляцию |
| Админка | BackOffice | Ознакомление с докладами, создание расписания, контроль трансляции, создание альтернативных каналов трансляции, просмотр метрик |
| Почтовый сервер | Email | Принимает материалы от выступающих |
| Очередь | Email | Работа с докладами |
| Метрики | Metrics | Сбор метрик на основе логов |
| Приложение | BackEnd | Управление расписанием, пользователями, трансляцией, метриками |
| База данных | DB | Хранение данных |

## [Модель предметной области](data/data.md)

## Требования к системе

### Сценарии использования (Use case)
<!-- Подробное описание сценариев использования системы с привязкой к ролям участников и задействованным бизнес-сущностям 
https://confluence.mts.ru/pages/viewpage.action?pageId=375782108 
https://confluence.mts.ru/pages/viewpage.action?pageId=375782119 
-->
#### Диаграмма сценариев использования (Use Case Diagram) <!-- omit in toc -->

```plantuml
@startuml exzample
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml


Person(customer0, "Партнер/Слушатель")
Person(customer1, "Выступающий")
Person(customer2, "Контент менеджер")
Person(customer3, "Фасилитатор")
Person(customer4, "Администратор")
Person(customer5, "Руководитель проекта")

System_Boundary(alias, "label"){
    


Container(webUI, "Web UI", "React", "Web приложение")
Container(webOf, "BackOffice", "React", "Web приложение")
Container(be, "BackEnd", "Java", "BE приложение")
Container(email, "Email", "Mail server", "Почтовый ящик")
Container(metr, "Metrics", "Grafana", "Сборщик метрик")
ContainerDb(db, "DB", "PostgreSQL", "База данных")
ContainerQueue(Queue, "Queue", "Kafka")

Rel(customer0, webUI, "Вход, регистрация,трансляции", "HTTPS")
Rel(customer1, webUI, "Загрузка докладов", "HTTPS")
Rel(webUI, be, "Вход, регистрация, трансляция", "HTTPS")
Rel(customer2, webOf, "Управление контентом", "HTTPS")
Rel(customer3, webOf, "Управление расписанием, обратная связь", "HTTPS")
Rel(customer4, webOf, "Управление трансляцией", "HTTPS")
Rel(customer5, webOf, "Контроль мероприятия", "HTTPS")
Rel(webOf, be, "Доклады, логи, трансляции, ретинги, расписание", "HTTPS")
Rel(email, be, "Загрузка докладов", "HTTPS")
Rel(webOf, Queue, "Работа с докладами", "HTTPS")
Rel(Queue, be, "Работа с докладами", "HTTPS")
Rel(be, db, "Загрузка докладов", "HTTPS")
Rel(metr, db, "Сбор метрик на основе логов", "HTTPS")
Rel(webOf, metr, "Сбор метрик на основе логов", "HTTPS")
Rel(webUI, email, "Загрузка доклада", "HTTPS")

}

@enduml
```

#### Список сценариев использования <!-- omit in toc -->

| ID     | Описание                                          |
|--------|---------------------------------------------------|
| UC.001 | *[Регистрация](uc/uc.001.md)* |
| UC.002 | *[Вход](uc/uc.002.md)* |
| UC.003 | *[Оценка](uc/uc.003.md)* |
| UC.004 | *[Вопросы](uc/uc.004.md)* |
| UC.005 | *[Подача заявки](uc/uc.005.md)* |
| UC.006 | *[Модерация доклада](uc/uc.006.md)* |
| UC.007 | *[Обратная связь](uc/uc.007.md)* |
| UC.008 | *[Расписание](uc/uc.008.md)* |
| UC.009 | *[Мониторин и контроль](uc/uc.009.md)* |


### Функциональные требования
<!-- Описание требований к функциям, реализуемым системой. Требование может быть привязано к сценарию использования или быть общим 
Подробнее: https://confluence.mts.ru/pages/viewpage.action?pageId=375782501 
-->
| ID     | Система должна позволять            | Функциональный блок                        |
|--------|---------------------------------------|---------------------------------------|
| FR.001 | Выступающим загружать свой доклад | Работа с докладчиками |
| FR.002 | Выступающим получать обратную связь | Работа с докладчиками |
| FR.003 | Контент менеджерам ознакомиться с выступающим и его докладом | Работа с докладчиками |
| FR.004 | Контент менеджерам давать обратную связь выступающим | Работа с докладчиками |
| FR.005 | Контент менеджерам проставлять рейтинг выступающему | Работа с докладчиками |
| FR.006 | Контент менеджерам проставлять рейтинг докладу | Работа с докладчиками |
| FR.007 | Фасилитатору создавать расписание конференции на основе рейтингов выступающего и доклада | Работа с расписаниями |
| FR.008 | Фасилитатору контролировать выступление участников конференции | Работа с расписаниями |
| FR.009 | Фасилитатору просматривать задаваемые вопросы | Проведение конференции (трансляция, сбор обратной связи) |
| FR.010 | Фасилитатору просматривать оценки конференции задавать вопросы| Проведение конференции (трансляция, сбор обратной связи) |
| FR.011 | Партнеру/слушателю регистрироваться| Проведение конференции (трансляция, сбор обратной связи) |
| FR.012 | Партнеру/слушателю оценивать конференцию | Проведение конференции (трансляция, сбор обратной связи) | 
| FR.013 | Партнеру/слушателю задавать вопросы | Проведение конференции (трансляция, сбор обратной связи) |
| FR.014 | Администратору мониторить трансляцию конференции | Проведение конференции (трансляция, сбор обратной связи) |
| FR.015 | Администратору использовать несколько каналов трансляции | Проведение конференции (трансляция, сбор обратной связи) |
| FR.016 | Руководителю проекта контролировать проведение конференции конференции | Проведение конференции (трансляция, сбор обратной связи) |
| FR.017 | Партнеру/слушателю подкючиться к конференции| Проведение конференции (трансляция, сбор обратной связи) |

### Нефункциональные требования/Требования к атрибутам качества
<!-- Требования к основным архитектурным характеристикам (атрибутам качества) системы - надежность, масштабируемость, ИБ, и др.
Подробнее: https://confluence.mts.ru/pages/viewpage.action?pageId=375782530
-->
| ID     | Бизнес метрика | Атрибут качества             | Описание требования                       |
|--------|------------------------------|------------------------------|-------------------------------------------|
| QR.001 | Пользователь ожидает не более | Вход в систему осуществляется менее 30 с. | Отклик системы должен быть не более 30 с. для 95% пользователе. Время входа в систему = время авторизация - время нажатия кнопки "Вход" |
| QR.002 | Пользователь ожидает не более | Запуск трансляции не более 30 с. | *Описание требования к атрибуту качества* Время загрузки трансляции = время загрузки трасляции - время нажатия кнопки "Смотреть трансляцию"|
| QR.003 | Выступающий ожидает не более | Ознакомление контент менеджера с докладом не более 23 ч. | Время взаимодествия контент менеджера и выступающего должно быть не более 24 ч. с момента подачи заявки, для 99% пользователей. Время обратной связи = Время ответа модератором - время подачи заявки|
| QR.004 | Выступающий ожидает не более | Обратная связь выступающему не более 24 ч. | Время взаимодествия контент менеджера и выступающего должно быть не более 24 ч. с момента подачи заявки, для 99% пользователей. Время обратной связи = Время ответа модератором - время подачи заявки |
| QR.005 | Выступающий ожидает не более | Максимально количество выступающих не более 10 человек | Время взаимодествия контент менеджера и выступающего должно быть не более 24 ч. с момента подачи заявки, для 99% пользователей. Время обратной связи = Время ответа модератором - время подачи заявки |

### Ограничения
<!-- Описываются ограничения, оказывающие влияние на архитектуру системы - временные, финансовые, технологические
Подробнее: https://confluence.mts.ru/pages/viewpage.action?pageId=375782592
-->
| ID     | Ограничение            |
|--------|------------------------|
| AC.001 | Максимальное количество пользователей - 10000 |

## Архитектура

### Журнал архитектурных решений
<!-- Записи о ключевых принятых архитектурных решениях (ADR) для реализации архитектурно-значимых требований.
Подробнее: https://confluence.mts.ru/pages/viewpage.action?pageId=421162308
-->
- [ADR.001 Определится с категорией системы (ACID|BASE|CAP|PACELC)](adr/adr-1.md)
- [ADR.002 Выбрать шаблон интеграции(метод, структура, взаимодействие)](adr/adr-2.md)

### Журнал архитектурных решений
[Ограниченные контексты решения](context/limited%D0%A1ontexts.md)
[Диаграмма контекстов](context/context.md)