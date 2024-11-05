# TimeTracker

[Деплой](Деплой%2070a1744bdb1b4ae79489eff72ee565e9.md)

- Репозиторий
    
    [https://backend-git.absc.local/backend/time-tracker](https://backend-git.absc.local/backend/time-tracker)
    
- Подключение к базе ТТ
    
    ```jsx
    host: 192.168.200.109
    port: 3306
    user: user_tt
    URL: jdbc:mysql://192.168.200.109:3306/
    
    mysql://user_tt:<passw>@192.168.200.109:3306/db_dev_tt01?server_version=8.0.27
    mysql://user_tt:<passw>@192.168.200.109:3306/db_dev_tt02?server_version=8.0.27
    mysql://user_tt:<passw>@192.168.200.109:3306/db_prod_tt01?server_version=8.0.27
    ```
    
- Структура БД
    
    ```jsx
    doctrine_migration_versions - версии миграции базы
    
    tt_comments - комменты дня
    
    tt_departments - отделы конторы
    
    tt_log - основная, временные интервалы работы сотрудников
    
    tt_positions - должности сотрудников
    
    tt_salary_bonus - повышенные коэфф за переработки в выбранные периоды
    
    tt_short_work_days - праздники фирмы, дни, когда в фирме выходной или укороченный день.
    
    tt_user - пользователи
    
    tt_user_info - параметры пользователей(ФИО, время прихода на работу, отдел, должность)
    
    tt_user_vacations - отпуска, больничные, отгулы пользователей
    
    tt_vacation_types - виды отпусков(пока там три: отпуск, больничный, отгул)
    ```
    
    Остальные базы - это остатки от старой базы или промежуточные уже не нужные базы после миграции старой базы
    
- Деплой ТТ
    1. в проекте зайти в папку деплоера
    
    ```jsx
    cd ./deploy/
    ```
    
    1. 
    
    ```jsx
    //Деплой дев тестового бека
    cap <имя ветки> tt-dev
    
    //Деплой прод бека
    cap <имя ветки> tt-prod
    ```
    
- Полезные ссылки
    
    Локальный админер
    
    [http://localhost:8080/?server=db&username=root](http://localhost:8080/?server=db&username=root)
    
    Локальный ТТ
    
    [http://tt.local/](http://tt.local/)
    
    Тестовый дев бек ТТ
    
    [https://tt-dev.dev-eft.local/](https://tt-dev.dev-eft.local/)
    
    Прод ТТ
    
    [https://tt.absc.local/](https://tt.absc.local/)
    
    Алиас для подключения по ssh
    
    ```php
    ssh i.kozulin@tt-dev.dev-eft.local
    ```
    
- Запуск ТТ локально
    
    Сбилдить контейнеры
    
    ```jsx
    sudo docker compose up -d --build
    ```
    
    контейнер с нджинксом не запустится, так как его порт занимает [gw_nginx](http://localhost:9000/#!/1/docker/containers/bb9b48585439e110dba767ee6e8e9e537ca3ecd4567107ed8d10f229649644a3), поэтому его надо предварительно стопнуть
    
    Запустить миграции доктрины внутри контейнера php-fpm
    
    ```jsx
    php bin/console doctrine:migrations:migrate
    ```
    
    Далее в контейнере запускаем команду генерации тестового юзера
    
    ```jsx
    app:fakedata:fill  1 user1234
    ```
    
    Где 1 - это 1 пользователь, а user1234 - это его пароль
    
    Входим в админер в коллекцию **tt_user и смотрим имя пользователя**
    
    ![Untitled](Работа/TimeTracker/Untitled.png)
    
    Далее входим по [ссылке](http://tt.local/)
    
    ![Untitled](Работа/TimeTracker/Untitled%201.png)
    
- консольные команды ТТ
    
    ### генерация фейковых пользователей(работает только в dev режиме)
    
    ```php
    bin/console app:fakedata:fill <кол-во пользователей> <пароль пользователей> --no-debug
    ```
    
    пример
    
    ```php
    bin/console app:fakedata:fill 100 newUser --no-debug
    ```
    
    ### Миграция с базы старого ТТ(не запускать!!!!)
    
    ```php
    bin/console app:migrate
    ```
    
- Запросы в базу
    
    Выгрузка кол-ва отпусков
    
    ```jsx
    SELECT id, last_name, first_name, department, weekends  FROM (SELECT  last_name, first_name, department_id, count(vacation_day) as weekends
    FROM db_prod_tt01.tt_user_vacations
             LEFT JOIN db_prod_tt01.tt_user_info USING(user_id)
    WHERE vacation_day >= '2022-01-01' AND vacation_day <= '2022-12-31'
    GROUP BY user_id) as tab1
        LEFT JOIN db_prod_tt01.tt_departments ON tab1.department_id = id
    ORDER BY last_name
    ```
    
    Выгрузка выработки в часах за период
    
    ```sql
    SELECT user_id,
           last_name,
           first_name,
           sum(TIMESTAMPDIFF(SECOND, start_time, finish_time))/3600 as workTime,
           date_format(start_time, '%Y-%m-%d')                 as day
    FROM db_prod_tt01.tt_log
             LEFT JOIN db_prod_tt01.tt_user_info USING (user_id)
    WHERE start_time >= '2022-09-28'
      AND deleted_at IS NULL
      AND start_time <= '2022-09-30'
    GROUP BY user_id, last_name, day
    ORDER BY last_name ASC;
    ```
    
    Выгрузка кол-ва опозданий за год
    
    ```sql
    SELECT last_name, first_name, user, count(date) as count FROM(SELECT * FROM (SELECT last_name, first_name, user_id AS user, DATE_FORMAT(min(start_time), '%H%i%s') AS start_time, DATE_FORMAT(start_time, '%Y-%m-%d') AS date
    FROM db_prod_tt01.tt_log
             LEFT JOIN db_prod_tt01.tt_user_info USING (user_id)
    WHERE start_time >= '2022-01-01'
    AND start_time <= '2022-12-31'
    AND deleted_at IS NULL
    GROUP BY user_id, date) AS tab1
                      LEFT JOIN db_prod_tt01.tt_user_vacations ON user_id = tab1.user AND tt_user_vacations.vacation_day = tab1.start_time
    WHERE user_id IS NULL) AS tab2
    WHERE ( tab2.start_time > '113000')
      AND date_format(date, '%a') NOT IN ('Sun', 'Sat')
    GROUP BY tab2.user
    ```
    
    Выгрузка кол-ва будних дней, в которые отработано меньше 7 часов
    
    ```sql
    SELECT last_name, first_name, user, count(day) AS days FROM (SELECT user_id as user,
           last_name,
           first_name,
           sum(TIMESTAMPDIFF(SECOND, start_time, finish_time))/3600 as workTime,
           date_format(start_time, '%Y-%m-%d')                 as day
    FROM db_prod_tt01.tt_log
             LEFT JOIN db_prod_tt01.tt_user_info USING (user_id)
    WHERE start_time >= '2022-01-01'
      AND deleted_at IS NULL
      AND start_time <= '2022-11-09'
      AND date_format(start_time, '%a') NOT IN ('Sun', 'Sat')
    GROUP BY user_id, last_name, day) AS tab1
        LEFT JOIN db_prod_tt01.tt_user_vacations ON user_id = tab1.user AND tt_user_vacations.vacation_day = tab1.day
    WHERE user_id IS NULL
    AND tab1.workTime < 8
    GROUP BY user, last_name, first_name
    ORDER BY last_name;
    ```
    
    Узнать, корректировали ли время пользователю в выбранный период
    
    ```jsx
    SELECT user_id,
           start_time,
           finish_time,
           date_format(start_time, '%Y-%m-%d') as date,
           deleted_at,
           last_name,
           first_name
    FROM db_prod_tt01.tt_log
             LEFT JOIN db_prod_tt01.tt_user_info USING (user_id)
    WHERE user_id = 109 AND start_time >= '2023-01-01' AND start_time <= '2023-08-01' AND deleted_at IS NOT NULL
    ORDER BY start_time asc;
    ```