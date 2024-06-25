## Database
Database adalah sebuah tempat untuk menyimpan sekumpulan data, pada umumnya
database dapat menyimpan data baik text, numerik, waktu, dan sebagainya. 

Data-data ini diatur dengan rapi dalam sistem yang memungkinkan kita untuk mengelolanya sesuai kebutuhan.
### Sejarah
**Database management system (DBMS)** pertama kali dikembangkan pada **tahun 1960** oleh ***Charles Bachman***, dengan tujuan untuk menyediakan cara **menyimpan** dan **memanipulasi data** dalam jumlah besar yang lebih efisien dan terorganisir.

Sistem awal ini didasarkan pada model relasional yang mengatur data kedalam tabel
dan **mendefinisikan hubungan** antar mereka. CODASYL (Conference of Data System
Language) membentuk dan menstandarisasi model ini. DBMS pertama dan
dikomersialkan adalah Information Management System IBM (IMS), dirilis pada tahun
1967.

*Model Relasional **hampir mirip** seperti class diagram atau tables pada spreadsheet*

Pada tahun 1980 model relasional database menjadi paradigma DBMS yang paling
dominasi. Bahasa query SQL dikembangkan untuk basis data relasional dan
distandarisasikan pada akhir tahun 1980. Pada tahun 1990 mulai bermunculan DBMS
seperti MySQL, PostgreSQL, MsSQL Server.

### DBMS (DataBase Management System)
Untuk mengolah sebuah database, kita tidak dapat melakukanya secara manual apalagi jika datanya sudah ribuan. 
Oleh karena itu, kita membutuhkan sebuah software yang disebut DBMS (DataBase Management System). Software ini yang akan membantu kita dalam me-manage database yang kita buat.

- **Relational Database Management Systems** (Database yang saling memiliki keterkaitan antara satu table dengan table lainnya/satu objek dengan objek lainnya yang menghasilkan **schema database**)
	*Macam-macam schema database dapat berupa erd, spreadsheet, class diagram (yang paling mudah dipelajari ialah erd)*
- Hierarchical Database Management Systems
- Network Database Systems
- Object-Oriented Database Systems
- NoSQL Database Systems

Namun, kita akan berfokus pada satu kategori database saja, yaitu **RDBMS atau**
**Relational Database Management Systems**. Dalam kategori database ini, terdapat
berbagai macam software yang menggunakannya, diantaranya Oracle, **MySQL**, SQL
Server, SQLite, DB2, dan lain sebagainya.
### Schema
Schema Database adalah seperti **gambaran rinci** yang menjelaskan **bagaimana data**
**disusun dalam database relasional**. Ini mencakup segala sesuatu mulai dari nama tabel, kolom, jenis data, hingga batasan logis lainnya. 
Schema database berfungsi sebagai **panduan visual** yang membantu kita **berkomunikasi dengan arsitektur database**.

*Dengan menggunakan schema ini, kita dapat memahami dengan jelas bagaimana setiap bagian data diorganisir dan berhubungan satu sama lain dalam database.*

![[contoh schema database.png]]
## ERD & Relasi antar entitas
ERD (Entity Relationship Diagram) dalam merancang database.

Entity Relationship Diagram adalah sebuah model penyajian data dengan
menggunakan Entity dan Relationship (MySQL, PostgreSQL, MariaDB).
Tujuan dari penyajian ini adalah agar desain database dapat dipahami dan dicancang dengan mudah. 

Dalam ERD terdapat berbagai macam simbol yang akan kita gunakan, antara
lain:

- Entitas merupakan representasi dari sebuah objek yang dapat didefinisikan dan diidentifikasikan dengan jelas. Entitas mewakili objek yang akan disimpan dalam database, Contoh seperti makhluk hidup, benda, konsep, tempat dan sebagainya