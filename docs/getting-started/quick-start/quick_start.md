---
title: "Quick Start"
sidebar_label: "Quick Start"
---

Right now GoBlitz doesn't support any native installation methods and is set as a template repository. You can fork the repository and start building your application. When new release comes up, you can pull the changes from the main repository.

**Step 1:** Fork the [GoBlitz](https://github.com/KostLinux/GoBlitz) Repository

**Step 2:** Clone the Repository

```bash
git clone git://github.com/LookinLabs/GoBlitz.git
```

**Step 3:** Install the Dependencies

```bash
go mod tidy && go mod vendor
```

**Step 4:** Configure environment variables
    
```bash
cp .env.example .env
```

**Step 5:** Setup the database and run migrations

```bash
docker-compose up -d db
make migrate-up
```

**Step 6:** Setup the API Key for the STATUSPAGE_API_KEY variable

```
python3 bin/api/generate_api_key.py

// Open .env
STATUSPAGE_API_KEY = "generated_key"
```

**Step 7:** Run the application

```bash
make run
```

**Step 8:** Visit the application in your browser

Feel free to visit the application at `localhost:8000` and move around available paths.