# 实验6：基于Oracle的食品冷库管理
## 黄耀辉 软件18-3 201810414311

 ## 一、实验目的：

```
自行设计一个信息系统的数据库项目，自拟某项目名称。
设计项目涉及的表及表空间使用方案。至少5张表和5万条数据，两个表空间。
设计权限及用户分配方案。至少两类角色，两个用户。
在数据库中建立一个程序包，在包中用PL/SQL语言设计一些存储过程和函数，实现比较复杂的业务逻辑，用模拟数据进行执行计划分析。
设计自动备份方案或则手工备份方案。
设计容灾方案。使用两台主机，通过DataGuard实现数据库整体的异地备份(可选)。
```

## 二、表设计

 逻辑结构设计：

（1） 分销员信息：（管理员编号，管理员用户名，管理员密码）。

（2） 仓库信息：（仓库编号，仓库名，仓库地址）

（3） 供应商信息：（供应商编号，供应商姓名，供应商地址，供应商电话）

（4） 客户信息：（客户编号，送货地址,客户电话）

（5） 货物信息：（货物编号，供应商编号，货物名，货物类型，生产日期，保质期）

（6） 入库订单信息：（入库订单编号，仓库编号，货物编号，管理员编号，入库数量，入库时间）

（7） 出库订单信息：（出库订单编号，仓库编号，货物编号，管理员编号，客户编号，出库数量，出库时间）

（8） 库存信息：（仓库编号，货物编号，库存量）



2.1分销员信息（admin）

| **表中列名**   | **数据类型** | **可否为空**   | **说明**   |
| -------------- | ------------ | -------------- | ---------- |
| adminId        | CHAR         | NOT NULL(主键) | 管理员编号 |
| admin_name     | VARCHAR      | NOT NULL       | 管理员名称 |
| admin_password | VARCHAR      | NOT NULL       | 管理员密码 |

2.2仓库信息（warehouse）

| **表中列名** | **数据类型** | **可否为空**   | **说明** |
| ------------ | ------------ | -------------- | -------- |
| warehouseId  | CHAR         | NOT NULL(主键) | 仓库编号 |
| wh_name      | VARCHAR      | NOT NULL       | 仓库名称 |
| wh_address   | VARCHAR      | NOT NULL       | 仓库地址 |

2.3供应商信息（supplier）

| **表中列名** | **数据类型** | **可否为空**   | **说明**       |
| ------------ | ------------ | -------------- | -------------- |
| supplierId   | CHAR         | NOT NULL(主键) | 供应商编号     |
| s_name       | VARCHAR      | NULL           | 供应商名称     |
| s_address    | VARCHAR      | NULL           | 供应商地址     |
| s_telephone  | CHAR         | NULL           | 供应商联系电话 |

 

 

2.4客户信息（clients）

| **表中列名** | **数据类型** | **可否为空**    | **说明**     |
| ------------ | ------------ | --------------- | ------------ |
| clientId     | CHAR         | NOT NULL (主键) | 客户编号     |
| destination  | VARCHAR      | NOT NULL        | 目的地       |
| c_telephone  | CHAR         | NOT NULL        | 客户联系电话 |

 

 

2.5货物信息（goods）

| **表中列名** | **数据类型** | **可否为空**      | **说明**   |
| ------------ | ------------ | ----------------- | ---------- |
| goodId       | CHAR         | NOT NULL (主键)   | 货物编号   |
| supplierId   | CHAR         | NOT NULL (外主键) | 供应商编号 |
| produce_date | DATETIME     | NOT NULL          | 生产日期   |
| g_name       | VARCHAR      | NOT NULL          | 货物名称   |
| g_type       | VARCHAR      | NULL              | 货物种类   |

 

2.6入库订单（storage_order）

| **表中列名** | **数据类型** | **可否为空**      | **说明**     |
| ------------ | ------------ | ----------------- | ------------ |
| storageId    | VARCHAR      | NOT NULL (主键)   | 入库订单编号 |
| warehouseId  | CHAR         | NOT NULL (外主键) | 仓库编号     |
| goodId       | CHAR         | NOT NULL (外主键) | 货物编号     |
| adminId      | CHAR         | NOT NULL (外主键) | 管理员编号   |
| storage_num  | INT          | NOT NULL          | 入库数量     |
| storage_time | DATETIME     |                   | 入库时间     |

2.7出库订单（output_order）

| **表中列名** | **数据类型** | **可否为空**      | **说明**     |
| ------------ | ------------ | ----------------- | ------------ |
| outputId     | VARCHAR      | NOT NULL (主键)   | 出库订单编号 |
| warehouseId  | CHAR         | NOT NULL (外主键) | 仓库编号     |
| goodId       | CHAR         | NOT NULL (外主键) | 货物编号     |
| adminId      | CHAR         | NOT NULL (外主键) | 管理员编号   |
| clientId     | CHAR         | NOT NULL (外主键) | 客户编号     |
| out_num      | INT          | NOT NULL          | 出库数量     |
| out_time     | DATETIME     |                   | 出库时间     |

2.8库存（inventory）

| **表中列名** | **数据类型** | **可否为空**    | **说明** |
| ------------ | ------------ | --------------- | -------- |
| goodId       | CHAR         | NOT NULL (主键) | 货物编号 |
| warehouseId  | CHAR         | NOT NULL (主键) | 仓库编号 |
| inventoryNum | INT          | NULL            | 库存数量 |

 

## 三、实验步骤
### 

### 1、创建表空间、创建角色、创建用户以及权限分配

![avatar](/test6/img/us01.png)

![avatar](/test6/img/us02.png)



### 2、创建表

**2.1****创建分销员信息表**

CREATE TABLE admin (

 adminId CHAR(5),

 admin_name VARCHAR (20) NOT NULL,

 admin_password VARCHAR (20) NOT NULL,

 PRIMARY KEY (adminId)

) ;

![avatar](/test6/img/c01.png)

**2.2**** 创建仓库信息表**

CREATE TABLE warehouse(

 warehouseId CHAR(5),

 wh_name VARCHAR(11) NOT NULL,

 wh_address VARCHAR(20) NOT NULL,

 PRIMARY KEY (warehouseId) 

);

![avatar](/test6/img/c02.png)

**2.3****创建供应商存储信息表**

CREATE TABLE supplier(

supplierId CHAR(5),

s_name VARCHAR(11),

s_address VARCHAR(11),

s_telephone CHAR(11),

PRIMARY KEY(supplierId)

);

 ![avatar](/test6/img/c03.png)

**2.4****客户信息表**

CREATE TABLE clients(

clientId CHAR(5),

destination VARCHAR(20) NOT NULL, -- 目的地

c_telephone CHAR(11) NOT NULL,

PRIMARY KEY(clientId)

);

![avatar](/test6/img/c04.png)

**2.5****货物信息表**

CREATE TABLE goods(

goodId CHAR(5),

supplierId CHAR(5),

g_name VARCHAR(11) NOT NULL, 

g_type VARCHAR(11) , -- 货物种类

PRIMARY KEY(goodId,supplierId),

FOREIGN KEY(supplierId) REFERENCES supplier(supplierId)

);

 ![avatar](/test6/img/c05.png)

**2.6****入库订单信息表**

CREATE TABLE storage_order(

storageId VARCHAR(10),

warehouseId CHAR(5),

goodId CHAR(5),

adminId CHAR(5) ,

storage_num INT NOT NULL, -- 入库量

storage_time DATETIME , -- 入库时间

PRIMARY KEY (storageId,warehouseId,goodId,adminId),

FOREIGN KEY(warehouseId) REFERENCES warehouse(warehouseId),

FOREIGN KEY(goodId) REFERENCES goods(goodId),

FOREIGN KEY(adminId) REFERENCES admin(adminId)

);

 ![avatar](/test6/img/c06.png)

**2.7****出库订单信息表**

CREATE TABLE output_order(

outputId VARCHAR(10),

warehouseId CHAR(5),

goodId CHAR(5),

adminId CHAR(5) ,

clientId CHAR(5), 

out_num INT NOT NULL, -- 出库量

out_time DATETIME, -- 出库时间

PRIMARY KEY (outputId,warehouseId,goodId,adminId,clientId),

FOREIGN KEY(warehouseId) REFERENCES warehouse(warehouseId),

FOREIGN KEY(goodId) REFERENCES goods(goodId),

FOREIGN KEY(adminId) REFERENCES admin(adminId),

FOREIGN KEY(clientId) REFERENCES clients(clientId)

);

![avatar](/test6/img/c07.png)

**2.8 **库存信息表**

CREATE TABLE inventory(

warehouseId CHAR(5),

goodId CHAR(5),

inventoryNum INT,

PRIMARY KEY(goodId,warehouseId) 

);

![avatar](/test6/img/c08.png)

### 三、插入数据

3.1向客户表中插入一万条数据

![avatar](/test6/img/in01.png)

3.2向供应商表中插入一万条数据

![avatar](/test6/img/in02.png)

### 四、存储过程

- 创建一个存储过程，求所有超过指定库存大小的的货物的平均库存（传入参数为指定库存大小）。 

CREATE PROCEDURE AvgInvent(IN minInvent SMALLINT, OUT avgInvent DECIMAL)

BEGIN 
  	SELECT AVG(inventoryNum) FROM inventory WHERE inventoryNum >= minInvent 
  	INTO avgInvent;   
  END;

![avatar](/test6/img/sq01.png)

- 创建一个存储过程，求出最大以及最小库存货物的数量。
  CREATE PROCEDURE MMInvent(OUT maxInvent DECIMAL,OUT minInvent DECIMAL)
    BEGIN 
    	SELECT MAX(inventory.`inventoryNum`),MIN(inventory.`inventoryNum`) INTO maxInvent,minInvent
    	FROM inventory ;
    END

![avatar](/test6/img/sq02.png)

### 五、Oracle备份

windows环境

```
@echo off
set t_date=%date% 
set t_time=%time%
set t_n=%t_date:~0,4%
set t_y=%t_date:~5,2% 
set t_r=%t_date:~8,2% 
set t_h=%t_time:~0,2%
set t_m=%t_time:~3,2%
set full_name=CTEurope%t_y%%t_r%%t_h%%t_m%
exp eutest/1@gentle file=%full_name%.dmp1
"C:\Program Files\WinRAR\Rar.exe" a -k -r -s -m1 %full_name%.rar %full_name%.dmp
del %full_name%.dmp
```



### 六、实验总结

​	本次实验是一个基于oracle的食品冷库管理数据库设计，实验内容是我根据之前设计过的mysql数据库进行的，通过这次实验体会到两种数据库在设计理念上的不同，同时也遇到了许多问题，自己对Oracle还是不够熟悉，还需要深入学习。