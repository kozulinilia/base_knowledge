# JS React

Содержание

1. [Проблема достижения предела File Watchers при yarn start](JS%20React.md)
2. Установка пакетного менеджера n
3. Установка пакетного менеджера npm

- Проблема достижения предела File Watchers при yarn start [(up)](JS%20React.md)
    
    в консоли ввести 
    
    ```bash
    echo fs.inotify.max_user_watches=524288 | sudo tee -a /etc/sysctl.conf && sudo sysctl -p
    ```
    
- Установка пакетного менеджера n
    
    ```bash
    npm install -g n
    ```
    
- Установка пакетного менеджера npm
    
    ```bash
    npm install -g npm
    ```