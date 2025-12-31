# Prisma

Steps to connect NextJS and Prisma

1.I followed the prisma for nextjs but with some tweaks, by default it is a postgreSQL in the documentation but we fixed it by referring to databases/drivers/sqlite

(npm install prisma @types/node @types/better-sqlite3 --save-dev )
(npm install @prisma/client @prisma/adapter-better-sqlite3) //We changed this instead of "@prisma/adapter-pg dotenv pg" because we are using sqlite
(npx prisma init --datasource-provider sqlite --output ../generated/prisma) //so we can start writing our models
(Make Model)
(npx prisma migrate dev --name init) //Migrate The Model
(npx prisma generate) To Access the database
(Seed.ts) //It means adding initial data to the database "completely optional but recommeneded" Note: In this instructions we didnt

(mkdir -p lib && touch lib/prisma.ts) // Refer the (start) -> (end) below:

(start)
lib/prisma.ts
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
///
(end)

!NOW YOU CAN CODE USING THE prisma.ts in the lib, just import it anywhere you like to code it in!

