# Learning how to use Prisma with Vercel

Prisma is an ORM that least you access your database with Node.js and TypeScript applications


## Setting up Prisma in your project
##### Run the following command

`npm install prisma --save-dev`

- this creates a schema.prisma which we will use for creating the structure of our database
- this also creates a .env file to hold our database url and any other environment variables we might need for the project

##### Create your database Schema in the schema.prisma file

An example schema looks like this:

```graphql
datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}

generator client {
  provider = "prisma-client-js"
}

model Post {
  id        Int     @default(autoincrement()) @id
  title     String
  content   String?
  published Boolean @default(false)
  author    User?   @relation(fields: [authorId], references: [id])
  authorId  Int?
}

model User {
  id            Int       @default(autoincrement()) @id
  name          String?
  email         String?   @unique
  createdAt     DateTime  @default(now()) @map(name: "created_at")
  updatedAt     DateTime  @updatedAt @map(name: "updated_at")
  posts         Post[]
  @@map(name: "users")
}
```

In the schema we provide:
- the datasource: the database where we get our data from
- the generator: which generates our prisma client 
- and the  model(s) that act as our tables in the database

##### Run this command next
`npx prisma db push`

This generates our tables within our database from our schema.prisma file

###### (Optional) Next we can add data to our database from the prisma GUI
`npx prisma studio`

This command opens up a GUI to manually input seed data into the database and test things

##### Next we need to install the prisma client so we can access our database through prisma in Node

`npm install @prisma/client`

`npx prisma generate`

We then need to create a file that will hold code to create a prisma instance every time we need it

That file will look like this: 
```ts
// lib/prisma.ts
import { PrismaClient } from '@prisma/client';

let prisma: PrismaClient;

if (process.env.NODE_ENV === 'production') {
  prisma = new PrismaClient();
} else {
  if (!global.prisma) {
    global.prisma = new PrismaClient();
  }
  prisma = global.prisma;
}

export default prisma;
```

Now we'll import this every where we want to access our database

Import with:
`import prisma from "{FILE_PATH}"`

Now in any endpoint where we want to manipulate data in our database we will put code like this:

```ts
const feed = await prisma.post.findMany({
    where: { published: true },
    include: {
      author: {
        select: { name: true },
      },
    },
  });
```

The above code selects all "Posts" where "published" is true and also includes the name of the "author" that is related to each "Post"