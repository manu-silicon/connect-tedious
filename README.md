connect-tedious [![Build Status](https://secure.travis-ci.org/mcartoixa/connect-tedious.png)](https://travis-ci.org/mcartoixa/connect-tedious)
===============

[connect](https://github.com/senchalabs/connect) session store for SQL Server, using [tedious](http://github.com/pekim/tedious).

## Usage
### Sample
The simplest sample requires a SQL Server 2008+ database with a table created as follows
```javascript
CREATE TABLE [dbo].[Sessions](
  [Sid] varchar(24) NOT NULL
    CONSTRAINT [PK_Sessions] PRIMARY KEY CLUSTERED ([Sid] ASC),
  [Expires] datetimeoffset NOT NULL,
  [Sess] nvarchar(MAX) NULL
)
```

The session store can then be created
```javascript
var express = require('express');
var TediousStore = require('connect-tedious')(express);

var app = express.createServer()
    .use(express.cookieParser())
    .use(express.session({
        secret: 'mysecret',
        store: new TediousStore({
            config: {
                userName: 'mydbuser',
                password: 'mydbpassword',
                server: 'localhost',
                options: {
                  instanceName: 'SQLEXPRESS',
                  database: 'mydatabase'
                }
            }
        })
    )
);
```

### Syntax
* Class `TediousStore`
  * `new TediousStore(options)`
    * `options`: Object
      * `config': Object The same configuration that would be used to [create a tedious Connection](http://pekim.github.com/tedious/api-connection.html#function_newConnection).
      * `tableName`: String The table name. Defaults to `[dbo].[Sessions]`.
      * `sidColumnName`: String The session Id column name. Defaults to `[Sid]`.
      * `sessColumnName`: String The session content column name. Defaults to `[Sess]`.
      * `expiresColumnName`: String The session expiration column name. Defaults to `[Expires]`.
      * `minConnections`: Number The minimum number of connections to keep in the pool. Defaults to `0`.
      * `maxConnections`: Number The maximum number of connections to keep in the pool. Defaults to `100`.
      * `idleTimeout`: Number The Number of milliseconds before closing an unused connection. Defaults to `30000`.

## License

View the [LICENSE](https://github.com/mcartoixa/connect-tedious/blob/master/LICENSE) file