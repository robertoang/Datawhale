1.1
CREATE TABLE Addressbook
(regist_no INTEGER NOT NULL ,
name VARCHAR(128) NOT NULL ,
address VARCHAR(256) NOT NULL ,
tel_no CHAR(10) ,
mail_address CHAR(20) ,
PRIMARY KEY (regist_no) 
);

1.2
ALTER TABLE Addressbook ADD COLUMN postal_code CHAR(8) NOT NULL;

1.3
DROP TABLE Addressbook;

1.4
CREATE TABLE Addressbook
(regist_no INTEGER NOT NULL ,
name VARCHAR(128) NOT NULL ,
address VARCHAR(256) NOT NULL ,
tel_no CHAR(10) ,
mail_address CHAR(20) ,
PRIMARY KEY (regist_no) 
);
