---
title: "How it works?"
sidebar_label: "How it works?"
---

GoBlitz uses default session-based authentication written in Go. Controller uses models to interact with the database. Middleware `Authentication` function validates the session and checks if the user is authorized or not.

The `Authentication` function is also used to make an apiKey based authentication for the Status Page.

All the authentication methods are POST, which can be seen in middleware router file.

## Sign Up

On the sign up the controller renders data from `loginFormData` struct and compares with the data in database. 

The controller uses model to check the user existence by getting a boolean value. If the user exists, the controller returns an error message. If the user doesn't exist, the controller creates a record in database about the user and sets the session.

## Sign In

On the sign in the controller renders data from `loginFormData` struct and compares with the data in database. 

The controller uses model to check for the user existance and compares the password input with the encrypted record in database.

If the user exists and the password is correct, the controller sets the session. If the user doesn't exist or the password is incorrect, the controller returns an error message.

## Sign Out

Sign Out controller just cleans up the session and removes it.

## Related files

The related files are files, where you can read the code, how does it work.

- `middleware/auth.go`
- `controller/auth.go`
- `helpers/sessions.go`
- `helpers/users.go`
- `model/user.go`
- `repository/db_connection.go`