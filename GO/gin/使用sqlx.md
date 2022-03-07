#  使用Sqlx操作Mysql 数据库

### 1. 连接数据库

```go
package main

//sql db

import (
	"fmt"
	_ "github.com/go-sql-driver/mysql"
	"github.com/jmoiron/sqlx"
)


var db *sqlx.DB


//连接数据库
func initDB()(err error){
	dsn := "root:root@tcp(127.0.0.1:3306)/ssm"
	db, err = sqlx.Connect("mysql", dsn)
	if err != nil {
		fmt.Printf("connect db failed,err:%v\n",err)

	}
	db.SetMaxOpenConns(20)
	db.SetMaxIdleConns(10)
	return

}


func main() {

	err := initDB()
	if err != nil {
		fmt.Printf("初始化连接失败",err)
		return
	}

	defer db.Close()
	//queryRowDemo()
	queryMultiRowDemo()

}


```



### 2.查询单条数据

```go
//查询单条数据

func queryRowDemo(){
	sqlStr :="select id,name,age from user where id=?"
	var u user
	//使用Get 进行单条数据查询
	err := db.Get(&u, sqlStr, 1)
	if err != nil {
		fmt.Printf("查询失败,%v\n",err)
		return
	}
	fmt.Printf("id:%d,age：%d,name:%s\n",u.Id,u.Age,u.Name)

}
```



### 3.查询多条数据

```go
// 查询多条数据
func queryMultiRowDemo(){
	sqlStr := "select id,name,age from user where id>?"
	var users []user
	err := db.Select(&users, sqlStr, 1)
	if err != nil {
		fmt.Printf("查询多条语句失败%v\n",err)
		return
	}
	fmt.Printf("%#v\n",users)

}
```



### 4.批量插入数据

```go
func main() {

	err := initDB()
	if err != nil {
		fmt.Printf("初始化连接失败",err)
		return
	}

	defer db.Close()
	//queryRowDemo()
	//queryMultiRowDemo()


	u1 := user{Name: "七米", Age: 18}
	u2 := user{Name: "q1mi", Age: 28}
	u3 := user{Name: "小王子", Age: 38}


	// 方法3
	users3 := []*user{&u1, &u2, &u3}
	err = BatchInsertUsers3(users3)
	if err != nil {
		fmt.Printf("BatchInsertUsers3 failed, err:%v\n", err)
	}

}


type user struct {
	Id int
	Age int `db:"age"`
	Name string `db:"name"`
}

//进行批量插入需要实现driver.Value
func (u user) Value() (driver.Value,error){
	return []interface{}{u.Name,u.Age},nil
}



func BatchInsertUsers3(users []*user) error {
	_, err := db.NamedExec("INSERT INTO user (name, age) VALUES (:name, :age)", users)
	return err
}
```



### 5. 根据给定的id 查询数据

```go
//根据给定的ID查询

func QueryByIDs(ids []int)(users []user,err error ) {
//	动态填充id
	query,args,err :=sqlx.In("SELECT name,age FROM user WHERE id IN(?)",ids)
	if err != nil {
		return
	}
	// sqlx.In 返回带 `?` bindvar的查询语句, 我们使用Rebind()重新绑定它
	query = db.Rebind(query)

	err = db.Select(&users, query, args...)
	return
}

func main() {

	err := initDB()
	if err != nil {
		fmt.Printf("初始化连接失败",err)
		return
	}

	defer db.Close()

	users, err := QueryByIDs([]int{3, 4, 5})
	if err != nil {
		fmt.Printf("Query By ID failed,err:%v\n",err)
		return
	}
	for _,user := range users{
		fmt.Printf("user:%#v\n",user)
	}
}
```

