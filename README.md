# Prisma

Steps to connect **Next.js** and **Prisma**

---

## 1. Install Dependencies

npm install prisma @types/node @types/better-sqlite3 --save-dev  
npm install @prisma/client @prisma/adapter-better-sqlite3

> **Note:** We changed this instead of `@prisma/adapter-pg dotenv pg` because we are using SQLite.

---

## 2. Initialize Prisma

npx prisma init --datasource-provider sqlite --output ../generated/prisma

> This allows you to start writing your models.

---

## 3. Create and Migrate Model

# Make your model in schema.prisma

npx prisma migrate dev --name init   # Migrate the model  
npx prisma generate                  # Access the database

> **Optional:** Seed.ts  
> Adding initial data to the database is completely optional but recommended.  
> *Note: In these instructions we didn’t include it.*

---

## 4. Setup Prisma Client

mkdir -p lib && touch lib/prisma.ts

---

### lib/prisma.ts

import { PrismaBetterSqlite3 } from "@prisma/adapter-better-sqlite3";  
import { PrismaClient } from "../generated/prisma/client";

const connectionString = `${process.env.DATABASE_URL}`;

const globalForPrisma = global as unknown as {  
  prisma: PrismaClient;  
};

const adapter = new PrismaBetterSqlite3({ url: connectionString });  
const prisma = globalForPrisma.prisma || new PrismaClient({ adapter });

if (process.env.NODE_ENV !== "production") globalForPrisma.prisma = prisma;

export default prisma;

---

## 5. Use Prisma in Your Code

> Now you can code using the `prisma.ts` in `lib`.  
> Just import it anywhere you want to interact with the database:

import prisma from "@/lib/prisma";

> That’s it! You’re ready to start using Prisma with Next.js.
