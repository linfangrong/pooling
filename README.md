# pooling
golang pooling


## Example ##
    var dbPool pooling.Pool = pooling.NewPool(
      maxIdle,
      func() (conn pooling.Conn, err error) {
        var db *sql.DB
        if db, err = sql.Open(); err != nil { return }
        return pooling.NewConn(db, db.Close), nil
      },
      func(c pooling.Conn, t time.Time) error {
        return c.Do(func(idle interface{}) error {
          return idle.(*sql.DB).Ping()
        })
      },
    )
    
    var (
      dbConn pooling.Conn
      err error
    )
    if dbConn, err = dbPool.Get(); err != nil {
      panic(err)
    }
    defer dbConn.Recycle()
    
    dbConn.Do(func(idle interfce{}) (err error) {
    // for some logic
      idle.(*sql.DB)
      ...
    })

