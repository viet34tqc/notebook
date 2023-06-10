# Prisma

Code examples: <https://github.com/prisma/prisma-examples/>

## Core concepts

Prisma is an ORM (Object relational mapping). In simple words, ORM is something that helps you interact with DB without using SQL command, it's like a 'bridge' that connect OOP code with database

### Prisma schema

`prisma.schema` file defines models. Model represents table in your DB. Each model includes fields which are columns in DB

```prisma
model Post {
  id        Int     @id @default(autoincrement())
  title     String
  content   String?
  published Boolean @default(false)
  author    User?   @relation(fields: [authorId], references: [id])
  authorId  Int?
}
```

### Prisma push and pull

- `npx prisma db push`: you have `schema.prisma` file and this command will generate table in your db
- `npx prisma db pull`: opposite to `npx prisma pull`, your db is ready and you want to synchronize the current db with prisma schema

### Prisma migrate

As its name suggests, it migrate your model into your db. Put another word, when you create/update your model and type `npx prisma migrate`, it creates/updates tables according to your model. Besides table initiation, it also generates a history of your sql file.

When you update your model, remember to `npx prisma migrate` to make your db in sync with your models. It will generate a `migration.sql`, update database schema and re-generate prisma client

### Prisma client

Prisma client provides JS methods for CRUD. First, you need to install `@prisma/client` as dependancy (not devDependencies). After you change your data model, you 'll need to manually re-generate prisma client to update it using `npx prisma generate` (not needed if you run `prisma migrate` already like above)

```js
import { PrismaClient } from '@prisma/client'

const prisma = new PrismaClient()
```

## Model field

<https://www.prisma.io/docs/concepts/components/prisma-schema/data-model>

Prisma convention for Model name is singular form and PascalCase, like 'Comment', that will create a 'Comment' table in DB. In case you want to change that name, you could use `@@map`, ex: `@@map('comments')`.

Model field composes of 4 parts

- fieldName: like 'id'
- fieldType: There are two kinds of field Scalar field (non-relational field) and relational field.
	+ Scalar field will have field type as: `Int`, `String`, `Boolean`...
	+ Relational field will have field type as another model, like `Comment[]`. **This kind of field is virtual and will not be showed up in DB**
- Type modifers: like ? means this field can be null or `[]` means this field can have many of another field (as `Comment[]`) above
- Attributes: an attribute starts with `@` character. A field can have multiple attributes which separated by a space, ex: ` @id @default(autoincrement())`. Some popular attributes: `@id`, `@unique`, `@default(value)`, `@relation`. For more details: <https://www.prisma.io/docs/concepts/components/prisma-schema/data-model#defining-attributes>

## Advanced prisma client query

<prisma.io/docs/concepts/components/prisma-client/aggregation-grouping-summarizing>

## Update on product when schema change

Run `npx prisma db push` or ` npx prisma db push --preview-feature`

## Delete cascade

Delete a record will also delete other related records
<https://www.prisma.io/docs/concepts/components/prisma-schema/relations/referential-actions>