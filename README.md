# devops-start
Старт в DevOps (напоминалки)

**DevOps** (development + operations) - методология активного взаимодействия специалистов по разработке со специалистами по информационно-технологическому обслуживанию и взаимная интеграция их рабочих процессов друг в друга для обеспечения качества продукта. Предназначена для эффективной организации создания и обновления программных продуктов и услуг.

**Linux**
- бесплатно
- открытый исзодный код
- можно запустит на любом оборудовании
- инструменты для обеспечения надежности и безопасности

**Преимущества консоли**
- является частью ядра
- потребляет мало ресурсов
- позволяет автоматизировать действия
- универсальна

**Chmod** - работа с правами

chmod {u g o a} / {+ - =} / {r w x} filename

## Пользователи
/etc/passwd - имя пользователя : пароль : uid : gid : полное имя, комментарий : домашний каталог : shell

**useradd** username -b /home/username -c "Username Pupkin" -g usergroup -p password - надо все указать

**adduser** username - все создат и спросит сама

**usermod**
- -l - поменять имя пользователя
- -d - поменять домашнюю директорию
- -m -d - поменять дом директорию и создать если она не существует
- L - заблокировать или разблокировать пользователя

/etc/shadow - имя пользователя : пароль : последнее изменение пароля : минимальный возраст пароля : максимальный возраст пароля : период предупреждени : период бездействия : срок годности : неиспользованный

**passwd** username - смена пароля

**Флаги доступа**
- **SUID** - запуск файла с теми правами которые есть у владельца файла - chmod u+s file
- **SGID** - запуск файла с правами группы владельца - chmod g+s dir
- **Sticky-bit** - файл в директории может удалить только тот кто этот файл создал - chmod +t dir

/etc/group/ - имя группы : пароль группы : gid : пользователи входящие в группу

**usermod**
- -g - задать основную группу для пользователя
- -G - задать дополнительную группу
- -a - добавить группы для пользователя

**groupdel** group - если группа будет основной для пользователя, то ее не удалить

**userdel** username

## Пакеты
Программы для работы с пакетами
- внешняя база - apt-get, apt
- внутренняя база - apt-cache, dokg, apt

- dpkg -l - список всех установленных пакетов
- dpkg -s имя пакета - создатель пакета, доп информация по пакету 
- dpkg -L имя пакета - какие файлы в системе принадлежат определенному пакету
- dpkg -i soft.deb - установка пакета
- dpkg -r soft.deb - удаление пакета

- apt-cache pkgnames - покажет все доступные для установки пакеты
- apt-cache search programm - найдет версии программы
- apt-cache policy programm - покажет доступные версии
- apt-cache install programm=1.1 - установит указанную версию

/etc/apt/sources.list - список репозиториев

## Vim
Режимы работы
- командный режим (esc)
- режим командной строки (:)
- режим ввода (i, a)
- визуальный режим (v)

vim -R file - только просмотр, без сохранения

- h - j - k - l - перемещение
- w - начало следующего слова
- shift + a, i - переход в конец(a) или начало(i) и редактирование
- :set number - вывод номеров строк
- v - визуальный режим - + shift - выделит блок
- y - скопировать
- p - вставить
- u - отменить
- dd - удаление строки
- /word - поиск word в документе - n следующее совпадение, shift + n - предыдущее
- %s/moon/Moon/g - замена всех moon на Moon по всему файлу

**Консольные команды**

- find / -name "my.txt"
- find / -mtime +7 (modify time)
- find / -atime +3 (access time)
- find / -type d
- find / -type f
- find /home/me/test/ -name "*.jpg" -exec mv {} /home/me/pic/ \;

Директория это файл в котором содержится сопостовление имени файлы и его метаданные.

- ps auxf | grep soft | grep -v grep | tr -s " " | cut -f2 -d " " | xargs kill -9
- tr - режет все пробелы до одного
- cut - берет 2 позицию с разделителем " " (пробел)
- xargs - возмет вывод и передаст как аргумет

date +'%d-%m'

**Специальные параметры**
- $? - код возврата
- $* - все переданные позиционные параметры
- $# - количество переданных позиционных параметров

test -d /dir = [ -d /dir ]
```sh
#!/bin/bash
if [ $# -ne 2 ]
then
        echo "2 need"
        exit 1
fi
echo "its 2 good"
```
```
if [  ]
then ...
elif [  ]
then ...
else ...
fi
```
- read  -p "Please enter something: " line
- read -s -p "Password: " - скрывает введенные символы

**DNS**
- host yandex.ru
- traceroute yandex.ru
- dig +trace yandex.ru

**shell**
- alias
- alias lsa="ls -la"
- alias pss="ps aufx"

- jobs - вывод запущенных фоновых процессов
- Ctrl + Z - остановить запущенный процесс (если запустили и он забрал консоль)
- bg 2 - запустить процесс в фоне

- let a=3+5; echo $a
- expr 3 + 5
- echo $((5+5))

set -x - отладка скриптов
```
case "$переменная" in
"вариант1" ) команда ;;
"вариант2" ) команда;;
*) команда
esac
```
**Heredoc**
```
command << end
line1
line2
line3
end
```
```
for i in ya.ru google.com vk.com
do
        ping -c4 -q $i &> /dev/null && echo "$i is available!" || echo "$i is unreachable"
done
```
```
for i in ya.ru google.com; do ping -c3 -q $i; done
for i in $(ls /tmp/); do echo $i; done
for ((i=1;i<=10;i++; do echo "Hi!"; done))
for i in $(seq 1 10); do echo "Hi!"; done
```
bc (basic calculator)
```
echo 3*3 | bc
echo "10>5" | bc
```
Функция пример
```
sqrt() {
        echo sqrt "($1) | bc -l"
}
```
```
tar -cf archive.tar file1 file2 file3 - просто архив
tar --create --file=archive.tar file1 file2 file3
tar -czf archive.tar.gz file1 file2 - +сжатие
tar -tf archive.tar.gz - просмотр
tar -xvf archive.tar.gz - распаковка с подробностями
```
**awk** - язык обработки текстовой информации - полноценный язык
```
awk 'условие { действие }'
awk 'BEGIN {print "Hello World!"}' - BEGIN - не ждем ввод, а сразу выводим
ps auxf | awk '{print $1,$3,$5}' - 1,3,5 столбец
awk '{print}' /etc/passwd/ - просто вывод
awk -F ":" '{print $3}' /etc/passwd - вывод 3 столбец
ps auxf | awk '/'bash'/ {print $2}' - вывод id с bash
awk 'NR==3{print}' /etc/psswd - вывод 3ей строки
```
Функции awk
- rand() - возвращает случайное число
- srand() - устанавливает исходное значение
- length() - возвращает длину строки
- tolower() - возвращает с преобразованием в строчные
- toupper() - возвращает с преобразованием в заглавные
```
awk 'BEGIN {srand();print int(rand()*100)}' - случайное число
awk '{print NR,$0}' /etc/passwd - пронумеровать строки
awk 'NF > 0 {print}' file - вывести без пустых строк
```
**sed** - потоковый текстовый редактор
```
sed инструкция файл
```
Операции sed
- p - печать
- d - удаление
- s - замена подстроки
- c - замена строки
```
sed -n 'p' /etc/passwd
```
Способы задания адресного пространства в sed:
- номер - номер строки в котороый надо выполнить команду
- $ - последняя строка
- /шаблон/ - любая строка, которая подходит по шаблону
- номер, номер - начиная со строки до строки
- номер,/шаблон/ - начиная со строки и до строки которая соответствует шаблону
- номер, +количество - начиня со строки плюс количество строк после нее
- номер, ~число - начиная со строки до строки номер которой будет кратный числу
```
sed -n '1p' /etc/passwd - первая строка
sed -n '1,+4p' /etc/passwd - 1-4 строка
sed -i '/^#/ d' file - удаляем строки с комментарием и сохраняем файл
sed '/^#/ d' file - удаляем строки с комментарием
sed '/^$/ d' file - удаляем пустые строки
sed 's/$/.example.com' file - добавляем в конец строки example.com
sed 's/Quote/***/' file
sed 's/i/I/g' file - g - global - меняем все i на I
sed -i.bak 's/i/I/g' file - делаем бэкап и меняем оригинал
```
## Системы инициализации

**SysV-init** - первая Unit System 5, есть уровни выполнения (0-6)

Init - /etc/inittab - /etc/init.d/rc 5 - /etc/rc5.d/ - выполняем скрипты

**Upstart** - работает со скриптами но лучше
- ориентация на события
- асинхронный старт сервисов
- функция супервизора
- упрощение скриптов запуска

**systemd**
- параллельный запуск сервисов
- автоматическое разрегение зависимостей между сервисами
- функция супервизора
- поддержка изоляции ресурсов
- логирование событий

Systemd работает со специального вида конфигурационными файлами - unit: (/etc/systemd/system или /usr/lib/systemd/system, ubuntu - lib/systemd/system )
- .target - позволяет группировать модули с run-lvl
- .service - отвечают за запуск сервисов
- .mount - подключение файловых систем
- .timer - запуск модулей по расписанию
- .socket - запуск сервиса на входящее соединение
- .slice - позволяет использовать изоляцию ресурсов
- .device - 
- .path - управляет иерархией фс, например следить за директорией
```
systemctl list-units
```
Unit-файл содержит 3 секции
- Unit - общая информация, зависимости
- Service - информация о сервисе, настройки относящиеся именно к сервису - ExecStart, ExecReload, ExecStop
- Install - рекоментации в каких ситуациях юнит будет активирован

**[Unit]**
- Description - описание текущего юнита
- Documentation - ссылка на документацию
- Requires - юнит от которого зависит текущий юнит
- Wants - юнит от которого зависит текущий юнит (менее строго чем requires)
- BindsTo - юнит при завершении работы которого завершится и текущий юнит
- After - юнит который должен запуститься прежде чем сможет запуститься текущий юнит
- Conflicts - юнит котороый не может быть запущен одновременно с текущим юнитом

**[Service]**
- Type - описание типа процесса (simple,oneshot,forking)
- PIDFile - путь до файла, содержащего PID процесса (необходимо для Type=forking)
- ExecStart - команда и параметры, необходимые для запуска процесса
- ExecReload - команда и параметры необходимые для перезагрузки процессв
- ExecStop - команда и параметры, необходимые для остановки процессв
- Restart - автоматически рестарт упавшего процессв (возможные значения в зависимости от вида завершения процесса - always, on-success, on-failure)

**[Iinstall]**
- WantedBy - принадлежность к группе юнитов
- Alias - дополнительное имя юнита

**[Timer]** - для юнита .timed
- Unit - юнит, который необходимо запустить в соответствии с описанным расписанием
- OnBootSec - время которое должно пройти от загрузки системы до запуска таймера
- OnStartupSec - время которое должно пройти с последнего запуска таймера до следующего запуска
- OnCalendar - расписание частоты запуска таймера в формате год-месяц-день часы:минуты:секунды

**[Path]** - для юнита path
- Unit - юнит который необходимо запустить если условие выполняется
- PathExests - если указанный путь существует - юнит будет запущен
- PathChanged - если файл по указанному пути меняется - юнит будет запущен
- DirectoryNotEmpty - если директория перестает быть пустой - юнит запускается

journalctl -u unit

/etc/systemd/system/apt-updater.service
```
[Unit]
Description=Example of system unit

[Service]
Type=oneshot
ExecStart=apt-get update

[Install]
WantedBy=multi-user.target
```
/etc/systemd/system/apt-updater.timer
```
[Unit]
Description=Runs apt-get update every hour

[Timer]
OnUnitActiveSec=1h
Unit=apt-updater.service

[Install]
WantedBy=multi-user.target
```

