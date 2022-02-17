# 使用database/sql 标准库连接MySql数据库

### 1.初始化数据库的连接

```go
// 定义一个全局对象db
var db *sql.DB

//初始化数据库连接
func initDB() (err error) {
	dsn := "root:root@tcp(39.105.117.240:3306)/ssm"
	//去初始化全局的db 变量 不是新声明 所以此时不要用 :=
	db, err = sql.Open("mysql", dsn) //open并不是真的连接数据库 只是检查传入的参数
	if err != nil {
		return err
	}

	err = db.Ping() //正式连接数据库
	if err != nil {
		return err

	}

	db.SetConnMaxLifetime(time.Second * 100) //最大存活时间
	db.SetMaxOpenConns(200)                  //最大连接数
	db.SetMaxIdleConns(10)                   //最大空闲连接数
	return nil

}


func main() {

	err := initDB()

	if err != nil {
		fmt.Printf("init db filed,%v\n", err)
		return
	}
	
  //关闭数据库的连接
	defer db.Close()
	
}
```



### 2. 单行查询

```go
//使用结构体来保存数据
type user struct {
	id   int
	name string
	age  int
}

//单行查询

func queryRowDemo() {
	sqlStr := "select id, name, age from user where id=?"
	var u user
	// 非常重要：确保QueryRow之后调用Scan方法，否则持有的数据库链接不会被释放
	err := db.QueryRow(sqlStr, 1).Scan(&u.id, &u.name, &u.age)
	if err != nil {
		fmt.Printf("scan failed, err:%v\n", err)
		return
	}
	fmt.Printf("id:%d name:%s age:%d\n", u.id, u.name, u.age)
}

```



### 3. 多行查询

```go
func queryMultiRowDemo() {
	sqlStr := "select id,name,age from user where id>?"

	rows, err := db.Query(sqlStr, 0) //多行查询使用Query
	if err != nil {
		fmt.Printf("query failed %v\n", err)
		return
	}

	//关闭rows 释放所持有的数据库连接
	defer rows.Close()

	//循环读取 
	for rows.Next() {
		var u user
		err := rows.Scan(&u.id, &u.name, &u.age)
		if err != nil {
			fmt.Printf("scan failed%v\n", err)
			return
		}
		fmt.Printf("id:%d name:%s age:%d\n", u.id, u.name, u.age)
	}

}
```



### 4. 插入数据

```go
//插入数据

func insertDemo() {
	sqlStr := "insert into user(name,age) values(?,?)"

	result, err := db.Exec(sqlStr, "钱六", 18)
	if err != nil {
		fmt.Printf("insert failed %v\n", err)
	}

	//返回新插入数据的id
	id, err := result.LastInsertId()
	if err != nil {
		fmt.Printf("get lastinsert ID failed, err:%v\n", err)
		return
	}
	fmt.Printf("insert success, the id is %d.\n", id)
}

```



### 5.更新数据

```go
//更新数据

func updateRowDemo() {
	sqlStr := "update user set age=? where id=?"

	res, err := db.Exec(sqlStr, 55, 4)
	if err != nil {
		fmt.Printf("update failed, err:%v\n", err)
		return
	}
	//影响行数
	affectlines, err := res.RowsAffected()
	if err != nil {
		fmt.Printf("get RowsAffected failed, err:%v\n", err)
		return
	}
	fmt.Printf("update success, affected rows:%d\n", affectlines)

}
```



### 6. 删除数据

```go
//删除数据

func deleteRow() {
	sqlstr := "delete from user where id=?"
	result, err := db.Exec(sqlstr, 2)
	if err != nil {
		fmt.Printf("update failed, err:%v\n", err)
		return
	}
	//影响行数
	deletelines, err := result.RowsAffected()
	if err != nil {
		fmt.Printf("get RowsAffected failed, err:%v\n", err)
		return
	}
	fmt.Printf("update success, affected rows:%d\n", deletelines)

}

```



### 7.Mysql预处理

为什么要预处理？

1. 优化MySQL服务器重复执行SQL的方法，可以提升服务器性能，提前让服务器编译，一次编译多次执行，节省后续编译的成本。
2. 避免SQL注入问题。



```go
//mysql预处理

func prepareDemo() {
	sqlstr := "select id, name, age from user where id > ?"
	prepare, err := db.Prepare(sqlstr)
	if err != nil {
		fmt.Printf("prepared failed, err:%v\n", err)
		return
	}
	defer prepare.Close()
	query, err := prepare.Query(0)
	if err != nil {
		fmt.Printf("prepar error, err:%v\n", err)
		return
	}
	defer query.Close()
	//循环读取结果
	for query.Next() {
		var u user
		err := query.Scan(&u.id, &u.name, &u.age)
		if err != nil {
			fmt.Printf("scan failed, err:%v\n", err)
			return
		}
		fmt.Printf("id:%d name:%s age:%d\n", u.id, u.name, u.age)
	}
}

```

