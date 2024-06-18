# SQL 查询任务

## 1. 查询雇员信息
从 `Employees` 表中查询所有雇员的以下信息：雇员编号，姓氏，名字，职务和出生日期。

```sql
SELECT 
    EmployeeID AS '雇员编号',
    LastName AS '姓氏',
    FirstName AS '名字',
    Title AS '职务',
    Birthdate AS '出生日期'
FROM Employees;
```

## 2. 查询产品信息
从 `Products` 表中查询出价格在 20 美元到 50 美元之间的产品的以下信息：产品编号，产品名字，类别编号，供货商编号，单价，库存量。
```sql

SELECT 
    ProductID AS '产品编号',
    ProductName AS '产品名字',
    CategoryID AS '类别编号',
    SupplierID AS '供货商编号',
    UnitPrice AS '单价',
    UnitsInStock AS '库存量'
FROM Products
WHERE UnitPrice BETWEEN 20 AND 50;
```

## 3. 查询特定时间段订单
查询一下 1997 年第二季度的所有订单信息。
```sql

SELECT *
FROM Orders
WHERE YEAR(OrderDate) = 1997 AND MONTH(OrderDate) IN (4, 5, 6);
```

## 4. 查询特定条件的客户信息
从 `Customers` 表中查询出 `ContactName` 以“n”结尾的客户公司信息。

```sql
SELECT *
FROM Customers
WHERE ContactName LIKE '%n';
```

# SQL 管理任务

## 1. 创建登录连接账户

创建一个登录连接账户 `ProjectAdmin2024`，密码为 `Admin2024`。

```sql
EXEC sp_addlogin 'ProjectAdmin2024', 'Admin2024';
```
## 2. 授予创建数据库的权限
为 `ProjectAdmin2024` 授予创建数据库的权限。
```sql
EXEC sp_addsrvrolemember 'ProjectAdmin2024', 'dbcreator';
```
## 3. 映射数据库用户
将 `ProjectAdmin2024` 映射成 `Northwind` 数据库中的 `NWind2024` 的用户。

```sql
USE Northwind;
EXEC sp_adduser 'ProjectAdmin2024', 'NWind2024';
```
## 4. 添加角色成员
使 `NWind2024` 成为 `Northwind` 的 `db_owner` 的成员。

```sql
USE Northwind;
EXEC sp_addrolemember 'db_owner', 'NWind2024';
```

# SQL 数据库管理任务

## 1. 创建 `Employees` 表

创建一个名为 `Employees` 的表，包含以下字段：`EmployeeID`（主键，整数类型，标识列），`Name`（字符串类型），`Department`（字符串类型），`Salary`（货币类型）。

```sql
CREATE TABLE Employees (
    EmployeeID INT IDENTITY PRIMARY KEY,
    Name VARCHAR(32),
    Department VARCHAR(64),
    Salary MONEY
);
```
## 2. 添加唯一约束
为 `Employees` 表添加一个唯一约束，确保每个员工的名称是唯一的。

```sql
ALTER TABLE Employees ADD CONSTRAINT unq_Employees_Name UNIQUE(Name);
```
## 3. 添加检查约束
为 `Employees` 表中的 `Salary` 列添加一个检查约束，确保薪水大于 0。

```sql
ALTER TABLE Employees ADD CONSTRAINT chk_Employees_Salary CHECK (Salary > 0);
```
## 4. 插入数据
向 `Employees` 表中加入 2 条数据。

```sql
INSERT INTO Employees (Name, Department, Salary) VALUES ('Antony Blinken', 'CS', 4999);
INSERT INTO Employees (Name, Department, Salary) VALUES ('Joe Biden', 'NT', 14999);
```
## 5. 修改数据
修改第二条数据的名字为 `Bill Clinton`。

```sql
UPDATE Employees SET Name = 'Bill Clinton' WHERE EmployeeID = 2;
```
## 6. 删除数据
删除第二条数据。

```sql
DELETE FROM Employees WHERE EmployeeID = 2;
```

# SQL 定义任务

## 1. 创建函数 `fn_CategoryName`
```sql
CREATE FUNCTION fn_CategoryName(@CategoryID int)
RETURNS varchar(32)
AS
BEGIN
    DECLARE @name varchar(32);
    SELECT @name = CategoryName FROM Categories WHERE CategoryID = @CategoryID;
    RETURN @name;
END;
```
## 2. 创建函数 `fn_YearOrderFrom`
```sql
CREATE FUNCTION fn_YearOrderFrom(@Year int)
RETURNS int
AS
BEGIN
    DECLARE @num int;
    SELECT @num = COUNT(*) FROM Orders WHERE YEAR(OrderDate) = @Year;
    RETURN @num;
END;
```
## 3. 创建存储过程 `proc_Employees_Insert`
```sql
CREATE PROCEDURE proc_Employees_Insert
    @Name varchar(32),
    @Department varchar(64),
    @Salary money
AS
BEGIN
    INSERT INTO Employees (Name, Department, Salary)
    VALUES (@Name, @Department, @Salary);
END;
```
## 4. 创建触发器 `try_OrderDetails_Insert`
```sql
CREATE TRIGGER try_OrderDetails_Insert
ON [Order Details]
FOR INSERT
AS
BEGIN
    PRINT 'Data has been inserted into Order Details';
END;
```
