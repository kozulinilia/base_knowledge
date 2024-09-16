# DOCKER

## Не пулится rpc контейнер repository does not exist or may require 'docker login’

Зайти в docker-compose.yml и закомментить образ на билд из фпм контейнера

![Untitled](Работа/Backend/DOCKER%205b156fd98c43445baeeb7ffff88ea50f/Untitled.png)

## Как запустить пустой докер контейнер проекта[(up)](%D0%94%D0%BE%D0%BA%D0%B5%D1%80%20%D0%B1%D0%B5%D0%BA%D0%B5%D0%BD%D0%B4%D1%8B%201cac185a613044b28d4c8ec0b83332ae.md)

![Untitled](Работа/Backend/DOCKER%205b156fd98c43445baeeb7ffff88ea50f/Untitled%201.png)

закомментитть строчку docker-compose.yml 

`entrypoint: ["/etc/rc.d/init.d/init.sh"]` 

и раскомментить

`*entrypoint: ["tail", "-f", "/dev/null"]*`

## Как смотреть логи контейнера[(up)](%D0%94%D0%BE%D0%BA%D0%B5%D1%80%20%D0%B1%D0%B5%D0%BA%D0%B5%D0%BD%D0%B4%D1%8B%201cac185a613044b28d4c8ec0b83332ae.md)

```jsx
vi var/logs/backend-2023-08-21.log
```

## Партейнер[(вверх)](%D0%94%D0%BE%D0%BA%D0%B5%D1%80%20%D0%B1%D0%B5%D0%BA%D0%B5%D0%BD%D0%B4%D1%8B%201cac185a613044b28d4c8ec0b83332ae.md)

Установка партейнера

```jsx
# sudo docker volume create portainer_data
```

Запуск партейнера

```jsx
# sudo docker run -d -p 9000:9000 --name portainer-ce --restart always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce
```

## Как узнать ошибки на докере, когда он отдает 500[(up)](%D0%94%D0%BE%D0%BA%D0%B5%D1%80%20%D0%B1%D0%B5%D0%BA%D0%B5%D0%BD%D0%B4%D1%8B%201cac185a613044b28d4c8ec0b83332ae.md)

Зайти в базу докер контейнера и посмотреть коллеекцию логгер. Там будут ошибки контейнера

Еще может не хватать параметров в настройках

Еще если арена, то могут неправильно прописаться в конфиге параметры монги в `./config/arena/parameters.yml`

должно быть

```sql
mongodb_dsn_default: 'mongodb://mongodb:27017/kozulin_eft_local_IKOZULIN'
```

После исправления проверить .что строка поменялась, напрямую в контейнере.

## Сохранить докер образ локально

Если нужно сохранить образ локально

```jsx
docker save <image_name> > <image_name>.tar
//например
docker save verse_gapminder > verse_gapminder.tar
```

если нужно загрузить с локальной копии образ

```jsx
docker load --input <image_name>.tar
//например
docker load --input verse_gapminder.tar
```

## Освободить место в wsl не работает docker

Посмотреть свободное место в wsl

```php
df -h
```

![Untitled](Работа/Backend/DOCKER%205b156fd98c43445baeeb7ffff88ea50f/Untitled%202.png)

очиcnbnm данные только которые не используются

```php
sudo docker image prune -a
```

```php
sudo docker builder prune
```

![Untitled](Работа/Backend/DOCKER%205b156fd98c43445baeeb7ffff88ea50f/Untitled%203.png)

![Untitled](Работа/Backend/DOCKER%205b156fd98c43445baeeb7ffff88ea50f/Untitled%204.png)

```php
sudo docker volume prune
```

## Перезапуск докер контейнеров без кеша[**(вверх)**](%D0%94%D0%BE%D0%BA%D0%B5%D1%80%20%D0%B1%D0%B5%D0%BA%D0%B5%D0%BD%D0%B4%D1%8B%201cac185a613044b28d4c8ec0b83332ae.md)

Выполнить в консоли

sudo docker-compose down -v
sudo docker-compose build --no-cache`
sudo docker-compose up -d

### Посмотреть статистику потребления ресурсов контейнеров

```sql
docker stats
```

[docker stats](https://docs.docker.com/engine/reference/commandline/stats/)

## Как локально запустить партейнер[(вверх)](%D0%94%D0%BE%D0%BA%D0%B5%D1%80%20%D0%B1%D0%B5%D0%BA%D0%B5%D0%BD%D0%B4%D1%8B%201cac185a613044b28d4c8ec0b83332ae.md)

- Скачать партейнер
    
    ```php
    https://www.portainer.io/installation/
    ```
    
- Запустить в терминале
    
    ```php
    sudo docker volume create portainer_data
    ```
    
    ```php
    sudo docker run -d -p 9000:9000 --name portainer-ce --restart always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce
    ```