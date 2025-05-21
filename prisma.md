# 📘 Prisma ORM 
### 📌 What is Prisma?
`Prisma` is a next-generation `TypeScript ORM` for `Node.js` and `JavaScript`. It provides a `clean`, `type-safe,` and performant way to interact with your relational databases like `PostgreSQL`, `MySQL`, `SQLite`, and `SQL Server`.
#
# ⚙️ Core Components
## Component	Description
- `schema.prisma`	Central configuration file to define data models, DB connection, and generators
- `Prisma Client`	Auto-generated and type-safe database client
- `Prisma Migrate`	Tool to manage DB schema and migrations
- `Prisma Studio`	GUI for visual database management
- `Introspection`	Generate models from existing DB
- `Seed Scripts`	For inserting initial data

```
       ┌─────────────────────────────┐
       │       Your App (Node.js)   │
       │       TypeScript/JS Code   │
       └────────────┬───────────────┘
                    │  (Import Prisma Client)
                    ▼
        ┌────────────────────────────┐
        │    Prisma Client (TS/JS)   │ ◀── Auto-generated from schema
        └────────────┬───────────────┘
                     │ (makes queries via)
                     ▼
        ┌────────────────────────────┐
        │  Prisma Query Engine (Rust)│ ◀── Binary file
        └────────────┬───────────────┘
                     │ (Executes raw SQL queries)
                     ▼
         ┌───────────────────────────┐
         │     Your Relational DB    │
         │ (Postgres, MySQL, etc.)   │
         └───────────────────────────┘

```
## 🖼️ Prisma Query Engine Flow

Below is the official diagram showing how Prisma processes a query at runtime:

![Prisma Query Engine Flow](https://www.prisma.io/docs/assets/images/typical-flow-query-engine-at-runtime-73ffdee4acc20a853bbd431dc12fb64f.png "Prisma Query Engine Runtime Flow")

## 🔢 Prisma Query Flow – Step-by-Step

1. `$connect()` is invoked on Prisma Client.
2. The Query Engine is started.
3. The Query Engine establishes connections to the database and creates a connection pool.
4. Prisma Client is now ready to send queries to the database.
5. Prisma Client sends a `findMany()` query to the Query Engine.
6. The Query Engine translates the query into SQL and sends it to the database.
7. The Query Engine receives the SQL response from the database.
8. The Query Engine returns the result as plain JavaScript objects to Prisma Client.
9. `$disconnect()` is invoked on Prisma Client.
10. The Query Engine closes the database connections.
11. The Query Engine is stopped.

#### `There are two main engines:`

| Engine	     |                 Role |
|--------------|-------------------------|
|`query-engine` |	Handles Prisma Client queries |
|`migration-engine` |	Handles schema diffing and migration logic |


# 🔄 Flow Example
-  You write this in your app:

``` ts
const user = await prisma.user.findFirst({ where: { email: 'a@a.com' } });
```
-  Prisma Client transforms it into a GraphQL-like internal query (DML/DSL format).

-  This internal query is sent to the Rust-based query engine.

-  The query engine converts that into optimized SQL:

``` sql
SELECT * FROM "User" WHERE email = 'a@a.com' LIMIT 1;
```
-  The database executes the SQL and returns the row(s).

-  The engine converts the result to JSON, sends it to Prisma Client, which returns a typed object.

# ⚠️ Prisma cli-commands
``` bash

# ✅ Initialize Prisma in your project
npx prisma init
#  Creates `schema.prisma`, `.env`, and `prisma/` folder

# ✅ Pull existing DB structure into Prisma schema
npx prisma db pull
#  Reads tables from database and generates Prisma models

# ✅ Push schema changes to your database (⚠️ destructive in production)
npx prisma db push
# └─ Updates your database without creating migration history

# ✅ Create and apply a new migration
npx prisma migrate dev --name init
#  Best for development, generates SQL and applies it to DB

# ✅ Format the schema.prisma file
npx prisma format
#  Cleans and formats your schema like Prettier

# ✅ Open Prisma Studio (GUI for your DB)
npx prisma studio
#  Opens a local GUI to explore and edit DB data

# ✅ Seed your database with test or default data
npx prisma db seed
# Executes your custom seed script (if defined)

# ✅ Generate Prisma Client based on schema
npx prisma generate
#  Generates TypeScript/JS client you can import in your app

# ✅ Reset database (dangerous for production!)
npx prisma migrate reset
# Drops all data, re-applies migrations, and runs seed script
```

# 🚀 Getting Started

### 1. Install Prisma CLI and Client

```bash
npm install prisma --save-dev
npm install @prisma/client
```

### 2. Initialize Prisma

```bash
npx prisma init
```

This creates a `prisma/` folder with:

- `schema.prisma`
- `.env` (for DATABASE_URL)

### 3. Configure `.env`

```env
DATABASE_URL="mysql://user:password@localhost:3306/mydb"
```

### 4. Generate Prisma Client

```bash
npx prisma generate
```

---

## 🛠️ Prisma Schema

### `prisma/schema.prisma`

```prisma
generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "mysql"
  url      = env("DATABASE_URL")
}

model User {
  id       Int    @id @default(autoincrement())
  name     String
  email    String @unique
  password String
}
```

## prisma.ts
```ts

// ✅ Import Prisma Client
import { PrismaClient } from '@prisma/client'

// ✅ Instantiate the client
const prisma = new PrismaClient()

// ✅ Connect manually (optional, usually not required)
await prisma.$connect()

// ✅ Disconnect manually (important in scripts)
await prisma.$disconnect()
```

## 📁 Project Structure Example 1

``` psql
my-app/
├── prisma/
│   └── schema.prisma
├── node_modules/
├── src/
│   └── index.ts
├── package.json
└── tsconfig.json
```
## 📁 Project Structure Example 2

```
📁 prisma-orm
├── 📁 src
│   ├── 📁 modules
│   │   ├── 📁 user
│   │   │   ├── user.controller.ts
│   │   │   ├── user.service.ts
│   │   │   └── user.routes.ts
│   │   └── 📁 auth
│   │       ├── auth.controller.ts
│   │       ├── auth.service.ts
│   │       └── auth.routes.ts
│   ├── prisma
│   │   ├── client.ts           // Prisma client instance
│   │   └── prisma.service.ts   // PrismaService abstraction
│   └── app.ts
├── 📁 prisma
│   └── schema.prisma
└── 📄 package.json
```


## 🛠️ Migrations & DB Sync

### Create a new migration and apply

```bash
npx prisma migrate dev --name init
```
### ⚙️ What Happens During `npx prisma migrate dev`
- Compares your current `schema.prisma` with migration history

- Generates a SQL migration file

- Applies that SQL to your database using `migration-engine`

- Updates `_prisma_migrations` table to keep track
### ✅ Apply migrations only
```
npx prisma migrate deploy
```

### ✅ Push changes directly (without migration files, useful for prototyping)
```
npx prisma db push
```
### 📥 Generate Prisma Client

```
npx prisma generate
```
### 🧪  What Happens During `npx prisma generate`
- Reads your schema.prisma

- Introspects your model definitions

- Generates TypeScript-safe Prisma Client code in `node_modules/.prisma/client`.

- This client wraps the query engine and gives you the nice DX you're familiar with

  
# 🧑‍💻 Querying the Database

### 🔍 Find Users with Posts

```ts
const users = await prisma.user.findMany({
  include: { posts: true }
});
```
### ➕ Create a User
```ts
const user = await prisma.user.create({
  data: {
    name: "Alice",
    email: "alice@example.com"
  }
});
```
### 🔄 Update a User
```ts
const updatedUser = await prisma.user.update({
  where: { id: 1 },
  data: { name: "Updated Name" }
});
```
### ❌ Delete a User
```ts
await prisma.user.delete({
  where: { id: 1 }
});
```
### 🔎 Filtering & Pagination
```ts
const users = await prisma.user.findMany({
  where: {
    email: { contains: "@example.com" }
  },
  skip: 10,
  take: 5,
  orderBy: { name: "asc" }
});
```
### 🔐 Raw SQL Queries
```ts
const result = await prisma.$queryRaw`SELECT * FROM "User" WHERE email = 'alice@example.com'`;
```
### 🔁 Transactions
```ts
const [user, post] = await prisma.$transaction([
  prisma.user.create({ data: { name: "John", email: "john@example.com" } }),
  prisma.post.create({ data: { title: "Hello", userId: 1 } })
]);
```
### 🧪 Seeding Data
```ts
// prisma/seed.ts
import { PrismaClient } from "@prisma/client";
const prisma = new PrismaClient();

async function main() {
  await prisma.user.create({
    data: {
      name: "Seed User",
      email: "seed@example.com"
    }
  });
}
main();
```

## Prisma seed
```
npx prisma db seed
```
## 📊 Prisma Studio (GUI)

```
npx prisma studio
```
Use this to explore and edit your database visually in the browser.

## Prisma example
```ts

// ✅ Simple find query
const users = await prisma.user.findMany()
//  Returns all users

// ✅ Create new record
const user = await prisma.user.create({
  data: {
    name: 'Alice',
    email: 'alice@prisma.io'
  }
})
//  Creates and returns new user

// ✅ Update a record
await prisma.user.update({
  where: { id: 1 },
  data: { email: 'new@prisma.io' }
})
//  Updates user by ID

// ✅ Delete a record
await prisma.user.delete({
  where: { id: 1 }
})
//  Deletes user by ID

// ✅ Nested writes
await prisma.user.create({
  data: {
    name: 'Bob',
    email: 'bob@prisma.io',
    posts: {
      create: [
        { title: 'First post' },
        { title: 'Second post' }
      ]
    }
  }
})
//  Creates user and multiple posts in one go


// ✅ Filtering and selecting
const activeUsers = await prisma.user.findMany({
  where: { isActive: true },
  select: { name: true, email: true }
})
// Fetches only selected fields from users


// ✅ Include related data (join-like behavior)
const userWithPosts = await prisma.user.findUnique({
  where: { id: 1 },
  include: { posts: true }
})
// Includes all posts of the user


```
## ✅ Best Practices

- Use npx prisma format to format schema.prisma

- Keep schema and migration history in version control

- Avoid direct usage of db.push in production

- Always regenerate client after schema change

- Use include and select to limit over-fetching

- Modularize your Prisma client in large projects

# 🆚 Prisma vs Sequelize 

| Feature	             |  Prisma	                      | Sequelize
|----------------------|---------------------------------|----------------------|
| Language Support     |	TypeScript-first              |	JS-first, TS support  |
| Type Safety	         |   ✅ Strong  	                | ❌ Weak              |
| Migrations	         | Declarative, built-in          |	Imperative, verbose   |
| Querying	           |  Fluent, type-safe	Chainable,  | less safe             |
| Developer Experience |	⭐⭐⭐⭐⭐                  |	⭐⭐⭐               |
| Visual Studio	       |  Prisma Studio                 |	❌ None               |
| Raw SQL              |	✅ Supported	                |✅ Supported           |
| Performance⚡        |  Fast for reads	              | Moderate               |

# 📘 Resources
- Official Docs: https://www.prisma.io/docs

- GitHub: https://github.com/prisma/prisma

- Prisma Examples: https://github.com/prisma/prisma-examples

# "Why Prisma over Sequelize?"

`Prisma offers a better developer experience, strong type safety, powerful query engine, and seamless integration with TypeScript — making it ideal for modern Node.js applications.`

## ⚖️ When to use Prisma vs Sequelize?

| Use Case                                     | Recommended ORM |
|---------------------------------------------|-----------------|
| TypeScript-first projects                    | ✅ Prisma        |
| Fast MVP with raw flexibility                | ✅ Sequelize     |
| Advanced relational queries with safety      | ✅ Prisma        |
| Need for dynamic model creation at runtime   | ✅ Sequelize     |
| Legacy systems or older databases            | ✅ Sequelize     |


# 💡 Why Prisma is So Fast and Safe

| Feature         | Why It’s Fast                                                                 |
|----------------|--------------------------------------------------------------------------------|
| Query Engine    | Written in Rust (compiled, low-level, super fast)                             |
| Typed Client    | No runtime type checking needed                                               |
| Lean SQL        | Only selects fields you request (`select`, `include`)                         |
| No ORM Overhead | No heavy model instantiation like Sequelize                                   |

---

# 🏁 Summary

| Layer           | Role                                                        |
|----------------|-------------------------------------------------------------|
| `Prisma Client`   | Type-safe JS/TS code                                        |
| `Query Engine `   | Rust binary that talks to DB                                |
| `Database `       | Executes final SQL queries                                  |
| `schema.prisma`   | Central schema definition for models and config             |
| `migrate`, `studio `| CLI tools that support the whole lifecycle                  |
