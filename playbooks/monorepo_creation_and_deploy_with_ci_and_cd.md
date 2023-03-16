# Monorepo Creation and Deploy with CI & CD

## Tags

## Why

## How

## What

### Installing T3 Stack

- Open up terminal
  - `npm create t3-app@latest app-name`
    - Write your app name at `app-name`
  - Pick your options, I'd personally would go with:
    - TypeScript
    - All (nextAuth, prisma, tailwind, trpc)
  - Also, create a new repo from this
  - Run `npm install`
  - Run `npm run dev`
    - You should see the default landing page for the T3 Stack
  - Commit initial files
    - `git add --all`
    - `git commit -m 'initial commit'`
Navigate to `http://localhost:3000` where you should see the landing page.

## Setting up the Database

### Deploying on Railway

- Go to railway.app
  - Login
  - Start a new project: <https://railway.app/new>
    - Go through everything
  - Deploy now

### Populate db credentials

Still on railway

- select database, connect.
- copy `Postgres Connection URL`
- Open up your `.env`
- Edit `DATABASE_URL` to be equal to the `Postgres Connection URL` you just copied
- It should look like this: `DATABASE_URL=postgresql://postgres:{password}@containers-blah-69.railway.app:7594/railway`

Go to `schema.prisma`
Paste the below lines:

```javascript
generator client {
 provider = "prisma-client-js"
}

datasource db {
 provider = "postgresql"
 url      = env("DATABASE_URL")
}
```

### Create database tables/schema/client

On Terminal:

```shell
npx prisma migrate dev --name init
```

Back to Railway:

- Verify if the tables have been generated

Terminal:

```shell
npx prisma generate  
  
npx prisma studio
```

Open up Prisma Studio at <http://localhost:5555> and will work as the database's interface

## Populate data and test app

Still on Prisma Studio @ <http://localhost:5555>

- open `Example` table
- `add record`
- `save 1 record`

That should automatically generate a new row.

On Railway:

- Validate that the `example` table has a new record.

On the terminal:

- Run `npm run dev`
- On the browser get to `http://localhost:3000/api/examples`
- check the API is running, should just return same record in JSON, like:

```json
[{"id":"cl5yxzjn70013ogxkndjskxr8"}]
```

## Deploy

Terminal:

```shell
npm add -D vercel
```

- git push changes
- open [Vercel dashboard](https://vercel.com/dashboard)
- click `+ New Project`
- Import Git Repository
- select your repo
- Configure Project
- Add environment variables - these need to be populated in a standard install:

| Name | Value |
| --- | --- |
| `DATABASE_URL` | `postgresql://postgres:{password}@containers-blah-69.railway.app:7594/railway` |
| `NEXTAUTH_SECRET` | `asdf` |
| `NEXTAUTH_URL` | `http://asdf:3000` |
| `DISCORD_CLIENT_ID` | `asdf` |
| `DISCORD_CLIENT_SECRET` | `asdf` |

Click `Deploy`.

Takes 1-2 mins.

Should now be deployed at `https://your-project-name.vercel.app`.
