- [[#Установка шрифтов Times New Roman|Установка шрифтов Times New Roman]]
	- [[#Установка шрифтов Times New Roman#Установка кодировки как в Windows|Установка кодировки как в Windows]]
	- [[#Установка шрифтов Times New Roman#Установка драйверов на Lenovo b590 сетевой карты|Установка драйверов на Lenovo b590 сетевой карты]]
- [[#Как переименовать группу файлов|Как переименовать группу файлов]]
- [[#Как настроить отключение тачпада при подключении мыши  ([up](Linux%20mint.md))|Как настроить отключение тачпада при подключении мыши  ([up](Linux%20mint.md))]]
- [[#Как отключить перетаскивание окон через Alt ([up](Linux%20mint.md))|Как отключить перетаскивание окон через Alt ([up](Linux%20mint.md))]]
- [[#Как добавить шрифты в линукс|Как добавить шрифты в линукс]]
- [[#Как достать закладки из файла от Chrome|Как достать закладки из файла от Chrome]]


### Установка шрифтов Times New Roman

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

#### Установка драйверов на Lenovo b590 сетевой карты

Если еще кто-то наткнется на эту тему, решение такое:

```jsx
sudo apt purge bcmwl-kernel-source
sudo apt install firmware-b43-installer
```

Через менеджер драйверов снова ставим драйвер от kernel, работает стабильно. После каждого действия я ребутил систему, не уверен, что это обязательно.

### Как переименовать группу файлов

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

### Как настроить отключение тачпада при подключении мыши  ([up](Linux%20mint.md))

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
    

### Как отключить перетаскивание окон через Alt ([up](Linux%20mint.md))

зайти  в настройки Окна

![Untitled](Linux%20mint/Untitled%203.png)

там отключить клавишу управления окнами

![Untitled](Linux%20mint/Untitled%204.png)

### Как добавить шрифты в линукс

Затем файлы шрифтов надо просто скопировать в эту созданную папку. Чтобы установить шрифты сразу для всех пользователей системы, их надо скопировать не в домашнюю папку, а в папку `**/usr/share/fonts**`, доступ в которую закрыт для обычного пользователя и требует знания пароля администратора.

Когда все новые шрифты скопированы в папку **/usr/share/fonts**, надо обновить кэш. Для этого в терминале выполняется команда

```jsx
fc-cache -f -v
```

Теперь надо закрыть программу LibreOffice и запустить её заново.

### Как достать закладки из файла от Chrome

```css
/home/user/.config/google-chrome/Default/Bookmarks
```

