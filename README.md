# gorm-sqlite-cipher

[![GoDoc Reference](https://godoc.org/github.com/CovenantSQL/go-sqlite3-encrypt?status.svg)](https://pkg.go.dev/github.com/jackfr0st13/gorm-sqlite-cipher)
[![Go Report Card](https://goreportcard.com/badge/github.com/CovenantSQL/go-sqlite3-encrypt)](https://goreportcard.com/report/github.com/jackfr0st13/gorm-sqlite-cipher)

### Description

Go sqlite3 driver for [GORM](https://gorm.io/) with an AES-256 encrypted sqlite3 database
conforming to the built-in database/sql interface. It is based on:

- Go sqlite3 driver: https://github.com/mattn/go-sqlite3
- SQLite extension with AES-256 codec: https://github.com/sqlcipher/sqlcipher
- AES-256 implementation from: https://github.com/libtom/libtomcrypt
- GORM sqlite driver : https://github.com/go-gorm/sqlite

SQLite itself is part of SQLCipher.

### Installation

This package can be installed with the go get command:

    go get github.com/ShaoQ1ang/gorm-sqlite-cipher

## USAGE

```go
package main

import (
	"fmt"
	sqliteEncrypt "github.com/ShaoQ1ang/gorm-sqlite-cipher"
	"gorm.io/gorm"
)

func main() {
	key := "3nJ0y7sT3mP0r4ryK3yF0rT3st1nGPurp0s3s"
	dbname := "test.db"
	dbnameWithDSN := dbname + fmt.Sprintf("?_pragma_key=%s&_pragma_cipher_page_size=4096", key)
	db, err := gorm.Open(sqliteEncrypt.Open(dbnameWithDSN), &gorm.Config{})
	if err != nil {
		panic(err)
	}

	type User struct {
		ID   uint
		Name string
	}

	db.AutoMigrate(&User{})
	db.Create(&User{Name: "Alice"})

	var user User
	db.First(&user)
	println("查询结果:", user.Name)
}

```

### License

The code of the originating packages is covered by their respective licenses.
See [LICENSE](LICENSE) file for details.
