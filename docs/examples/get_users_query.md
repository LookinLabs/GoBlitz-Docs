---
title: "GET Users Query"
sidebar_label: "Get Users Query"
---

The client calls up `https://APP_HOST/api/v1/users` which triggers the GET API request against Middleware, which sends the request into controller.

Meanwhile the repository layer uses User model (Data structure) to set up the SQL Query with necessary parameters to return the value into controller, when it will be triggered.

Controller calls up `sql.GetUsers()`, which contains SELECT query against users table. The `sql.GetUsers()` is alias of `repository.GetUsers()`. The controller gets the response and sends the response to the user.

The example can be seen here ->

- [Controller](https://github.com/LookinLabs/GoBlitz/blob/master/controller/api/get_users.go)
- [Model](https://github.com/LookinLabs/GoBlitz/blob/master/model/user.go)
- [Repository](https://github.com/LookinLabs/GoBlitz/blob/master/repository/db/get_users.go)
- [Middleware](https://github.com/LookinLabs/GoBlitz/blob/da8cb4993e412d326518beeb20b7d0fc4315625c/middleware/router.go#L56)