===========================================================================
1. Установить на текущую систему GIT, если установлен перейти к пункту №2.
sudo apt update
sudo apt install git
...
===========================================================================
2. Клонировать гит репозиторий с тестовым заданием
git clone https://github.com/spin-spin/mytest.git
cd mytest //перейти в директорию клонированого репозитория
...
===========================================================================

3. Установить ПО docker для ОС Ubuntu согласно инструкции с офф. сайта
	https://docs.docker.com/engine/installation/linux/ubuntu/

Открыть файл README.md  ознакомиться
Вытянуть образ докера mysql из оффициального хаба
docker pull mysql

провести инициализацию mysql

===========================================================================

3.1 запустить MySQL сервис в качестве docker container с возможностью доступа с других хостов по портк 3306
			имя контейнера: my1			порт локальный:порт докера пароль рута mysql=123 

docker run -d -t --name my1 -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123 mysql
...
===========================================================================
3.2 наполнить базу какими-нибудь данными
===========================================================================

mysql -h 127.0.0.1 -P 3306 -u root -p
Enter password
show databases; #// показать имеющиеся базы данных
select User,Host from mysql.user; #// показать имеющихся пользователей
create database test; #// создаем базу с именем test
CREATE USER 'ut'@'localhost' IDENTIFIED BY 'ut123'; #// создать пользователя с именеми test паролем ut123
GRANT ALL PRIVILEGES ON test.* TO 'ut'@'localhost'; #// предоставить рпава доступа для нового пользователя
FLUSH PRIVILEGES;
use test 
create table userlink (
	PersonID int,
    LastName varchar(255),
    FirstName varchar(255),
    Address varchar(255),
    City varchar(255)
);
show tables; #// проверяем что таблица создана
show columns from userlink from test; #// просматриваем поля таблицы userlink из базы test
insert into userlink (LastName,FirstName,Address,City) #// вставить данные в таблицу userlink
values ('Petrenko','Andrey','Volkova','Cherkasy')
;
quit 
===========================================================================
3.3 - сделать бекап базы
===========================================================================
mysqldump -h 127.0.0.1 -P 3306 -u root -p --databases test>test_dump.sql
===========================================================================
3.4 - "залить" бекап в репозиторий
===========================================================================
cp test_dump.sql ~/mytest/
cd ~/mytest/
git status	// проверяем статус репозитория
git add README.md test_dump.sql // добавляем измененные файлы
git commit -m "< your message>" // записать изменения в локальный репозитория
git push origin master // выгрузить изменения на сервер
===========================================================================
3.5 - запустить второй MySQL сервис в качестве docker container с возможностью доступа с других хостов
===========================================================================
docker ps -a // просмотр имеющихся контейнеров
docker run -d -t --name second -p 3307:3306 -e MYSQL_ROOT_PASSWORD=123 mysql // запускаем второй экземпляр на порту 3307 из образа mysql
===========================================================================
3.7- наполнить его данными из бекапа
===========================================================================
mysql -h 127.0.0.1 -P 3307 -u root -p //подключаемся к новой базе в контейнере second на порте 3307
Enter password...
create database test; //создать базу с именем test
CREATE USER 'ut'@'localhost' IDENTIFIED BY 'ut123'; #// создать пользователя с именеми test паролем ut123
GRANT ALL PRIVILEGES ON test.* TO 'ut'@'localhost'; #// предоставить рпава доступа для нового пользователя
FLUSH PRIVILEGES;
Клонируем репозиторий себе на ПК
git clone https://github.com/spin-spin/mytest.git
<<<<<<< HEAD
// заходим в каталог репозитория 
cd ~/mytest/
#// загружаем дамп в базу
mysql -h 127.0.0.1 -P 3307 -u root -p test<~/mytest/test_dump.sql
mysql -h 127.0.0.1 -P 3307 -u root -p // Входим в mysql
use test #// выбираем базу
select*from userlink; #// проверяем содержимое таблицы userlink из бызы test
quit
===========================================================================

=======

Считаем сам докер уже установлен
Загружаем образ докера как указано на офф сайте mysql
docker pull mysql/mysql-server
Просматриваем доступные образы
docker images
Запускаем контейнер указав пароль рута=123 и версию sql=5.6
docker run --name my-container-name -e MYSQL_ROOT_PASSWORD=123 -d mysql/mysql-server:5.6
>>>>>>> aa6b232c002a1a34b46fddd812b74a9966f22b47
