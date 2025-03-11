# **ИТОГОВАЯ КОНТРОЛЬНАЯ РАБОТА**
__________________________________________________________________
## *Автор: Домбаев Асламбек*

### 1. Используя команду cat в терминале операционной системы Linux, создать два файла Домашние животные (заполнив файл собаками, кошками, хомяками) и Вьючные животными заполнив файл Лошадьми, верблюдами и ослы), а затем объединить их. Просмотреть содержимое созданного файла. Переименовать файл, дав ему новое имя (Друзья человека).

> mkdir work  
cd work  
*#создание и переход в директорию work* 

>cat > pets.txt << EOF  
cat  
dog  
hamster  
EOF  
*#заполнение данных животными*  

> cat > pack_animals.txt << EOF  
horse  
camel  
donkey  
EOF  
*#создание и заполнение данных "вьючные животные"*  

>cat pets.txt pack_animals.txt > assembled.txt  
*#объединение данных*
>>cat assembled.txt  
*#просмотр данных*  

>mv assembled.txt human_friends.txt  
*#переименование в "друзья человека"*

### 2. Создать директорию, переместить файл туда.

>mkdir  final_attestation  
mv human_friends.txt  final_attestation/  
ls final_attestation/

### 3. Подключить дополнительный репозиторий MySQL. Установить любой пакет из этого репозитор

>mysql --version  
sudo apt install mysql-client  

### 4. Установить и удалить deb-пакет с помощью dpkg.

>sudo apt-get install curl  
dpkg -l | grep curl        *#проверка*  
sudo dpkg -r curl *#удаление*  
dpkg -l | grep curl  

### 5. Выложить историю команд в терминале ubuntu.

>history > commands_history.txt

### 6. Нарисовать диаграмму, в которой есть класс родительский класс, домашние животные и вьючные животные, в составы которых в случае домашних животных войдут классы: собаки, кошки, хомяки, а в класс вьючные животные войдут: Лошади, верблюды и ослы).

> Выложена в отдельном файле  

### 7. В подключенном MySQL репозитории создать базу данных “Друзья человека”
> sudo mysql -u root -p  
CREATE database HumanFriends;  
USE HumanFriends;  
SHOW DATABASES;  

### 8. Создать таблицы с иерархией из диаграммы в БД  

> CREATE TABLE HumanFriends ( 
         id_human_friend INT AUTO_INCREMENT PRIMARY KEY,
         type_human_friend VARCHAR(255)
     );  

 >CREATE TABLE Pets (
         id_pet INT AUTO_INCREMENT PRIMARY KEY,
         type_pet VARCHAR(255),
         id_human_friend INT,
         FOREIGN KEY (id_human_friend) REFERENCES HumanFriends(id_human_friend)
     );  

>CREATE TABLE PackAnimals (
         id_pack_animal INT AUTO_INCREMENT PRIMARY KEY,
         type_pack_animal VARCHAR(255),
         id_human_friend INT,
         FOREIGN KEY (id_human_friend) REFERENCES HumanFriends(id_human_friend)
     );  

> CREATE TABLE Cats (
         id_cat INT AUTO_INCREMENT PRIMARY KEY,
         name VARCHAR(255),
         birth_day DATE,
         commands TEXT,
         id_pet INT,
         FOREIGN KEY (id_pet) REFERENCES Pets(id_pet)
     );  

> CREATE TABLE Dogs (
         id_dog INT AUTO_INCREMENT PRIMARY KEY,
         name VARCHAR(255),
         birth_day DATE,
         commands TEXT,
         id_pet INT,
         FOREIGN KEY (id_pet) REFERENCES Pets(id_pet)
     );  

> CREATE TABLE Hamsters (
         id_hamster INT AUTO_INCREMENT PRIMARY KEY,
         name VARCHAR(255),
         birth_day DATE,
         commands TEXT,
         id_pet INT,
         FOREIGN KEY (id_pet) REFERENCES Pets(id_pet)
     );  

> CREATE TABLE Horses (
         id_horse INT AUTO_INCREMENT PRIMARY KEY,
         name VARCHAR(255),
         birth_day DATE,
         commands TEXT,
         id_pack_animal INT,
         FOREIGN KEY (id_pack_animal) REFERENCES PackAnimals(id_pack_animal)
     );  

> CREATE TABLE Camels (
         id_camel INT AUTO_INCREMENT PRIMARY KEY,
         name VARCHAR(255),
         birth_day DATE,
         commands TEXT,
         id_pack_animal INT,
         FOREIGN KEY (id_pack_animal) REFERENCES PackAnimals(id_pack_animal)
     );  

> CREATE TABLE Donkeys (
         id_donkey INT AUTO_INCREMENT PRIMARY KEY,
         name VARCHAR(255),
         birth_day DATE,
         commands TEXT,
         id_pack_animal INT,
         FOREIGN KEY (id_pack_animal) REFERENCES PackAnimals(id_pack_animal)
     );  

> SHOW TABLES; 

### 9. Заполнить низкоуровневые таблицы именами(животных), командами которые они выполняют и датами рождения.  

> INSERT INTO HumanFriends (type_human_friend) VALUES ('Pet'), ('PackAnimal');  
INSERT INTO Pets (type_pet, id_human_friend) VALUES ('Cat', 1), ('Dog', 1), ('Hamster', 1);  
INSERT INTO PackAnimals (type_pack_animal, id_human_friend) VALUES ('Horse', 2), ('Camel', 2), ('Donkey', 2);  
INSERT INTO Cats (name, birthday, commands, id_pet) VALUES ('Whiskers', '2019-03-01', 'sit, jump', 1);  
INSERT INTO Dogs (name, birthday, commands, id_pet) VALUES ('Grom', '2019-02-11', 'attack, sit', 2);  
INSERT INTO Hamsters (name, birthday, commands, id_pet) VALUES ('Boby', '2019-01-01', 'run', 3);  
INSERT INTO Horses (name, birthday, commands, id_pack_animal) VALUES ('Bucefal', '2019-01-02', 'run', 1);  
INSERT INTO Camels (name, birthday, commands, id_pack_animal) VALUES ('General', '2019-03-03', 'carry', 2);  
INSERT INTO Donkeys (name, birth_day, commands, id_pack_animal) VALUES ('Misha', '2019-03-01', 'drink', 3);  

### 10. Удалив из таблицы верблюдов, т.к. верблюдов решили перевезти в другой питомник на зимовку. Объединить таблицы лошади, и ослы в одну таблицу.   

> DELETE FROM Camels;  

>CREATE TABLE HorsesAndDonkeys AS
     SELECT id_horse AS id, name, birth_day, commands, 'Horse' AS type FROM Horses
     UNION
     SELECT id_donkey AS id, name, birth_day, commands, 'Donkey' AS type FROM Donkeys;  

### 11. Создать новую таблицу “молодые животные” в которую попадут все животные старше 1 года, но младше 3 лет и в отдельном столбце с точностью до месяца подсчитать возраст животных в новой таблице.  

> CREATE TABLE YoungAnimals AS  
     SELECT id, name, birth_day, commands, type,  
            TIMESTAMPDIFF(MONTH, birthday, CURDATE()) AS   age_in_months  
     FROM (
         SELECT id_cat AS id, name, birthday, commands, 'Cat' AS type FROM Cats
         UNION ALL
         SELECT id_dog AS id, name, birthday, commands, 'Dog' AS type FROM Dogs
         UNION ALL
         SELECT id_hamster AS id, name, birthday, commands, 'Hamster' AS type FROM Hamsters
         UNION ALL
         SELECT id AS id, name, birthday, commands, type FROM HorsesAndDonkeys
     ) AS AllAnimals
     WHERE TIMESTAMPDIFF(MONTH, birthday, CURDATE()) BETWEEN 12 AND 36;  

### 12. Объединить все таблицы в одну, при этом сохраняя поля, указывающие на прошлую принадлежность к старым таблицам.  

> CREATE TABLE AllAnimals AS
     SELECT id_cat AS id, name, birth_day, commands, 'Cat' AS type FROM Cats
     UNION
     SELECT id_dog AS id, name, birth_day, commands, 'Dog' AS type FROM Dogs
     UNION
     SELECT id_hamster AS id, name, birth_day, commands, 'Hamster' AS type FROM Hamsters
     UNION
     SELECT id AS id, name, birth_day, commands, type FROM HorsesAndDonkeys
     UNION
     SELECT id AS id, name, birth_day, commands, type FROM YoungAnimals;  

> SELECT * FROM AllAnimals;  
exit
