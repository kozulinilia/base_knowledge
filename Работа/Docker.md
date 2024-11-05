# Docker

[Docker. Начало](https://habr.com/ru/post/353238/)

[Как установить Docker на Linux Mint 20 - INFOIT.COM.UA](https://infoit.com.ua/linux/kak-ustanovit-docker-na-linux-mint-20/)

[Dockerise your PHP application with Nginx and PHP7-FPM](http://geekyplatypus.com/dockerise-your-php-application-with-nginx-and-php7-fpm/)

[Compose V2](https://docs.docker.com/compose/cli-command/#install-on-linux)

- Как установить докер и докер компоуз на линукс
    
    ```jsx
    sudo apt update
    sudo apt install apt-transport-https ca-certificates curl gnupg-agent software-properties-common
    ```
    
    Выполните следующую команду, чтобы установить пакеты Snap и Docker
    
    ```jsx
    sudo rm /etc/apt/preferences.d/nosnap.pref
    sudo apt update
    sudo apt install snapd
    ```
    
    - установка из snap
        
        Чтобы установить Docker, просто используйте следующую команду:
        
        ```jsx
        sudo snap install docker
        ```
        
        Теперь мы добавляем официальный ключ Docker в вашу систему Linux Mint:
        
        ```jsx
        curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
        ```
        
        Затем добавьте репозиторий Docker в Linux Mint
        
        ```jsx
        sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(. /etc/os-release; echo "$UBUNTU_CODENAME") stable"
        ```
        
        Затем установите Docker CE
        
        ```jsx
        sudo apt update
        sudo apt install docker-ce
        ```
        
    - установка из официального источника
        
        Теперь мы добавляем официальный ключ Docker в вашу систему Linux Mint:
        
        ```
        curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
        ```
        
        Затем добавьте репозиторий Docker в Linux Mint:
        
        ```
        sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(. /etc/os-release; echo "$UBUNTU_CODENAME") stable"
        ```
        
        Затем установите Docker CE с помощью следующей команды:
        
        ```
        sudo apt update
        sudo apt install docker-ce
        ```
        
    
    После установки будет создана группа докеров. Добавьте своего пользователя в группу, которая будет запускать команды докеров:
    
    ```jsx
    sudo usermod -aG docker $USER
    ```
    
    После этого проверьте установку Docker, используя следующую команду:
    
    ```jsx
    sudo docker --version
    ```
    
    Docker используется с синтаксисом, как показано ниже:
    
    ```jsx
    $ docker help
    ```
    
    Run this command to download the current stable release of Docker Compose:
    
    ```jsx
    sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
    ```
    
    Apply executable permissions to the binary:`$`
    
    ```jsx
    sudo chmod +x /usr/local/bin/docker-compose
    ```
    
    > **Note**: If the command `docker-compose` fails after installation, check your path. You can also create a symbolic link to `/usr/bin` or any other directory in your path.
    > 
    
- Настройка nginx контейнера
    
    Мы начнем с веб-сервера, и, исходя из наших требований, это будет контейнер с официальным образом [Nginx](https://hub.docker.com/_/nginx/) . Поскольку мы будем использовать Docker Compose, мы создадим следующий файл **docker-compose.yml** , который будет запускать последний образ Nginx и будет предоставлять его порт 80 для порта 8080:
    
    ```jsx
    web:
     image: nginx:latest
     ports:
     - "8080:80"
    ```
    
    Теперь мы можем запустить
    
    ```jsx
    docker-compose up
    ```
    
    Это должно дать вам экран Nginx по умолчанию на порту 8080 для локального хоста или IP-адрес вашего док-машины.
    
    ![Untitled](Работа/Docker/Untitled.png)
    
    Теперь, когда у нас есть сервер, давайте добавим немного кода. Сначала нам нужно обновить **файл docker-compose.yml** , чтобы смонтировать локальный каталог. Я буду использовать папку с именем **code** , которая находится в том же каталоге, что и мой файл **docker-compose.yml** , и она будет смонтирована как **код** корневой папки в контейнере.
    
    ```php
    web:
        image: nginx:latest
        ports:
            - "8080:80"
        volumes:
            - ./code:/code
    ```
    
    Следующий шаг — сообщить Nginx, что эта папка существует.Давайте создадим следующий сайт .conf на том же уровне, что и файл docker-compose.yml :
    
    ```php
    server {
        index index.html;
        server_name php-docker.local;
        error_log  /var/log/nginx/error.log;
        access_log /var/log/nginx/access.log;
        root /code;
    }
    ```
    
    Если у вас нет большого опыта работы с Nginx, вот что мы определяем здесь — index.html будет нашим индексом по умолчанию, имя сервера — php-docker.local, и он должен указывать (обновить файл hosts) на ваша среда Docker (localhost, если вы работаете в Linux, или машина docker, если вы работаете на Mac или Windows) , мы указываем журналы ошибок как те, которые выставлены контейнером по умолчанию, чтобы мы видели ошибки в нашем журнале компоновки docker. , и, наконец, мы указываем корневую папку как ту, которую мы смонтировали в контейнере.Чтобы активировать эту настройку, нам нужно применить еще одну модификацию к нашему файлу docker-compose.yml :
    
    ```php
    web:
        image: nginx:latest
        ports:
            - "8080:80"
        volumes:
            - ./code:/code
            - ./site.conf:/etc/nginx/conf.d/site.conf
    ```
    

[Изучаем Docker, часть 5: команды](https://habr.com/ru/company/ruvds/blog/440660/)