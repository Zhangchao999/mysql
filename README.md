# mysql
---------
# TOP
[触发器](#触发器)<br>
[存储过程](#存储过程)<br>


---------






## 触发器

前提 <br>
有两个表 t_student(id,studentName,studentDesc) t_user(id,userName)

t_user 用来收集 insert 操作的t_student 数据
例如 在 t_student 表中添加了一个字段（null,'张三','3年2班'）的话，会自动的在t_user 上生成 一行数据（null,'张三'）；这就是触发器。 <br>

添加操作：
``` sql
create TRIGGER insert_student_to_user 
AFTER INSERT ON t_student FOR EACH ROW
BEGIN
INSERT INTO t_user VALUES(null,NEW.studentName);
END
```
``` sql
create trigger 触发器名称
触发条件(before | after )(insert | update| delete) ON 哪一个表  FOR EACH ROW (FOR EACH ROW为固定格式)
BEGIN 
 操作
END
```

查看结果：<br>
![图片1](https://github.com/Zhangchao999/mysql/raw/master/picture/trigger/1.png)
![图片2](https://github.com/Zhangchao999/mysql/raw/master/picture/trigger/2.png)
<br>

删除操作：
```sql
create TRIGGER delete_student_to_user 
AFTER DELETE ON t_student FOR EACH ROW
BEGIN
DELETE FROM t_user WHERE userName=OLD.studentName;
END
```

查看结果：<br>
![图片3](https://github.com/Zhangchao999/mysql/raw/master/picture/trigger/3.png)
![图片4](https://github.com/Zhangchao999/mysql/raw/master/picture/trigger/4.png)
<br>
```sql
触发器的NEW 和 OLD：
NEW 为添加的字段对象；
OLD 为删除的字段对象 ;
```


## 存储过程

简单的实例：(查询studentDesc 字段为good的数据)
```sql
CREATE PROCEDURE select_student_good()
BEGIN
	SELECT * from t_student where studentDesc='good';
END
```
查询：
```sql
call select_student_good();
```

查看结果：<br>
开始数据：<br>
![图片5](https://github.com/Zhangchao999/mysql/raw/master/picture/procedure/5.png)<br>
查询后的数据：<br>
![图片6](https://github.com/Zhangchao999/mysql/raw/master/picture/procedure/6.png)
<br>

带IN参数的：
```sql
CREATE PROCEDURE select_student(
	IN description VARCHAR(20)
)
BEGIN

	SELECT * from t_student where studentDesc=description;
END
```
查询：
```sql
call select_student('bad');
```
查看结果：<br>
查询后的数据：<br>
![图片7](https://github.com/Zhangchao999/mysql/raw/master/picture/procedure/7.png)
<br>

带IN 和 OUT 的存储过程：
![图片8](https://github.com/Zhangchao999/mysql/raw/master/picture/procedure/8.png)
<br>
