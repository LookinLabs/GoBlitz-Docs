---
title: "GoBlitz Native"
sidebar_label: "GoBlitz Native"
---

GoBlitz Native is an frontend framework used to develop frontend via Go and HTML Templates. The framework also supports **global assets** and **assets per page**. GoBlitz Native is meant to be used with static JS and CSS files.

## Folder Structure

GoBlitz Native main folders are `views` and `middleware` and `helpers`. Middleware is a little component which serves HTML Templates and calls helpers for different HTTP processes (like serving assets based on user's frontend choice).

The folder is `views` which is used for the frontend development. It contains multiple folders:

- `assets` - Contains the global assets which can be used in all pages
- `components` - Contains the components which can be used in the pages
- `error` - Contains the client side error pages, which can be served via `controller/error/client_http.go` and `middleware/router.go`
- `*_page` - Contains the pages which are served via middleware
- `templates` - Contains the template values which can be used in the pages

## Developing with GoBlitz Native

### Assets

As mentioned before, GoBlitz Native is a simple frontend framework used to develop browser views, which supports assets per page and global assets. Each page should've been created as a folder with the `_page` suffix. 

The HTTP Helper (`helpers/http.go`) which is used in Middleware, looks for the all pages with the `_page` suffix and creates a route for the assets for each page. So for example if we have a page `welcome_page`, which's located in `views/welcome_page` and we have some css file stored in `views/welcome_page/assets/main.css`, then the route for the css in href parameter is `/welcome_page/assets/main.css`.

Due to different purposes of using HTML Templated sites, the automatic routes for the pages themselves are not created. You have to setup a site in `sites.go` middleware and then create a route for the page in `router.go`. The route will call the site and serve the page.

> **Note** Be careful of using assets per page, cause they might override the global assets.

### Components

GoBlitz Native supports components similar to React. You can define the components in `views/components` folder and then use them in the pages. By default the components use global assets.

### Custom Client Side Errors

The Custom Client Side Errors are handled by the `controller/error/client_http.go` and `middleware/router.go`. The error pages are stored in `views/error` folder and can be served via the middleware. Right now GoBlitz doesn't support Server Side Custom Errors (starting from 500, cause Gin doesn't support them).

## HTML Templates

As mentioned before, GoBlitz supports HTML Templating. You can define the template values in `views/templates` folder and use them in `views/pagename_page/index.html` file.

Example of using [HTML Templating in Status Page](https://github.com/KostLinux/GoBlitz/blob/master/views/templates/statuspage.go).

## Setting up a Status Page (example)

1. Set up and Middleware `sites.go` which checks for the template

```
// middleware/sites.go


// StatusPageMiddleware handles the status page
func StatusPageMiddleware() gin.HandlerFunc {
	return func(c *gin.Context) {
		statuses := c.MustGet("statuses").([]map[string]string)
		c.HTML(http.StatusOK, "status.html", gin.H{
			"services": statuses,
		})
	}
}
```
2. Setup an route for the status_page

```
// middleware/router.go

router.GET("/status", httpTemplates.StatusPageResponse(), StatusPageMiddleware())
```

3. Configure HTML Template values generator

```
// views/templates/statuspage.go

package views

import (
	"log"
	"net/http"
	"os"
	model "web/model"

	"github.com/gin-gonic/gin"
)

func CheckServicesStatus(services []model.StatusPage) []map[string]string {
	serviceStatuses := make([]map[string]string, 0)
	for _, service := range services {
		status := servicesHealthHandler(service)
		serviceStatuses = append(serviceStatuses, status)
	}

	return serviceStatuses
}

func StatusPageResponse() gin.HandlerFunc {
	urlPrefix := "http://"
	if os.Getenv("FORCE_TLS") == "true" {
		urlPrefix = "https://"
	}

	return func(c *gin.Context) {
		services := []model.StatusPage{
			{
				Name: "API",
				URL:  urlPrefix + os.Getenv("APP_HOST") + ":" + os.Getenv("APP_PORT") + os.Getenv("API_PATH") + "ping",
			},
			{
				Name: "Users API",
				URL:  urlPrefix + os.Getenv("APP_HOST") + ":" + os.Getenv("APP_PORT") + os.Getenv("API_PATH") + "users",
			},
		}

		statuses := CheckServicesStatus(services)

		c.Set("statuses", statuses)
		c.Next()
	}
}

func servicesHealthHandler(serviceInfo model.StatusPage) map[string]string {
	resp, err := http.Get(serviceInfo.URL)

	service := map[string]string{
		"name":   serviceInfo.Name,
		"status": "up",
	}

	if err != nil {
		log.Println("Error checking service status:", err)
	} else {
		defer resp.Body.Close()
		if resp.StatusCode != http.StatusOK {
			service["status"] = "down"
		}
	}

	return service
}
```

4. Setup an status_page

```
// views/status_page/status.html

<!DOCTYPE html>
<html>
<head>
    <title>Status Page</title>
    <link rel="stylesheet" href="/assets/main.css">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.2/css/all.min.css">
    <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-100 flex flex-col min-h-screen justify-between">
    {{template "header.html"}}
    <main class="px-4 py-8">
        <h1 class="text-2xl font-bold mb-4 text-center">Status Page</h1>
        {{range .services}}
        <div class="service bg-white shadow rounded-lg p-4 mb-4 flex justify-between items-center w-full">
            <h2 class="text-xl font-bold mb-2">{{.name}}</h2>
            {{if eq .status "up"}}
            <div class="status text-green-500 mb-2">
                <span>&#10003;</span>
            </div>
            {{else}}
            <div class="status text-red-500">
                <span>&#10007;</span>
            </div>
            {{end}}
        </div>
        {{end}}
    </main>
    {{template "footer.html"}}
</body>
</html>
```

Navigate into "APP_HOST:APP_PORT/status" for the results.

In development case it can be `http://localhost:8000/status".

**Note** The header and footer HTML files are defined in the `views/components` folder.