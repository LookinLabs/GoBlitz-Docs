---
title: "Quick Start"
sidebar_label: "Quick Start"
---

Right now GoBlitz doesn't support any native installation methods and is set as a template repository. You can fork the repository and start building your application. When new release comes up, you can pull the changes from the main repository.

**Step 1:** Fork the [GoBlitz](https://github.com/KostLinux/GoBlitz) Repository

**Step 2:** Clone the Repository

```bash
git clone git://github.com/<your-username>/GoBlitz.git
```

**Step 3:** Install the Dependencies

```bash
go mod tidy && go mod vendor
```

**Step 4:** Configure environment variables
    
```bash
cp .env.example .env
```

**Step 5:** Run the Application

```bash
go run main.go
```