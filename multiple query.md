# multiple query
- 트랜잭션과 rollback을 고려하여 multiple query를 처리할 수 있는 node함수 생성함

```js
function multipleQuery(queries) {
  return new Promise((resolve, reject) => {
    const pool = new sql.ConnectionPool(dbConfig);

    return pool.connect().then((p) => {
        const transaction = new sql.Transaction(p);
        return transaction.begin((err) => {
          const request = new sql.Request(transaction);
            if (err) {
              reject(err);
            }
        return async.eachSeries(queries, async (query, callback) => {
          return request.query(query);
        }, async (err2) => {
          if ( err2 ) {
            await transaction.rollback(() => {
              pool.close();
              reject(err2);
            });
          } else {
            await transaction.commit(() => {
              pool.close();
              resolve(true);
            });
          }
        });
      });
    });
  });
}
```
