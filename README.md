# Steps

* create project dir
* run: `gql create prisma-tweets --boilerplate node-basic` to bootstrap a prisma based graphql server
* configure datamodel.graphql

```graphql
type Tweet {
  id: ID! @unique
  createdAt: DateTime!
  text: String!
  owner: User!
  location: Location!
}

type User {
  id: ID! @unique
  createdAt: DateTime!
  updatedAt: DateTime!
  handle: String! @unique
  name: String
  tweets: [Tweet!]!
}

type Location {
  latitude: Float!
  longitude: Float!
}
```

## prisma deploy

* in this example use local
* run: `prisma deploy`, choose local. On running the install from first step, this will automatically run `prisma deploy`

* returns endpoints

```sh
  HTTP:  http://localhost:4466/prisma-tweets/dev
  WS:    ws://localhost:4466/prisma-tweets/dev
```

* every time we rerun `prisma deploy` `prisma.graphql` gets updated.

## get the schema

* `gql get-schema -p database -o schema.graphql`

## src/generated/prisma.graphql

* src/generated/prisma.graphql (Prisma schema): Defines the Prisma API with CRUD functionality for your database. This file is auto-generated based on your data model and should never be manually altered.

* Whenever you make changes to your data model, this file is (automatically) updated as well.

# First resolver

* create a dir for resolvers `mkdir src/resolvers/Query.js`
* write your first query
* import the query into src/index.js

- run: prisma deploy and yarn dev to update graphql schema and open playground

## schema.graphql

* make sure to import your types from generated prisma graphql file and use them in schema.grapqhl
