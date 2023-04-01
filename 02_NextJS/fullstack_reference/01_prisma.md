## Objective
- A reference to piece together various technologies

## Stack
- NextJS
- GraphQL Yoga
- Pothos
- Apollo Client
- Prisma
- PostgreSQL
- AWS S3
- Auth0
- TypeScript
- TailwindCSS
- Vercel

## Setup Prisma
```
git clone -b part-1 https://github.com/m-abdelwahab/awesome-links.git

cd awesome-links
npm install
supabase init
supabase start
npm install --save-dev prisma
npx prisma init
```

## Data Model
```
// prisma/schema.prisma

datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}

generator client {
  provider = "prisma-client-js"
}

model User {
  id        Int      @id @default(autoincrement())
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
  email     String?  @unique
  image     String?
  role      Role     @default(USER)
  bookmarks Link[]
}

enum Role {
  USER
  ADMIN
}

model Link {
  id          Int      @id @default(autoincrement())
  createdAt   DateTime @default(now())
  updatedAt   DateTime @updatedAt
  title       String
  description String
  url         String
  imageUrl    String
  category    String
  users       User[]
}
```

## Generate Table
```
npx prisma generate
npx prisma migrate dev --name init
npm install @prisma/client
```

## Seeding the database
### create a new file /prisma/seed.ts
```
// prisma/seed.ts

import { PrismaClient } from '@prisma/client'
import { links } from '../data/links'
const prisma = new PrismaClient()

async function main() {
  await prisma.user.create({
    data: {
      email: `testemail@gmail.com`,
      role: 'ADMIN',
    },
  })

  await prisma.link.createMany({
    data: links,
  })
}

main()
  .catch(e => {
    console.error(e)
    process.exit(1)
  })
  .finally(async () => {
    await prisma.$disconnect()
  })
```

## Install ts node
By default, Next.js forces the use of ESNext modules, we need to override this behavior or else we will not be able to execute the seeding script. To do so, first install ts-node as a development dependency:
```
npm install --save-dev ts-node
```

## Add the following in tsconfig.json
```
  "ts-node": {
    "compilerOptions": {
      "module": "CommonJS"
    }
  }
```

## Update package json
```
  "prisma": {
    "seed": "ts-node --transpile-only ./prisma/seed.ts"
  }
```

## Run seed data command
```
npx prisma db seed
```

## Update db
```
npx prisma db push
```

## Inspect Records
```
npx prisma studio
```

## References
- https://www.prisma.io/blog/fullstack-nextjs-graphql-prisma-oklidw1rhw