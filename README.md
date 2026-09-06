# Home Expense Notebook - Angular 22 + JSON Server

This version keeps the existing UI and month-wise behavior, but moves the mock data from browser memory to a JSON Server REST API backed by `db.json`.

## Local setup

### Terminal 1 - Mock API

```bash
npm install
npm run mock-api
```

The mock API runs at:

`http://localhost:3000`

Useful endpoints:

- `GET /majorCategories`
- `GET /minorCategories`
- `GET /expenses`
- `POST /majorCategories`
- `POST /minorCategories`
- `POST /expenses`

### Terminal 2 - Angular

```bash
npm start
```

Open `http://localhost:4200`.

## Persistence

New categories, minor categories, and expenses are written through JSON Server to `db.json`. Refreshing the Angular browser page therefore reloads the data from the API instead of resetting the in-memory state.

## Important: deployment

The Angular app and JSON Server are separate deployable pieces:

```text
Angular on Vercel
      |
      | HTTP
      v
JSON Server on Render
      |
      v
db.json
```

### Deploy JSON Server to Render

This repo already contains `render.yaml` and a `mock-server.cjs` script.

1. Push this project to GitHub.
2. In Render, create a **Web Service** from the GitHub repository.
3. Render can use the included `render.yaml`, or set:
   - Runtime: Node
   - Build Command: `npm install`
   - Start Command: `npm run mock-api:prod`
   - Plan: Free
4. Render will give you a URL similar to:
   `https://home-expense-mock-api.onrender.com`

The server binds to `0.0.0.0` and uses Render's `PORT` environment variable.

### Connect the Angular app to Render

Before building Angular for Vercel, edit:

`src/app/core/api-config.ts`

Change:

```ts
export const API_BASE_URL = 'http://localhost:3000';
```

to your Render URL, for example:

```ts
export const API_BASE_URL = 'https://home-expense-mock-api.onrender.com';
```

Then build/deploy Angular:

```bash
npm run build
```

## Important free-hosting limitation

JSON Server writes to the service filesystem. Render's free service filesystem is not a durable database, so data can be lost when the service is restarted/redeployed. This setup is intended as a learning/demo mock API. For permanent production data, replace JSON Server with the existing Spring Boot APIs/MySQL later.

## Angular learning path

The app now gives a realistic path for learning:

`Component -> Template -> Forms -> Service -> Dependency Injection -> HttpClient -> Observable -> RxJS -> API -> Spring Boot`
