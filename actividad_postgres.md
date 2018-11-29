Crear base de datos en base a los requerimientos enviados por el cliente.

Crear una base de datos llamada call_list
David=# CREATE DATABASE call_list;
CREATE DATABASE

En la base de datos recien creada, crear la tabla users con los campos id (clave primaria), first_name, email.
call_list=# CREATE TABLE users(id SERIAL PRIMARY KEY, first_name VARCHAR(30), email VARCHAR(30));
CREATE TABLE

Ingresar un usuario, llamado Carlos (el resto de los datos deben inventarlos).
call_list=# INSERT INTO users(first_name, email) VALUES ('Carlos', 'carlos@gmail.com');
INSERT 0 1

Ingresar un usuario, llamada Laura (el resto de los datos deben inventarlos).\
call_list=# INSERT INTO users(first_name, email) VALUES ('Laura', 'laura@gmail.com');
INSERT 0 1

Crear una tabla llamada calls con los campos id (clave primaria), phone, date, user_id (foreign key relacionado a users).
call_list=# CREATE TABLE calls(id SERIAL PRIMARY KEY, phone  VARCHAR(15), date VARCHAR(15), user_id INTEGER REFERENCES users(id));
CREATE TABLE

Agregar a la tabla users el campo last_name.
call_list=# ALTER TABLE users ADD COLUMN last_name  varchar(30);
ALTER TABLE

Ingresar el apellido del usuario Carlos.
call_list=# UPDATE users SET last_name = 'Gonzalez' WHERE ID = '1';
UPDATE 1

Ingresar el apellido del usuario Laura.
call_list=# UPDATE users SET last_name = 'Perez' WHERE first_name = 'Laura';
UPDATE 1

Ingresar 6 llamadas asociadas al usuario Laura.
call_list=# INSERT INTO calls (phone, date, user_id) VALUES('56977685250', '01/01/2018', '2');
INSERT 0 1
call_list=# INSERT INTO calls (phone, date, user_id) VALUES('56977685250', '01/02/2018', '2');
INSERT 0 1
call_list=# INSERT INTO calls (phone, date, user_id) VALUES('56977685255', '01/03/2018', '2');
INSERT 0 1
call_list=# INSERT INTO calls (phone, date, user_id) VALUES('56977685260', '01/04/2018', '2');
INSERT 0 1
call_list=# INSERT INTO calls (phone, date, user_id) VALUES('56977685265', '01/05/2018', '2');
INSERT 0 1
call_list=# INSERT INTO calls (phone, date, user_id) VALUES('56977685270', '01/06/2018', '2');
INSERT 0 1

Ingresar 4 llamadas asociadas al usuario Carlos.
call_list=# INSERT INTO calls (phone, date, user_id) VALUES('56977685270', '01/02/2018', '1');
INSERT 0 1
call_list=# INSERT INTO calls (phone, date, user_id) VALUES('56977685265', '01/03/2018', '1');
INSERT 0 1
call_list=# INSERT INTO calls (phone, date, user_id) VALUES('56977685255', '01/04/2018', '1');
INSERT 0 1
call_list=# INSERT INTO calls (phone, date, user_id) VALUES('56977685250', '01/05/2018', '1');
INSERT 0 1

Crear un nuevo usuario.
call_list=#  INSERT INTO users(first_name, email, last_name) VALUES ('pedro', 'pedro@gmail.com', 'muños');
INSERT 0 1

Seleccionar la cantidad de llamados de cada uno de los usuarios (nombre de usuario y cantidad de llamadas).
call_list=# SELECT COUNT(*) FROM calls  WHERE user_id = '1';
count 
-------
4
(1 row)

call_list=# SELECT COUNT(*) FROM calls  WHERE user_id = '2';
count 
-------
7
(1 row)



Seleccionar los llamados del usuario llamado Carlos ordenados por fecha en orden descendente.
call_list=# SELECT * FROM calls WHERE user_id = '1' ORDER BY date ASC;
id |    phone    |    date    | user_id 
----+-------------+------------+---------
8 | 56977685270 | 01/02/2018 |       1
9 | 56977685265 | 01/03/2018 |       1
10 | 56977685255 | 01/04/2018 |       1
11 | 56977685250 | 01/05/2018 |       1
(4 rows)

Nuevos cambios solicitados por cliente.
"Necesito agregar a la base una tabla de auditoría que registre el motivo del borrado de una llamada y el usuario que lo efectuó."

call_list=# ALTER TABLE calls ADD COLUMN delete BOOLEAN DEFAULT FALSE;
ALTER TABLE

call_list=# CREATE TABLE auditorias(id SERIAL PRIMARY KEY, name  VARCHAR(15), razon VARCHAR(50), call_id INTEGER REFERENCES calls(id));
CREATE TABLE


call_list=# \d auditorias
Table "public.auditorias"
Column  |         Type          | Collation | Nullable |                Default                 
---------+-----------------------+-----------+----------+----------------------------------------
id      | integer               |           | not null | nextval('auditorias_id_seq'::regclass)
name    | character varying(15) |           |          | 
razon   | character varying(50) |           |          | 
call_id | integer               |           |          | 
Indexes:
"auditorias_pkey" PRIMARY KEY, btree (id)
Foreign-key constraints:
"auditorias_call_id_fkey" FOREIGN KEY (call_id) REFERENCES calls(id)


A partir de la base anterior, agregar este requerimiento y modelar la base de datos (agregar print de pantalla [utilizar https://www.draw.io/]).


7) usuario y su imagen respectiva

pintagram=# select * from users inner join images on(users.id = images.users_id);
id | first_name | last_name |      email      | username | id |  titulo   |      url_img      |    descriptions    | users_id 
----+------------+-----------+-----------------+----------+----+-----------+-------------------+--------------------+----------
1 | david      | kevin     | david@gmail.com | deivid   |  2 | gato      | www.gato.com      | gato comiendo      |        1
1 | david      | kevin     | david@gmail.com | deivid   |  1 | perro     | www.perro.com     | perro comiendo     |        1
2 | daniela    | fernanda  | dani@gmail.com  | dani     |  4 | cocodrilo | www.cocodrilo.com | cocodrilo comiendo |        2
2 | daniela    | fernanda  | dani@gmail.com  | dani     |  3 | elefante  | www.elefante.com  | elefante comiendo  |        2
3 | juan       | carlos    | juan@gmail.com  | jean     |  6 | paloma    | www.paloma.com    | paloma comiendo    |        3
3 | juan       | carlos    | juan@gmail.com  | jean     |  5 | pollo     | www.pollo.com     | pollo comiendo     |        3
(6 rows)
