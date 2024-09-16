# MySQL-Doctrine

- Запрос через entity-manager в репозитории
    
    ```php
    /**
     *@paramint$userId
    *@param\DateTimeInterface$startOfPeriod
    *@param\DateTimeInterface$endOfPeriod
    *
     *@returnarray
     */
    public function loadLogsByUserId(int $userId, \DateTimeInterface $startOfPeriod, \DateTimeInterface $endOfPeriod): array
    {
       $query = $this->_em->createQuery(
          'SELECT l
             FROM App\Entity\TimeLog l
             WHERE l.startTime >= :startOfPeriod
             AND l.startTime <= :endOfPeriod
             AND l.userId = :userId
             ORDER BY l.startTime DESC'
       )
          ->setParameters([
             'startOfPeriod' => $startOfPeriod,
             'endOfPeriod'   => $endOfPeriod,
             'userId'       => $userId,
          ]);
    
       return $query->getResult();
    }
    ```
    
- Запрос через query-builder в репозитории
    
    ```php
    /**
     *@paramint$departmentId
    *@returnDepartment|null
     */
    public function findOneById(int $departmentId): ?Department
    {
       return $this->createQueryBuilder('d')
          ->andWhere('d.id = :val')
          ->setParameter('val', $departmentId)
          ->getQuery()
          ->getOneOrNullResult();
    }
    ```
    
- Добавление или обновление данных в репозитории
    
    ```php
    /**
     *@paramDepartment$department
    *@returnint
     */
    public function updateDepartment(Department $department): int
    {
       $this->_em->persist($department);
       $this->_em->flush();
    
       return $department->getId();
    }
    ```
    
- Пример миграции
    
    ```php
    /**
     * Auto-generated Migration: Please modify to your needs!
     */
    final class Version20211125004647 extends AbstractMigration
    {
        public function getDescription(): string
        {
            return '';
        }
    
        public function up(Schema $schema): void
        {
            // this up() migration is auto-generated, please modify it to your needs
            $this->addSql('CREATE TABLE tt_log (id INT AUTO_INCREMENT NOT NULL, user_id INT DEFAULT NULL, start_time DATETIME NOT NULL, finish_time DATETIME DEFAULT NULL, INDEX IDX_CE3306589D86650F (user_id), PRIMARY KEY(id)) DEFAULT CHARACTER SET utf8mb4 COLLATE `utf8mb4_unicode_ci` ENGINE = InnoDB');
            $this->addSql('ALTER TABLE tt_log ADD CONSTRAINT FK_CE3306589D86650F FOREIGN KEY (user_id) REFERENCES tt_user (id)');
            $this->addSql('ALTER TABLE tt_user RENAME INDEX uniq_8d93d649f85e0677 TO UNIQ_36C35DEF85E0677');
        }
    
        public function down(Schema $schema): void
        {
            // this down() migration is auto-generated, please modify it to your needs
            $this->addSql('DROP TABLE tt_log');
            $this->addSql('ALTER TABLE tt_user RENAME INDEX uniq_36c35def85e0677 TO UNIQ_8D93D649F85E0677');
        }
    }
    ```
    
- Команды миграции в Symfony
    
    ```php
    #создать сущность через doctrine
    php bin/console make:entity
    
    #создать миграцию на основе созданной сущности
    php bin/console make:migration
    
    #запустить миграцию(процесс создания коллекции в БД MySQL
    php bin/console doctrine:migrations:migrate
    
    #откатить миграцию на одну назад
    php bin/console doctrine:migrations:migrate prev
    
    #вычислить разницу между состояниями миграций и состояниями сущностей и на основе разницы создать миграцию
    php bin/console doctrine:migrations:diff
    ```
    
- Вывод всех данных, у которых поле deleteAt равно NULL
    
    ```php
    /**
     *@paramint$userId
    *@param\DateTimeInterface$startOfPeriod
    *@param\DateTimeInterface$endOfPeriod
    *
     *@returnarray
     */
    public function loadLogsByUserId(int $userId, \DateTimeInterface $startOfPeriod, \DateTimeInterface $endOfPeriod): array
    {
       $query = $this->_em->createQuery(
          'SELECT l
             FROM App\Entity\TimeLog l
             WHERE l.startTime >= :startOfPeriod
             AND l.startTime <= :endOfPeriod
             AND l.userId = :userId
             AND l.deleteAt IS NULL
             ORDER BY l.startTime DESC'
       )
          ->setParameters([
             'startOfPeriod' => $startOfPeriod,
             'endOfPeriod'   => $endOfPeriod,
             'userId'       => $userId,
          ]);
    
       return $query->getResult();
    }
    ```
    
- Фиктивное Удаление элемента через изменение deleteAt
    
    ```php
    /**
     *@paramint$shortWorkId
    *@returnint|null
     */
    public function deleteShortDay(int $shortWorkId): ?int
    {
       if ($shortDay = $this->shortWorkDayRepository->findOneBy(['id' => $shortWorkId])) {
          $this->shortWorkDayRepository->updateShortDay($shortDay->setDeleteAt(new DateTime()));
    
          return $shortWorkId;
       }
    
       return null;
    }
    ```
    
- Примеры запросов
    
    Запрос с аггрегацией и присоединением таблички
    
    ```sql
    SELECT last_name,
           first_name,
           sum(TIMESTAMPDIFF(SECOND, start_time, finish_time))/3600 as workTime,
           date_format(start_time, '%Y-%m-%d')                 as day
    FROM db_prod_tt01.tt_log
             LEFT JOIN db_prod_tt01.tt_user_info USING (user_id)
    WHERE start_time >= '2021-09-20'
      AND deleted_at IS NULL
      AND start_time <= '2021-09-22'
    GROUP BY user_id, last_name, day
    ORDER BY last_name ASC
    ```
    
    ```sql
    SELECT  last_name, first_name, count(vacation_day)  as count
    FROM db_dev_tt02.tt_user_vacations
             LEFT JOIN db_dev_tt02.tt_user_info USING(user_id)
    WHERE vacation_day > '2021-01-01' AND vacation_day < '2021-12-31'
    GROUP BY user_id
    ```