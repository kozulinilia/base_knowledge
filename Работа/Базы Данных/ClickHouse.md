- [[#Как подключить табличку|Как подключить табличку]]
- [[#Выбрать из таблички только те строки, которые удовлетворяют услови|Выбрать из таблички только те строки, которые удовлетворяют услови]]
- [[#Как сгруппировать данные и соединить с другой табличкой по параметру|Как сгруппировать данные и соединить с другой табличкой по параметру]]
- [[#Как модифицировать тип существующего столбца|Как модифицировать тип существующего столбца]]
- [[#Как добавить столбец в табличку|Как добавить столбец в табличку]]
- [[#Как создать табличку|Как создать табличку]]
- [[#Как вставить строку|Как вставить строку]]
- [[#Как сделать аггрегирующую табличку|Как сделать аггрегирующую табличку]]

## Как подключить табличку

- Запустить DataGrip. Выбрать Clickhouse localhost, зайти в консоль и прописать команду для миграции
    
    ![ClickHouse%200dbec1dbcba4442f96b5c728b09808e0/Untitled.png](Работа/Базы%20Данных/ClickHouse/Untitled.png)
    
    Например миграция mysql_account_ban_history с полями id, uid, datacreated, comment, ban_type, status
    
    ```php
    create table mysql_account_ban_history
    (
        id          UInt32,
        uid         UInt32,
        datecreated UInt32,
        comment     String,
        ban_type    Int8,
        status      Int8
    )
        engine = Memory;
    ```
    

## Выбрать из таблички только те строки, которые удовлетворяют услови

Выборка из базы 'ragfair_transactions_stats'

выбирают столбцы с названиями DateTime, buyerProfileId, sellerProfileId, sellerAccountId, buyerAccountId, transactionCost, itemsCost и т.д.

Выбираются только те, где дата больше или равна $startTime, и меньше или равна $endTime и значение в столбце transactionCost больше или равно $parameters['cost']

Упорядочить по столбцу со значением $sortBy по убыванию или возрастанию в зависимости от значения $sortDesc(DESC или ASC)

Вывести значений на странице не больше $parameters['limit']

Начать выводить начиная с записи по номеру $parameters['offset']

```jsx
SELECT 
       				DateTime,
       				buyerProfileId,
       				sellerProfileId,
       				sellerAccountId,
       				buyerAccountId,
       				transactionCost,
       				itemsCost,
			       `purchasedItems.tpl` as purchasedTpl,
			       `purchasedItems.price` as purchasedPrice,
			       `purchasedItems.count` as purchasedCount,
			       `barterItems.tpl` as barterTpl,
			       `barterItems.price` as barterPrice,
			       `barterItems.count` as barterCount
			    FROM ragfair_transactions_stats
				WHERE DateTime >= {$startTime} AND DateTime <= {$endTime} AND transactionCost >= {$parameters['cost']}
				ORDER BY {$sortBy} {$sortDesc}
				LIMIT {$parameters['offset']}, {$parameters['limit']}
```

## Как сгруппировать данные и соединить с другой табличкой по параметру

Общая схема

```sql
SELECT * FROM( SELECT field FROM ...) as tab1 ANY LEFT JOIN (SELECT field FROM ...) AS tab2 USING field
```

```php
"SELECT * FROM (SELECT aid,
		        any(uid) as uid,
		        sum(equipmentFullCost-startEquipmentFullCost) as diff,
		        sum(equipmentFullCost+startEquipmentFullCost) as vol,
				sum(valuableFoundInRaidTriggersSum) as valuableFoundInRaidTriggersSum,
		      	sum(valuableNotFoundInRaidTriggersSum) as valuableNotFoundInRaidTriggersSum,
		        count() as count
		    FROM mm_result
		    WHERE side NOT IN ('','Savage')
		        AND timestamp >= {$startTs} AND timestamp <= {$endTs}
		        AND group = '343lknvsdkgdlsfge'
		    GROUP BY aid HAVING **diff >= 0.5 AND aid = 345678**) AS tab1
		    ANY LEFT JOIN(SELECT comment AS banReason, uid as aid FROM mysql_account_ban_history WHERE status > {$now} OR status = -1 ) AS tab2
		    USING aid
		     ORDER BY {$sortBy} " . ($sortDesc ? 'DESC' : '')
		;
```

## Как модифицировать тип существующего столбца

в примере переделан тип столбца virtualRegion со стринг на аррей

```jsx
alter table matchmaker_ticket_statistic modify column virtualRegion Array(String);
```

## Как добавить столбец в табличку

```sql
ALTER TABLE events_by_day
    ADD COLUMN cost Float64,
    ADD COLUMN browser String after event_type,
    MODIFY ORDER BY (event_type, ts, browser);
    
alter table client_metric_timing add column `settings_EDLSSMode` String

ALTER TABLE matchmaker_room_statistic
    ADD COLUMN tier String,
    ADD COLUMN rankingMode String;
```

как добавить столбец с дефолтным значением

```sql
alter table matchmaker_room_statistic
	add column dateCancel UInt64 default 0;
```

## Как создать табличку

```sql
CREATE TABLE client_screen_types
(
    name        String,
    types       Array(Int8),
    description String
) engine = MergeTree()
      ORDER BY name
      SETTINGS index_granularity = 8192;
```

## Как вставить строку

```sql
INSERT INTO client_screen_types (name, types, description) VALUES ('ArenaInventoryScreen', [1], '') ;
```

## Как сделать аггрегирующую табличку

есть табличка с сырыми данными

```sql
create table cheat_reports
			(
			    date               Date,
			    id                 UInt32,
			    parameter1         UInt64,
			    parameter2         UInt64,
			) engine = MergeTree() PARTITION BY date ORDER BY date;
```

сводная таблица с движком AggregatingMergeTree()

```sql

			create table cheat_reports_days
			(
			    date               Date,
			    id                 UInt32,
			    parameter1         SimpleAggregateFunction(sum, Int64),
			    parameter2         SimpleAggregateFunction(sum, Int64),
			) engine = AggregatingMergeTree() PARTITION BY date ORDER BY (id, date) SETTINGS index_granularity = 8192;
```

и табличка MATERIALIZED_VIEW, чтобы данные из первой таблички копировать во вторую

```sql
CREATE MATERIALIZED VIEW cheat_reports_days_mv TO cheat_reports_days
		AS
		SELECT date, id, parameter1, parameter	FROM cheat_reports;
```
