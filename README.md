# Пояснительная записка к домашнему заданию №1 по дисциплине "Безопасность ОС"
## Студент - Денисов М.О. (43935)

## Задача №1

### Описание: Создание пользователя, присвоение идентификационного значения, группы students и установка необходимости изменения пароля каждые 3 месяца.

### Процесс:

Создал группу students, проверил группу через cat /etc/group, создал пользователя с указанием группы students и id 1234.

Проверил id user1 # uid=1234(user1) gid=1001(students) groups=1001(students)

Проверил строчку user1 в файле /etc/passwd

Проверил строчку user1 в файле /etc/shadow (Проверил отсутствие пароля и кол-во дней на изменение пароля 99999)

Изменил пароль user1 passwd

Перепроверил строчку в /etc/shadow на появление информации о пароле

Установил командой chage user1 смену пароля каждые 90 дней

Проверил командами chage -l user1 и cat /etc/shadow значение времени для смены пароля


## Задача №2

### Описание: Мониторинг файлов и процессов

### Процесс:

Произвёл поиск всех файлов с наличием SUID

Произвёл поиск процессов в первый раз, не обнаружив необходимого результата  ps -eo pid,ruid,euid,user,euser

Запустил passwd под user1

![alt text](/screenshots/screen0.png)

Произвёл поиск процессов во второй раз, и обнаружил необходимый результат (см. в proccesses-monitoring.txt)

## Задача №3

### Описание: Изучение механизма set-UID


### Процесс:

Нашёл программу cat в /usr/bin, копировал в директорию /home/user1/bin

Создал test.txt root'ом в домашней директории user1 и наполнил содержимым через nano.

Попытался прочесть test.txt user1 через скопированный cat - permission denied.

Установил SUID бит программе cat в директории пользователя /home/user1/bin/

Попытался прочесть через скопированный cat с SUID битом - содержимое прочлось. - Hello, Mephi from test.txt!

![alt text](/screenshots/screen1.png)

## Задача №4

### Описание: Изучение механизма привилегий


### Процесс:

Создал root testfile в домашней директории user1

Скопировал программу chown в директорию /home/user1/bin

Установил привилегию cap_chown с effective и permitted программе chown в директории /home/user1/bin

User1 воспользовался программой для изменения у файла testfile владельца и группы. - Получилось.

Потом вернул testfile владельца и группу root:root

Убрал привилегии программе /home/user1/bin/chown под root

Попробовал вызвать программу /home/user1/bin/chown под user1, получил Operation not permitted.

![alt text](/screenshots/screen2.png)

## Задача №5

### Описание: Изучение механизма sudo


### Процесс:

Открыл shell под user1, вызвал sudo date - user1 is not in the sudoers file.

Под root открыл /etc/sudoers, вставил в конце строчку user1 ALL=(ALL) /usr/bin/date, сохранил.

Вызвал user1 sudo date - Дата изменилась.

Попробовал вызвать другую команду, например chown, получил - Sorry, user user1 is not allowed to execute '/usr/sbin/chown' as root on fedora.

![alt text](/screenshots/screen3.png)