# Render setup

Create a Render Web Service from this GitHub repository.

Build Command:
`npm install`

Start Command:
`npm run mock-api:prod`

The server listens on `0.0.0.0` and automatically uses Render's `PORT` environment variable.

Local:
`npm install`
`npm run mock-api`

Then run Angular separately with `npm start`.


### Why `public/` exists
JSON Server 1.0 beta expects a `public` directory for static assets. Keep the directory in GitHub (the `.gitkeep` file ensures it is committed).
