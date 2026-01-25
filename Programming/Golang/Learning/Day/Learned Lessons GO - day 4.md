When accessing data in a SQL database you have to open a connection -> ask for data -> transform to go -> close connection.

sql.Rows (SELECT statement that returns multiple rows) has some methods on it that helps.

**Next()** method prepares the next result row for reading with the scan method
**Err()** returns the error in iteration
**Scan()** copies the columns in the current row into values pointed to by the dest. must be same amount of values and columns)
**Close()** closes the [Rows](https://pkg.go.dev/database/sql@go1.25.6#Rows), preventing further enumeration.