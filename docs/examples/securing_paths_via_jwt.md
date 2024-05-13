## Securing Paths via JWT

GoBlitz provides a simple way to protect API Paths and Web Routes using the built-in authentication mechanisms. The framework provides authentication mechanism based on JWT + AWS Cognito.

GoBlitz framework provides helper functions, which also contain `JWTInjector` function to inject the Header with JWT Token value to the request. 

Example:

```go
	// API Handling
	apiGroup := router.Group(os.Getenv("API_PATH"))
    apiGroup.Use(helpers.JWTInjector()) // Fetch & Inject JWT Token into Header
    apiGroup.Use(CognitoAuth())         // Validate the JWT Token with Cognito

	{
		apiGroup.GET("/ping", api.StatusOkPingResponse)
		apiGroup.GET("/users", api.GetUsers)
	}
```

**Note** Before setting up the example, please see the [Getting Started Cognito page](/authentication/aws/cognito) to setup the JWT Authentication with Cognito.