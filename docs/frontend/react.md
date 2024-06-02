---
title: "Frameworks"
sidebar_label: "Frameworks"
---

In case of using React, Vue or any other framework, it's recommended to run [bin/prepare_other_frontend](https://github.com/KostLinux/GoBlitz/blob/master/bin/prepare_other_frontend.sh) script.

It automatically clean ups the views (welcome page only) and sets up basic index.html into public folder. It also creates `frontend` which should be used for the React, Vue or any other frontend framework.

In case of using other frameworks, the assets are served under /assets path in `public/assets` folder. So if you want to use some CSS file, you should put it in `public/assets/css` folder and then link it in the index.html file.

> [!WARNING]
> When running `yarn build` or `npm run build` - you must move all the static files to the `public` folder.

## Getting started

The GoBlitz Web Framework and frontend framework are separated. Each of them are running on different ports.

GoBlitz recommends to use a single .env file for both frameworks under GoBlitz root folder.

GoBlitz support Docker Compose and Native development.

### Docker Compose (Hot Reload Not Enabled in Go)

1. Configure .env

```
cp .env.example .env
```

2. Run docker compose

```
docker-compose up
```

3. Navigate to `http://localhost:8000` for the Go App

4. Navigate to `http://localhost:5173` for React App

### Native (Hot Reload enabled in Go & React)

#### Setup GoBlitz as Backend framework

**Step 1:** Clone the Repository

```bash
git clone git://github.com/LookinLabs/GoBlitz.git
```

**Step 2:** Install the Dependencies

```bash
go mod tidy && go mod vendor
```

**Step 3:** Configure environment variables
    
```bash
cp .env.example .env
```

**Step 4:** Setup the database and run migrations

```bash
docker-compose up -d db
make migrate-up
```

**Step 5:** Run the application

```bash
make run
```

By default the application runs on port 8000.

#### Setup React as Frontend framework

**Step 1:** Setting up React + Typescript framework via Vite

```
➤ npm create vite@latest
✔ Project name: … frontend
✔ Select a framework: › React
✔ Select a variant: › TypeScript
```

**Step 2:** Run frontend app

```
cd frontend
npm install
npm run dev
```

**Step 3** Navigate to `localhost:8000` for GoBlitz app

**Step 4** Navigate to `localhost:5173` for React App

## Setting up production-ready environment locally

**Step 1:** Setting up React + Typescript framework via Vite

```
➤ npm create vite@latest
✔ Project name: … frontend
✔ Select a framework: › React
✔ Select a variant: › TypeScript
```

**Step 2:** Making some changes in frontend app

Write some code in frontend folder

**Step 3:** Building up application

```
cd frontend
npm install & npm run build
cp -rf dist/* ../public/
```

**Step 4:** Run `make` to serve static files

```
make
```

**Step 5:** Navigate to `localhost:8000` to check that everything's fine

**Step 6:** Build your image from dockerfile and push to the remote