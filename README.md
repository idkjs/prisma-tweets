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

## TEMP_APP_SECRET

* see: https://www.howtographql.com/graphql-js/4-signup-and-login/

```js
const TEMP_APP_SECRET = "mysecret123";

module.exports = {
  TEMP_APP_SECRET
};
```

## using signup mutation

* output:
* mutation:

```gql
mutation {
  signup(
    email: "aarmand.inbox@gmail.com"
    password: "graphql"
    username: "idkjs"
    fullName: "Alain Armand"
  ) {
    token
    user {
      id
    }
  }
}
```

```json
{
  "data": {
    "signup": {
      "token":
        "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VySWQiOiJjamQ1d3RucTcwMW82MDE4NXlmYmUzd3dzIiwiaWF0IjoxNTE3NTc0NzQ4fQ.i5rDF7omlnqtWYq6HQLPKwkLqWF6R_kG2OgHY-VNm_Q",
      "user": {
        "id": "cjd5wtnq701o60185yfbe3wws"
      }
    }
  }
}
```

## using login mutation

* get header from response and use in variables section of playground
* mutation:

```graphql
mutation {
  login(email: "aarmand.inbox@gmail.com", password: "graphql") {
    token
  }
}
```

* output:

```json
{
  "data": {
    "login": {
      "token":
        "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VySWQiOiJjamQ1d3RucTcwMW82MDE4NXlmYmUzd3dzIiwiaWF0IjoxNTE3NTc1MTE3fQ.a7CHdRbLOBOwbB1DvTpk-liUn8xKBjFRTPWS4hBcMT0"
    }
  }
}
```

* header

```json
{
  "Authorization":
    "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VySWQiOiJjamQ1d3RucTcwMW82MDE4NXlmYmUzd3dzIiwiaWF0IjoxNTE3NTc1MTE3fQ.a7CHdRbLOBOwbB1DvTpk-liUn8xKBjFRTPWS4hBcMT0"
}
```

## Using Feed Query to get tweets

* the feed query returns tweets posted on this server, not actually interacting with twitter

* query

```gql
{
  feed {
    tweets {
      id
      postedBy {
        username
      }
      body
    }
  }
}
```

* ouptu:

```json
{
  "data": {
    "feed": {
      "tweets": [
        {
          "id": "cjd5x6taj01sy0185djv9t6hu",
          "postedBy": {
            "username": "idkjs"
          },
          "body": "https://www.graphqlweekly.com"
        }
      ]
    }
  }
}
```

## deploy with Now

* run: `now --public`

- see it here: prisma-tweets.now.sh
