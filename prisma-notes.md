# Prisma ORM

An ORM (Object-Relational Mapping) is a technique or a tool that allows developers to interact with a database using an object-oriented approach, rather than writing SQL queries.

## What is Prisma?

Prisma is an advanced Object-Relational Mapping (ORM) tool that simplifies database access and manipulation in software development.

It allows developers to interact with databases using a type-safe and auto-generated query builder, streamlining the process of handling database operations.

## Installing Prisma

Prisma is not used in production. It is instead used in development as a development dependency.

First, we need to initialize a package.json file. To do this run:

```
npm init y
```

Then run the following command:

```
npm install prisma -D
```

or

```
npm install prisma --save-dev
```

# Prisma Set Up

To set up Prisma in order to use it in our project, run:

```
npx prisma init --datasource-provider DATABASE
```

Go ahead to replace DATABASE with the one you are using, for example if you are using Postgresql:

```
npx prisma init --datasource-provider postgresql
```

By default, Prisma uses PostgreSQL by default, so if your database is postgresql, you can run the command without passing the ```--datasource-provider> flag.

```
npx prisma init
```

## Models

The data model definition part of the Prisma schema defines your application models (also called Prisma models). Models:

- Represent the entities of your application domain
- Map to the tables (relational databases like PostgreSQL) or collections (MongoDB) in your database

## Key concepts of Prisma Models:

1. Data Models - define the tables and their columns (fields) in your database.
1. Field types - Specify the data types for each field (e.g., String, Int, Boolean).
1. Relations - Describe how models relate to each other using relation fields.
1. Attributes - Add adtional metadata to fields and models. Examples include: @default, @relation.

1. Field name (required) e.g productTitle, name etc.
1. Data type of the field (required) e.g. String, Boolean, Integer etc
1. Field type modifier (optional): an example is the <strong>?</strong> modifier. It means that a field is optional.
1. attrobutes (optional) - these are special annotations to define constraints, behaviours and relationships. THey start with the @symbol.

## Model Example

```
model Course {
    id                 Int    @id @default(autoincrement())
  courseTitle       String @map("course_title")
  courseDescription String @map("course_description")
  @@map("products_table")
}
```

## Migrations

Prisma Migrations are a tool that helps manage changes to your database schema over time, especially when working with relational databases like PostgreSQL.

They allow you to track, version, and apply database schema changes in a reliable and repeatable way.

To run a migration:

```
npx prisma migrate dev --name MIGRATION-NAME
```

The --name parameter allows you to give a descriptive name to the migration.

## Example:

```
npx prisma migrate dev --name initial-migration
```

## Prisma Client

Prisma Client is an auto-generated and type-safe query builder that's tailored to your data.

You can use it as an alternative to traditional ORMs such as Sequelize, TypeORM or SQL query builders like knex. js. It is part of the Prisma ecosystem.

To generate a client:

```
npx prisma generate
```

This command reads your schema and generates the Prisma Client inside the node_modules directory.

If it is not installed automatically, run:

```
npm install @prisma/client
```

Prisma then gives us a set of APIs to help us perform CRUD operations.

## CRUD Operations - Create

```
import { PrismaClient } from '@prisma/client';
const client = new PrismaClient();
const createCourse = async () => {
  const newCourse = await client.Course.create({
      data: {
          CourseTitle: "Software Engineerint",
          CourseDescription: "a discipline that applies engineering principles to design, develop, test, and maintain software systems.",

      }
  })

  console.log(newCourse);
}
```

## CREATE multiple Records

We use createMany()

## Example:

```
import { PrismaClient } from "@prisma/client";
const client = new PrismaClient();

const createCourses = async () => {
  const newcourse = await client.course.createMany({
    data: [
      {
        courseTitle: "Machine learning",
        courseDescription: "a powerful course",
        courseCost: 34000,
        unitsLeft: 15,
      },
      {
        courseTitle: "Medicine",
        courseDescription: "Let's save lives!",
        courseCost: 3400,
        unitsLeft: 30,
      },
    ],
  });

  console.log(newcourse);
};

createCourses();
```

## CRUD OPERATIONS - Read

### Get all records using findMany()

```
import { PrismaClient } from "@prisma/client";
const client = new PrismaClient();

const getCourses = async () => {
  const courses = await client.course.findMany();
  console.log(courses)
};

getCourses();
```

## CRUD Operations - Update

### Update a single record using update()

```
import { PrismaClient } from "@prisma/client"; <br>
const client = new PrismaClient();

const updatedCourse = async () => {
  const updatedCourse = await client.course.update({
    where: { id: 3 },
    data: { courseTitle: 'Data Science' }
  })
  console.log(updatedCourse);
};

updatedCourse();
```

### Update Multiple records using the updateMany()

```
import { PrismaClient } from "@prisma/client";<br>
const client = new PrismaClient();<br>

const updatedCourse = async () => {
  const updatedCourses = await client.course.updateMany({
    where: { unitsLeft: { gt: 10 } },
    data: { unitsLeft: 500 }
  })
  console.log(updatedCourses);
};

updatedCourse();
```

## CRUD Operations - Delete

### Delete a single record using delete()

```
import { PrismaClient } from "@prisma/client";
const client = new PrismaClient();

const deleteCourse = async () => {
  const deletedCourse = await client.course.delete({
    where: { id: 1 },
  })
  console.log(deletedCourse);
};

deleteCourse();
```

### Delete multiple records using deleteMany()

```
import { PrismaClient } from "@prisma/client";
const client = new PrismaClient();

const deleteCourse = async () => {
  const deletedCourse = await client.course.deleteMany({
    where: { courseCost: { gte: 15000} },
  })
  console.log(deletedCourse);
};

deleteCourse();
```

## Relationships

In databases, there are three main types of relationships between tables: one-to-one, one-to-many, and many-to-many. These relationships define how data in different tables is connected and ensures data integrity and allows for complex queries.

1. One-to-one:
   Each item in one table is related to only one item in the other table.
   For example, a person might have only one passport.
   A marriage might have only one spouse.
2. One-to-many:
   One item in one table can be related to multiple items in another table.
   For example, a customer can have many orders.
   An employee can have many projects assigned to them.
3. Many-to-many:
   Multiple items in one table can be related to multiple items in another table.
   For example, students can take many courses, and each course can be taken by many students.
   Employees can be on many projects, and each project can have many employees.

## One-to-one:

```
model Author {
  id           String   @id @default(uuid())
  firstName    String
  lastName     String
  emailAddress String
  profile      Profile?

  @@map("authors")
}

model Profile {
  avatarUrl String
  gender    String
  age       Int
  authorId  String @unique
  author    Author @relation(fields: [authorId], references: [id])

  @@map("profiles")
}
```

In the above code, there exists a one-to-one relationship between the Author and Profile. This is indicated by the Profile model having an authorid field that uniquely identifies one Author record.

Each Profile entry is linked to exactly one Author, and each Author can have at most one Profile.

The @relation attribute is used to explicitly define the relationship. It links the authorId field in the Profile model to the id field in the Author model.

## One-to-many relationships

```
model User {
  id    Int    @id @default(autoincrement())
  posts Post[]
}

model Post {
  id       Int  @id @default(autoincrement())
  author   User @relation(fields: [authorId], references: [id])
  authorId Int
}
```

In the above code:

- "a user can have zero or more posts"
- "a post must always have an author"

## Multi-field relations in relational databases

```
model User {
 firstName String
 lastName  String
 post      Post[]

 @@id([firstName, lastName])
}

model Post {
 id              Int    @id @default(autoincrement())
 author          User   @relation(fields: [authorFirstName, authorLastName], references: [firstName, lastName])
 authorFirstName String // relation scalar field (used in the `@relation` attribute above)
 authorLastName  String // relation scalar field (used in the `@relation` attribute above)
}

```
