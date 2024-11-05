# MongoDb

## Insert

The following example inserts a new document into the inventory collection. If the document does not specify an _id field, the PHP driver adds the _id field with an ObjectId value to the new document. See Insert Behavior.

```php
$insertOneResult = $db->inventory->insertOne([
    'item' => 'canvas',
    'qty' => 100,
    'tags' => ['cotton'],
    'size' => ['h' => 28, 'w' => 35.5, 'uom' => 'cm'],
]);
```

## Insert Single Document

Insert a Single Document[¶](https://docs.mongodb.com/manual/tutorial/insert-documents/#insert-a-single-document)

[`MongoDB\Collection::insertOne()`](https://docs.mongodb.com/manual/tutorial/insert-documents/) inserts a *single* [document](https://docs.mongodb.com/manual/core/document/#std-label-bson-document-format) into a collection.

```php
$insertOneResult = $db->inventory->insertOne([
    'item' => 'canvas',
    'qty' => 100,
    'tags' => ['cotton'],
    'size' => ['h' => 28, 'w' => 35.5, 'uom' => 'cm'],
]);
```

Для доступа к только что созданному документу используйте find

```php
$cursor = $db->inventory->find(['item' => 'canvas']);
```

## Insert Multiple Documents

[`MongoDB\Collection::insertMany()`](https://docs.mongodb.com/manual/tutorial/insert-documents/) can insert *multiple* [documents](https://docs.mongodb.com/manual/core/document/#std-label-bson-document-format) into a collection. Pass an array of documents to the method.

The following example inserts three new documents into the `inventory` collection. If the documents do not specify an `_id` field, the PHP driver adds the `_id` field with an ObjectId value to each document. See [Insert Behavior](https://docs.mongodb.com/manual/tutorial/insert-documents/#std-label-write-op-insert-behavior).

```php
$insertManyResult = $db->inventory->insertMany([
    [
        'item' => 'journal',
        'qty' => 25,
        'tags' => ['blank', 'red'],
        'size' => ['h' => 14, 'w' => 21, 'uom' => 'cm'],
    ],
    [
        'item' => 'mat',
        'qty' => 85,
        'tags' => ['gray'],
        'size' => ['h' => 27.9, 'w' => 35.5, 'uom' => 'cm'],
    ],
    [
        'item' => 'mousepad',
        'qty' => 25,
        'tags' => ['gel', 'blue'],
        'size' => ['h' => 19, 'w' => 22.85, 'uom' => 'cm'],
    ],
]);
```

Upon successful insert, the insertMany() method returns an instance of MongoDB\InsertManyResult whose getInsertedIds() method returns the _id of each newly inserted document.

## Select All Documents in a Collection

To select all documents in the collection, pass an empty document as the query filter parameter to the find method. The query filter parameter determines the select criteria:

```php
$cursor = $db->inventory->find([]);
```

## Specify Equality Condition

To specify equality conditions, use <field> => <value> expressions in the query filter document:

```php
$cursor = $db->inventory->find(['status' => 'D']);
```

## Specify Conditions Using Query Operators

The following example retrieves all documents from the inventory collection where status equals either "A" or "D":

```php
$cursor = $db->inventory->find(['status' => ['$in' => ['A', 'D']]]);
```

## Запросы к MongoDb

Общая структура

```php
$this->findMany($query, $options, $cursor = false)
```

```php
$query = [
	'$and' => [
							['$or' => [
													['region' => $region], 
													['region' => null]
												]
							], 
							['$or' => [
													['country' => $country], 
													['country' => [] ]
												]
							]
						]
];
```

```php
$this->findMany([
   'status' => ActionLogDocument::SEND_TO_MODERATION
], [
   'limit' => $limit,
   'skip'  => ($cursor - 1) * $limit,
   'sort'  => ['dateTime' => -1]
]);
```

## Условные операторы

- $in
    
    The [`$in`](https://www.mongodb.com/docs/v4.4/reference/operator/query/in/#mongodb-query-op.-in) operator selects the documents where the value of a field equals any value in the specified array. To specify an [`$in`](https://www.mongodb.com/docs/v4.4/reference/operator/query/in/#mongodb-query-op.-in) expression, use the following prototype:
    
    ```php
    { field: { $in: [<value1>, <value2>, ... <valueN> ] } }
    ```
    
    For comparison of different BSON type values, see the [specified BSON comparison order](https://www.mongodb.com/docs/v4.4/reference/bson-type-comparison-order/#std-label-bson-types-comparison-order).
    If the `field` holds an array, then the [`$in`](https://www.mongodb.com/docs/v4.4/reference/operator/query/in/#mongodb-query-op.-in) operator selects the documents whose `field` holds an array that contains at least one element that matches a value in the specified array (for example, `<value1>`, `<value2>`, and so on).
    The [`$in`](https://www.mongodb.com/docs/v4.4/reference/operator/query/in/#mongodb-query-op.-in) operator compares each parameter to each document in the collection, which can lead to performance issues. To improve performance:
    
    Example:
    
    ```php
    db.inventory.find( { qty: { $in: [ 5, 15 ] } } )
    ```
    
    выберет 
    
- $and $or
    
    ```sql
    $profiles = $collection->find(
    			[
    
    				'$and' =>
    					[
    						[
    							'aid'                   => ['$gt' => $lastAid],
    							'savage'                => ['$ne' => null],
    							'Info.RegistrationDate' => ['$lte' => $endDate]
    						],
    						[
    							'$or' =>
    								[
    									['Info.NicknameChangeDate' => ['$gte' => $startDate]],
    									['Info.NicknameChangeDate' => 0]
    								]
    						]
    					]
    
    			],
    			[
    				'limit' => $limit,
    				'sort'  => [
    					'aid' => 1
    				],
    			]
    		);
    ```
    

## $regex

```sql

```

## $elemMatch. запрос документов, где выбранный элемент есть в массиве

следующая операция запрашивает все документы, в которых массив dim_cm содержит хотя бы один элемент, значение которого больше 25.

```php
db.inventory.find( { dim_cm: { $gt: 25 } } )
```

Поиск в поле players, представл масств игроков. Игроков со значением поля name  Petr 

```php
['players' => 
	[
		'$elemMatch' => ['name' => 'Petr']]
	]
]
```

В монге

```php
{
    "requests" : {
        $elemMatch : {
            "extra.data.nickname" : "Player_64a27f71d8c59469c002bf07"
        }
    }
}
```

## TypeMap

![Untitled](Работа/Базы%20Данных/MongoDb/Untitled.png)

Рабочий примео массива документов класса TraderStandingSettings

![Untitled](Работа/Базы%20Данных/MongoDb/Untitled%201.png)

## update operators

- findAndModify
    
    ```sql
    $result = $this->db->findAndModify(
                    [
                        "status" => RoomStatus::MatchAssigned->value,
                    ],
                    [
                        '$inc' => ['generatedBackFill' => 1]
                    ],
                    [
                        'sort' => ['_id' => 1]
                    ]
                );
    ```
    
- updateOne
    
    ```php
    $this->updateOne(
    			[
    				'_id' => $uid
    			],
    			[
    				'$set' => [
    					'Info.Nickname'           => $nickname,
    					'Info.LowerNickname'      => $lowerNickname,
    					'Info.NicknamePref'       => $nicknamePref,
    					'Info.NicknameChangeDate' => $nicknameChangeDate,
    				]
    			]
    		);
    ```
    
    ```sql
    db.products.updateOne(
       { _id: 100 },
       { $set:
          {
            quantity: 500,
            details: { model: "2600", make: "Fashionaires" },
            tags: [ "coats", "outerwear", "clothing" ]
          }
       }
    )
    ```
    

### **Behavior**

Starting in MongoDB 5.0, update operators process document fields with string-based names in lexicographic order. Fields with numeric names are processed in numeric order.

In MongoDB 4.4 and earlier, update operators process all document fields in lexicographic order.

Consider this example [`$set`](https://www.mongodb.com/docs/manual/reference/operator/update/set/#mongodb-update-up.-set) command:

```jsx
{ : { : <new value>, "a.10": <new value>, } }
```

In MongoDB 5.0 and later, `"a.2"` is processed before `"a.10"` because `2` comes before `10` in numeric order.

In MongoDB 4.4 and earlier, `"a.10"` is processed before `"a.2"` because `10` comes before `2` in lexicographic order.

- Fields
    
    
    | **Name** | **Description** |
    | --- | --- |
    | [`$currentDate`](https://www.mongodb.com/docs/manual/reference/operator/update/currentDate/#mongodb-update-up.-currentDate) | Sets the value of a field to current date, either as a Date or a Timestamp. |
    | [`$inc`](https://www.mongodb.com/docs/manual/reference/operator/update/inc/#mongodb-update-up.-inc) | Increments the value of the field by the specified amount. |
    | [`$min`](https://www.mongodb.com/docs/manual/reference/operator/update/min/#mongodb-update-up.-min) | Only updates the field if the specified value is less than the existing field value. |
    | [`$max`](https://www.mongodb.com/docs/manual/reference/operator/update/max/#mongodb-update-up.-max) | Only updates the field if the specified value is greater than the existing field value. |
    | [`$mul`](https://www.mongodb.com/docs/manual/reference/operator/update/mul/#mongodb-update-up.-mul) | Multiplies the value of the field by the specified amount. |
    | [`$rename`](https://www.mongodb.com/docs/manual/reference/operator/update/rename/#mongodb-update-up.-rename) | Renames a field. |
    | [`$set`](https://www.mongodb.com/docs/manual/reference/operator/update/set/#mongodb-update-up.-set) | Sets the value of a field in a document. |
    | [`$setOnInsert`](https://www.mongodb.com/docs/manual/reference/operator/update/setOnInsert/#mongodb-update-up.-setOnInsert) | Sets the value of a field if an update results in an insert of a document. Has no effect on update operations that modify existing documents. |
    | [`$unset`](https://www.mongodb.com/docs/manual/reference/operator/update/unset/#mongodb-update-up.-unset) | Removes the specified field from a document. |
- **Array**
    
    ### **Operators**
    
    ![https://www.mongodb.com/docs/manual/assets/link.svg](https://www.mongodb.com/docs/manual/assets/link.svg)
    
    | **Name** | **Description** |
    | --- | --- |
    | [`$`](https://www.mongodb.com/docs/manual/reference/operator/update/positional/#mongodb-update-up.-) | Acts as a placeholder to update the first element that matches the query condition. |
    | [`$[]`](https://www.mongodb.com/docs/manual/reference/operator/update/positional-all/#mongodb-update-up.---) | Acts as a placeholder to update all elements in an array for the documents that match the query condition. |
    | [`$[<identifier>]`](https://www.mongodb.com/docs/manual/reference/operator/update/positional-filtered/#mongodb-update-up.---identifier--) | Acts as a placeholder to update all elements that match the `arrayFilters` condition for the documents that match the query condition. |
    | [`$addToSet`](https://www.mongodb.com/docs/manual/reference/operator/update/addToSet/#mongodb-update-up.-addToSet) | Adds elements to an array only if they do not already exist in the set. |
    | [`$pop`](https://www.mongodb.com/docs/manual/reference/operator/update/pop/#mongodb-update-up.-pop) | Removes the first or last item of an array. |
    | [`$pull`](https://www.mongodb.com/docs/manual/reference/operator/update/pull/#mongodb-update-up.-pull) | Removes all array elements that match a specified query. |
    | [`$push`](https://www.mongodb.com/docs/manual/reference/operator/update/push/#mongodb-update-up.-push) | Adds an item to an array. |
    | [`$pullAll`](https://www.mongodb.com/docs/manual/reference/operator/update/pullAll/#mongodb-update-up.-pullAll) | Removes all matching values from an array. |
    
    ### **Modifiers**
    
    ![https://www.mongodb.com/docs/manual/assets/link.svg](https://www.mongodb.com/docs/manual/assets/link.svg)
    
    | **Name** | **Description** |
    | --- | --- |
    | [`$each`](https://www.mongodb.com/docs/manual/reference/operator/update/each/#mongodb-update-up.-each) | Modifies the [`$push`](https://www.mongodb.com/docs/manual/reference/operator/update/push/#mongodb-update-up.-push) and [`$addToSet`](https://www.mongodb.com/docs/manual/reference/operator/update/addToSet/#mongodb-update-up.-addToSet) operators to append multiple items for array updates. |
    | [`$position`](https://www.mongodb.com/docs/manual/reference/operator/update/position/#mongodb-update-up.-position) | Modifies the [`$push`](https://www.mongodb.com/docs/manual/reference/operator/update/push/#mongodb-update-up.-push) operator to specify the position in the array to add elements. |
    | [`$slice`](https://www.mongodb.com/docs/manual/reference/operator/update/slice/#mongodb-update-up.-slice) | Modifies the [`$push`](https://www.mongodb.com/docs/manual/reference/operator/update/push/#mongodb-update-up.-push) operator to limit the size of updated arrays. |
    | [`$sort`](https://www.mongodb.com/docs/manual/reference/operator/update/sort/#mongodb-update-up.-sort) | Modifies the [`$push`](https://www.mongodb.com/docs/manual/reference/operator/update/push/#mongodb-update-up.-push) operator to reorder documents stored in an array. |
- **Bitwise**
    
    
    | **Name** | **Description** |
    | --- | --- |
    | [`$bit`](https://www.mongodb.com/docs/manual/reference/operator/update/bit/#mongodb-update-up.-bit) | Performs bitwise `AND`, `OR`, and `XOR` updates of integer values. |
- Unset
    
    то, что написано в значении поля не имеет занчения. Поле будет удалено
    
    ```sql
    {
        $unset : {
            id : ""
        }
    }
    ```
    

## Вставка элемента массива

если надо заменить элемент массива по параметру

```jsx
$replaceResult = $this->updateOne(
			[
				'appInstanceName' => $instanceName,
				'services.serviceName' => $backendService->getServiceName()
			],
			[
				'$set' =>
					[
						'services.$' => $backendService
					]
			],
		);
```

структура документа

![Untitled](Работа/Базы%20Данных/MongoDb/Untitled%202.png)

если надо добавить в элемент новый

```jsx
$addResult = $this->updateOne(
				[
					'appInstanceName' => $instanceName,
				],
				[
					'$addToSet' =>
						[
							'services' => $backendService
						]
				],
			);
```

## update с pipeline

при таком апдейте можно делать вычисления с соседними полями

```sql
$this->storage->update(
            [
                "agentId" => $agentId,
            ],
            [
                [
                    '$set' => [
                        'metrics' => $metrics
                    ],
                ],
                [
                    '$addFields' => [
                        "totalRAMUsagePercent" => [
                            '$multiply' => [
                                ['$divide' => [$metrics->getTotalRAMUsage(), '$totalRam']],
                                100
                            ]
                        ]
                    ]
                ]
            ],
        );
```

## Создание индексов

как создается в проекте БСГ

```sql
return $this->ensureIndexes($this->collection, [
            [
                'key'  => [
                    'accounts' => 1,
                ],
                'name' => 'accounts_1',
                'unique' => true
            ],
            [
                'key'    => [
                    'leader' => 1,
                ],
                'name'   => 'leader_1',
                'unique' => true
            ],
            [
                'key'    => [
                    'accounts' => 1,
                    'leader'   => 1
                ],
                'name'   => 'accounts_1_leader_1',
                'unique' => true
            ],
            [
                'key'    => [
                    'raid_settings' => 1,
                ],
                'name'   => 'raid_settings_1',
            ],
        ], $dropNotListed);
```

как создается по простому

сделать индекс по полю `source` по убыванию

```sql
$this
            ->db
            ->createIndexes(
                [
                    'key' => ['source' => -1]
                ]
            );
```

## Аггрегация

- Group
    - Примеры
        - Задача 1
            
            Из записей за последний месяц с полем aid равным $aid, сделать группу, в которую собрать массив из полей newNickname, oldNickname, dt по каждой записи.
            
        - Код
            
            ```php
            $pipeline = [
            			[
            				'$match' => [
            					'$and' =>
            						[
            							[
            								'dt' =>
            									[
            										'$gt' => new UTCDateTime((new \DateTime())->modify("-1 month"))
            									]
            							],
            							['aid' => $aid]
            
            						]
            				]
            			],
            			['$sort' => ['dt' => -1]],
            			[
            				'$group' =>
            					[
            						'_id' => '$aid',
            						'historyLog' =>
            							[
            								'$addToSet' =>
            									[
            										'newNickname' => '$newNickname',
            										'oldNickname' => '$oldNickname',
            										'dt'          => '$dt',
            									]
            							]
            					]
            			]
            		];
            ```
            
        - Результат
            
            ![Untitled](Работа/Базы%20Данных/MongoDb/Untitled%203.png)
            
        - Задача 2
            
            Сгруппировать записи в интервале между startDate и endDate, где approver не null, по полям log и reporter и в переменную count вывести кол-во элементов в группе
            
        - Код
            
            ```php
            $pipeline = [
            				[
            					'$match' =>
            						[
            							'$and' =>
            								[
            									[
            										'dateTime' =>
            											[
            												'$gt' => $startDate,
            												'$lt' => $endDate
            											]
            									],
            									['approver' => ['$ne' => null]]
            								]
            						]
            				],
            				[
            					'$group' => [
            						'_id'   =>
            							[
            								'log'  => '$log',
            								'name' => '$reporter',
            							],
            						'count' => ['$sum' => 1]
            
            					]
            				]
            			];
            
            $this->aggregate($pipeline);
            ```
            
        - Результат
            
            ![Untitled](Работа/Базы%20Данных/MongoDb/Untitled%204.png)
            
        - Задача 3 Аггрегация по элементам массива
            
            Нужно сагрегировать по уникальным сочетаниям region и virtualRegion с учетом, что эти поля - массивы с одним элементом внутри
            
            ```sql
            [
                    {
                        "$match" : {
                            "region" : {
                                "$ne" : [
            
                                ]
                            },
                            "virtualRegion" : {
                                "$ne" : [
            
                                ]
                            }
                        }
                    }, 
                    {
                        "$group" : {
                            "_id" : {
                                "combo" : {
                                    "$concat" : [
                                        {
                                            "$arrayElemAt" : [
                                                "$region",
                                                NumberInt(0)
                                            ]
                                        },
                                        {
                                            "$arrayElemAt" : [
                                                "$virtualRegion",
                                                NumberInt(0)
                                            ]
                                        }
                                    ]
                                },
                                "virtualRegion" : {
                                    "$arrayElemAt" : [
                                        "$region",
                                        NumberInt(0)
                                    ]
                                },
                                "region" : {
                                    "$arrayElemAt" : [
                                        "$virtualRegion",
                                        NumberInt(0)
                                    ]
                                }
                            }
                        }
                    }
                ], 
            ```
            
- Out
    
    группировка с записью результата в другую коллекцию
    
    ```sql
    $pipeline =
                [
                    [
                        '$match' =>
                            [
                                'lastSessionDate' => ['$lt' => $expireTimestamp]
                            ]
                    ],
                    [
                        '$project' =>
                            [
                                'points' => $this->getProjectQuery($factors)
                            ]
                        ,
                    ],
                    [
                        '$group' =>
                            [
                                '_id'   => '$points',
                                'count' => ['$sum' => 1]
                            ]
                    ],
                    [
                        '$out' =>
                            [
                                'db'   => $this->collection->getDatabaseName(),
                                'coll' => self::KARMA_POINTS_COLLECTION,
                            ]
                    ]
                ];
            $this->aggregate($pipeline);
    ```
    
- Accumulator
    
    пример с расчетом перцентиля
    
    в аккумуляторе:
    
    **init**: задаем изначальное состояние state. Переменная state, которая будет переходить от элемента к элементу
    
    **accumulateArgs**: аргументы из каждого из перебираемых элементов, которые мы  используем для аггрегации
    
    **accumulate**: здесь строкой передается js функция аггрегации, кот принимает переменную состояния state и аргументы, которые указали в accumulateArgs
    
    **merge**: что мы делаем с состояниями(переменными state) между шагами аггрегации. Объединяем, или оставляем состояние последнего и т.д.
    
    **finalize**: финальные действия, которые сделаем с результатом аггрегации(переменной state), когда по всему массиву пройдем.
    
    ```sql
    public function calculatePercentiles(int $count): array
        {
            $pipeline =
                [
                    [
                        '$sort' =>
                            [
                                '_id' => 1,
                            ]
                    ],
                    [
                        '$group' =>
                            [
                                '_id'        => null,
                                'percentile' =>
                                    [
                                        '$accumulator' =>
                                            [
                                                'init'       =>
                                                    "function() {
                                                        return {
                                                            level: 0.05,
                                                            percentiles: {},
                                                            count: $count,
                                                            index: 0,
                                                            next: false,
                                                            rest: 0,
                                                            previous: 0,
                                                            }
                                                        }",
                                                'accumulateArgs' => ['$_id', '$count'],
                                                'accumulate' => $this->getPercentileJsFunction(),
                                                'merge'      =>
                                                    'function(state1, state2) {
                                                        return state2;
                                                    }',
                                                'finalize'   =>
                                                    'function(state) {
                                                        return state.percentiles;
                                                    }'
                                            ]
                                    ]
                            ],
                    ],
                ];
            /** @var array<int, array<string, mixed>> $percentiles */
            $percentiles = $this->aggregate($pipeline);
    
            return $percentiles[0]['percentile'];
      }
      
      private function getPercentileJsFunction(): string
        {
            return
                'function (state, points, count) {
                    const calc = (state, points) => {
                           let level = parseFloat(state.level)
                           let floatBarrierIndex = ((level * (state.count + 1))) - 1;
    	                   let barrierIndex = Math.floor(floatBarrierIndex);
    	                   if (state.next) {
    	                   let diff = level - 0.05;
    	                   	state.percentiles[diff.toFixed(2)] = (points - state.previous) * state.rest + state.previous;
    	                   	state.next = false;
    	                   }
    	                   if (state.index >= barrierIndex) {
    	                    let sum = level + 0.05;
    	                   	state.level = sum.toFixed(2);
    	                   	state.previous = points;
    	                   	state.next = true;
    	                   	state.rest = floatBarrierIndex - barrierIndex;
    	                   }
    	                   state.index++;
    
                    }
                    if (count > 1) {
                        for(let i = count; i > 1; i--) {
                            calc(state, points)
                        }
                    }
    	            calc(state, points);
    	            return state;
                }';
        }
    ```
    
- Дока Монги
    
    [Aggregation Pipeline Quick Reference](https://www.mongodb.com/docs/v5.0/meta/aggregation-quick-reference/#std-label-aggregation-expressions)