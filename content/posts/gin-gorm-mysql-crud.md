---
categories:
  - Golang
tags:
  - Golang
  - Go
  - Gin
  - MySQL
  - GORM
title: "RESTful C.R.U.D.S. API by Gin + GORM + MySQL"
date: 2023-04-26T19:17:45+07:00
draft: false
---

File `main.go`

```go
package main

import (
	"github.com/gin-gonic/gin"
	"gorm.io/driver/mysql"
	"gorm.io/gorm"
	"log"
	"net/http"
	"strings"
	"time"
)

type ToDoItem struct {
	Id        int        `json:"id" gorm:"column:id;"`
	Title     string     `json:"title" gorm:"column:title;"`
	Status    string     `json:"status" gorm:"column:status"`
	CreatedAt *time.Time `json:"created_at" gorm:"column:created_at"`
	UpdatedAt *time.Time `json:"updated_at" gorm:"column:update_at"`
}

func main() {
	dsn := "root:example@tcp(127.0.0.1:3306)/vy_gorm?charset=utf8mb4&parseTime=True&loc=Local"
	db, err := gorm.Open(mysql.Open(dsn), &gorm.Config{})
	if err != nil {
		log.Fatalln("Cannot connect to MySQL:", err)
	}
	log.Println("Connected:", db)

	db.AutoMigrate(&ToDoItem{})

	router := gin.Default()
	v1 := router.Group("/v1")
	{
		v1.POST("/items", createItem(db))
	}
	router.Run(":8088")
}

func createItem(db *gorm.DB) gin.HandlerFunc {
	return func(context *gin.Context) {
		var dataItem ToDoItem
		if err := context.ShouldBind(&dataItem); err != nil {
			context.JSON(http.StatusBadRequest, gin.H{"error": err.Error()})
			dataItem.Title = strings.TrimSpace(dataItem.Title)

		}
		if dataItem.Title == "" {
			context.JSON(http.StatusBadRequest, gin.H{"error": "title don't leave blank."})
			return
		}
		dataItem.Status = "Doing"
		if e := db.Create(&dataItem).Error; e != nil {
			context.JSON(http.StatusBadRequest, gin.H{"foo": "bar"})
			return
		}
		context.JSON(http.StatusOK, gin.H{"inserted_record": dataItem.Id})
	}

}
```
![gorm_gin_mysql.png](/img/gin_gorm_mysql/gorm_gin_mysql.png)

[//]: # (// select * from to_do_items;)
[//]: # (// https://200lab.io/blog/lap-trinh-rest-api-todo-list-voi-golang/)
