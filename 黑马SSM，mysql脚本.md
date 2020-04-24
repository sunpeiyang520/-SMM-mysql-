SSM框架

# 1、环境搭建

1、数据库环境

```sql
/*创建表，此处不要定义主键，虽然会影响索引啊什么的，但练习不需要*/
CREATE DATABASE SSM;
USE ssm;
CREATE TABLE product(
id VARCHAR(32) DEFAULT '1' PRIMARY KEY,
productNum VARCHAR(50) NOT NULL,
productName VARCHAR(50),
cityName VARCHAR(50),
DepartureTime TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
productPrice DOUBLE DEFAULT NULL,
productDesc VARCHAR(500),
productStatus INT,
UNIQUE KEY product(id,productNum)
)ENGINE=INNODB DEFAULT CHARSET=utf8;

/*创建uuid触发器*/
DELIMITER $
CREATE TRIGGER `product_before_insert` BEFORE INSERT ON `product` FOR EACH ROW 
BEGIN
IF new.Id = '1' THEN
SET new.id =REPLACE(UUID(),"-","");
END IF;
END$
DELIMITER ;
/*插入数据*/
insert into PRODUCT (id, productnum, productname, cityname, departuretime, productprice,
productdesc, productstatus)
values ('676C5BD1D35E429A8C2E114939C5685A', 'sher6j-002', '北京三日游', '北京', STR_TO_DATE('10-10-2018 10:10:00.000000', '%d-%m-%Y %H:%i:%s.%f'), 1200, '不错的旅行', 1);
insert into PRODUCT (id, productnum, productname, cityname, departuretime, productprice,
productdesc, productstatus)
values ('12B7ABF2A4C544568B0A7C69F36BF8B7', 'sher6j-003', '上海五日游', '上海', STR_TO_DATE('25-
04-2018 14:30:00.000000', '%d-%m-%Y %H:%i:%s.%f'), 1800, '魔都我来了', 0);
insert into PRODUCT (id, productnum, productname, cityname, departuretime, productprice,
productdesc, productstatus)
values ('9F71F01CB448476DAFB309AA6DF9497F', 'sher6j-001', '北京三日游', '北京', STR_TO_DATE('10-
10-2018 10:10:00.000000', '%d-%m-%Y %H:%i:%s.%f'), 1200, '不错的旅行', 1);

#先创建会员表
DROP TABLE IF EXISTS member;
CREATE TABLE member(
       id VARCHAR(32) DEFAULT '1' PRIMARY KEY ,
       NAME VARCHAR(20),
       nickname VARCHAR(20),
       phoneNum VARCHAR(20),
       email VARCHAR(20) 
)ENGINE=INNODB DEFAULT CHARSET=utf8;
#创建uuid触发器
DELIMITER $
CREATE TRIGGER `member_before_insert` BEFORE INSERT ON `member` FOR EACH ROW 
BEGIN
IF new.Id = '1' THEN
SET new.id =REPLACE(UUID(),"-",""); 
END IF;
END$
DELIMITER ;
INSERT INTO MEMBER (id, NAME, nickname, phonenum, email)
VALUES ('E61D65F673D54F68B0861025C69773DB', '张三', '小三', '18888888888', 'zs@163.com');



#订单表
DROP TABLE IF EXISTS orders;
CREATE TABLE orders(  
id VARCHAR(32) DEFAULT '1' PRIMARY KEY,  
orderNum VARCHAR(20) NOT NULL UNIQUE,  
orderTime TIMESTAMP,  
peopleCount INT, 
orderDesc VARCHAR(500),  
payType INT, 
orderStatus INT,  
productId VARCHAR(32), 
memberId VARCHAR(32),  
FOREIGN KEY (productId) REFERENCES product(id),
FOREIGN KEY (memberId) REFERENCES member(id)
)ENGINE=INNODB DEFAULT CHARSET=utf8;

DELIMITER $
CREATE TRIGGER `orders_before_insert` BEFORE INSERT ON `orders` FOR EACH ROW 
BEGIN
IF new.Id = '1' THEN
SET new.id =REPLACE(UUID(),"-",""); 
END IF;
END$
DELIMITER ;

---插入一些订单记录
insert into ORDERS (id, ordernum, ordertime, peoplecount, orderdesc, paytype, orderstatus, productid, memberid)
values ('0E7231DC797C486290E8713CA3C6ECCC', '12345', STR_TO_DATE('02-03-2018 12:00:00.000000', '%d-%m-%Y %H:%i:%s.%f'), 2, '没什么', 0, 1, '676C5BD1D35E429A8C2E114939C5685A', 'E61D65F673D54F68B0861025C69773DB');
insert into ORDERS (id, ordernum, ordertime, peoplecount, orderdesc, paytype, orderstatus, productid, memberid)
values ('5DC6A48DD4E94592AE904930EA866AFA', '54321', STR_TO_DATE('02-03-2018 12:00:00.000000', '%d-%m-%Y %H:%i:%s.%f'), 2, '没什么', 0, 1, '676C5BD1D35E429A8C2E114939C5685A', 'E61D65F673D54F68B0861025C69773DB');
insert into ORDERS (id, ordernum, ordertime, peoplecount, orderdesc, paytype, orderstatus, productid, memberid)
values ('2FF351C4AC744E2092DCF08CFD314420', '67890', STR_TO_DATE('02-03-2018 12:00:00.000000', '%d-%m-%Y %H:%i:%s.%f'), 2, '没什么', 0, 1, '12B7ABF2A4C544568B0A7C69F36BF8B7', 'E61D65F673D54F68B0861025C69773DB');
insert into ORDERS (id, ordernum, ordertime, peoplecount, orderdesc, paytype, orderstatus, productid, memberid)
values ('A0657832D93E4B10AE88A2D4B70B1A28', '98765', STR_TO_DATE('02-03-2018 12:00:00.000000', '%d-%m-%Y %H:%i:%s.%f'), 2, '没什么', 0, 1, '12B7ABF2A4C544568B0A7C69F36BF8B7', 'E61D65F673D54F68B0861025C69773DB');
insert into ORDERS (id, ordernum, ordertime, peoplecount, orderdesc, paytype, orderstatus, productid, memberid)
values ('E4DD4C45EED84870ABA83574A801083E', '11111', STR_TO_DATE('02-03-2018 12:00:00.000000', '%d-%m-%Y %H:%i:%s.%f'), 2, '没什么', 0, 1, '12B7ABF2A4C544568B0A7C69F36BF8B7', 'E61D65F673D54F68B0861025C69773DB');
insert into ORDERS (id, ordernum, ordertime, peoplecount, orderdesc, paytype, orderstatus, productid, memberid)
values ('96CC8BD43C734CC2ACBFF09501B4DD5D', '22222', STR_TO_DATE('02-03-2018 12:00:00.000000', '%d-%m-%Y %H:%i:%s.%f'), 2, '没什么', 0, 1, '12B7ABF2A4C544568B0A7C69F36BF8B7', 'E61D65F673D54F68B0861025C69773DB');
insert into ORDERS (id, ordernum, ordertime, peoplecount, orderdesc, paytype, orderstatus, productid, memberid)
values ('55F9AF582D5A4DB28FB4EC3199385762', '33333', STR_TO_DATE('02-03-2018 12:00:00.000000', '%d-%m-%Y %H:%i:%s.%f'), 2, '没什么', 0, 1, '9F71F01CB448476DAFB309AA6DF9497F', 'E61D65F673D54F68B0861025C69773DB');
insert into ORDERS (id, ordernum, ordertime, peoplecount, orderdesc, paytype, orderstatus, productid, memberid)
values ('CA005CF1BE3C4EF68F88ABC7DF30E976', '44444', STR_TO_DATE('02-03-2018 12:00:00.000000', '%d-%m-%Y %H:%i:%s.%f'), 2, '没什么', 0, 1, '9F71F01CB448476DAFB309AA6DF9497F', 'E61D65F673D54F68B0861025C69773DB');
insert into ORDERS (id, ordernum, ordertime, peoplecount, orderdesc, paytype, orderstatus, productid, memberid)
values ('3081770BC3984EF092D9E99760FDABDE', '55555', STR_TO_DATE('02-03-2018 12:00:00.000000', '%d-%m-%Y %H:%i:%s.%f'), 2, '没什么', 0, 1, '9F71F01CB448476DAFB309AA6DF9497F', 'E61D65F673D54F68B0861025C69773DB');

-- 旅客
DROP TABLE IF EXISTS traveller;
CREATE TABLE traveller(
  id VARCHAR(32) DEFAULT '1' PRIMARY KEY,
  NAME VARCHAR(20),
  sex VARCHAR(20),
  phoneNum VARCHAR(20),
  credentialsType INT,
  credentialsNum VARCHAR(50),
  travellerType INT
)ENGINE=INNODB DEFAULT CHARSET=utf8;
insert into TRAVELLER (id, name, sex, phonenum, credentialstype, credentialsnum, travellertype)
values ('3FE27DF2A4E44A6DBC5D0FE4651D3D3E', '张龙', '男', '13333333333', 0, '123456789009876543', 0);
insert into TRAVELLER (id, name, sex, phonenum, credentialstype, credentialsnum, travellertype)
values ('EE7A71FB6945483FBF91543DBE851960', '张小龙', '男', '15555555555', 0, '987654321123456789', 1);


-- 订单与旅客中间表
drop table order_traveller;
CREATE TABLE order_traveller(
  orderId varchar2(32),
  travellerId varchar2(32),
  PRIMARY KEY (orderId,travellerId),
  FOREIGN KEY (orderId) REFERENCES orders(id),
  FOREIGN KEY (travellerId) REFERENCES traveller(id)
)ENGINE=INNODB DEFAULT CHARSET=utf8;

insert into ORDER_TRAVELLER (orderid, travellerid)
values ('0E7231DC797C486290E8713CA3C6ECCC', '3FE27DF2A4E44A6DBC5D0FE4651D3D3E');
insert into ORDER_TRAVELLER (orderid, travellerid)
values ('2FF351C4AC744E2092DCF08CFD314420', '3FE27DF2A4E44A6DBC5D0FE4651D3D3E');
insert into ORDER_TRAVELLER (orderid, travellerid)
values ('3081770BC3984EF092D9E99760FDABDE', 'EE7A71FB6945483FBF91543DBE851960');
insert into ORDER_TRAVELLER (orderid, travellerid)
values ('55F9AF582D5A4DB28FB4EC3199385762', 'EE7A71FB6945483FBF91543DBE851960');
insert into ORDER_TRAVELLER (orderid, travellerid)
values ('5DC6A48DD4E94592AE904930EA866AFA', '3FE27DF2A4E44A6DBC5D0FE4651D3D3E');
insert into ORDER_TRAVELLER (orderid, travellerid)
values ('96CC8BD43C734CC2ACBFF09501B4DD5D', 'EE7A71FB6945483FBF91543DBE851960');
insert into ORDER_TRAVELLER (orderid, travellerid)
values ('A0657832D93E4B10AE88A2D4B70B1A28', '3FE27DF2A4E44A6DBC5D0FE4651D3D3E');
insert into ORDER_TRAVELLER (orderid, travellerid)
values ('CA005CF1BE3C4EF68F88ABC7DF30E976', 'EE7A71FB6945483FBF91543DBE851960');
insert into ORDER_TRAVELLER (orderid, travellerid)
values ('E4DD4C45EED84870ABA83574A801083E', 'EE7A71FB6945483FBF91543DBE851960');


select * from orders;

select * from member;

select * from traveller;

select * from order_traveller;




-- 用户表
CREATE TABLE users(
id VARCHAR(32) DEFAULT '1' PRIMARY KEY,
email VARCHAR(50) UNIQUE NOT NULL,
username VARCHAR(50) UNIQUE NOT NULL,
PASSWORD VARCHAR(100),
phoneNum VARCHAR(20),
`STATUS` INT
)ENGINE=INNODB DEFAULT CHARSET=utf8;

DELIMITER $
CREATE TRIGGER `users_before_insert` BEFORE INSERT ON `users` FOR EACH ROW 
BEGIN
IF new.Id = '1' THEN
SET new.id =REPLACE(UUID(),"-",""); 
END IF;
END$
DELIMITER ;

-- 角色表
CREATE TABLE role(
id VARCHAR(32) DEFAULT '1' PRIMARY KEY,
roleName VARCHAR(50) ,
roleDesc VARCHAR(50)
)ENGINE=INNODB DEFAULT CHARSET=utf8;

DELIMITER $
CREATE TRIGGER `role_before_insert` BEFORE INSERT ON `role` FOR EACH ROW 
BEGIN
IF new.Id = '1' THEN
SET new.id =REPLACE(UUID(),"-",""); 
END IF;
END$
DELIMITER ;


-- 用户角色关联表
CREATE TABLE users_role(
userId VARCHAR(32),
roleId VARCHAR(32),
PRIMARY KEY(userId,roleId),
FOREIGN KEY (userId) REFERENCES users(id),
FOREIGN KEY (roleId) REFERENCES role(id)
)ENGINE=INNODB DEFAULT CHARSET=utf8;

-- 资源权限表

CREATE TABLE permission(
id VARCHAR(32) DEFAULT '1' PRIMARY KEY,
permissionName VARCHAR(50) ,
url VARCHAR(50)
)ENGINE=INNODB DEFAULT CHARSET=utf8;

DELIMITER $
CREATE TRIGGER `permission_before_insert` BEFORE INSERT ON `permission` FOR EACH ROW 
BEGIN
IF new.Id = '1' THEN
SET new.id =REPLACE(UUID(),"-",""); 
END IF;
END$
DELIMITER ;

-- 角色权限关联表
CREATE TABLE role_permission(
permissionId varchar2(32),
roleId varchar2(32),
PRIMARY KEY(permissionId,roleId),
FOREIGN KEY (permissionId) REFERENCES permission(id),
FOREIGN KEY (roleId) REFERENCES role(id)
)ENGINE=INNODB DEFAULT CHARSET=utf8;


INSERT INTO users VALUES('111','278755058@qq.com','root1','root','12345678911',1);
INSERT INTO users VALUES('222','nihao@qq.com','root2','root','12345678911',1);

INSERT INTO role VALUES('1111','ADMIN','VIP');
INSERT INTO role VALUES('2222','USER','VIP');

INSERT INTO users_role VALUES('111','1111');
INSERT INTO users_role VALUES('111','2222');
INSERT INTO users_role VALUES('222','2222');


INSERT INTO permission(id,permissionname, url)VALUES('220BBB0940064ECABD2DB62B1FC221D3','user findAll','/user/findAll.do');
INSERT INTO permission(id,permissionname, url)VALUES('92FD2F5648C240B1AD80E0AA7A773F18','user findById','/user/findById.do');

INSERT INTO role_permission VALUES('220BBB0940064ECABD2DB62B1FC221D3','1111');
INSERT INTO role_permission VALUES('92FD2F5648C240B1AD80E0AA7A773F18','1111');
INSERT INTO role_permission VALUES('220BBB0940064ECABD2DB62B1FC221D3','2222');


---创建AOP日志表
CREATE TABLE sysLog(
id VARCHAR(32) DEFAULT '1' PRIMARY KEY,
visitTime TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
username VARCHAR(50),
ip VARCHAR(30),
url VARCHAR(50),
executionTime INT,
method VARCHAR(200)
)ENGINE=INNODB DEFAULT CHARSET=utf8;

DELIMITER $
CREATE TRIGGER `sysLog_before_insert` BEFORE INSERT ON `sysLog` FOR EACH ROW 
BEGIN
IF new.Id = '1' THEN
SET new.id =REPLACE(UUID(),"-",""); 
END IF;
END$
DELIMITER ;

select * from sysLog;

---清空日志信息
truncate table sysLog;



```

