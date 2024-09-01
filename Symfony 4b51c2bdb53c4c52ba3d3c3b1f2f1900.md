# Symfony

[План по созданию своего проекта](Symfony%204b51c2bdb53c4c52ba3d3c3b1f2f1900/%D0%9F%D0%BB%D0%B0%D0%BD%20%D0%BF%D0%BE%20%D1%81%D0%BE%D0%B7%D0%B4%D0%B0%D0%BD%D0%B8%D1%8E%20%D1%81%D0%B2%D0%BE%D0%B5%D0%B3%D0%BE%20%D0%BF%D1%80%D0%BE%D0%B5%D0%BA%D1%82%D0%B0%203787bca0e66544948dd2ebffbca631e0.md)

[Журнал по проекту](Symfony%204b51c2bdb53c4c52ba3d3c3b1f2f1900/%D0%96%D1%83%D1%80%D0%BD%D0%B0%D0%BB%20%D0%BF%D0%BE%20%D0%BF%D1%80%D0%BE%D0%B5%D0%BA%D1%82%D1%83%20cd56115f1910492883db4f6a4ef0901c.md)

[Команды консоли ](Symfony%204b51c2bdb53c4c52ba3d3c3b1f2f1900/%D0%9A%D0%BE%D0%BC%D0%B0%D0%BD%D0%B4%D1%8B%20%D0%BA%D0%BE%D0%BD%D1%81%D0%BE%D0%BB%D0%B8%20ea8c2198b5744586a136a32374d8dc30.md)

[Ссылки](Symfony%204b51c2bdb53c4c52ba3d3c3b1f2f1900/%D0%A1%D1%81%D1%8B%D0%BB%D0%BA%D0%B8%20cee70dba082a49038c1c87dab2e3ad44.md)

## Установка в проект [(up)](Symfony%204b51c2bdb53c4c52ba3d3c3b1f2f1900.md)

в директории проекта

```jsx
curl -1sLf 'https://dl.cloudsmith.io/public/symfony/stable/setup.deb.sh' | sudo -E bash
sudo apt install symfony-cli
```

[установить PHP](https://www.notion.so/PHP-8ce8cb421d7b4fa6887b26eb74c73e64?pvs=21)

[установить расширение xml php](https://www.notion.so/PHP-8ce8cb421d7b4fa6887b26eb74c73e64?pvs=21)

установка композера в директорию проекта

```jsx
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php -r "if (hash_file('sha384', 'composer-setup.php') === '55ce33d7678c5a611085589f1f3ddf8b3c52d662cd01d4ba75c0ee0459970c2200a51f492d557530c71c15d8dba01eae') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
php composer-setup.php
php -r "unlink('composer-setup.php');"
sudo mv composer.phar /usr/local/bin/composer
```

установить композер

```bash
sudo apt install composer
```

создать новое приложение

веб приложение

```jsx
composer create-project symfony/skeleton my_project -s dev
```

## Настройка встроенного веб-сервера Symfony [(up)](Symfony%204b51c2bdb53c4c52ba3d3c3b1f2f1900.md)

Установка certutils

```bash
apt install libnss3-tools
```

Установка защищенного сертификата

```bash
symfony server:ca:install
```

- Настройка прокси сервера
    
    Зайти в настройки сети
    
    ![Untitled](Symfony%204b51c2bdb53c4c52ba3d3c3b1f2f1900/Untitled.png)
    
    Выать прокси сервер, метод автоматический и вставить 
    
    ```bash
    http://127.0.0.1:7080/proxy.pac
    ```
    
    ![Untitled](Symfony%204b51c2bdb53c4c52ba3d3c3b1f2f1900/Untitled%201.png)
    
    Затем запустить в проекте прокси симфони
    
    ```bash
    symfony proxy:start
    ```
    
    Затем перезагрузить симфони сервер
    
    ```bash
    symfony server:stop
    symfony server:start
    ```
    
- установить именованый домен на локальный сервер
    
    ```bash
    symfony proxy:domain:attach my-domain
    ```
    

## Установка и настройка Webpack Encore [(up)](Symfony%204b51c2bdb53c4c52ba3d3c3b1f2f1900.md)

установка yarn

```bash
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
sudo apt-get update && sudo apt-get install yarn
```

Установка npm

```bash
sudo apt install npm
```

**Installing Encore in Symfony Applications**

```bash
composer require symfony/webpack-encore-bundle
```

app.js симфони считает отдельным js приложением, которое затребует все остальные зависимости(Vue, React и т.д)

Все в Encore конфигурируется через `webpack.config.js`

Установка пакетного менеджера n

```bash
sudo npm install -g n
```

обновление Node js до последней версии

```bash
sudo n latest
```

если `node -v` показывает старую версию, скинуть хеш

```bash
hash -r
```

запустить билд фронта

```bash
npm run dev
```

## Cделать контроллер, в котором DI можно кидать в аргумент метода [(up)](Symfony%204b51c2bdb53c4c52ba3d3c3b1f2f1900.md)

![Untitled](Symfony%204b51c2bdb53c4c52ba3d3c3b1f2f1900/Untitled%202.png)

## Использование Bootstrap в проекте [(up)](Symfony%204b51c2bdb53c4c52ba3d3c3b1f2f1900.md)

установить бутстрап

```bash
npm install bootstrap --save-dev
```

установить бутстрап вью

```jsx
npm install vue bootstrap bootstrap-vue
```

## Подключение Vue в проект [(up)](Symfony%204b51c2bdb53c4c52ba3d3c3b1f2f1900.md)

First enable it in `webpack.config.js`

```bash
// webpack.config.js
  // ...

  Encore
      // ...
      .addEntry('main', './assets/main.js')

+     .enableVueLoader()
  ;
```

Добавить настройки для версионирования

```jsx
// configure Babel
    // .configureBabel((config) => {
    //     config.plugins.push('@babel/a-babel-plugin');
    // })

    // enables and configure @babel/preset-env polyfills
    .configureBabelPresetEnv((config) => {
        config.useBuiltIns = 'usage';
        config.corejs = '3.23';
    })
    .configureFilenames({
        js: "js/[name].[fullhash:8].js",
        css: "css/[name].[fullhash:8].css"
    })
    .enableVueLoader(() => {}, {
        runtimeCompilerBuild: false
    })
```

Установить vue

```bash
npm install vue@^2.5
npm install vue-loader@^15.9.5
npm install vue-template-compiler --save-dev
```

Затем перезапустить Encore

```bash
npm run dev
```

Установить vuex

```bash
npm install vuex@3
```

Добавить файл управления сторой `store.js`

```jsx
import Vue from "vue"
import Vuex from "vuex"

Vue.use(Vuex)

const store = new Vuex.Store( {
    state: {

    },
    getters: {

    },
    mutations: {

    },
    actions: {

    }
})

export default store
```

Добавить стору в основной js файл(`app.js`)

```jsx
import store from "./store/store"
```

И в компонент Vue(`app.js`)

```jsx
const app = new Vue( {
    el: "#app",
    router: router,
    store: store,
})
```

Установить vue router

```php
npm install vue-router@3
```

Добавить файл с роутером(`routes.js`)

```jsx
import VueRouter from "vue-router";

const BooksList = () => import("./vue/Pages/BooksPage")

const routes = [
    {
        path: "/books",
        component: BooksList,
        meta: {}
    }
]

const router = new VueRouter({
    mode: "history",
    routes: routes
})

export {router}
```

Добавить `routes.js` в основной файл(`app.js`)

```jsx
import {router} from "./router"
```

И в компонент Vue(`app.js`)

```jsx
const app = new Vue( {
    el: "#app",
    router: router,
    store: store,
})
```

Установка axios

```php
npm i axios -S
```

Создание компонента навигации(`Navigation.vue`)

```jsx
<template>
<header class="p-3 bg-dark text-white">
  <div class="container">
    <b-navbar-nav align="center">
      <b-nav-item to="/books">Список книг</b-nav-item>
    </b-navbar-nav>
  </div>
</header>
</template>

<script>
export default {
  name: "Navigation"
}
</script>

<style scoped>

</style>
```

Создание основного вью компонента(`Main.vue`)

```jsx
<template>
  <div>
    <navigation></navigation>
    <router-view></router-view>
  </div>
</template>

<script>
import Navigation from "./vue/Navigation";
export default {
  name: "Main",
  components: {Navigation}
}
</script>

<style scoped>

</style>
```

Создание app.js корневого файла js в который монтируется Вью

```jsx
// any CSS you import will output into a single css file (app.css in this case)
import './styles/app.css';

// start the Stimulus application
import './bootstrap';

import Vue from 'vue'
import VueRouter from "vue-router"
import Navigation from "./vue/Navigation"
import store from "./store/store"
import {router} from "./router"
import {BootstrapVue, BootstrapVueIcons} from "bootstrap-vue"

import "bootstrap/dist/css/bootstrap.css"
import "bootstrap-vue/dist/bootstrap-vue.css"

import Main from "./Main"

Vue.use(VueRouter)
Vue.use(BootstrapVue)
Vue.use(BootstrapVueIcons)

const app = new Vue( {
    el: "#app",
    router: router,
    store: store,
    components: {Main},
    render: h => h(Main)
})
```

Добавление энтрипоинта для загрузки фронта

```php
<?php

namespace App\Controller;

use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Component\HttpFoundation\Response;
use Symfony\Component\Routing\Annotation\Route;

class AdminVueController extends AbstractController
{
    /**
     * @Route ("/", name="index", methods={"GET"})
     */
    public function index()
    {
        return $this->render('entry.twig');
    }
}
```

Внесение в проект шаблонизатора twig

```jsx

```

Создание темплейта, в который будет грузиться вью(`base.html.twig`)

```jsx
<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Учет книг</title>
    {% block stylesheets %}
        {{ encore entry_link_tags('app') }}
    {% endblock %}
</head>
<body>
    <div id="app"></div>

    {% block javascripts %}
        {{ encore_entry_script_tags('app') }}
    {% endblock %}
</body>
</html>
```

## Докеризация Symfony [(up)](Symfony%204b51c2bdb53c4c52ba3d3c3b1f2f1900.md)

создать в корне проекта файл `docker-compose.yml`

создать в проекте папку `docker`

- nginx
    
    создать в папке docker файл `Dockerfile-nginx` и заполнить его
    
    ```jsx
    FROM nginx:latest
    COPY build/nginx/default.conf /etc/nginx/conf.d/
    RUN echo "upstream php-upstream { server fpm:9000; }" > /etc/nginx/conf.d/upstream.conf
    RUN usermod -u 1000 www-data
    ```
    
    в этом скрипте 
    
    - тянется последний актуальный докер образ нджинкс,
    - копируется дефолтный файл конфигурации в папку контейнера
    - Then we configure the upstream to `php-upsteam`and `fpm`
     as a reference to the proxy and fastcgi_pass
    - явно задаем права , под какими работает контейнер, чтобы нельзя было зайти под рутом
    
    Создать две поддиректории `build/nginx`
    
    внутри nginx создать файл дефолтной конфигурации сервера `default.conf` с стандартной конфигурацией
    
    ```jsx
    server {
    listen 80;
    server_name  localhost;
    root /var/www/project/public;
    location / {
    try_files $uri @rewriteapp;
    }
    location @rewriteapp {
    rewrite ^(.*)$ /index.php/$1 last;
    }
    client_max_body_size 200M;
    location ~ ^/index.php(/|$) {
    fastcgi_pass php-upstream;
    fastcgi_split_path_info ^(.+\.php)(/.*)$;
    include fastcgi_params;
    fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    fastcgi_param HTTPS off;
    client_max_body_size 200M;
    fastcgi_read_timeout 600;
    }
    error_log /var/log/nginx/symfony_error.log;
    access_log /var/log/nginx/symfony_access.log;
    }
    ```
    
    Затем прописываем необходимый нам контейнер в `docker-compose.yml`
    
    ```jsx
    version: "3.7"
    services:
      nginx:
        restart: unless-stopped
        build:
          context: ./docker
          dockerfile: Dockerfile-nginx
        volumes:
          - ./:/var/www/project/:cached
        ports:
          - 10302:80 # Ports that are exposed, you can connect to port 8001 to port 80 of the container.
        networks:
          - symfony
    ```
    
    3.7 это версия композера
    
    мы добавили nginx сервис и ссылку на его докерфайл, прописали тома, порты и сеть.
    
- php-fpm
    
    [**FPM**](https://www.php.net/manual/en/install.fpm.php)
     (FastCGI Process Manager) is an alternative **PHP**
     FastCGI implementation with some additional features (mostly) useful for heavy-loaded sites. We will use it to install PHP and add configurations. It works perfectly with Nginx
    
    внутри папки `docker` создаем файл `Dockerfile-php`
    
    ```jsx
    FROM php:7.4-fpm
    RUN apt-get update && apt-get install -y --no-install-recommends \
        git \
        zlib1g-dev \
        libxml2-dev \
        libpng-dev \
        libzip-dev \
        vim curl debconf subversion git apt-transport-https apt-utils \
        build-essential locales acl mailutils wget nodejs zip unzip \
        gnupg gnupg1 gnupg2 \
        sudo \
        ssh \
        && docker-php-ext-install \
        pdo_mysql \
        soap \
        zip \
        opcache \
        gd \
        intl \
        && pecl install mongodb && docker-php-ext-enable mongodb \
        && docker-php-ext-enable mcrypt
    COPY build/php/opcache.ini /usr/local/etc/php/conf.d/
    COPY build/php/custom.ini /usr/local/etc/php/conf.d/
    RUN curl -sS https://getcomposer.org/installer | php && mv composer.phar /usr/local/bin/composer
    RUN composer self-update 1.9.0
    RUN wget --no-check-certificate https://phar.phpunit.de/phpunit-6.5.3.phar && \
    mv phpunit*.phar phpunit.phar && \
    chmod +x phpunit.phar && \
    mv phpunit.phar /usr/local/bin/phpunit
    RUN usermod -u 1000 www-data
    RUN usermod -a -G www-data root
    RUN mkdir -p /var/www
    RUN chown -R www-data:www-data /var/www
    RUN mkdir -p /var/www/.composer
    RUN chown -R www-data:www-data /var/www/.composer
    RUN echo "extension=mongodb.so" >> /usr/local/etc/php/php.ini
    WORKDIR /var/www/project/
    ```
    
    здесь мы в основном вытягиваем версию php-fpm 7.4 и подключаем разные зависимости 
    
    так же здесь используются два конфигурационных файла 
    
    `build/php/opcache.ini` для конфигурации кеша и `build/php/custom.ini` для расширенной настройки конфигурации PHP. Эти два файла не обязательны, поэтому их настройку можно пропустить
    
    добавляем в папку `docker/build/php` файл `opcache.ini`
    
    ```jsx
    opcache.enable=1
    opcache.enable_cli=0
    opcache.memory_consumption=256
    opcache.max_accelerated_files=100000
    ```
    
    в эту же папку `docker/build/php` добавляем `custom.ini`
    
    ```jsx
    date.timezone = "Europe/Moscow"
    memory_limit = 4096M
    upload_max_filesize = 200M
    post_max_size = 200M
    ```
    
    Так же как и с nginx нам нужно добавить как сервис php-fpm в `docker-compose.yml` файл
    
    ```jsx
    fpm:
        restart: unless-stopped
        build: # Info to build the Docker image
          context: ./docker # Specify where the Dockerfile is located (e.g. in the root directory of the project)
          dockerfile: Dockerfile-php # Specify the name of the Dockerfile
        environment: # You can use this section to set environment variables. But you can also use the .env file.
           - DATABASE_URL=mysql://root:root@db/docker_sf
        volumes:
          - ./:/var/www/project/:cached # Location of the project for php-fpm. Note this should be the same for NGINX.*
        networks:
          - symfony # Docker containers (services) that need to connect to each other should be on the same network.
    ```
    
    Мы связали fpm сервис с томами, докерфайлом и т.д.
    
    в поле `environment` мы можем задать необходимые переменные окружения
    
- MariaDb
    
    конфигурация базы данных прописываем напрямую в `docker-compose.yml` файле
    
    ```jsx
    db: #address
    		restart: unless-stopped
        image: mariadb:10.2.29
        ports:
          - "3317:3306" #outside:inside docker container from-within
        environment:
          - MYSQL_DATABASE=docker_sf
          - MYSQL_ROOT_PASSWORD=root
        volumes:
          - persistent:/var/lib/mysql
          - ./docker/build/db/:/docker-entrypoint-initdb.d/
        networks:
          - symfony
    ```
    

Полное содержание `docker-compose.yml`

```jsx
version: "3.7"
services:
  nginx:
    restart: unless-stopped
    build:
      context: ./docker
      dockerfile: Dockerfile-nginx
    volumes:
      - ./:/var/www/project/:cached
    ports:
      - 10302:80 # по этому порту и адресу можно подключиться к серваку http://127.0.1.1:10302/
    networks:
      - symfony

  fpm:
    restart: unless-stopped
    build: # Info to build the Docker image
      context: ./docker # Specify where the Dockerfile is located (e.g. in the root directory of the project)
      dockerfile: Dockerfile-php # Specify the name of the Dockerfile
    environment: # You can use this section to set environment variables. But you can also use the .env file.
       - "MONGODB_PORT=27017"
       - "MONGODB_USER=${MONGODB_USER}"
       - "MONGODB_PASS=${MONGODB_PASS}"
    volumes:
      - ./:/var/www/project/:cached # Location of the project for php-fpm. Note this should be the same for NGINX.*
    networks:
      - symfony # Docker containers (services) that need to connect to each other should be on the same network

  mongo:
    image: mongo:4.4.6
    container_name: mongodb
    restart: unless-stopped
    volumes:
      - mongodb_data:/var/lib/mongodb:rw
    ports:
      - "27017:27017"
    networks:
      - symfony

networks:
  symfony:

volumes:
  persistent:
  mongodb_data:
```

Чтобы сбилдить контейнеры нужно выполнить команду

```jsx
docker-compose build
```

после этого можно запустить как сервис

```jsx
docker-compose up -d
```

И наконец чтобы войти в свой контейнер

```jsx
docker-compose exec -u 1000 fpm bash
```

## Подключение БД в Symfony [(up)](Symfony%204b51c2bdb53c4c52ba3d3c3b1f2f1900.md)

### MongoDB

установка расширения PHP для mongoDb

```jsx
sudo pecl install mongodb
```

Добавить расширение

- докер php-fpm
    
    добавить строчки в докер файл php-fpm
    
    ```jsx
    FROM php:8.0-fpm
    RUN apt-get update && apt-get install -y --no-install-recommends \
        git \
        zlib1g-dev \
        libxml2-dev \
        libpng-dev \
        libzip-dev \
        vim curl debconf subversion git apt-transport-https apt-utils \
        build-essential locales acl mailutils wget nodejs zip unzip \
        gnupg gnupg1 gnupg2 \
        sudo \
        ssh \
        && docker-php-ext-install \
        pdo_mysql \
        soap \
        zip \
        opcache \
        gd \
        intl \
        && pecl install mongodb  \
        && docker-php-ext-enable mongodb \
        && pecl install xdebug \
        && docker-php-ext-enable xdebug
    RUN apk add curl-dev openssl-dev
    RUN pecl config-set php_ini /etc/php.ini
    COPY build/php/opcache.ini /usr/local/etc/php/conf.d/
    COPY build/php/custom.ini /usr/local/etc/php/conf.d/
    RUN curl -sS https://getcomposer.org/installer | php && mv composer.phar /usr/local/bin/composer
    RUN composer self-update 2.0.0
    RUN wget --no-check-certificate https://phar.phpunit.de/phpunit-6.5.3.phar && \
    mv phpunit*.phar phpunit.phar && \
    chmod +x phpunit.phar && \
    mv phpunit.phar /usr/local/bin/phpunit
    RUN usermod -u 1000 www-data
    RUN usermod -a -G www-data root
    RUN mkdir -p /var/www
    RUN chown -R www-data:www-data /var/www
    RUN mkdir -p /var/www/.composer
    RUN chown -R www-data:www-data /var/www/.composer
    RUN echo "extension=mongodb.so" >> /usr/local/etc/php/php.ini
    WORKDIR /var/www/project/
    ```
    
    перебилдить контейнер php-fpm
    
- Добавить образ монго контейнера в `docker-compose.yml`
    
    Добавить образ монго контейнера в `docker-compose.yml`
    
    ```jsx
    mongo:
        image: mongo:4.4.6
        container_name: mongodb
        restart: unless-stopped
    		environment:
          MONGO_INITDB_ROOT_USERNAME: ${MONGODB_USER}
          MONGO_INITDB_ROOT_PASSWORD: ${MONGODB_PASS}
        volumes:
          - mongodb_data:/var/lib/mongodb:rw
        ports:
          - "27017:27017"
        networks:
          - symfony
    ```
    
    где mongodb - имя контейнера, к которому будут коннектиться
    
    27017 стандартный порт для монги
    
    ```jsx
    mongodb:27017
    ```
    
    MONGO_INITDB_ROOT_USERNAME и MONGO_INITDB_ROOT_PASSWORD
    
    [https://hub.docker.com/_/mongo](https://hub.docker.com/_/mongo)
    
    пароль и логин рутового пользователя для монги
    
- Установка расширений для монги
    
    зайти в контейнер php-fpm и установить через Composer
    
    ```jsx
    sudo apt-get install php-mongodb
    composer require ext-mongodb ^1.5
    composer require doctrine/mongodb-odm ^2.0.0
    composer require doctrine/mongodb-odm-bundle
    ```
    
    Добавиться файл `config/packages/doctrine_mongodb.yaml`
    
    Добавятся записи в `.env` и в  `*config/services.yaml*`
    
- Правка в конфиге
    
    Добавить в `*config/services.yaml`* строчку
    
    ```jsx
    parameters:
        mongodb_server: "mongodb://%env(MONGODB_USER)%:%env(MONGODB_PASS)%@localhost:27017"
    ```
    
    Где `MONGODB_USER` и `MONGODB_PASS` параметры из `.env`
    
- Правка в докер файле php-fpm
    
    Так же добавить 2 строчки в докер файл fpm, который сделает enable SSL для монги
    
    ```jsx
    ssh \
        && docker-php-ext-install \
        pdo_mysql \
        soap \
        zip \
        opcache \
        gd \
        intl \
        && pecl install mongodb  \
        && docker-php-ext-enable mongodb \
        && pecl install xdebug \
        && docker-php-ext-enable xdebug
    RUN apk add curl-dev openssl-dev
    RUN pecl config-set php_ini /etc/php.ini
    COPY build/php/opcache.ini /usr/local/etc/php/conf.d/
    COPY build/php/custom.ini /usr/local/etc/php/conf.d/
    RUN curl -sS https://getcomposer.org/installer | php && mv composer.phar /usr/local/bin/composer
    ```
    
- Настройка enable SSL для монги(иначе не будет писать туда)
    
    On Ubuntu 18.04 LTS, PHP 7.2.7
    
    I had to install some extra packages:
    
    ```csharp
    sudo apt-get install -y libcurl4-openssl-dev pkg-config libssl-dev
    
    ```
    
    Then re-install mongodb:
    
    ```
    sudo pecl uninstall mongodb
    sudo pecl install mongodb
    
    ```
    
    Then check that SSL is enabled:
    
    ```perl
    php -i | grep mongo
    
    ```
    
    > /etc/php/7.2/cli/conf.d/20-mongodb.ini, mongodb
    > 
    > 
    > libmongoc bundled version => 1.11.0 libmongoc SSL => enabled libmongoc SSL library => OpenSSL libmongoc crypto => enabled libmongoc crypto library => libcrypto
    > 
    
    Restart php:
    
    ```protobuf
    sudo service php7.2-fpm restart
    ```
    
    **6**
    
    For those who have the error on an **Alpine docker**. You must add in your **Dockerfile** :
    
    `RUN` `apt-get update && apt-get install curl-dev openssl-dev`
    
    instead of sudo apt-get install -y libcurl4-openssl-dev pkg-config libssl-dev
    
    I used `RUN pecl config-set php_ini /etc/php.ini` too
    
- Если хочется брать креды из базы
    
    в конце можно добавить ?authSource=auth-db, как в примере
    
    ```jsx
    "mongodb://%env(MONGODB_USER)%:%env(MONGODB_PASS)%@localhost:27017/?authSource=auth-db"
    ```
    
    но это если у вас будет база с кредами пользователей монги. Соответственно ее надо создать сразу в контейнере монги.
    
- Если стоит доктрина бандл там можно добавить использование memcache
    
    Добавить использование memcache для mongodb
    
    ```jsx
    document_managers:
            default:
                auto_mapping: true
                mappings:
                    mappings:
                        AcmeDemoBundle: ~
                    metadata_cache_driver:
                        type: memcache
                        class: Doctrine\Common\Cache\MemcacheCache
                        host: localhost
                        port: 11211
                        instance_class: Memcache
                    App:
                        is_bundle: false
                        dir: '%kernel.project_dir%/src/Document'
                        prefix: 'App\Document'
                        alias: App
    ```
    
    Единственная необходимая здесь настройка - это описание сопоставляемых документов. И для этого есть несколько опций:
    
    - `type` One of `annotation`, `attribute`, `xml`, `php` or `staticphp`. This specifies which type of metadata type your mapping uses.
    - `dir` Path to the mapping or document files (depending on the driver). If this path is relative it is assumed to be relative to the bundle root. This only works if the name of your mapping is a bundle name. If you want to use this option to specify absolute paths you should prefix the path with the kernel parameters that exist in the DIC (for example `%kernel.project_dir%`).
    - `prefix` A common namespace prefix that all documents of this mapping share. This prefix should never conflict with prefixes of other defined mappings otherwise some of your documents cannot be found by Doctrine. This option defaults to the bundle namespace + `Document`, for example for an application bundle called `AcmeHelloBundle`, the prefix would be `Acme\HelloBundle\Document`.
    - `alias` Doctrine offers a way to alias document namespaces to simpler, shorter names to be used in queries or for Repository access.
    - `is_bundle` This option is a derived value from `dir` and by default is set to true if dir is relative proved by a `file_exists()` check that returns false. It is false if the existence check returns true. In this case an absolute path was specified and the metadata files are most likely in a directory outside of a bundle.
- Как создавать документы
    
    Чтобы с документами не было путаницы, лучше придерживаться следующих соглашений
    
    - располагайте документы в папке `Documents`, например `/src/Documents`
    - если вы используете xml yml или php маппинг, расположите все ваши конфигурационные файлы в какой-либо директории по пути `Resources/config/doctrine/` с названием `mongodb.xml`, `mongodb.yml` или `mongodb.php` соответственно.
    - Аннотации работают если найден каталог `Documents\` но не найден `config/doctrine/` или `Resources/config/doctrine/`
    
    Создание документа
    
    ```jsx
    namespace App\Document;
    
    class Book
    {
        protected $name;
    
        protected $price;
    }
    ```
    
- Просмотр базы данных в PHPStorm
    
    ![Untitled](Symfony%204b51c2bdb53c4c52ba3d3c3b1f2f1900/Untitled%203.png)
    
    В УРЛ вводим
    
    ```jsx
    mongodb://localhost:27017
    ```
    
    В настройках БД указать пользователя и пароль и жмем Test Connnections
    
    ![Untitled](Symfony%204b51c2bdb53c4c52ba3d3c3b1f2f1900/Untitled%204.png)
    
    Когда он напишет Success, закрываем.
    
    Потом выбираем, какую БД отображать в выборе схемы
    
    ![Untitled](Symfony%204b51c2bdb53c4c52ba3d3c3b1f2f1900/Untitled%205.png)
    
    [https://linuxhint.com/how-to-run-a-mongodb-server-with-docker-compose/#1](https://linuxhint.com/how-to-run-a-mongodb-server-with-docker-compose/#1)
    

## PostgreSql

добавляем в docker-compose.yaml

```bnf
  postgresql:
    image: postgres
    restart: always
    # set shared memory limit when using docker-compose
    shm_size: 128mb
    # or set shared memory limit when deploy via swarm stack
    #volumes:
    #  - type: tmpfs
    #    target: /dev/shm
    #    tmpfs:
    #      size: 134217728 # 128*2^20 bytes = 128Mb
    environment:
      POSTGRES_PASSWORD: ${POSTGRES_PASSWORD}
      POSTGRES_USER: ${POSTGRES_USER}
      POSTGRES_DB: ${POSTGRES_DB}

  adminer:
    image: adminer
    restart: always
    ports:
      - 8080:8080

```

прописываем в энве параметры

```bnf
POSTGRES_DB="postgreDb"
POSTGRES_PASSWORD="Silentium2"
POSTGRES_USER="archivist"
```

в services.yaml

```bnf
parameters:
    postgre_db_url: "postgresql://%env(POSTGRES_USER)%:%env(POSTGRES_PASSWORD)%@127.0.0.1:5432/%env(POSTGRES_DB)%?serverVersion=15&charset=utf8"

```

заходим на [localhost:8080](http://localhost:8080) и попадаем в админер

## Партейнер [(up)](Symfony%204b51c2bdb53c4c52ba3d3c3b1f2f1900.md)

Установка партейнера

```jsx
# sudo docker volume create portainer_data
```

Запуск партейнера

```jsx
# sudo docker run -d -p 9000:9000 --name portainer-ce --restart always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce
```

в браузере перейти по ссылке [localhost:900](http://localhost:9000)0

создать аккаунт и зайти в проект

## Аутентификация [(up)](Symfony%204b51c2bdb53c4c52ba3d3c3b1f2f1900.md)

Устанавливаем бандл безопасности в контейнере

```jsx
composer require symfony/security-bundle
```

Сначала создать класс User

Для этого зайти в контейнер php-fpm и установить **The Symfony MakerBundle**

```jsx
composer require --dev symfony/maker-bundle
```

Далее так же в контейнере создаем пользователя

```jsx
php bin/console make:user
```

![Untitled](Symfony%204b51c2bdb53c4c52ba3d3c3b1f2f1900/Untitled%206.png)

Бандл спросит имя класса пользователя, хотите ли хранить пользователей через доктрину, уникальное поле ,по которому разделяются пользователи и хешировать ли пароль.

Бандл создаст класс юзер и юзер провайдер, сгенерирует файл security.yaml с настройками безопасности и что-то еще.

- настройка урлов, на которые попадает неавторизованный пользователь
    
    чтобы прописать урлы, по которым будет редиректится неавторизованный пользователь, надо их прописать в security.yaml
    
    ```jsx
    firewalls:
            dev:
                pattern: ^/(_(profiler|wdt)|css|images|js)/
                security: false
            main:
                lazy: true
                provider: app_user_provider
                form_login:
                    login_path: /login
                    check_path: /login
    ```
    
- 

## Установка xdebug [(up)](Symfony%204b51c2bdb53c4c52ba3d3c3b1f2f1900.md)

[https://matthewsetter.com/setup-step-debugging-php-xdebug3-docker/](https://matthewsetter.com/setup-step-debugging-php-xdebug3-docker/)

Добавить в `docker/Dockerfile-php` в php-fpm

```jsx
RUN pecl install xdebug \
    && docker-php-ext-enable xdebug
```

![Untitled](Symfony%204b51c2bdb53c4c52ba3d3c3b1f2f1900/Untitled%207.png)

создать файлы `docker/php/conf.d/xdebug.ini` и `docker/php/conf.d/error_reporting.ini` в проекте

в xdebug.ini добавить код

```jsx
zend_extension=xdebug

[xdebug]
xdebug.mode=develop,debug
xdebug.client_host=host.docker.internal
xdebug.start_with_request=yes
```

- **mode** [**This setting**](https://xdebug.org/docs/all_settings#mode) controls which Xdebug features are enabled. We’ve set `develop` to enable [**development aids**](https://xdebug.org/docs/develop), such as getting better error messages, and `debug` to enable step debugging.
- **client_host** [**This setting**](https://xdebug.org/docs/all_settings#client_host) tells Xdebug the IP address or hostname of the machine that is running your text editor or IDE.
- **start_with_request** [**This setting**](https://xdebug.org/docs/all_settings#client_host) determines whether a function trace, garbage collection statistics, profiling, or step debugging are activated at the start of a PHP request. Setting it to `yes` instructs Xdebug to always initiate a debugging session.

в файле`error_reporting.ini` добавить

```jsx
error_reporting=E_ALL
```

Затем в docker-compose.yml добавить два тома 

```jsx
- ./docker/php/conf.d/xdebug.ini:/usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
- ./docker/php/conf.d/error_reporting.ini:/usr/local/etc/php/conf.d/error_reporting.ini
```

![Untitled](Symfony%204b51c2bdb53c4c52ba3d3c3b1f2f1900/Untitled%208.png)

Затем перезапустить контейнеры с билдом

```jsx
docker-compose up -d --build php
```

флаг `--build` означает, что образы будут сбилжены перед запуском контейнера

Далее можно запускать xdebug в шторме

Если брейкпоинты не ловятся, то можно перед брейк-поинтом прописать такой код

```jsx
xdebug_info();
exit;
```

```jsx
php-fpm:
    build: docker_local/php-fpm
    restart: unless-stopped
    container_name: app-php
    tmpfs:
      - /opcache_files
    volumes:
      - ${LOCAL_PATH}:/www_store:rw
      - xhprof:/xhprof:rw
      - gitdumper:/gitdump/gitdumper:rw
      - gitdumper_autosave:/gitdump/gitdumper_autosave:rw
      - /xhprof_log
      - local_infra_be_var_logs:/www_store/var/logs:rw
      - ${SSH_KEY_PATH}:/srv/.ssh/id_rsa.key
    environment:
      - "BACKEND_NAME=${NET}_${INSTANCE_NAME}"
      - "BACKEND_DEFAULT_USER=${BACKEND_DEFAULT_USER}"
      - "BACKEND_DEFAULT_PASS=${BACKEND_DEFAULT_PASS}"
      - "RPC_KEY=${RPC_KEY}"
      - "MONGODB_CONNECT1=${MONGODB_HOST}:${MONGODB_PORT}"
      - "MONGODB_CONNECT2=${MONGODB_USER}:${MONGODB_PASS}@${MONGODB_HOST}:${MONGODB_PORT}"
      - "MONGODB_USERS_CONNECT1=${MONGODB_USERS_HOST}"
      - "MONGODB_USERS_CONNECT2=${MONGODB_USERS_USER}:${MONGODB_USERS_PASS}@${MONGODB_USERS_HOST}"
      - "MONGODB_HOST=${MONGODB_HOST}"
      - "MONGODB_PORT=${MONGODB_PORT}"
      - "MONGODB_USER=${MONGODB_USER}"
      - "MONGODB_PASS=${MONGODB_PASS}"
      - "MONGODB_DB_AUTH=${MONGODB_DB_AUTH}"
      - "MONGODB_USERS_USER=${MONGODB_USERS_USER}"
      - "MONGODB_USERS_PASS=${MONGODB_USERS_PASS}"
      - "MONGODB_DB_USERS_NAME=${MONGODB_DB_USERS_NAME}"
      - "MONGODB_DB_USERS_AUTH=${MONGODB_DB_USERS_AUTH}"
      - "REDIS_CONNECT=${REDIS_CONNECT}"
      - "CLICKHOUSE_HOST=${CLICKHOUSE_HOST}"
      - "CLICKHOUSE_HTTP_PORT=${CLICKHOUSE_HTTP_PORT}"
      - "CLICKHOUSE_USER=${CLICKHOUSE_USER}"
      - "CLICKHOUSE_PASS=${CLICKHOUSE_PASS}"
      - "RABBITMQ_HOST=${RABBITMQ_HOST}"
      - "RABBITMQ_PORT=${RABBITMQ_PORT}"
      - "RABBITMQ_USER=${RABBITMQ_USER}"
      - "RABBITMQ_PASS=${RABBITMQ_PASS}"
      - "COMPOSER_INSTALL=${COMPOSER_INSTALL}"
      - "NPM_CMD_ENABLE=${NPM_CMD_ENABLE}"
      - "TASKS_ENABLE=${TASKS_ENABLE}"
      - "SCHEDULER_ENABLE=${SCHEDULER_ENABLE}"
      - "DUMP_ENV=${DUMP_ENV}"
      - "ENCORE_MODE=${ENCORE_MODE}"
      - "ENV_CONFIG_ENABLE=${ENV_CONFIG_ENABLE}"
      - "XDEBUG_ENABLE=${XDEBUG_ENABLE}"
      - "XDEBUG_MODE=${XDEBUG_MODE}"
      - "XDEBUG_REMOTE_PORT=${XDEBUG_REMOTE_PORT}"
      - "XDEBUG_REMOTE_HOST=${XDEBUG_REMOTE_HOST}"
      - "PHP_IDE_CONFIG=${PHP_IDE_CONFIG}"
      - "XHPROF_ENABLED=${XHPROF_ENABLED}"
      - "PINBA_ENABLE=${PINBA_ENABLE}"
      - "PINBA_HOST=${PINBA_HOST}"
      - "PINBA_PORT=${PINBA_PORT}"
      - "ELASTIC_ENABLE=${ELASTIC_ENABLE}"
    entrypoint: ["/etc/rc.d/init.d/init.sh"]
    #entrypoint: ["tail", "-f", "/dev/null"]
    networks:
      - dockernet
```

## Создание команды для создания нового пользователя [(up)](Symfony%204b51c2bdb53c4c52ba3d3c3b1f2f1900.md)

создание консольной команды

```php
<?php

namespace App\Command;

use App\Repository\UserRepository;
use App\Security\User;
use Symfony\Component\Console\Command\Command;
use Symfony\Component\Console\Attribute\AsCommand;
use Symfony\Component\Console\Input\InputInterface;
use Symfony\Component\Console\Output\OutputInterface;
use Symfony\Component\Console\Question\ChoiceQuestion;
use Symfony\Component\Console\Question\ConfirmationQuestion;
use Symfony\Component\Console\Question\Question;

#[AsCommand(name: 'app:create-user')]
class CreateOneUserCommand extends Command
{
    private UserRepository $userRepository;
    public function __construct(UserRepository $userRepository, string $name = null)
    {
        $this->userRepository = $userRepository;
        parent::__construct($name);
    }

    protected function execute(InputInterface $input, OutputInterface $output): int
    {
        $helper = $this->getHelper('question');
        $question = new Question('Input user e-mail?');
        $email = $helper->ask($input, $output, $question);
        $question = new Question('Input user password?');
        $password = $helper->ask($input, $output, $question);
        $question = new ChoiceQuestion(
            'Input user role?(ADMIN_ROLE, USER_ROLE)',
            ['ADMIN_ROLE', 'USER_ROLE']
        );
        $role = $helper->ask($input, $output, $question);
        $user = (new User())->setEmail($email)->setPassword($password)->setRoles([$role]);
        $result = $this->userRepository->insertOne($user);
        $output->writeln($result ? 'user create sucessfull' : 'user not created');

        return Command::SUCCESS;
    }

}
```

## настройка хеширования пароля [(up)](Symfony%204b51c2bdb53c4c52ba3d3c3b1f2f1900.md)

установить пассворд хешер в контейнере

```php
composer require symfony/password-hasher
```

алгоритмы шифрования

 [https://symfony.com/doc/current/security/passwords.html#passwordhasher-supported-algorithms](https://symfony.com/doc/current/security/passwords.html#passwordhasher-supported-algorithms)

в security.yaml настроить хеширование

```php
security:
    enable_authenticator_manager: true
    # https://symfony.com/doc/current/security.html#registering-the-user-hashing-passwords
    password_hashers:
        Symfony\Component\Security\Core\User\PasswordAuthenticatedUserInterface:
            algorithm: 'bcrypt'
            cost: 15
```

password_hashers - тег, обозначающий хеширующую функцию

ниже пишем интерфейс пользователя. Ко всем инстансам этого интерфейса будет применена хеш функция для пароля

алгоритм bcrypt - один из алгоритмов симфони

cost - цена, насколько тяжелым(надежным) будет шифрование

## настройка Access token [(up)](Symfony%204b51c2bdb53c4c52ba3d3c3b1f2f1900.md)

- попробовать для jwt
    
    ```sql
    https://lcobucci-jwt.readthedocs.io/en/latest/
    ```
    

добавляем настройку акцесс токена в security.yaml настройках

```jsx
# config/packages/security.yaml
security:
    firewalls:
        main:
            access_token:
                token_handler: App\Security\AccessTokenHandler
```

Создаем свой App\Security\AccessTokenHandler

```jsx
// src/Security/AccessTokenHandler.php
namespace App\Security;

use App\Repository\AccessTokenRepository;
use Symfony\Component\Security\Http\AccessToken\AccessTokenHandlerInterface;
use Symfony\Component\Security\Http\Authenticator\Passport\Badge\UserBadge;

class AccessTokenHandler implements AccessTokenHandlerInterface
{
    public function __construct(
        private AccessTokenRepository $repository
    ) {
    }

    public function getUserBadgeFrom(string $accessToken): UserBadge
    {
        // e.g. query the "access token" database to search for this token
        $accessToken = $this->repository->findOneByValue($accessToken);
        if (null === $accessToken || !$accessToken->isValid()) {
            throw new BadCredentialsException('Invalid credentials.');
        }

        // and return a UserBadge object containing the user identifier from the found token
        return new UserBadge($accessToken->getUserId());
    }
}
```

прописываем его в ямле

```php
# config/packages/security.yaml
security:
    firewalls:
        main:
            access_token:
                token_handler: App\Security\AccessTokenHandler
```

## настройка аутентификации

Для аутентификации будем использовать JWT token

Для это установим лексик бандл

```php
composer.phar require "lexik/jwt-authentication-bundle"
```

Генерируем ssh ключи

```php
bin/console lexik:jwt:generate-keypair
```

В энве появится пути к ssh ключам и парольная фраза

```php
JWT_SECRET_KEY=%kernel.project_dir%/config/jwt/private.pem
JWT_PUBLIC_KEY=%kernel.project_dir%/config/jwt/public.pem
JWT_PASSPHRASE=
```

В проекте появится настроечный ямл файл лексика со ссылкой на энв

```php
lexik_jwt_authentication:
    secret_key: '%env(resolve:JWT_SECRET_KEY)%'
    public_key: '%env(resolve:JWT_PUBLIC_KEY)%'
    pass_phrase: '%env(JWT_PASSPHRASE)%'
```

в ямле `security.yml` раздел `firewall` проверьте что `login` расположен до раздела `api`, и если есть `main`, поставить его после `api`, иначе роут `/api/login_check` не будет найден.

```php
login:
            pattern: ^/api/login
            stateless: true
            json_login:
                check_path: /api/login_check
                success_handler: lexik_jwt_authentication.handler.authentication_success
                failure_handler: lexik_jwt_authentication.handler.authentication_failure
            
 api:
            pattern: ^/api
            stateless: true
            jwt: ~

    access_control:
        - { path: ^/api/login, roles: IS_AUTHENTICATED_ANONYMOUSLY }
        - { path: ^/api,       roles: IS_AUTHENTICATED_FULLY }
```

## Настройка Симфони сериализатора

установить сериализатор симфони

```jsx
composer require symfony/serializer
```

Настроить его в ямле

```jsx
# config/packages/framework.yaml
framework:
    # ...
    serializer:
        default_context:
            enable_max_depth: true
            yaml_indentation: 2
```

Иcпользовать в контроллере

```php
<?php

namespace App\Application\Controller;

use App\Application\Access\Entity\CredentialsDTO;
use App\Manager\UserManager;
use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Component\HttpFoundation\JsonResponse;
use Symfony\Component\HttpFoundation\Request;
use Symfony\Component\Routing\Annotation\Route;
use Symfony\Component\Serializer\SerializerInterface;

#[Route("/api/login", methods: ["POST"])]
class ApiLoginController extends AbstractController
{
    public function __construct(
        private UserManager $userManager,
        private SerializerInterface $serializer,
    )
    {

    }

    public function __invoke(Request $request): JsonResponse
    {
        $requestDto = $this->serializer->deserialize(
            $request->getContent(),
            CredentialsDTO::class,
            'json',
        );
        if($this->userManager->checkUser($requestDto)) {
            
        }
    }

}
```

## Пример Аутентификации из тайм трекера

- security.yaml
    
    ```php
    security:
      encoders:
        App\Entity\User:
          algorithm: bcrypt
          cost: 12
    
      providers:
        user_manager:
          id: app.manager.user_manager
    
      firewalls:
        refresh:
          pattern: ^/api/token/refresh
          stateless: true
          anonymous: true
        dev:
          pattern: ^/(_(profiler|wdt)|css|images|js)/
          security: false
        login:
          pattern: ^/api/login_check
          stateless: true
          anonymous: true
          json_login:
            provider: user_manager
            check_path: /api/login_check
            success_handler: lexik_jwt_authentication.handler.authentication_success
            failure_handler: lexik_jwt_authentication.handler.authentication_failure
            require_previous_session: false
        api:
          pattern: ^/api
          stateless: true
          guard:
            authenticators:
              - lexik_jwt_authentication.jwt_token_authenticator
    
      access_control:
        - { path: ^/, role: IS_AUTHENTICATED_ANONYMOUSLY }
        - { path: ^/resetting, role: IS_AUTHENTICATED_ANONYMOUSLY }
        - { path: ^/api/token/refresh, roles: IS_AUTHENTICATED_ANONYMOUSLY }
        - { path: ^/api/, role: IS_AUTHENTICATED_FULLY }
    
      role_hierarchy:
        ROLE_LEAD: ROLE_USER
        ROLE_ADMIN: ROLE_LEAD
    ```
    
- `lexik_jwt_authentication`.yaml
    
    ```php
    lexik_jwt_authentication:
      secret_key: '%env(resolve:JWT_SECRET_KEY)%'
      public_key: '%env(resolve:JWT_PUBLIC_KEY)%'
      pass_phrase: '%env(JWT_PASSPHRASE)%'
      token_ttl: 3600
    ```
    
- gesdinet_jwt_refresh_token.yaml
    
    ```php
    gesdinet_jwt_refresh_token:
      ttl: 7776000
      single_use: true
    ```
    
- bundles.php
    
    ```php
    <?php
    
    return [
        Symfony\Bundle\FrameworkBundle\FrameworkBundle::class => ['all' => true],
        Sensio\Bundle\FrameworkExtraBundle\SensioFrameworkExtraBundle::class => ['all' => true],
        Symfony\Bundle\TwigBundle\TwigBundle::class => ['all' => true],
        Symfony\Bundle\MonologBundle\MonologBundle::class => ['all' => true],
        Doctrine\Bundle\DoctrineBundle\DoctrineBundle::class => ['all' => true],
        Doctrine\Bundle\MigrationsBundle\DoctrineMigrationsBundle::class => ['all' => true],
        Symfony\Bundle\SecurityBundle\SecurityBundle::class => ['all' => true],
        Twig\Extra\TwigExtraBundle\TwigExtraBundle::class => ['all' => true],
        Symfony\WebpackEncoreBundle\WebpackEncoreBundle::class => ['all' => true],
        SymfonyBundles\JsonRequestBundle\JsonRequestBundle::class => ['all' => true],
        Lexik\Bundle\JWTAuthenticationBundle\LexikJWTAuthenticationBundle::class => ['all' => true],
        Gesdinet\JWTRefreshTokenBundle\GesdinetJWTRefreshTokenBundle::class => ['all' => true],
    ];
    ```
    
- services.yaml
    
    ```php
    parameters:
      app_env: '%env(APP_ENV)%'
    services:
        _defaults:
            autowire: true
            autoconfigure: true
    
        App\:
            resource: '../src/'
            exclude:
                - '../src/DependencyInjection/'
                - '../src/Entity/'
                - '../src/Kernel.php'
                - '../src/Tests/'
    
        App\Controller\:
            resource: '../src/Controller/'
            tags: ['controller.service_arguments']
    
        app.manager.user_manager:
          class: App\Manager\UserManager
          arguments:
          - '@app.repository.user_repository'
          - '@app.repository.time_log_repository'
          - '@App\Manager\UserInfoManager'
          - '@Symfony\Component\Security\Core\Encoder\UserPasswordEncoderInterface'
          - '@security.token_storage'
          - '@Symfony\Component\DependencyInjection\ContainerInterface'
    
        app.repository.user_repository:
          class: App\Repository\UserRepository
          tags:
            - { name: doctrine.repository_service }
    
        app.repository.time_log_repository:
          class: App\Repository\TimeLogRepository
          tags:
            - { name: doctrine.repository_service }
    ```
    
- store.js
    
    ```css
    import Vue from "vue"
    import Vuex from "vuex"
    import axios from "axios"
    import moment from "moment";
    import {router} from "@/vue/router/routes";
    
    let token = localStorage.getItem("BSGApiToken")
    let refresh_token = localStorage.getItem("refresh_token")
    if (token) {
    	axios.defaults.headers.common["Authorization"] = `Bearer ${token}`
    }
    
    axios.interceptors.response.use(function (response) {
    	return response
    }, async (error) => {
    	if (error.response && error.response.data && error.response.data.code === 401 && error.response.data.message === "Expired JWT Token" && !error.config._retry) {
    		error.config._retry = true
    		let refresh = await store.dispatch("refreshToken")
    			.then((response) => {
    				error.config.headers.Authorization = `Bearer ${response}`
    				return error.config
    			})
    		return axios.request(refresh)
    	} else if (error.response && error.response.data && error.response.data.code === 401 && error.response.data.message === "An authentication exception occurred." && !error.config._retry){
    		error.config._retry = true
    		let logout = await store.dispatch("logout")
    			.then(response => {
    				router.go()
    			})
    		return axios.request(logout)
    	}
    })
    
    Vue.use(Vuex)
    const store = new Vuex.Store({
    	state: {
    		currentUser: {},
    		authenticated: false,
    		timeLogs: [],
    		departments: [],
    		positions: [],
    		usersInfo: [],
    		users: [],
    		vacationTypes: [],
    		editedUserTimeLogCalendar: {},
    		BSGApiToken: token,
    		usedVacations: [],
    		usersWorkList: [],
    		firstJobTime: "",
    		salaryBonuses: [],
    		refreshToken: refresh_token,
    		roles: [],
    		selectedPeriod: {
    			endTime: "",
    			startTime: "",
    		},
    		timePeriodOptions: {
    			months: [],
    			years: [],
    		},
    		updateCalendarFlag: 0,
    	},
    	mutations: {
    		setCurrentUser(state, currentUser) {
    			state.currentUser = currentUser
    		},
    		setTimeLogs(state, value) {
    			state.timeLogs = value
    		},
    		setDepartments(state, departments) {
    			state.departments = departments
    		},
    		setPositions(state, positions) {
    			state.positions = positions
    		},
    		setUsersInfo(state, usersInfo) {
    			state.usersInfo = usersInfo
    		},
    		setUserInfo(state, usersInfo) {
    			state.currentUser = Object.assign({}, state.currentUser, usersInfo)
    		},
    		setUsers(state, users) {
    			state.users = users.map(user => {
    					return {
    						id: user.id,
    						email: user.email,
    						username: user.username,
    						roles: user.roles,
    						userInfoId: user.userInfo.id,
    						departmentId: user.userInfo.departmentId,
    						department: this.getters["getDepartmentName"](user.userInfo.departmentId),
    						positionId: user.userInfo.positionId,
    						position: this.getters["getPositionName"](user.userInfo.positionId),
    						fullName: user.userInfo.fullName,
    						firstName: user.userInfo.firstName,
    						secondName: user.userInfo.secondName,
    						lastName: user.userInfo.lastName,
    						startJobTime: user.userInfo.startJobTime,
    						online: user.online,
    					}
    				}
    			)
    		},
    		setVacationTypes(state, vacationTypes) {
    			state.vacationTypes = vacationTypes
    		},
    		setEditedUserTimeLogCalendar(state, editedUserTimeLogCalendar) {
    			let timelogs = {...editedUserTimeLogCalendar.timeLogs}
    			if (timelogs.tableItems) {
    				let timeLogCalendarObject = {}
    				timelogs.tableItems.forEach(day => {
    					day.date = day.day
    					timeLogCalendarObject[day.day] = day
    				})
    				state.editedUserTimeLogCalendar = {
    					timeLogs: timeLogCalendarObject,
    					user: {...editedUserTimeLogCalendar.user}
    				}
    			}
    		},
    		setBSGApiToken(state, token) {
    			localStorage.setItem("BSGApiToken", token)
    			state.BSGApiToken = token
    			axios.defaults.headers.common["Authorization"] = `Bearer ${token}`
    		},
    		removeToken(state) {
    			localStorage.removeItem("BSGApiToken")
    			state.BSGApiToken = null
    		},
    		setAuthenticated(state, authState) {
    			state.authenticated = authState
    		},
    		setUsedVacations(state, usedVacations) {
    			state.usedVacations = usedVacations
    		},
    		setUsersWorkList(state, usersWorkList) {
    			state.usersWorkList = usersWorkList
    		},
    		setFirstJobTime(state, firstJobTime) {
    			state.firstJobTime = firstJobTime
    		},
    		setSalaryBonuses(state, salaryBonuses) {
    			state.salaryBonuses = salaryBonuses
    		},
    		setRefreshToken(state, refreshToken) {
    			localStorage.setItem("refresh_token", refreshToken)
    			state.refreshToken = refreshToken
    		},
    		removeRefreshToken(state) {
    			localStorage.removeItem("refresh_token")
    			state.refreshToken = ""
    		},
    		setRoles(state, roles) {
    			let outputRoles = Object.keys(roles)
    			outputRoles.push("ROLE_USER")
    			state.roles = outputRoles
    		},
    		setSelectedPeriod(state, period) {
    			state.selectedPeriod = period
    		},
    		setTimePeriodOptions(state, options) {
    			state.timePeriodOptions = options
    		},
    		setUpdateCalendarFlag(state, updateCalendarFlag) {
    			state.updateCalendarFlag = updateCalendarFlag
    		},
    		setCurrentUserOnline(state, isOnline) {
    			state.currentUser.isOnline = isOnline
    		},
    	},
    	getters: {
    		getCurrentUser(state) {
    			return state.currentUser
    		},
    		isAuthenticated(state) {
    			return state.authenticated
    		},
    		getTimeLogs(state) {
    			return state.timeLogs
    		},
    		getDepartmentName: state => (id) => {
    			let department = state.departments.find(department => department.id === id)
    			return (department && department.department) ? department.department : ""
    		},
    		getDepartments(state) {
    			return state.departments
    		},
    		getPositionName: state => (id) => {
    			let position = state.positions.find(position => position.id === id)
    			return (position && position.position) ? position.position : ""
    		},
    		getPositions(state) {
    			return state.positions
    		},
    		getUser: state => (userId) => {
    			if (state.users) {
    				let user = state.users.find(item => item.id === Number(userId))
    				if (user) {
    					return user
    				}
    			}
    			return {}
    		},
    		getCurrentUserName: state => (id) => {
    			return state.users[id] ? state.users[id].username : ""
    		},
    		getVacationTypes(state) {
    			return state.vacationTypes
    		},
    		getVacationTypeName: state => (id) => {
    			let vacationType = state.vacationTypes.find(type => type.id === id)
    			if (vacationType) {
    				return vacationType.name
    			}
    		},
    		getEditedUserTimeLogCalendar(state) {
    			if (state.editedUserTimeLogCalendar) {
    				return state.editedUserTimeLogCalendar
    			}
    			return {}
    		},
    		getBSGApiToken(state) {
    			return state.BSGApiToken
    		},
    		getUsers(state) {
    			return state.users
    		},
    		getUsedVacations(state) {
    			return state.usedVacations
    		},
    		getUsersWorkList(state) {
    			return state.usersWorkList
    		},
    		getFirstJobTime(state) {
    			return state.firstJobTime
    		},
    		getSalaryBonuses(state) {
    			return state.salaryBonuses
    		},
    		getSalaryBonus: state => ({month, year}) => {
    			let index =  state.salaryBonuses.findIndex((item) => {
    				return month === item.month && year === item.year
    			})
    			if(index > 0) {
    				return state.salaryBonuses[index]
    			}
    		},
    		getRefreshToken(state) {
    			return state.refreshToken
    		},
    		getRoles(state) {
    			return state.roles
    		},
    		getSelectedYearPeriod(state) {
    			const period = {...state.selectedPeriod}
    			let startTime = (period.startTime !== "" ? moment(period.startTime) : moment()).startOf('year')
    			let startJob = moment(state.timePeriodOptions.startJob)
    			period.startTime = (startJob > startTime ? startJob : startTime).format("YYYY-MM-DD")
    			period.endTime = startTime.endOf("year").format("YYYY-MM-DD")
    			return period
    		},
    		getSelectedMonthPeriod(state) {
    			const period = {...state.selectedPeriod}
    			const startTime = period.startTime !== "" ? moment(period.startTime) : moment().startOf("month")
    			let startJob = moment(state.timePeriodOptions.startJob)
    			period.startTime = (state.timePeriodOptions.startJob !== undefined && startJob > startTime ? startJob : startTime).format("YYYY-MM-DD HH:mm:ss")
    			period.endTime = startTime.endOf("month").endOf("day").format("YYYY-MM-DD HH:mm:ss")
    			return period
    		},
    		getMonthOptions(state) {
    			return state.timePeriodOptions.months
    		},
    		getYearOptions(state) {
    			return state.timePeriodOptions.years
    		},
    		getUpdateCalendarFlag(state) {
    			return state.updateCalendarFlag
    		},
    	},
    	actions: {
    		updateCsrfToken({commit}, token) {
    			commit("setCsrfToken", token)
    		},
    		isUserOnline({commit}) {
    			return axios.get("/api/log/is-online")
    				.then(response => {
    					commit("setCurrentUserOnline", response.data)
    					return response.data
    				})
    		},
    		logTime({commit, dispatch}, parameter = null) {
    			return axios.post("/api/log", parameter)
    				.then(() => {
    					return dispatch("loadCurrentUser")
    						.then(() => {
    							localStorage.setItem('updateOnlineStatus', String(this.getters['getCurrentUser'].isOnline))
    							commit("setSelectedPeriod", {endTime: "", startTime: ""})
    						})
    				})
    		},
    		loadLogs({commit}, parameters) {
    			return axios.post("/api/log/date", parameters)
    				.then(response => {
    					commit("setEditedUserTimeLogCalendar", response.data)
    					return response.data
    				})
    		},
    		addOrUpdateUser({dispatch}, user) {
    			return axios.post("/api/user", user)
    		},
    		loadUsers({dispatch, commit}) {
    			return axios.get("/api/user/all")
    				.then(response => {
    					commit("setUsers", response.data)
    					return dispatch("loadRoles")
    				})
    		},
    		checkAuth({commit}) {
    			return axios.post("/api/login", {})
    				.then(response => {
    					if(response) {
    						commit("setAuthenticated", response.data)
    					}
    				})
    		},
    		login({dispatch, commit, state}, credentials) {
    			return axios.post("/api/login_check", credentials)
    				.then(response => {
    					if (response) {
    						commit("setBSGApiToken", response.data.token)
    						commit("setRefreshToken", response.data.refresh_token)
    						commit("setAuthenticated", true)
    						dispatch("loadCurrentUser")
    						return response
    					} else {
    						return null
    					}
    				})
    		},
    		refreshToken({commit, state}) {
    			return axios.post("/api/token/refresh", {refresh_token: state.refreshToken})
    				.then(response => {
    					if (response.data) {
    						commit("setBSGApiToken", response.data.token)
    						commit("setRefreshToken", response.data.refresh_token)
    						return response.data.token
    					}
    				})
    		},
    		changeCurrentUser({commit}, currentUser) {
    			commit("setCurrentUser", currentUser)
    		},
    		logout({commit}) {
    			commit("removeToken")
    			commit("setAuthenticated", false)
    			commit("removeRefreshToken")
    			commit("setCurrentUser", {})
    		},
    		addComment({dispatch}, parameters) {
    			return axios.post("/api/comments", parameters)
    		},
    		updateComment({dispatch}, parameters) {
    			return axios.put("/api/comments", parameters)
    		},
    		loadComments({dispatch}, parameters) {
    			return axios.get(`/api/comments/${parameters.startDate}/${parameters.endDate}`)
    		},
    		deleteComment({dispatch}, commentId) {
    			return axios.delete("/api/comments/" + commentId)
    		},
    		loadDepartments({dispatch, commit}) {
    			return axios.get("/api/admin/user-info/departments")
    				.then(response => {
    					commit("setDepartments", response.data)
    					return response.data
    				})
    		},
    		addDepartment({dispatch}, parameters) {
    			return axios.post("/api/admin/user-info/departments", parameters)
    		},
    		updateDepartment({dispatch}, parameters) {
    			return axios.put("/api/admin/user-info/departments/" + parameters.id, parameters)
    		},
    		deleteDepartment({dispatch}, id) {
    			return axios.delete("/api/admin/user-info/departments/" + id)
    		},
    		loadPositions({dispatch, commit}) {
    			return axios.get("/api/admin/user-info/positions")
    				.then(response => {
    					commit("setPositions", response.data)
    					return response.data
    				})
    		},
    		addPosition({dispatch}, parameters) {
    			return axios.post("/api/admin/user-info/positions", parameters)
    		},
    		updatePosition({dispatch}, parameters) {
    			return axios.put("/api/admin/user-info/positions/" + parameters.id, parameters)
    		},
    		deletePosition({dispatch}, id) {
    			return axios.delete("/api/admin/user-info/positions/" + id)
    		},
    		loadCurrentUser({commit}) {
    			return axios.get("/api/user/user-info")
    				.then(response => {
    					if(response !== undefined) {
    						commit("setUserInfo", response.data)
    						return response.data
    					}
    					return {}
    				})
    		},
    		addUserInfo({dispatch}, parameters) {
    			return axios.post("/api/admin/user-info", parameters)
    		},
    		addAdminComment({}, parameters) {
    			return axios.post("/api/admin/comments", parameters)
    		},
    		updateAdminComment({}, parameters) {
    			return axios.put("/api/admin/comments/" + parameters.commentId, parameters)
    		},
    		deleteAdminComment({}, id) {
    			return axios.delete("/api/admin/comments/" + id)
    		},
    		loadAdminComments({}, parameters) {
    			return axios.get(`/api/admin/comments/${parameters.startDate}/${parameters.endDate}/${parameters.userId}`)
    		},
    		loadVacationDaysByYearFromUser({}, parameters) {
    			return axios.get(`/api/admin/vacations/${parameters.startDate}/${parameters.finishDate}/${parameters.userId}`)
    		},
    		addVacationPeriod({}, parameters) {
    			return axios.post("/api/admin/vacations", parameters)
    		},
    		deleteVacationPeriod({}, parameters) {
    			return axios.post("/api/admin/vacations/delete", parameters)
    		},
    		updateVacationPeriod({}, parameters) {
    			return axios.put("/api/admin/vacations", parameters)
    		},
    		loadVacationTypes({commit}) {
    			return axios.get("/api/vacation-types")
    				.then(response => {
    					commit("setVacationTypes", response.data)
    				})
    		},
    		loadCommonDays({}, parameters) {
    			return axios.post("/api/log/common", parameters)
    		},
    		addShortDay({}, parameters) {
    			return axios.post("/api/short-days", parameters)
    		},
    		updateShortDay({}, parameters) {
    			return axios.put("/api/short-days", parameters)
    		},
    		deleteShortDay({}, shortDayId) {
    			return axios.delete("/api/short-days/" + shortDayId)
    		},
    		addVacationType({}, parameters) {
    			return axios.post("/api/vacation-types", parameters)
    		},
    		updateVacationType({}, parameters) {
    			return axios.put("/api/vacation-types", parameters)
    		},
    		deleteVacationType({}, id) {
    			return axios.delete("/api/vacation-types/" + id)
    		},
    		loadUsedVacations({commit}) {
    			return axios.get("/api/admin/vacations/used")
    				.then(response => {
    					commit("setUsedVacations", response.data)
    				})
    		},
    		updateManualTimeLogs({}, parameters) {
    			return axios.put("/api/log/manual", parameters)
    		},
    		loadWorkTimeAllUsers({commit}, parameters) {
    			return axios.post("/api/log/agregate", parameters)
    				.then(response => {
    					commit("setUsersWorkList", response.data)
    				})
    		},
    		loadFirstJobTime({commit}) {
    			return axios.get("/api/admin/user-info/job-start")
    				.then(response => {
    					commit("setFirstJobTime", response.data)
    				})
    		},
    		loadSalaryBonuses({commit}, parameters) {
    			return axios.post("/api/salary-bonus/get", parameters)
    				.then(response =>{
    					commit("setSalaryBonuses", response.data)
    				})
    		},
    		addSalaryBonuses({}, parameters) {
    			return axios.post("/api/salary-bonus", parameters)
    		},
    		updateSalaryBonuses({}, parameters) {
    			return axios.put("/api/salary-bonus", parameters)
    		},
    		deleteSalaryBonuses({}, parameters) {
    			return axios.post("/api/salary-bonus/delete", parameters)
    		},
    		loadLateStatistics({}, parameters) {
    			return axios.put("/api/log/statistics/late", parameters)
    		},
    		loadResetLink({}, userId) {
    			return axios.get("/api/user/resetting/" + userId)
    		},
    		resetPassword({}, parameters) {
    			return axios.put("/resetting/reset", parameters)
    		},
    		loadRoles({commit}) {
    			return axios.get("/api/user/roles")
    				.then(response => {
    					commit("setRoles", response.data)
    				})
    		},
    		changeSelectedPeriod({commit}, parameters) {
    			let endTime = ""
    			let startTime = ""
    			if(parameters) {
    				endTime = moment(parameters.value).endOf(parameters.type).format("YYYY-MM-DD")
    				startTime = parameters.value
    			}
    			commit("setSelectedPeriod", {
    				endTime: endTime,
    				startTime: startTime
    			})
    		},
    		formMonthsYearOptions({commit}, startJob) {
    			let monthYearOptions = {years: [], months: [], startJob: startJob}
    			let year = 0
    			for(let date = moment(startJob, "YYYY.MM.DD"); date < moment(); date.add(1, 'month').startOf("month")) {
    				if(year !== date.year()) {
    					monthYearOptions.years.unshift({text: date.format("YYYY"), value: date.format("YYYY-MM-DD"), type: "year"})
    					year = date.year()
    				}
    				monthYearOptions.months.unshift({text: date.format("MMMM YYYY") , value: date.format("YYYY-MM-DD"), type: "month"})
    			}
    			commit("setTimePeriodOptions", monthYearOptions)
    		},
    		incCalendarFlag({commit}) {
    			commit("setUpdateCalendarFlag", this.getters['getUpdateCalendarFlag'] + 1)
    		}
    	}
    })
    
    export default storeimport Vue from "vue"
    import Vuex from "vuex"
    import axios from "axios"
    import moment from "moment";
    import {router} from "@/vue/router/routes";
    
    let token = localStorage.getItem("BSGApiToken")
    let refresh_token = localStorage.getItem("refresh_token")
    if (token) {
    	axios.defaults.headers.common["Authorization"] = `Bearer ${token}`
    }
    
    axios.interceptors.response.use(function (response) {
    	return response
    }, async (error) => {
    	if (error.response && error.response.data && error.response.data.code === 401 && error.response.data.message === "Expired JWT Token" && !error.config._retry) {
    		error.config._retry = true
    		let refresh = await store.dispatch("refreshToken")
    			.then((response) => {
    				error.config.headers.Authorization = `Bearer ${response}`
    				return error.config
    			})
    		return axios.request(refresh)
    	} else if (error.response && error.response.data && error.response.data.code === 401 && error.response.data.message === "An authentication exception occurred." && !error.config._retry){
    		error.config._retry = true
    		let logout = await store.dispatch("logout")
    			.then(response => {
    				router.go()
    			})
    		return axios.request(logout)
    	}
    })
    
    Vue.use(Vuex)
    const store = new Vuex.Store({
    	state: {
    		currentUser: {},
    		authenticated: false,
    		timeLogs: [],
    		departments: [],
    		positions: [],
    		usersInfo: [],
    		users: [],
    		vacationTypes: [],
    		editedUserTimeLogCalendar: {},
    		BSGApiToken: token,
    		usedVacations: [],
    		usersWorkList: [],
    		firstJobTime: "",
    		salaryBonuses: [],
    		refreshToken: refresh_token,
    		roles: [],
    		selectedPeriod: {
    			endTime: "",
    			startTime: "",
    		},
    		timePeriodOptions: {
    			months: [],
    			years: [],
    		},
    		updateCalendarFlag: 0,
    	},
    	mutations: {
    		setCurrentUser(state, currentUser) {
    			state.currentUser = currentUser
    		},
    		setTimeLogs(state, value) {
    			state.timeLogs = value
    		},
    		setDepartments(state, departments) {
    			state.departments = departments
    		},
    		setPositions(state, positions) {
    			state.positions = positions
    		},
    		setUsersInfo(state, usersInfo) {
    			state.usersInfo = usersInfo
    		},
    		setUserInfo(state, usersInfo) {
    			state.currentUser = Object.assign({}, state.currentUser, usersInfo)
    		},
    		setUsers(state, users) {
    			state.users = users.map(user => {
    					return {
    						id: user.id,
    						email: user.email,
    						username: user.username,
    						roles: user.roles,
    						userInfoId: user.userInfo.id,
    						departmentId: user.userInfo.departmentId,
    						department: this.getters["getDepartmentName"](user.userInfo.departmentId),
    						positionId: user.userInfo.positionId,
    						position: this.getters["getPositionName"](user.userInfo.positionId),
    						fullName: user.userInfo.fullName,
    						firstName: user.userInfo.firstName,
    						secondName: user.userInfo.secondName,
    						lastName: user.userInfo.lastName,
    						startJobTime: user.userInfo.startJobTime,
    						online: user.online,
    					}
    				}
    			)
    		},
    		setVacationTypes(state, vacationTypes) {
    			state.vacationTypes = vacationTypes
    		},
    		setEditedUserTimeLogCalendar(state, editedUserTimeLogCalendar) {
    			let timelogs = {...editedUserTimeLogCalendar.timeLogs}
    			if (timelogs.tableItems) {
    				let timeLogCalendarObject = {}
    				timelogs.tableItems.forEach(day => {
    					day.date = day.day
    					timeLogCalendarObject[day.day] = day
    				})
    				state.editedUserTimeLogCalendar = {
    					timeLogs: timeLogCalendarObject,
    					user: {...editedUserTimeLogCalendar.user}
    				}
    			}
    		},
    		setBSGApiToken(state, token) {
    			localStorage.setItem("BSGApiToken", token)
    			state.BSGApiToken = token
    			axios.defaults.headers.common["Authorization"] = `Bearer ${token}`
    		},
    		removeToken(state) {
    			localStorage.removeItem("BSGApiToken")
    			state.BSGApiToken = null
    		},
    		setAuthenticated(state, authState) {
    			state.authenticated = authState
    		},
    		setUsedVacations(state, usedVacations) {
    			state.usedVacations = usedVacations
    		},
    		setUsersWorkList(state, usersWorkList) {
    			state.usersWorkList = usersWorkList
    		},
    		setFirstJobTime(state, firstJobTime) {
    			state.firstJobTime = firstJobTime
    		},
    		setSalaryBonuses(state, salaryBonuses) {
    			state.salaryBonuses = salaryBonuses
    		},
    		setRefreshToken(state, refreshToken) {
    			localStorage.setItem("refresh_token", refreshToken)
    			state.refreshToken = refreshToken
    		},
    		removeRefreshToken(state) {
    			localStorage.removeItem("refresh_token")
    			state.refreshToken = ""
    		},
    		setRoles(state, roles) {
    			let outputRoles = Object.keys(roles)
    			outputRoles.push("ROLE_USER")
    			state.roles = outputRoles
    		},
    		setSelectedPeriod(state, period) {
    			state.selectedPeriod = period
    		},
    		setTimePeriodOptions(state, options) {
    			state.timePeriodOptions = options
    		},
    		setUpdateCalendarFlag(state, updateCalendarFlag) {
    			state.updateCalendarFlag = updateCalendarFlag
    		},
    		setCurrentUserOnline(state, isOnline) {
    			state.currentUser.isOnline = isOnline
    		},
    	},
    	getters: {
    		getCurrentUser(state) {
    			return state.currentUser
    		},
    		isAuthenticated(state) {
    			return state.authenticated
    		},
    		getTimeLogs(state) {
    			return state.timeLogs
    		},
    		getDepartmentName: state => (id) => {
    			let department = state.departments.find(department => department.id === id)
    			return (department && department.department) ? department.department : ""
    		},
    		getDepartments(state) {
    			return state.departments
    		},
    		getPositionName: state => (id) => {
    			let position = state.positions.find(position => position.id === id)
    			return (position && position.position) ? position.position : ""
    		},
    		getPositions(state) {
    			return state.positions
    		},
    		getUser: state => (userId) => {
    			if (state.users) {
    				let user = state.users.find(item => item.id === Number(userId))
    				if (user) {
    					return user
    				}
    			}
    			return {}
    		},
    		getCurrentUserName: state => (id) => {
    			return state.users[id] ? state.users[id].username : ""
    		},
    		getVacationTypes(state) {
    			return state.vacationTypes
    		},
    		getVacationTypeName: state => (id) => {
    			let vacationType = state.vacationTypes.find(type => type.id === id)
    			if (vacationType) {
    				return vacationType.name
    			}
    		},
    		getEditedUserTimeLogCalendar(state) {
    			if (state.editedUserTimeLogCalendar) {
    				return state.editedUserTimeLogCalendar
    			}
    			return {}
    		},
    		getBSGApiToken(state) {
    			return state.BSGApiToken
    		},
    		getUsers(state) {
    			return state.users
    		},
    		getUsedVacations(state) {
    			return state.usedVacations
    		},
    		getUsersWorkList(state) {
    			return state.usersWorkList
    		},
    		getFirstJobTime(state) {
    			return state.firstJobTime
    		},
    		getSalaryBonuses(state) {
    			return state.salaryBonuses
    		},
    		getSalaryBonus: state => ({month, year}) => {
    			let index =  state.salaryBonuses.findIndex((item) => {
    				return month === item.month && year === item.year
    			})
    			if(index > 0) {
    				return state.salaryBonuses[index]
    			}
    		},
    		getRefreshToken(state) {
    			return state.refreshToken
    		},
    		getRoles(state) {
    			return state.roles
    		},
    		getSelectedYearPeriod(state) {
    			const period = {...state.selectedPeriod}
    			let startTime = (period.startTime !== "" ? moment(period.startTime) : moment()).startOf('year')
    			let startJob = moment(state.timePeriodOptions.startJob)
    			period.startTime = (startJob > startTime ? startJob : startTime).format("YYYY-MM-DD")
    			period.endTime = startTime.endOf("year").format("YYYY-MM-DD")
    			return period
    		},
    		getSelectedMonthPeriod(state) {
    			const period = {...state.selectedPeriod}
    			const startTime = period.startTime !== "" ? moment(period.startTime) : moment().startOf("month")
    			let startJob = moment(state.timePeriodOptions.startJob)
    			period.startTime = (state.timePeriodOptions.startJob !== undefined && startJob > startTime ? startJob : startTime).format("YYYY-MM-DD HH:mm:ss")
    			period.endTime = startTime.endOf("month").endOf("day").format("YYYY-MM-DD HH:mm:ss")
    			return period
    		},
    		getMonthOptions(state) {
    			return state.timePeriodOptions.months
    		},
    		getYearOptions(state) {
    			return state.timePeriodOptions.years
    		},
    		getUpdateCalendarFlag(state) {
    			return state.updateCalendarFlag
    		},
    	},
    	actions: {
    		updateCsrfToken({commit}, token) {
    			commit("setCsrfToken", token)
    		},
    		isUserOnline({commit}) {
    			return axios.get("/api/log/is-online")
    				.then(response => {
    					commit("setCurrentUserOnline", response.data)
    					return response.data
    				})
    		},
    		logTime({commit, dispatch}, parameter = null) {
    			return axios.post("/api/log", parameter)
    				.then(() => {
    					return dispatch("loadCurrentUser")
    						.then(() => {
    							localStorage.setItem('updateOnlineStatus', String(this.getters['getCurrentUser'].isOnline))
    							commit("setSelectedPeriod", {endTime: "", startTime: ""})
    						})
    				})
    		},
    		loadLogs({commit}, parameters) {
    			return axios.post("/api/log/date", parameters)
    				.then(response => {
    					commit("setEditedUserTimeLogCalendar", response.data)
    					return response.data
    				})
    		},
    		addOrUpdateUser({dispatch}, user) {
    			return axios.post("/api/user", user)
    		},
    		loadUsers({dispatch, commit}) {
    			return axios.get("/api/user/all")
    				.then(response => {
    					commit("setUsers", response.data)
    					return dispatch("loadRoles")
    				})
    		},
    		checkAuth({commit}) {
    			return axios.post("/api/login", {})
    				.then(response => {
    					if(response) {
    						commit("setAuthenticated", response.data)
    					}
    				})
    		},
    		login({dispatch, commit, state}, credentials) {
    			return axios.post("/api/login_check", credentials)
    				.then(response => {
    					if (response) {
    						commit("setBSGApiToken", response.data.token)
    						commit("setRefreshToken", response.data.refresh_token)
    						commit("setAuthenticated", true)
    						dispatch("loadCurrentUser")
    						return response
    					} else {
    						return null
    					}
    				})
    		},
    		refreshToken({commit, state}) {
    			return axios.post("/api/token/refresh", {refresh_token: state.refreshToken})
    				.then(response => {
    					if (response.data) {
    						commit("setBSGApiToken", response.data.token)
    						commit("setRefreshToken", response.data.refresh_token)
    						return response.data.token
    					}
    				})
    		},
    		changeCurrentUser({commit}, currentUser) {
    			commit("setCurrentUser", currentUser)
    		},
    		logout({commit}) {
    			commit("removeToken")
    			commit("setAuthenticated", false)
    			commit("removeRefreshToken")
    			commit("setCurrentUser", {})
    		},
    		addComment({dispatch}, parameters) {
    			return axios.post("/api/comments", parameters)
    		},
    		updateComment({dispatch}, parameters) {
    			return axios.put("/api/comments", parameters)
    		},
    		loadComments({dispatch}, parameters) {
    			return axios.get(`/api/comments/${parameters.startDate}/${parameters.endDate}`)
    		},
    		deleteComment({dispatch}, commentId) {
    			return axios.delete("/api/comments/" + commentId)
    		},
    		loadDepartments({dispatch, commit}) {
    			return axios.get("/api/admin/user-info/departments")
    				.then(response => {
    					commit("setDepartments", response.data)
    					return response.data
    				})
    		},
    		addDepartment({dispatch}, parameters) {
    			return axios.post("/api/admin/user-info/departments", parameters)
    		},
    		updateDepartment({dispatch}, parameters) {
    			return axios.put("/api/admin/user-info/departments/" + parameters.id, parameters)
    		},
    		deleteDepartment({dispatch}, id) {
    			return axios.delete("/api/admin/user-info/departments/" + id)
    		},
    		loadPositions({dispatch, commit}) {
    			return axios.get("/api/admin/user-info/positions")
    				.then(response => {
    					commit("setPositions", response.data)
    					return response.data
    				})
    		},
    		addPosition({dispatch}, parameters) {
    			return axios.post("/api/admin/user-info/positions", parameters)
    		},
    		updatePosition({dispatch}, parameters) {
    			return axios.put("/api/admin/user-info/positions/" + parameters.id, parameters)
    		},
    		deletePosition({dispatch}, id) {
    			return axios.delete("/api/admin/user-info/positions/" + id)
    		},
    		loadCurrentUser({commit}) {
    			return axios.get("/api/user/user-info")
    				.then(response => {
    					if(response !== undefined) {
    						commit("setUserInfo", response.data)
    						return response.data
    					}
    					return {}
    				})
    		},
    		addUserInfo({dispatch}, parameters) {
    			return axios.post("/api/admin/user-info", parameters)
    		},
    		addAdminComment({}, parameters) {
    			return axios.post("/api/admin/comments", parameters)
    		},
    		updateAdminComment({}, parameters) {
    			return axios.put("/api/admin/comments/" + parameters.commentId, parameters)
    		},
    		deleteAdminComment({}, id) {
    			return axios.delete("/api/admin/comments/" + id)
    		},
    		loadAdminComments({}, parameters) {
    			return axios.get(`/api/admin/comments/${parameters.startDate}/${parameters.endDate}/${parameters.userId}`)
    		},
    		loadVacationDaysByYearFromUser({}, parameters) {
    			return axios.get(`/api/admin/vacations/${parameters.startDate}/${parameters.finishDate}/${parameters.userId}`)
    		},
    		addVacationPeriod({}, parameters) {
    			return axios.post("/api/admin/vacations", parameters)
    		},
    		deleteVacationPeriod({}, parameters) {
    			return axios.post("/api/admin/vacations/delete", parameters)
    		},
    		updateVacationPeriod({}, parameters) {
    			return axios.put("/api/admin/vacations", parameters)
    		},
    		loadVacationTypes({commit}) {
    			return axios.get("/api/vacation-types")
    				.then(response => {
    					commit("setVacationTypes", response.data)
    				})
    		},
    		loadCommonDays({}, parameters) {
    			return axios.post("/api/log/common", parameters)
    		},
    		addShortDay({}, parameters) {
    			return axios.post("/api/short-days", parameters)
    		},
    		updateShortDay({}, parameters) {
    			return axios.put("/api/short-days", parameters)
    		},
    		deleteShortDay({}, shortDayId) {
    			return axios.delete("/api/short-days/" + shortDayId)
    		},
    		addVacationType({}, parameters) {
    			return axios.post("/api/vacation-types", parameters)
    		},
    		updateVacationType({}, parameters) {
    			return axios.put("/api/vacation-types", parameters)
    		},
    		deleteVacationType({}, id) {
    			return axios.delete("/api/vacation-types/" + id)
    		},
    		loadUsedVacations({commit}) {
    			return axios.get("/api/admin/vacations/used")
    				.then(response => {
    					commit("setUsedVacations", response.data)
    				})
    		},
    		updateManualTimeLogs({}, parameters) {
    			return axios.put("/api/log/manual", parameters)
    		},
    		loadWorkTimeAllUsers({commit}, parameters) {
    			return axios.post("/api/log/agregate", parameters)
    				.then(response => {
    					commit("setUsersWorkList", response.data)
    				})
    		},
    		loadFirstJobTime({commit}) {
    			return axios.get("/api/admin/user-info/job-start")
    				.then(response => {
    					commit("setFirstJobTime", response.data)
    				})
    		},
    		loadSalaryBonuses({commit}, parameters) {
    			return axios.post("/api/salary-bonus/get", parameters)
    				.then(response =>{
    					commit("setSalaryBonuses", response.data)
    				})
    		},
    		addSalaryBonuses({}, parameters) {
    			return axios.post("/api/salary-bonus", parameters)
    		},
    		updateSalaryBonuses({}, parameters) {
    			return axios.put("/api/salary-bonus", parameters)
    		},
    		deleteSalaryBonuses({}, parameters) {
    			return axios.post("/api/salary-bonus/delete", parameters)
    		},
    		loadLateStatistics({}, parameters) {
    			return axios.put("/api/log/statistics/late", parameters)
    		},
    		loadResetLink({}, userId) {
    			return axios.get("/api/user/resetting/" + userId)
    		},
    		resetPassword({}, parameters) {
    			return axios.put("/resetting/reset", parameters)
    		},
    		loadRoles({commit}) {
    			return axios.get("/api/user/roles")
    				.then(response => {
    					commit("setRoles", response.data)
    				})
    		},
    		changeSelectedPeriod({commit}, parameters) {
    			let endTime = ""
    			let startTime = ""
    			if(parameters) {
    				endTime = moment(parameters.value).endOf(parameters.type).format("YYYY-MM-DD")
    				startTime = parameters.value
    			}
    			commit("setSelectedPeriod", {
    				endTime: endTime,
    				startTime: startTime
    			})
    		},
    		formMonthsYearOptions({commit}, startJob) {
    			let monthYearOptions = {years: [], months: [], startJob: startJob}
    			let year = 0
    			for(let date = moment(startJob, "YYYY.MM.DD"); date < moment(); date.add(1, 'month').startOf("month")) {
    				if(year !== date.year()) {
    					monthYearOptions.years.unshift({text: date.format("YYYY"), value: date.format("YYYY-MM-DD"), type: "year"})
    					year = date.year()
    				}
    				monthYearOptions.months.unshift({text: date.format("MMMM YYYY") , value: date.format("YYYY-MM-DD"), type: "month"})
    			}
    			commit("setTimePeriodOptions", monthYearOptions)
    		},
    		incCalendarFlag({commit}) {
    			commit("setUpdateCalendarFlag", this.getters['getUpdateCalendarFlag'] + 1)
    		}
    	}
    })
    
    export default store
    ```
    
- routes.js
    
    ```jsx
    import VueRouter from "vue-router"
    import store from "@/vue/store/store"
    
    const TimeTracker = () => import(/* webpackChunkName: "TimeTracker" */ "@/vue/Components/TimeTracker")
    const UserManagement = () => import(/* webpackChunkName: "UserManagement" */ "@/vue/Components/UserManagement")
    const Calendar = () => import(/* webpackChunkName: "Calendar" */ "@/vue/Components/CalendarPage")
    const UserVacationPage = () => import(/* webpackChunkName: "UserVacationPage" */ "@/vue/Components/UserVacationPage")
    const CommonCalendar = () => import(/* webpackChunkName: "CommonCalendar" */ "@/vue/Components/CommonCalendar")
    const JobInfoPage = () => import(/* webpackChunkName: "SettingsPage" */ "@/vue/Components/JobInfoPage")
    const WorkSalaryTableList = () => import(/* webpackChunkName: "WorkSalaryTable" */ "@/vue/Components/WorkSalaryTable")
    const LateStatistics = () => import(/* webpackChunkName: "LateStatistics" */ "@/vue/Components/LateStatistics")
    const NotFound = () => import(/* webpackChunkName: "LateStatistics" */ "@/vue/Components/NotFound")
    
    const routes = [
    	{
    		path: "/resetting/:token",
    		component: TimeTracker,
    		meta: {
    			text: "Сброс пароля",
    			requiresAuth: false,
    			roles: ["IS_AUTHENTICATED_ANONYMOUSLY"]
    		}
    	},
    	{
    		path: "/calendar",
    		component: Calendar,
    		meta: {
    			text: "Календарь",
    			requiresAuth: true,
    			roles: ["ROLE_USER", "ROLE_LEAD", "ROLE_ADMIN"]
    		},
    		props: {
    			userId: store.getters.getCurrentUser.id
    		}
    	},
    	{
    		path: "/admin",
    		component: UserManagement,
    		meta: {
    			text: "Управление",
    			requiresAuth: true,
    			roles: ["ROLE_LEAD", "ROLE_ADMIN"]
    		},
    	},
    	{
    		path: "/admin/vacations/:userId",
    		component: UserVacationPage,
    		meta: {
    			text: "Календарь пользователя",
    			requiresAuth: true,
    			roles: ["ROLE_LEAD", "ROLE_ADMIN"]
    		},
    		props: true,
    	},
    	{
    		path: "/admin/common-calendar",
    		component: CommonCalendar,
    		meta: {
    			text: "Общий календарь",
    			requiresAuth: true,
    			roles: ["ROLE_ADMIN"]
    		},
    	},
    	{
    		path: "/settings",
    		component: JobInfoPage,
    		meta: {
    			text: "Настройки",
    			requiresAuth: true,
    			roles: ["ROLE_ADMIN"]
    		}
    	},
    	{
    		path: "/admin/salary",
    		component: WorkSalaryTableList,
    		meta: {
    			text: "Рабочее время",
    			requiresAuth: true,
    			roles: ["ROLE_ADMIN"]
    		},
    	},
    	{
    		path: "/admin/statistics/late",
    		component: LateStatistics,
    		meta: {
    			text: "Статистика опозданий",
    			requiresAuth: true,
    			roles: ["ROLE_ADMIN"]
    		},
    	},
    	{
    		path: "/",
    		component: TimeTracker,
    		meta: {
    			text: "Трекер времени",
    			requiresAuth: false,
    			roles: ["ROLE_USER", "ROLE_LEAD", "ROLE_ADMIN"]
    		}
    	},
    	{
    		path: "*",
    		component: NotFound,
    		meta: {
    			text: "Страница не найдена",
    			requiresAuth: true,
    			roles: ["ROLE_USER", "ROLE_LEAD", "ROLE_ADMIN"]
    		},
    	},
    ]
    
    const router = new VueRouter({
    	mode: "history",
    	routes: routes
    })
    
    router.beforeEach(async (to, from, next) => {
    	if(to.matched.every(record => !record.meta.requiresAuth)) {
    		next()
    		return true
    	}
    	let user = store.getters.getCurrentUser
    	let userRoles = await new Promise((resolve) => {
    		if (!('roles' in user)) {
    			store.dispatch("loadCurrentUser")
    				.then(response => {
    					return resolve(store.getters.getCurrentUser.roles)
    				})
    		} else {
    			return resolve(user.roles)
    		}
    	})
    	if (
    		to.matched.some(
    			record => !record.meta.roles.some(
    				role => userRoles.includes(role)
    			)
    		)
    	) {
    		console.log("to.matched.some", to)
    		next({
    			path: "/not-found",
    			query: {redirect: to.fullPath}
    		})
    	} else {
    		next()
    	}
    })
    
    export {router}
    ```
    

## Настройка HTTPS

настройка чтобы локально сайт работал с https

- Зачем HTTPS
    
    Чтобы использовать HTTPS с вашим локальным сайтом разработки и получить доступ к `https://localhost` или `https://mysite.example` (настраиваемое имя хоста), вам нужен [сертификат TLS](https://en.wikipedia.org/wiki/Public_key_certificate#TLS/SSL_server_certificate) , подписанный объектом, которому доверяет ваше устройство и браузер, который называется доверенным [*центром сертификации* (CA). )](https://en.wikipedia.org/wiki/Certificate_authority) . Браузер проверяет, подписан ли сертификат вашего сервера разработки доверенным центром сертификации, прежде чем создавать HTTPS-соединение.
    
    Мы рекомендуем использовать [mkcert](https://github.com/FiloSottile/mkcert) , кросс-платформенный центр сертификации, для создания и подписи сертификата. Другие полезные параметры см. в разделе [Запуск сайта локально с помощью HTTPS: другие параметры](https://web.dev/articles/how-to-use-local-https/(?hl=ru#other-options)) .
    
- Установка mkcert
    
    ```jsx
    curl -JLO "https://dl.filippo.io/mkcert/latest?for=linux/amd64"
    chmod +x mkcert-v*-linux-amd64
    sudo cp mkcert-v*-linux-amd64 /usr/local/bin/mkcert
    ```
    
- Настройка HTTPS
    
    установка корневых сертификаттов
    
    ```jsx
    mkcert -install
    ```
    
    При этом создается локальный центр сертификации (CA). Ваш локальный центр сертификации, созданный mkcert, пользуется доверием только **локально** , на вашем устройстве.
    
    ```jsx
    mkcert localhost
    ```
    
    Эта команда делает две вещи:
    
    - Создает сертификат для указанного вами имени хоста.
    - Позволяет mkcert подписать сертификат.
    
    Теперь ваш сертификат готов и подписан центром сертификации, которому локально доверяет ваш браузер.
    
    ```jsx
    symfony server:ca:install
    ```
    
    появится надпись
    
    ![Untitled](Symfony%204b51c2bdb53c4c52ba3d3c3b1f2f1900/Untitled%209.png)