---
title: "Architecture"
sidebar_label: "Architecture"
---

GoBlitz is a web framework built in layered + MVC Mesh architecture. GoBlitz main layer is middleware, which acts like a HTTP Router with necessary security checks for the HTTP Protocol. Customer request comes into middleware, where middleware constructs the response depending on the controller (C) and views (V) component. 

Controller itself communicates with the model (M) component and repository layer. Model is responsible for the datastructure management while the repository layer is responsible for communication and queries between external services (like PSQL or Redis).

You can take a look for the [GET Users Query](examples/get_users_query) which explains the works between layers and components.

<img src="/img/image.png" alt="Architectural overview" width="1000" height="400"></img>

## Features

GoBlitz comes with multiple built-in features like:

- **Air** - The one and only hot reload library for Go
- **Goose** - The most fastest database migration tools in Go
- **Comprehensive Quality Scan** - GoBlitz comes with a comprehensive quality scan for the codebase
- **Security** - GoBlitz comes with a set of security features to protect web applications from common web vulnerabilities
- **Templating** - GoBlitz has multiple templating engines like Go & HTML Templates.
- **First API Routes** - GoBlitz has built-in API routes like `/api/v1/ping`, `/api/v1/users` and `/status`.
- **Compatibility** - GoBlitz Web Framework supports other frontend frameworks like React, Angular, Vue, etc.

## Summary

The GoBlitz web framework is built in a layered + MVC Mesh architecture. It has 2 layers and all components of MVC.

- **Middlewar Layer**: Main layer of the framework, responsible for HTTP Routing and Security checks.
- **Repository Layer**: Responsible for communication and queries between external services.
- **Controller Component**: Responsible for the business logic and communication with the model and repository layer.
- **Model Component**: Responsible for the datastructure management.
- **Views Component**: Responsible for the browser view.
