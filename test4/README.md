# 实验4：对象管理
## 黄耀辉 软件18-3 201810414311

 ## 一、实验目的：
   了解Oracle表和视图的概念，学习使用SQL语句Create Table创建表，学习Select语句插入，修改，删除以及查询数据，学习使用SQL语句创建视图，学习部分存储过程和触发器的使用。
## 二、实验内容
  ### 录入数据：

  要求至少有1万个订单，每个订单至少有4个详单。至少有两个部门，每个部门至少有1个员工，其中只有一个人没有领导，一个领导至少有一个下属，并且它的下属是另一个人的领导（比如A领导B，B领导C）。

  ### 序列的应用

  插入ORDERS和ORDER_DETAILS 两个表的数据时，主键ORDERS.ORDER_ID, ORDER_DETAILS.ID的值必须通过序列SEQ_ORDER_ID和SEQ_ORDER_ID取得，不能手工输入一个数字。

  ### 触发器的应用：

  维护ORDER_DETAILS的数据时（insert,delete,update）要同步更新ORDERS表订单应收货款ORDERS.Trade_Receivable的值。
  ### 查询数据：
1.查询某个员工的信息
2.递归查询某个员工及其所有下属，子下属员工。
3.查询订单表，并且包括订单的订单应收货款: Trade_Receivable= sum(订单详单表.ProductNum*订单详单表.ProductPrice)- Discount。
4.查询订单详表，要求显示订单的客户名称和客户电话，产品类型用汉字描述。
5.查询出所有空订单，即没有订单详单的订单。
6.查询部门表，同时显示部门的负责人姓名。
7.查询部门表，统计每个部门的销售总金额


## 三、实验步骤
### 首先创建自己的账号harris，然后以system身份登录

### 一、用户授权

![avatar](/test4/01-用户授权.png)

### 二、用户登录

![avatar](/test4/02-harris登录.png)

### 三、删除可能存在的表和序列

![avatar](/test4/04-删除原有.png)

### 四、创建表

![avatar](/test4/05-表的创建.png)

### 五、分区设计

![avatar](/test4/06-分区设计.png)

### 六、数据录入

![avatar](/test4/07-数据录入.png)

### 七、查询
#### 1.查询某个员工的信息

![avatar](/test4/查询某员工.png)

#### 2.递归查询某个员工及其所有下属，子下属员工。

![avatar](/test4/查询02.png)

#### 3.查询订单表，并且包括订单的订单应收货款: Trade_Receivable= sum(订单详单表.ProductNum*订单详单表.ProductPrice)- Discount。

![avatar](/test4/查询订单表.png)

#### 4.查询订单详表，要求显示订单的客户名称和客户电话，产品类型用汉字描述。

![avatar](/test4/订单查询汉字显示.png)

#### 5.查询出所有空订单，即没有订单详单的订单。

 ' ' '

SELECT * 
FROM orders
WHERE order_id NOT in(
    SELECT o.order_id FROM orders o,order_details d 
    WHERE o.order_id = d.order_id
)

  ' ' '

![avatar](/test4/空订单.png)

#### 6.查询部门表，同时显示部门的负责人姓名。

![avatar](/test4/部门表.png)

#### 7.查询部门表，统计每个部门的销售总金额。

![avatar](/test4/.png)

SELECT d.department_name, SUM(SUM1)
FROM(
SELECT (d.product_num*d.product_price) sum1
FROM order_details d,orders o,departments d,employees e
WHERE d.department_id =e.department_id
and o.employee_id = e.employee_id
and o.order_id = o.order_id
),departments d
group by d.department_name