---
title: "Defining Controller"
sidebar_label: "Defining Controller"
---

This guide shows the example of how to define a controller in GoBlitz Web Framework. It's based on `/api/v1/users` endpoint, which returns the list of users from the database.

1. Create `model/get_users.go` structure in model folder

```
package model

import (
	"gorm.io/gorm"
)

type User struct {
	gorm.Model
	Email    string `gorm:"size64, index"`
	Username string `gorm:"size64, index"`
	Password string `gorm:"size255"`
}

```

2. Create a SQL Query in `repository/db/users.go`, which makes an SQL Query against database

```
package sql

import (
	"web/model"
)

var DB *gorm.DB

func GetUsers() (model.User, error) {
	var user model.User

	transaction := DB.First(&user)
	if transaction.Error != nil {
		if errors.Is(transaction.Error, gorm.ErrRecordNotFound) {
			return model.User{}, errors.New("user not found")
		}

		return model.User{}, transaction.Error
	}

	return user, nil
}

```

3. Create API Controller in `controller/api/users.go`

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

`make migrate-up`

`make`

6. Make an request against `/api/v1/users`

```
curl http://localhost:8000/api/v1/users
[{"user_id":"1","name":"Alice"}]⏎   
```

**Note** If you will secure your API Path with Authentication middleware, it will get 401. You can check the endpoint via browser, by sign-up a user and then heading to /api/v1/users