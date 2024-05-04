---
title: "Folder Structure"
sidebar_label: "Folder Structure"
---

GoBlitz Web Framework has the following folder structure:

- `bin` - Binary folder mostly used for different scripts for contribution
- `controller` - The Controller component of MVC architecture. Responsible of receiving the request and sending the response while communicating with the model and repository layer
- `middleware` - The Middleware layer of the framework. A HTTP Router which routes the requests to the controller and returns the respones. It also has necessary security checks for the HTTP Protocol
- `migrations` - The Migrations folder for the Goose (SQL) migrations
- `model` - The Model component of MVC architecture. Responsible for the datastructure management
- `public` - The Public folder for the static assets like CSS, JS, and images
- `repository` - The Repository layer of the framework. Responsible for communication and queries between external services.
- `templates` - The GO Templates folder mostly used as configuration templates in controller
- `tests` - The Tests folder for the unit tests
- `views` - The Views component of MVC architecture. Responsible for the browser view
- `views/error` - The Error Views folder for the error pages
- `views/components` - The Components folder for the reusable components in the views
- `views/templates` - The Templates folder for generating the HTML template values
- `views/_page` - Website shown from Go application
