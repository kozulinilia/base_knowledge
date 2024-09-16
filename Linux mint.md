# Linux mint

# Настройка Linux через граф оболочку

## Установка шрифтов Times New Roman

<aside>
💡 sudo apt-get install ttf-mscorefonts-installer

</aside>

### Установка кодировки как в Windows

Очень понравилось начинание владельца сайта! Сам я, как полный нуб в Ubuntu, попробую описать решение очередной своей нубовской проблемы. А именно - иероглифы и крякозябры в текстовых файлах, которые создавались под Виндой. Для тех, кто также столкнулся с подобной проблемой и не нашел решения - привожу два варианта.

**Первый** (и самый простой) - открываем терминал (Ctrl+Alt+T) и вводим в него команду

gsettings set org.gnome.gedit.preferences.encodings candidate-encodings "['UTF-8', 'WINDOWS-1251', 'CURRENT', 'ISO-8859-15', 'UTF-16']"

Все должно заработать.

Если нет - пробуем **ВТОРОЙ** вариант:

1. Устанавливаем Редактор Dconf (Ctrl+Alt+T) и командаsudo apt-get install dconf-tools
2. Открываем редактор и, с его помощью, переходим в раздел /org/gnome/gedit/preferences/encodings/Там открываем единственный в директории файл.
3. Переключаем ползунок "use default value"/"использовать стандартное значение" в выключенное положение и в ставшую активной строку вводим все так, чтобы получилось следующее:['UTF-8', 'WINDOWS-1251', 'CURRENT', 'ISO-8859-15', 'UTF-16']Жмем подтвержающую галочку (при наличии) и проверяем работу;-)

### Установка драйверов на Lenovo b590 сетевой карты

Если еще кто-то наткнется на эту тему, решение такое:

```jsx
sudo apt purge bcmwl-kernel-source
sudo apt install firmware-b43-installer
```

Через менеджер драйверов снова ставим драйвер от kernel, работает стабильно. После каждого действия я ребутил систему, не уверен, что это обязательно.

## Как переименовать группу файлов

убрать из названий файлов пробелы

```jsx
rename "s/\s+//g" *
```

Убрать название каждого файла и оставить только порядковый номер

```jsx
j = 0;
for i in *.mp4; 
mv $i $j.mp4; 
done
```

## Как настроить отключение тачпада при подключении мыши  ([up](Linux%20mint.md))

- Подробнее
    
    устанавливаем touchpad-indicator
    
    ![Untitled](Linux%20mint/Untitled.png)
    
    Из консоли
    
    ```bash
    sudo add-apt-repository ppa:atareao/atareaosudo apt-get update
    sudo apt-get install touchpad-indicator
    ```
    
    заходим в настройки
    
    ![Untitled](Linux%20mint/Untitled%201.png)
    
    Выполнить настройку отключать тачпад при подключении мыши и отключить подключить мышь
    
    ![Untitled](Linux%20mint/Untitled%202.png)
    

## Как отключить перетаскивание окон через Alt ([up](Linux%20mint.md))

зайти  в настройки Окна

![Untitled](Linux%20mint/Untitled%203.png)

там отключить клавишу управления окнами

![Untitled](Linux%20mint/Untitled%204.png)

## Как добавить шрифты в линукс

Затем файлы шрифтов надо просто скопировать в эту созданную папку. Чтобы установить шрифты сразу для всех пользователей системы, их надо скопировать не в домашнюю папку, а в папку `**/usr/share/fonts**`, доступ в которую закрыт для обычного пользователя и требует знания пароля администратора.

Когда все новые шрифты скопированы в папку **/usr/share/fonts**, надо обновить кэш. Для этого в терминале выполняется команда

```jsx
fc-cache -f -v
```

Теперь надо закрыть программу LibreOffice и запустить её заново.

## Как достать закладки из файла от Chrome

```css
/home/user/.config/google-chrome/Default/Bookmarks
```

# Установка программ

## Программа записи с экрана ([up](Linux%20mint.md))

```jsx
sudo apt install simplescreenrecorder
```

## Установка VeraCrypt([up](Linux%20mint.md))

[VeraCrypt - Free Open source disk encryption with strong security for the Paranoid](https://www.veracrypt.fr/en/Downloads.html)

Выбрать Debian 10

## Установка LAMP ([up](Linux%20mint.md))

# **Install the necessary packages**

You will need to install the following packages for the LAMP server. You can install them all at once by separating each package by a space, or one at a time like shown.

I prefer to download one at a time because it is easier to see if there were any errors.

Enter the terminal and type the following:

- `sudo apt-get install apache2`
- `sudo apt-get install php`
- `sudo apt-get install php-mysql`
- `sudo apt-get install mysql-server`

You should then be prompted to set a password for the MySQL root user. After setting the password continue to install:

- Установка mcrypt
    
    ```jsx
    sudo apt update
    sudo apt install -y build-essential
    ```
    
    Сначала вам нужно установить расширения PHP, dev и pear в Ubuntu 20.04 | 18.04.
    
    ```
    sudo apt install php php-pear php-dev libmcrypt-dev
    ```
    
    Убедитесь, что команда pecl доступна в вашей системе.
    
    [Установка PECL](Linux%20mint.md)
    
    ```jsx
    which pecl 
    ```
    
    /usr/bin/pecl
    
    Обновление каналов:
    
    ```
    sudo pecl channel-update pecl.php.net
    sudo pecl update-channels
    ```
    
    Вы можете установить расширение mcrypt с помощью команды pecl с опцией install.
    
    ```jsx
    **sudo pecl install mcrypt**
    ```
    
    Включите расширение в файле php.ini.**Ubuntu 20.04**
    
    ```bash
    sudo nano /etc/php/7.4/cli/php.ini
    #впишите туда строчку: extension=mcrypt.so
    
    sudo nano /etc/php/7.4/apache2/php.ini
    #впишите туда строчку: extension=mcrypt.so
    ```
    
    Вы можете подтвердить, что модуль был установлен и включен с помощью команды:
    
    ```bash
    php -m | grep mcrypt
    # должно вывести mcrypt
    ```
    
- `sudo apt-get install phpmyadmin`

You should then be prompted which server to use. Select Apache by pressing enter. Select no for advanced server setup.

# **Change permissions to the /var/www/html**

In order for PHP scripts and files to be run by the LAMP server they need to be saved in the /var/www/html directory. You can think of this location as your local server.

In order to make changes to this directory we need to change the permissions on it. In the terminal enter the command:

```bash
sudo chown {your ubuntu username} /var/www/html
```

пример

```bash
sudo chown ann /var/www/html
```

# **Create a symbolic link to phpMyAdmin**

By default, phpMyAdmin is installed in the /usr/share/ directory. We need to move it to our local server directory.

We navigate to the server directory that we want the link in by: 

```bash
cd /var/www/html
```

Then create the link by entering the command 

```bash
ln -s /usr/share/phpmyadmin phpmyadmin
```

# **Restart Apache and test**

Run the following command to restart Apache, setting the changes that were made:

```bash
sudo systemctl restart apache2
```

You should then be able to create an info.php file in the /var/www/html directory with this command: 

```bash
touch /var/www/html/info.php
```

In the file type the following php code:

```bash
<?php phpinfo(); ?>
```

Then, open a browser and type in `localhost/info.php` You should see a page from the php file you just wrote that gives you information about php.

Finally, to access phpMyAdmin go to `localhost/phpmyadmin` in your browser. The default root username is ‘root’ and the password is the password you chose earlier for the MySQL database.

## Установка PECL ([up](Linux%20mint.md))

### Для установки **php-pecl-http-dev** в Ubuntu / Linux Mint / Debian, введите в Терминал:

```jsx
sudo apt update
sudo apt install php-pecl-http-dev
```

## Установка телеграмм ([up](Linux%20mint.md))

[https://geekkies.in.ua/linux/ustanovka-telegram-v-ubuntu-i-linux-mint.html](https://geekkies.in.ua/linux/ustanovka-telegram-v-ubuntu-i-linux-mint.html)

Скачиваем архив

```php
wget https://telegram.org/dl/desktop/linux
```

Распаковываем архив в /opt/

```php
sudo tar xJf linux -C /opt/
```

Чтобы система видела исполняемый файл, делаем символическую ссылку в /usr/local/bin:

```php
sudo ln -s /opt/Telegram/Telegram /usr/local/bin/telegram-desktop
```

Запустите программ

```php
telegram-desktop
```

## Установка крякнутого PHPStorm ([up](Linux%20mint.md))

Скачать официальный дистрибутив для Linux

[Download PhpStorm: Lightning-Smart PHP IDE](https://www.jetbrains.com/phpstorm/download/#section=linux)

Скачать архив PhpStorm_версия.tar.gz

из папки загрузок распаковать архив

```bash
tar -xzvf PhpStorm-2022.2.3.tar.gz
```

скопировать папку в opt/phpstorm

```bash
sudo mv PhpStorm-222.4345.15/ /opt/phpstorm
```

Зайти на [https://3.jetbra.in/](https://3.jetbra.in/)

Скачать  [jetbra.zip](https://ipfs.fleek.co/ipfs/bafybeiatyghkzrrtodzt3stm652rkrjxndg4hq2ublfdmifk7plg5k5brq/files/jetbra-1126574a2f82debceb72e7f948eb7d4f616ffddf.zip)

Распаковать его. 

Зайти на 

```bash
/opt/phpstorm/current/bin
```

Скопировать содержимое из jetbra.zip

Там найти файл **phpstorm64.vmoptions**

Открыть и дописать строчку из реадми из jebra.zip

```bash
-javaagent:/opt/phpstorm/bin/ja-netfilter.jar=jetbrains
--add-opens=java.base/jdk.internal.org.objectweb.asm=ALL-UNNAMED
--add-opens=java.base/jdk.internal.org.objectweb.asm.tree=ALL-UNNAMED
```

Из папки запустить install.sh

```bash
/opt/phpstorm/bin/scripts/install.sh
```

зайти на [https://3.jetbra.in/](https://3.jetbra.in/), там скачать ключ для шторма

![Untitled](Linux%20mint/Untitled%205.png)

зайти в шторм

запустить файл программы

```bash
/opt/phpstorm/bin/phpstorm.sh
```

в поле Activate code вставить код и нажать Activate

![Untitled](Linux%20mint/Untitled%206.png)

# Служебные команды

## Свободное место в системе([up](Linux%20mint.md))

Свободное место в системе

```jsx
df
```

![Untitled](Linux%20mint/Untitled%207.png)

Свободное место в системе удобночитаемый формат(в гигабайтах)

```jsx
df -h
```

![Untitled](Linux%20mint/Untitled%208.png)

Вывести по разделу ФС

```jsx
sudo du -hsx /* | sort -rh | head -n 40
```

![Untitled](Linux%20mint/Untitled%209.png)

Вывести по конкретному разделу

```jsx
sudo du -hsx /home/* | sort -rh | head -n 35
```

![Untitled](Linux%20mint/Untitled%2010.png)

## П**роверить порты прослушивания и приложений на Linux**([up](Linux%20mint.md))

Можно выполнить любую из след команд

```jsx
sudo lsof -i -P -n | grep LISTEN 
sudo netstat -tulpn | grep LISTEN
sudo nmap -sTU -O IP-address-Here
```

## Узнать, чем заняты порты

в примере чем занят порт 8080

```jsx
sudo netstat -ntpl | grep 8080
```

## Редактировать файл hosts([up](Linux%20mint.md))

```jsx
sudo nano /etc/hosts
```

## Как показать список пользователей в Mysql ([up](Linux%20mint.md))

```bash
mysql -u root -p
```

в mysql

```bash
SELECT User, Host FROM mysql.user;
```

## Подключиться в терминале через конфигурационный файл к VPN ([up](Linux%20mint.md))

```jsx
sudo openvpn --config /home/ann/vpn/OINA.conf --auth-retry interact --log /var/log/openvpn.log
```

чтобы посмотреть логи ошибок

```jsx
sudo cat /var/log/openvpn.log
```

## Как перейти в каталог из консоли, если он на русском языке ([up](Linux%20mint.md))

на самый крайний (зато абсолютно универсальный) случай можно воспользоваться конструкцией 

```jsx
cd "$(...)"
```

, где вместо ... должна идти команда, возвращающая имя нужного каталога (кавычки нужны для тех случаев, когда в названии каталога есть пробелы).
это может быть, например, команда 

```jsx
ls | sed -n 2p
```

где 2 — номер нужного каталога в выдаче ls.
посмотреть пронумерованную выдачу ls можно, например, так:

```jsx
$ ls | nl
```

1 bin 2 boot 3 dev 4 etc ...

т.е., для перехода в каталог boot надо выполнить:

```jsx
$ cd "$(ls | sed -n 2p)"
```

## Удалить клиент яндекс диска с линукса ([up](Linux%20mint.md))

```jsx
sudo apt-get purge yandex-disk
sudo apt-get autoremove
```

## Остановить/запустить Apache([up](Linux%20mint.md))

запустить apache

```jsx
sudo systemctl start apache2
```

остановить apache

```jsx
sudo systemctl stop apache2
```

## Показать показатели системы

```css
inxi -Fxpmrz
```

## Добавление директории в $PATH

Все программы, запускаемые с консоли, находятся в разных папках. Линукс их находит благодаря тому, что все эти директории прописаны в переменной $PATH вот таким образом

```php
echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin
```

Соответственно если хотите, чтобы новая команда выполнялась в консоли из любого места, нужно добавить папку, где она находится в $PATH

Чтобы это было постоянно, а не на одну сессию, можно добавить скрипт в bashrc

```php
nano ~/.bashrc
```

и добавляем строчку

```php
export PATH="$HOME/bin:$PATH"
```

## Как посмотреть логи падения системы

```css
sudo journalctl | grep ‘error’
```

если ничего не находит, посмотртеь конец лога

```css
sudo journalctl -e
```

Показывать последние 1000 строк

```css
journalctl -n 1000 -e
```

## Смена пароля на MySql ([up](Linux%20mint.md))

1. Stop the MySQL server

```bash
sudo /etc/init.d/mysql stop
```

2. Restart the MySQL server skipping the grant tables: 

```jsx
mysqld_safe --skip-grant-tables --skip-networking
```

1. Connect to the MySQL server passwordlessly: 

```jsx
mysql -uroot
```

1. Reload the grant tables

```jsx
FLUSH PRIVILEGES;
```

1. Change the root password

```jsx
ALTER USER 'root'@'localhost' IDENTIFIED BY 'new_password';
```

1. Stop the MySQL server and restart it normally

```jsx
FLUSH PRIVILEGES;
sudo /etc/init.d/mysql stop
sudo /etc/init.d/mysql start
```

1. You can now connect to MySQL as root with password `new_password`
- Если выдал ошибку, то сделать следующее
    
    ```jsx
    mkdir -p /var/run/mysqld
    chown mysql:mysql /var/run/mysqld
    sudo /etc/init.d/mysql start
    ```
    
    If I use $ mysql -u root
    
    ```jsx
    mysql -u root
    ```
    
    I'll get 
    
    ```jsx
    Server version: 5.7.18-0ubuntu0.16.04.1 (Ubuntu)
    ```
    

## **КОМАНДА CHOWN LINUX**

### **1. СИНТАКСИС И ОПЦИИ**

Синтаксис chown, как и других подобных команд linux очень прост:

```jsx
**$ chown пользователь опции /путь/к/файлу**
```

В поле пользователь надо указать пользователя, которому мы хотим передать файл. Также можно указать через двоеточие группу, например, **пользователь:группа**. Тогда изменится не только пользователь, но и группа. Вот основные опции, которые могут вам понадобиться:

- **c, --changes** - подробный вывод всех выполняемых изменений;
- **f, --silent, --quiet** - минимум информации, скрыть сообщения об ошибках;
- **-dereference** - изменять права для файла к которому ведет символическая ссылка вместо самой ссылки (поведение по умолчанию);
- **h, --no-dereference** - изменять права символических ссылок и не трогать файлы, к которым они ведут;
- **-from** - изменять пользователя только для тех файлов, владельцем которых является указанный пользователь и группа;
- **R, --recursive** - рекурсивная обработка всех подкаталогов;
- **H** - если передана символическая ссылка на директорию - перейти по ней;
- **L** - переходить по всем символическим ссылкам на директории;
- **P** не переходить по символическим ссылкам на директории (по умолчанию).

Утилита имеет ещё несколько опций, но это самые основные и то большинство из них вам не понадобится. А теперь давайте посмотрим как пользоваться chown.

## Отключить/включить PULSE AUDIO

включить

```jsx
pulseaudio --start
```

отключить

```jsx
pulseaudio --kill
```

## Как создать кнопку запуска скрипта

Создать файл sh и там прописать код в bash, который надо будет выолнить

Дать права на выполнение для этого файла

```jsx
sudo chmod 566 <файл скрипта>.sh
```

на рабочем столе правой кнопкой, выбрать `Создать кнопку запуска здесь` и выбрать тот скрипт, который надо запустить.

# Работа с программами

## Как пользоваться регуляркой в sumlime text ([up](Linux%20mint.md))

если нужно чтобы что-то добавилось к результату поиска, то нужно добавлять `/1` на каждое нахождение

Например добавит подчеркивания на каждую букву

в поиске включаем по регулярке и вводим

```php
(/w)
```

в замене вводим

```php
/1_
```

в итоге получаем

![Untitled](Linux%20mint/Untitled%2011.png)

## Как вернуть старые визуальный вид Google Chrome

встроке браузера вбить

```jsx
chrome://flags/#chrome-refresh-2023
```

отключить следующее

![Untitled](Linux%20mint/Untitled%2012.png)

# Полезные утилиты

## Поиск текста в файлах

Поиск "mydomain.com" в файлах папки /etc/apache2/

```jsx
grep -r "mydomain.com" /etc/apache2/
```

русские слова

```jsx
LANG=ru_RU.cp1251 grep -r `echo "ОЛОЛО" |iconv -t cp1251` * |iconv -f cp1251
```

## Диагностика hdd в линукс ([up](Linux%20mint.md))

установить smartdisc

```jsx
sudo apt install smartmontools
```

Проврить какие есть диски

```jsx
ls -l /dev | grep -E 'sd|hd'
```

В терминале будет что-то такое

```jsx
ann@ann-K52F:~$  ls -l /dev | grep -E 'sd|hd'
brw-rw----  1 root disk      8,   0 сен 16 10:16 sda
brw-rw----  1 root disk      8,   1 сен 16 10:16 sda1
brw-rw----  1 root disk      8,   2 сен 16 10:16 sda2
brw-rw----  1 root disk      8,   5 сен 16 10:16 sda5
brw-rw----  1 root disk      8,  16 сен 20 20:44 sdb
```

Выбрать нужный и посмотреть по нему инфу(у меня внешний диск на USB ) - `sdb`

```jsx
sudo smartctl --info /dev/sdb
```

вот такой вывод

```jsx
smartctl 7.1 2019-12-30 r5022 [x86_64-linux-5.4.0-153-generic] (local build)
Copyright (C) 2002-19, Bruce Allen, Christian Franke, www.smartmontools.org

=== START OF INFORMATION SECTION ===
Model Family:     Seagate Barracuda 3.5
Device Model:     ST1000DM010-2EP102
Serial Number:    W9AT04QN
LU WWN Device Id: 5 000c50 0f13b4c3f
Firmware Version: CC46
User Capacity:    1 000 204 886 016 bytes [1,00 TB]
Sector Sizes:     512 bytes logical, 4096 bytes physical
Rotation Rate:    7200 rpm
Form Factor:      3.5 inches
Device is:        In smartctl database [for details use: -P show]
ATA Version is:   ATA8-ACS T13/1699-D revision 4
SATA Version is:  SATA 3.0, 6.0 Gb/s (current: 6.0 Gb/s)
Local Time is:    Wed Sep 20 20:58:54 2023 MSK
SMART support is: Available - device has SMART capability.
SMART support is: Enabled
```

чтобы проверить диск, запустить

```jsx
sudo smartctl -s on -a /dev/sdb
```

вот такой будет вывод

```jsx
=== START OF INFORMATION SECTION ===
Model Family:     Seagate Barracuda 3.5
Device Model:     ST1000DM010-2EP102
Serial Number:    W9AT04QN
LU WWN Device Id: 5 000c50 0f13b4c3f
Firmware Version: CC46
User Capacity:    1 000 204 886 016 bytes [1,00 TB]
Sector Sizes:     512 bytes logical, 4096 bytes physical
Rotation Rate:    7200 rpm
Form Factor:      3.5 inches
Device is:        In smartctl database [for details use: -P show]
ATA Version is:   ATA8-ACS T13/1699-D revision 4
SATA Version is:  SATA 3.0, 6.0 Gb/s (current: 6.0 Gb/s)
Local Time is:    Wed Sep 20 21:00:20 2023 MSK
SMART support is: Available - device has SMART capability.
SMART support is: Enabled

=== START OF ENABLE/DISABLE COMMANDS SECTION ===
SMART Enabled.

=== START OF READ SMART DATA SECTION ===
SMART Status not supported: Incomplete response, ATA output registers missing
SMART overall-health self-assessment test result: PASSED
Warning: This result is based on an Attribute check.

General SMART Values:
Offline data collection status:  (0x00)	Offline data collection activity
					was never started.
					Auto Offline Data Collection: Disabled.
Self-test execution status:      (   0)	The previous self-test routine completed
					without error or no self-test has ever 
					been run.
Total time to complete Offline 
data collection: 		(    0) seconds.
Offline data collection
capabilities: 			 (0x73) SMART execute Offline immediate.
					Auto Offline data collection on/off support.
					Suspend Offline collection upon new
					command.
					No Offline surface scan supported.
					Self-test supported.
					Conveyance Self-test supported.
					Selective Self-test supported.
SMART capabilities:            (0x0003)	Saves SMART data before entering
					power-saving mode.
					Supports SMART auto save timer.
Error logging capability:        (0x01)	Error logging supported.
					General Purpose Logging supported.
Short self-test routine 
recommended polling time: 	 (   1) minutes.
Extended self-test routine
recommended polling time: 	 ( 109) minutes.
Conveyance self-test routine
recommended polling time: 	 (   2) minutes.
SCT capabilities: 	       (0x1085)	SCT Status supported.

SMART Attributes Data Structure revision number: 10
Vendor Specific SMART Attributes with Thresholds:
ID# ATTRIBUTE_NAME          FLAG     VALUE WORST THRESH TYPE      UPDATED  WHEN_FAILED RAW_VALUE
  1 Raw_Read_Error_Rate     0x000f   083   076   006    Pre-fail  Always       -       205717269
  3 Spin_Up_Time            0x0003   098   098   000    Pre-fail  Always       -       0
  4 Start_Stop_Count        0x0032   100   100   020    Old_age   Always       -       7
  5 Reallocated_Sector_Ct   0x0033   100   100   010    Pre-fail  Always       -       0
  7 Seek_Error_Rate         0x000f   100   253   045    Pre-fail  Always       -       79212
  9 Power_On_Hours          0x0032   100   100   000    Old_age   Always       -       1
 10 Spin_Retry_Count        0x0013   100   100   097    Pre-fail  Always       -       0
 12 Power_Cycle_Count       0x0032   100   100   020    Old_age   Always       -       3
183 Runtime_Bad_Block       0x0032   100   100   000    Old_age   Always       -       0
184 End-to-End_Error        0x0032   100   100   099    Old_age   Always       -       0
187 Reported_Uncorrect      0x0032   100   100   000    Old_age   Always       -       0
188 Command_Timeout         0x0032   100   100   000    Old_age   Always       -       1 1 1
189 High_Fly_Writes         0x003a   100   100   000    Old_age   Always       -       0
190 Airflow_Temperature_Cel 0x0022   072   059   040    Old_age   Always       -       28 (Min/Max 22/28)
193 Load_Cycle_Count        0x0032   100   100   000    Old_age   Always       -       7
194 Temperature_Celsius     0x0022   028   021   000    Old_age   Always       -       28 (0 21 0 0 0)
195 Hardware_ECC_Recovered  0x001a   027   025   000    Old_age   Always       -       205717269
197 Current_Pending_Sector  0x0012   100   100   000    Old_age   Always       -       0
198 Offline_Uncorrectable   0x0010   100   100   000    Old_age   Offline      -       0
199 UDMA_CRC_Error_Count    0x003e   200   200   000    Old_age   Always       -       0
240 Head_Flying_Hours       0x0000   100   253   000    Old_age   Offline      -       1h+23m+33.877s
241 Total_LBAs_Written      0x0000   100   253   000    Old_age   Offline      -       205425345
242 Total_LBAs_Read         0x0000   100   253   000    Old_age   Offline      -       291924

SMART Error Log Version: 1
No Errors Logged

SMART Self-test log structure revision number 1
No self-tests have been logged.  [To run self-tests, use: smartctl -t]

SMART Selective self-test log data structure revision number 1
 SPAN  MIN_LBA  MAX_LBA  CURRENT_TEST_STATUS
    1        0        0  Not_testing
    2        0        0  Not_testing
    3        0        0  Not_testing
    4        0        0  Not_testing
    5        0        0  Not_testing
Selective self-test flags (0x0):
  After scanning selected spans, do NOT read-scan remainder of disk.
If Selective self-test is pending on power-up, resume after 0 minute delay.
```

Здесь опция **-s** включает флаг SMART на указном устройстве. Вы можете его убрать если поддержка SMART уже включена. Информация о диске разделена на несколько разделов, В разделе **READ SMART DATA** находится общая информация о здоровье жесткого диска.

`=== START OF READ SMART DATA SECTION ===SMART overall-health self-assessment rest result: PASSED`

Этот тест может быть пройден (**PASSED**) или нет (**FAILED**). В последнем случае сбой неизбежен, начинайте резервное копирование данных с этого диска.

Следующая вещь которую можно посмотреть, когда выполняется диагностика HDD в linux, это таблица SMART атрибутов.

В SMART таблице записаны параметры, определенные для конкретного диска разработчиком, а также порог отказа для этих параметров. Таблица заполняется автоматически и обновляется на основе прошивки диска.

- **ID #** идентификатор атрибута, как правило, десятичное число между 1 и 255;
- **ATTRIBUTE_NAME** - название атрибута;
- **FLAG** - флаг обработки атрибута;
- **VALUE** - это поле представляет нормальное значение для состояния данного атрибута в диапазоне от 1 до 253, 253 - лучшее состояние, 1 - худшее. В зависимости от свойств, начальное значение может быть от 100 до 200;
- **WORST** - худшее значение value за все время;
- **THRESH** - самое низкое значение value, после перехода за которое нужно сообщить что диск непригоден для эксплуатации;
- **TYPE** - тип атрибута, может быть Pre-fail или Old_age. Все атрибуты по умолчанию считаются критическими, то-есть если диск не прошел проверку по одному из атрибутов, то он уже считается не пригодным (FAILED) но атрибуты old_age не критичны;
- **UPDATED** - показывает частоту обновления атрибута;
- **WHEN_FAILED** - будет установлено в **FAILING_NOW** если значение атрибута меньше или равно THRESH, или в "****" если выше. В случае **FAILING_NOW**, лучше как можно скорее выполнить резервное копирование, особенно если тип атрибута **pre-fail**.
- **RAW_VALUE** - значение, определенное производителем.

Сейчас вы думаете, да smartctl хороший инструмент, но у меня нет возможности запускать его каждый раз вручную, было бы неплохо автоматизировать все это дело чтобы программа запускалась периодически и сообщала мне о результатах проверки. И это возможно, с помощью smartd.

## Смена кодировки текста ([up](Linux%20mint.md))

установить программу работы с кодировкой

```jsx
sudo apt install enca
```

Узнать кодировку файла

```jsx
enca -i ./ulitka.txt
```

![Untitled](Linux%20mint/Untitled%2013.png)

перекодировать в другой формат

```jsx
iconv -f CP1251 -t UTF-8//TRANSLIT ulitka.txt -o ulitkaRus.txt
```

## Настройка микрофона через гарнитуру

- Подробнее
    
    [https://www.notion.so/Linux-mint-6427fe00eaed4727aa5b231f474dd488#8268c5ca2f814c408365f02874636b47](Linux%20mint.md)
    
    [Linux Mint Forums](https://forums.linuxmint.com/viewtopic.php?t=299427)
    
    In the terminal run 
    
    ```php
    aplay -l 
    ```
    
    and you'll get a card number for the ALC270 Analog card. Use that number to then run amixer -c# (replacing # with the number you got in the first step. Paste that output here, along with the full output from 
    
    ```php
    inxi -Fxz
    ```
    
     please.
    
    All right. In the terminal enter 
    
    ```bash
    xed admin:///etc/modprobe.d/alsa-base.conf
    ```
    
     then enter your password and ignore the warnings. When the file opens scroll to the bottom and this new line:
    
    ```php
    options snd_hda_intel model=dell-headset-multi
    ```
    
    Перезагрузить комп
    
    Зайти в терминале в 
    
    ```bash
    pavucontrol
    ```
    
    Там появится микрофон
    

## Вывод звука с микрофона сразу в колонки ([up](Linux%20mint.md))

В командной строке ввести

```jsx
arecord -f cd --period-size=128 | aplay --buffer-time=50000
```

## Как переводить pdf в картинки

в консоли проверить что установлен пакет `pdftoppm`

```php
pdftoppm -v
```

![Untitled](Linux%20mint/Untitled%2014.png)

выбрать настройки конвертации и куда сохранять картинки

```php
mkdir -p <название папки куда сохранять> && pdftoppm -jpeg -jpegopt quality=100 -r 300 <имя файла>.pdf images/pg
# например
mkdir -p images && pdftoppm -jpeg -jpegopt quality=100 -r 300 mypdf.pdf images/pg
```

создаст каталог images и запустить конвертацию в jpeg с качеством в 300dpi из файла mypdf.pdf

Команда скрипта, который находит все pdf файлы и распаковывает картинки из них

```jsx
for FILE in /home/ann/BookSource/*.pdf;do PDFDIR=${FILE%.*};echo Converting file $FILE;mkdir -p $PDFDIR && pdftoppm -jpeg -jpegopt quality=100 -r 300 $FILE  $PDFDIR/pg && rm -f $FilE;echo End converting file $FILE;done;echo Deleting pdf files;for FILE in /home/ann/BookSource/*.pdf;do rm -f $FILE;done
```

# Железо

## Подходящий процессор более менее

```sql
Процессор Intel Core i5-12400F
```

# Bash срипты

Чтобы создать скрипт, нужно создать пустой файл, написать первой строчкой #!/bin/bash и сделать файл исполняемым 

```bash
touch myscript
chmod +x ./myscript
```

## Мануал

[docLinux/articles/Bash-скрипты.md at master · hightemp/docLinux](https://github.com/hightemp/docLinux/blob/master/articles/Bash-скрипты.md#user-content-bash-скрипты-начало)

## Распределение файлов по 6 шт в папке

```bash
#!/bin/bash
LIMIT=0
DIR=1
mkdir ./$DIR/
for FILE in ./*.jpg
do
if [ $LIMIT -eq 6 ]
then DIR=$(( $DIR+1 ))
mkdir $DIR
LIMIT=0
fi
mv $FILE ./$DIR
LIMIT=$(( $LIMIT+1 ))
done
```

## Перебор pdf файлов в каталоге циклом c конвертацией в pdf

```jsx
for FILE in /home/ann/BookSource/*.pdf;do PDFDIR=${FILE%.*};echo $FILE;echo $PDFDIR;mkdir -p $PDFDIR && pdftoppm -jpeg -jpegopt quality=100 -r 300 $FILE  $PDFDIR/pg;done
```

## Переименовывание файлов  в папке по порядку, начиная с 1 <название папки>_<1.jpg>

```bash
#!/bin/bash
for DIR in ./*
do
	if [ -d "$DIR" ]
	then
		INDEX=1
		for FILE in $DIR/*
			do
				DIRNAME=`basename $DIR`
				mv "$FILE" "$DIR/$DIRNAME"_"$INDEX.jpg"
				INDEX=$(($INDEX+1))
		done
fi
done
```

```jsx

```

## Конвертация pdf документа в папки с файлами поименованными по порядку. И внутри файлы по порядку

```bash
#!/bin/bash
echo -n "С какого номера начинать?: "
read begin
photo=/home/ann/BookSource/photo
mkdir $photo
for filePdf in /home/ann/BookSource/*.pdf
do 
	pdfDir="/home/ann/BookSource/K"_"$begin"
	echo "/home/ann/BookSource/K"_"$begin"
	mkdir $pdfDir
	pdftoppm -jpeg -jpegopt quality=50 -r 50 $filePdf  $pdfDir/"$begin"
	for fileJpg in $pdfDir/*.jpg
			do
				DIRNAME=`basename $pdfDir`
				cp "$fileJpg" "$photo"
			done
	begin=$(($begin+1))
done
```

## Уменьшение разрешения картинки через graph magic

```bash
#!/bin/bash
for photo in /home/ann/BookSource/*.jpg
do
	gm convert -thumbnail 1024x768 $photo $photo
done
```

## Надпись на файл в image magic

```sql
cd dir
ls *.jpg | xargs -I{} convert -gravity southwest -annotate 0 'Text' {} label_{}

```

Утилита convert находится в пакете ImageMagick

## Команды graphic magic

http://www.graphicsmagick.org/GraphicsMagick.html#details-resize

## Удаление файлов по списку из csv файла

```bash
#!/bin/bash
homeDir=/home/ann/BookSource;
photoDir=/run/user/1000/gvfs/google-drive:host=gmail.com,user=newacropol.spb.pp/1DX84jm0-1aTN8x3r4XGOfq0A6zGOBuZK/1RbdDWCBANY3WyLtvnxSgrZ4lcy1xc2XK/1jGmo7J4e3Vv-oMG7FCJpq3YXVNYVeON4/1wDr07-R0iUeY8wthnl7dCNR8iYYrZ5rz/1jRPmXYCmmbaglfwb5Mo5scw_LCsdoXJw/1dwSgRaiX3f352XrqgfuqiWjlITSfXEDa
sentDir=/run/user/1000/gvfs/google-drive:host=gmail.com,user=newacropol.spb.pp/1DX84jm0-1aTN8x3r4XGOfq0A6zGOBuZK/1RbdDWCBANY3WyLtvnxSgrZ4lcy1xc2XK/1jGmo7J4e3Vv-oMG7FCJpq3YXVNYVeON4/1Gzr0AdszduKmT7Zz0JNLYGhHeBwvuWwU/1sSG8KmCjaesPEfCXAWPmjySMGj1_rJAQ
IFS=$'\n'
for row in $(cat $homeDir/delete.csv)
do 
	rowInfo=();
	IFS=';'
	index=0;
	for column in $row
	do
		rowInfo[$index]=$column
		index=$index+1;
	done
if [ ! -d "$sentDir/${rowInfo[1]}" ]
then
	mkdir $sentDir/${rowInfo[1]};
fi;
mv $photoDir/${rowInfo[0]}.jpg $sentDir/${rowInfo[1]}/${rowInfo[0]}.jpg
echo ${rowInfo[0]}.jpg перенесена в ${rowInfo[1]}
done
```