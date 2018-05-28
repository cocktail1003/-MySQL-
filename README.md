# -MySQL-
外键约束作用

## 外键约束
所谓外键约束，就是指外键的作用。之前所讲的外键的作用都是默认的作用，实际上，可以通过对外键的需求，进行定制操作。

外键约束有三种模式，分别为：

district：严格模式（默认），父表不能删除或更新一个已经被子表数据引用的记录；
cascade：级联模式，父表的操作，对应子表关联的数据也跟着被删除；
set null：置空模式，父表的操作之后，子表对应的数据（外键字段）被置空。
在此需要注意：以上三种模式，都是对父表的约束。

基本语法：foreign key(外键字段) + references + 父表(主键字段) + [on delete + 模式 + on update + 模式];
通常一个合理的做法（约束模式）是：删除的时候，	子表被置空；更新的时候，子表进行级联操作。

执行如下 SQL 语句，进行测试：

-- 创建外键，指定模式：删除置空，更新级联
create table my_foreign3(
	id int primary key auto_increment,
	name varchar(20) not null,
	c_id int,
	-- 增加外键
	foreign key(c_id)
	-- 引用父表
	references class(id)
	-- 指定删除模式
	on delete set null
	-- 指定更新模式
	on update cascade
)charset utf8;
foreign8

如上图所示，在我们指定外键的约束模式之后，通过查看表的创建语句，可以看到具体的约束语句。

接下来，执行如下 SQL 语句，继续进行测试：

-- 插入数据
insert into my_foreign3 values(null,'Jobs',1),
(null,'Bill',1),
(null,'Mark',1),
(null,'Swift',2),
(null,'Sellen',1);
foreign9

如上图所示，我们向表my_foreign3中插入了 5 条记录。接下来，我们就可以测试外键的级联模式和置空模式啦！呃，对啦，前提是我们需要把与父表class相关联的除my_foreign3之外的其他子表，也就是my_foreign1和my_foreign2的外键删除掉，否则的话，由于这两个子表的外键使用了严格模式，会干扰我们接下来的测试。

在我们删除表my_foreign1和my_foreign2的外键之后，执行如下 SQL 语句，测试级联模式：

-- 更新父表主键
update class set id = 8 where id = 1;
foreign10

执行如下 SQL 语句，测试置空模式：

-- 删除父表主键
delete from class where id = 2;
foreign11

通过以上测试，我们已经验证了级联模式和置空模式的效果。其实，在我们进行删除置空操作的时候，有一个前提，那就是：子表的外键字段必须允许为空，否则的话，操作是无法成功的。

至此，我们已经把外键的相关操作都演示了一遍。在这里，我们会发现外键的功能非常强大，能够进行各种的约束，也正是由于外键这种约束的强大，其降低了开发语言对数据的可控性，因此在实际的开发中，很少使用外键来处理数据。

