# PHPStorm

Содержание

[Настройка XDebug](PHPStorm.md)

[Настройка Git](PHPStorm.md)

Настройка Линтера

Выключить файл вотчер

Настройка PHPStan

[Как установить PHPStorm](Как%20установить%20PHPStorm%20cbac2960e2c04cf8b4a8a0920859c397.md)

[Настройка SSH Конфигурации для деплоя на DEV-ы в PHPStorm](Настройка%20SSH%20Конфигурации%20для%20деплоя%20на%20DEV-ы%20в%20%2070f9d79e887f4d05a8e331ea11eb4858.md)

## Настройка XDebug

- File->Settings
    
    ![PHPStorm%20dd5fe1e2681c49c28a7c7533fcf6fb4f/Untitled.png](Работа/PHPStorm/Untitled.png)
    
- Language & Frameworks->PHP
    
    ![PHPStorm%20dd5fe1e2681c49c28a7c7533fcf6fb4f/Untitled%201.png](Работа/PHPStorm/Untitled%201.png)
    
- PHP->Debug
    
    ![PHPStorm%20dd5fe1e2681c49c28a7c7533fcf6fb4f/Untitled%202.png](Работа/PHPStorm/Untitled%202.png)
    
- Debug->DBGp Proxy
    
    ![PHPStorm%20dd5fe1e2681c49c28a7c7533fcf6fb4f/Untitled%203.png](Работа/PHPStorm/Untitled%203.png)
    
- PHP->Servers
    
    ![PHPStorm%20dd5fe1e2681c49c28a7c7533fcf6fb4f/Untitled%204.png](Работа/PHPStorm/Untitled%204.png)
    
- CLI Interprises
    
    ![PHPStorm%20dd5fe1e2681c49c28a7c7533fcf6fb4f/Untitled%205.png](Работа/PHPStorm/Untitled%205.png)
    
    ![Untitled](Работа/PHPStorm/Untitled%206.png)
    

## Настройка Git

File->Settings

![PHPStorm%20dd5fe1e2681c49c28a7c7533fcf6fb4f/Untitled%207.png](Работа/PHPStorm/Untitled%207.png)

Version Control

![PHPStorm%20dd5fe1e2681c49c28a7c7533fcf6fb4f/Untitled%208.png](Работа/PHPStorm/Untitled%208.png)

Если путь к проекту с гитом указан неверно, выбрать карандашик и найти нужную папку(со своим проектом)

Так примерно должна выглядеть вкладка git

![PHPStorm%20dd5fe1e2681c49c28a7c7533fcf6fb4f/Untitled%209.png](Работа/PHPStorm/Untitled%209.png)

## Настройка подключения к базе

![Untitled](Работа/PHPStorm/Untitled%2010.png)

![Untitled](Работа/PHPStorm/Untitled%2011.png)

![Untitled](Работа/PHPStorm/Untitled%2012.png)

![Untitled](Работа/PHPStorm/Untitled%2013.png)

![Untitled](Работа/PHPStorm/Untitled%2014.png)

![Untitled](Работа/PHPStorm/Untitled%2015.png)

![Untitled](Работа/PHPStorm/Untitled%2016.png)

![Untitled](Работа/PHPStorm/Untitled%2017.png)

![Untitled](Работа/PHPStorm/Untitled%2018.png)

## Настройка линтера

- Настроить Нод интерпретатор
    
    ![Untitled](Работа/PHPStorm/Untitled%2019.png)
    
    ![Untitled](Работа/PHPStorm/Untitled%2020.png)
    
    ![Untitled](Работа/PHPStorm/Untitled%2021.png)
    
    ![Untitled](Работа/PHPStorm/Untitled%2022.png)
    
- Отключить плагин ангуляра
    
    ![Untitled](Работа/PHPStorm/Untitled%2023.png)
    
- Настройка webpack
    
    ![Untitled](Работа/PHPStorm/Untitled%2024.png)
    
- Настройка ESLint
    
    ![Untitled](Работа/PHPStorm/Untitled%2025.png)
    
- Запуск
    
    ![Untitled](Работа/PHPStorm/Untitled%2026.png)
    

## Установка PHPStorm на Linux

[Установка PHPSTORM в Ubuntu 20.04 + бесконечный пробный период | FRESHNOTES.TOP](https://freshnotes.top/2020/05/ustanovka-phpstorm-v-ubuntu-20-04-beskonechnyj-probnyj-period/)

- Когда надо сбросить триал, зайти в папку home/egik/.config
    
    ![Untitled](Работа/PHPStorm/Untitled%2027.png)
    
- Найти там папку JetBrains и удалить ее.
    
    ![Untitled](Работа/PHPStorm/Untitled%2028.png)
    

## Выключчить файл вотчер

![Untitled](Работа/PHPStorm/Untitled%2029.png)

там добавить строчку

```jsx
-Didea.filewatcher.disabled=true
```

![Untitled](Работа/PHPStorm/Untitled%2030.png)

Перезапустить PHPStorm

## Настройка PHPStan

зайти в контейнер

```html
composer linte
```

пойдет проверка phpstan в контейнере

запуск csfixer

```html
composer phpcs
```

сдедать 

```html
chmod -x phpstan.phar 
```

Проверить докер

![Untitled](Работа/PHPStorm/Untitled%2031.png)

Тулзы

![Untitled](Работа/PHPStorm/Untitled%2032.png)

Зайти в интерпретатор PHP

![Untitled](Untitled%2033.png)

настроить через докер

![Untitled](Untitled%2034.png)

Зайти в пхп стан

![Untitled](Untitled%2035.png)

создать новый

![Untitled](Untitled%2036.png)

![Untitled](Untitled%2037.png)