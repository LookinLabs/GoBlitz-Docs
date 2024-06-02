---
title: "Defining Controller"
sidebar_label: "Defining Controller"
---

This guide shows the example of how to define a controller in GoBlitz Web Framework. It's based on `/api/v1/users` endpoint, which returns the list of users from the database.

1. Create `model/get_users.go` structure in model folder

```
package model

type User struct {
	UserID string `json:"user_id"`
	Name   string `json:"name"`
}
```

2. Create a SQL Query in `repository/db/get_users.go`, which makes an SQL Query against database

```
package sql

import (
	"web/model"
)

func GetUsers() ([]model.User, error) {
	var users []model.User
	result := DB.Find(&users)
	if result.Error != nil {
		return nil, result.Error
	}
	return users, nil
}

```

3. Create API Controller in `controller/api/get_users.go`

```
package api

import (
	"net/http"

	sql "web/repository/db"

	"github.com/gin-gonic/gin"
)

func GetUsers(c *gin.Context) {
	users, err := sql.GetUsers()
	if err != nil {
		c.JSON(http.StatusInternalServerError, gin.H{"error": err.Error()})
		return
	}

	c.JSON(http.StatusOK, users)
}


```

4. Add into middleware your API Path

```
	// API Handling
	apiGroup := router.Group(os.Getenv("API_PATH"))
	{
		apiGroup.GET("/users", api.GetUsers)
	}
```

5. Setup environment and run migrations

**Note!** Check that you have PostgreSQL enabled and running

`make`

`make migrate-up`

6. Make an request against `/api/v1/users`

```
curl http://localhost:8000/api/v1/users
[{"user_id":"1","name":"Alice"}]⏎   
```