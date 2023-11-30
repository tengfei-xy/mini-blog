# MySQL

-   初始化
    ```go
    import(
      "database/sql"
      _ "github.com/go-sql-driver/mysql"
    )

    ```
    ```go
    var DB * sql.DB
      DB, err = sql.Open("mysql", fmt.Sprintf("%s:%s@tcp(%s:%s)/%s", I.Mysql.User, I.Mysql.Password, I.Mysql.Ip, I.Mysql.Port, I.Mysql.Database))
      if err != nil {
        logrus.Error("open database fail")
        return
      }
      defer DB.Close()
      DB.SetConnMaxLifetime(100)
      DB.SetMaxIdleConns(10)
      if err := DB.Ping(); err != nil {
        logrus.Error("ping database fail")
        return
      }
    ```
-   查询单行
    ```go
    DB.QueryRow(`select shorturl,query_count from url where longurl=?`, longurl).Scan(&shorturl, &query_count)
    ```
-   插入
    ```go
    DB.Exec(`INSERT INTO url(longurl,shorturl) values(?,?)`, longurl, shorturl);
    ```
