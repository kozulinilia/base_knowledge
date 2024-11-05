- [[#Мануал|Мануал]]
- [[#Распределение файлов по 6 шт в папке|Распределение файлов по 6 шт в папке]]
- [[#Перебор pdf файлов в каталоге циклом c конвертацией в pdf|Перебор pdf файлов в каталоге циклом c конвертацией в pdf]]
- [[#Переименовывание файлов  в папке по порядку, начиная с 1 <название папки>_<1.jpg>|Переименовывание файлов  в папке по порядку, начиная с 1 <название папки>_<1.jpg>]]
- [[#Конвертация pdf документа в папки с файлами поименованными по порядку. И внутри файлы по порядку|Конвертация pdf документа в папки с файлами поименованными по порядку. И внутри файлы по порядку]]
- [[#Уменьшение разрешения картинки через graph magic|Уменьшение разрешения картинки через graph magic]]
- [[#Надпись на файл в image magic|Надпись на файл в image magic]]
- [[#Команды graphic magic|Команды graphic magic]]
- [[#Удаление файлов по списку из csv файла|Удаление файлов по списку из csv файла]]


Чтобы создать скрипт, нужно создать пустой файл, написать первой строчкой #!/bin/bash и сделать файл исполняемым 

```bash
touch myscript
chmod +x ./myscript
```

### Мануал

[docLinux/articles/Bash-скрипты.md at master · hightemp/docLinux](https://github.com/hightemp/docLinux/blob/master/articles/Bash-скрипты.md#user-content-bash-скрипты-начало)

### Распределение файлов по 6 шт в папке

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

### Перебор pdf файлов в каталоге циклом c конвертацией в pdf

```jsx
for FILE in /home/ann/BookSource/*.pdf;do PDFDIR=${FILE%.*};echo $FILE;echo $PDFDIR;mkdir -p $PDFDIR && pdftoppm -jpeg -jpegopt quality=100 -r 300 $FILE  $PDFDIR/pg;done
```

### Переименовывание файлов  в папке по порядку, начиная с 1 <название папки>_<1.jpg>

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

### Конвертация pdf документа в папки с файлами поименованными по порядку. И внутри файлы по порядку

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

### Уменьшение разрешения картинки через graph magic

```bash
#!/bin/bash
for photo in /home/ann/BookSource/*.jpg
do
	gm convert -thumbnail 1024x768 $photo $photo
done
```

### Надпись на файл в image magic

```sql
cd dir
ls *.jpg | xargs -I{} convert -gravity southwest -annotate 0 'Text' {} label_{}

```

Утилита convert находится в пакете ImageMagick

### Команды graphic magic

http://www.graphicsmagick.org/GraphicsMagick.html#details-resize

### Удаление файлов по списку из csv файла

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