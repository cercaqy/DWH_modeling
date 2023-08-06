# Проектирование логической модели DWH
## Задача
Описать предметную область, задачи по ее анализу и спроектировать логическую модель по методологиям Dimensional Modeling, Data Vault, Anchor Modeling

## Описание предметной области "Такси"
В таксопарке есть свои машины с определенными государственными номерами
маркой, цветом, классом (эконом, комфорт, комфорт+, бизнес и премиум) и данными по страховке: номер страховки, дата начала действия, дата окончания действия, информация об одной или нескольких страховках, а именно, дата начала действия, дата окончания действия, тип страховки(КАСКО, ОСАГО, ДСАГО, страховка от несчастного случая, защищающая здоровье и жизнь, зеленая карта). Также хранится информация о статусе машины (в работе, в ремонте, списана)

Каждой машине присваивается определенный водитель, также водитель может работать на своей собственной машине.

О водителе известны СНИЛС, ФИО, пол, дата рождения, паспортные данные, номер телефона, рейтинг от 1 до 5. 

Компания сохраняет следующие данные о водительском удостоверении водителя: дата начала действия актуального водительского удостоверения, дата окончания действия актуального водительского удостоверения, год отсчета стажа вождения

С водителем заключается рабочий договор, о котором известен его уникальный номер, дата заключения договора, дата прекращения действия рабочего договора, процент, который водитель будет получать от общего дохода с его поездок. Договор может меняться, а именно процент с дохода в зависимости от различных факторов.

Водители осуществляют поездки, по которым известны время заказа, время принятия заказа водителем, время прибытия водителя на место назначения, время отправления, время окончания поездки, стоимость поездки, количество чаевых, стоимость ожидания водителем, общая выручка за поездку, координаты точки отправления, координаты точки назначения, расстояние поездки, статус поездки (отменена, выполнена, в процессе), тип оплаты (карта, наличные). После поездки водитель может поставить оценку пассажиру, а пассажир может поставить оценку водителю по шкале от 1 до 5. Также пассажир может написать отзыв о поездке.

О пассажирах известны их ФИО, пол, дата рождения, номер телефона, рейтинг от 1 до 5

## Задачи по анализу
Анализ поездок: подсчет общего количества поездок, поиск популярных маршрутов, вычисление средней длительности или стоимости поездок, доминирующие типы оплаты и тд

## Проектирование хранилища по Кимбалу, а именно, Data Mart Layer по методологии Dimensional Modeling (схема снежинка)
![taxi_dim_modeling drawio](https://github.com/cercaqy/DWH_modeling/assets/101037358/c4224624-fd2c-4bc2-b8b2-4fa07b4456de)

**taxi_dim_modeling.drawio.png** - схема в png

**taxi_dim_modeling.drawio** - схема в XML (открыть [здесь](https://app.diagrams.net))

## Проектирование хранилища по Инмону, а именно, Data Core Layer по методологии Data Vault (DV 2.0)
![taxi_DV drawio](https://github.com/cercaqy/DWH_modeling/assets/101037358/db81b9d3-a1b9-4b53-8343-e3c7bdb4db11)

**taxi_DV.drawio.png** - схема в png

**taxi_DV.drawio** - схема в XML (открыть [здесь](https://app.diagrams.net))

## Проектирование хранилища по Инмону, а именно, Data Core Layer по методологии Anchor Modeling
Для примера возьмем часть схемы модели DV
![taxi_DV drawio_part](https://github.com/cercaqy/DWH_modeling/assets/101037358/3f8a830f-2331-4bda-a933-3c005278eea4)

Между якорем an_trip и его атрибутами не провожу связи, чтобы не загромождать схему. Везде 1:М по примеру связи с at_trip_driver_pickup_datetime
![taxi_anchor_part drawio](https://github.com/cercaqy/DWH_modeling/assets/101037358/02df202f-5fd7-4360-9281-84943d79181b)

**taxi_anchor_part.drawio.png** - схема в png

**taxi_anchor_part.drawio** - схема в XML (открыть [здесь](https://app.diagrams.net))

## Возможные точки развития бизнеса

- сотрудничество с различными таксопарками для увеличения кол-ва автомобилей в сервисе
- добавление справочной таблицы с соответствием модели машины ее классу (внешний источник или таблица в БД?)
- добавления тарифа “Share”, когда в поездке может быть более одного пассажира (для более удобного обслуживания пассажиров во время высокого спроса и уменьшения стоимости для пассажиров)
- добавление класса “минивэн”
- блокировочные санкции по отношению к водителю в зависимости от рейтинга
- добавить на схему процесс поддержки пассажиров



