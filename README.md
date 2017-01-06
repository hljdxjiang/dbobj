# dbobj

[golang数据库接口](https://github.com/hzwy23/dbobj)
### 这个包,封装了golang与数据库之间的接口,目前支持oracle,mysql数据库

### 依赖包:
   [oracle依赖mattn](https://github.com/mattn/go-oci8)
   [mysql依赖包](https://github.com/go-sql-driver/mysql)
   
###使用方法:
*  首先引入上边的包,oracle数据库请引入第一个包,mysql请引入第二个包.如果选择的是oracle数据库,请按照go-oci8包的要求配置pkgconfig和oracle instantclient
*  配置dbobj包需要的环境变量,请将GOSYSCONFIG目录指向system.properties所在目录.
```shell
    export GOSYSCONFIG=/opt/go/hcloud/conf
```


*  在指定的配置文件目录中创建配置文件,配置文件名称指定为:system.properties,在文件中输入下面信息:
###mysql配置文件
```
    DB.type=mysql
    DB.tns = "tcp(localhost:3306)/bigdata"
    DB.user = root
    DB.passwd= huang
```
---
###oracle配置文件
```
    DB.type=oracle
    DB.tns = "192.168.1.101:1521/orcl"
    DB.user = test
    DB.passwd= huang
```
*  系统启动后,会默认自动对密码进行加密.

###例子
```go
package main

import (
	"fmt"

	"github.com/hzwy23/dbobj"
)

func main() {

	rows, err := dbobj.Query("SELECT user_id,user_name FROM sys_user_info where user_id = ?", "admin")
	defer rows.Close()
	if err != nil {
		fmt.Println("query failed.")
		return
	}
	for rows.Next() {
		var userId string
		var userName string
		err = rows.Scan(&userId, &userName)
		if err != nil {
			fmt.Println("query failed. scan failed.")
			return
		}
		fmt.Println("user id is :", userId, "user name is :", userName)
	}
}
```
