## Agenda
* What it is?
* Why?
* Details
* Demos
--
## GraphQL
* A query language for your API
* Persistence agnostic
* Implementations in many languages
--
## Why?
### REST is fine..
#### Until it's not.
--
Let's display some customers.
## GET /customers
```javascript
[
  {
    id: 1
    name: "Bill"
    address: "123 happy trail"
  }
  {
    id: 2
    name: "Joe"
    address: "123 happy trail"
  }  
]
```
--
And of their pet's names
### GET /customers/1/pets
```javascript
[
  {
    name: "Fido",
    species: "Doge",
    owner_id: 1
  },
  {
    name: "Fluffy",
    species: "Iguana",
    owner_id: 2
  }
]
```
--
### GET /customers/1/pets
### GET /customers/2/pets
### ...
## OR
### GET /customers?includePets=true
--
![kramer](kramer-whichdata.jpg)
--
## Your first GraphQL query
```
query {
  customers {
    id
    name
  }
}
```
--
## Response
```
{
  "data": {
    "customers": [
      {
        "name": "Bill",
        "id": "1",
      },
      {
        "name": "Test customer",
        "id": "2",
      }
    ]
  }
}
```
--
## [Let's try it in GraphiQL](http://localhost:4000/graphiql?query=query%20%7B%0A%20%20customers%20%7B%0A%20%20%20%20id%0A%20%20%20%20name%0A%20%20%7D%0A%7D)
--
## It's the schema that makes it go
--
### A schema is a Graph of Types
--
## Scalar types
* Int
* Float
* String
* Boolean
* ID
* _Your custom type_
--
## Object types
### can have fields
```
type Customer {
  address: String
  id: ID
  name: String!
  pets: [Pet]
}
```
--
## Fields can take arguments
```
type User {
  name: String
  posts(limit: Int)
}
```
--
### Using Arguments
```
query {
  users {
    name
    posts(limit: 3)
  }
}
```
### Result
```
{
  "data": {
    "users": {
      "name": "Test 2",
      "posts": [
        ...
      ]
    }
  }
}
```
--
## Root types
### query
A type that defines all possible queries
```
type RootQueryType {
  customers: [Customer]
  customer(id: ID!): Customer
}
```
### mutation
A type whose fields define all possible mutations
```
type RootMutationType {
  createPost(name: String!, body: String!): Post
}
```
--
## Variables
```
query myCustomer($customerId:ID!) {
  customer(id: $customerId) {
    name
  }
}
```
Let's try it in GraphiQL
--
## Enums
```
enum CustomerType {
  INDIVIDUAL
  CORPORATION
}
```
```
type Customer {
  customer_type: CustomerType
}
```
--
## Interfaces
```
interface Animal {
  name: String!
  species: String!
}
```
```
type Pet implements Animal {
  name: String!
  species: String!
}
```
```
type LiveStock implements Animal {
  name: String!
  species: String!
}
```
--
## Unions
```
union LivingThing = Plant | Animal
```
```
type Virus {
  infects: LivingThing
}
```
--
## Fragments
```
fragment AddressFields on Address {
  line1
  city
  state
  zip
}

query {
  customers {
    name
    address {
      ...AddressFields
    }
  }
}
```
[In graphiql](http://localhost:4000/graphiql?query=fragment%20AddressFields%20on%20Address%20%7B%0A%20%20line1%0A%20%20city%0A%20%20state%0A%20%20zip%0A%7D%0A%0Aquery%20%7B%0A%20%20customers%20%7B%0A%20%20%20%20name%0A%20%20%20%20address%20%7B%0A%20%20%20%20%20%20...AddressFields%0A%20%20%20%20%7D%0A%20%20%7D%0A%7D)
--
## Mutations
```
mutation {
  createCustomer(name: "Test 2", address: "345 Trail Street") {
    id
    name
  }
}
```
Result:
```
{
  "data": {
    "createCustomer": {
      "name": "Test 2",
      "id": "3"
    }
  }
}
```
--
## Input Types
```
input AddressInput {
  line1: String!
  city: String!
  state: String!
  zip: String!
}
```
```
input CustomerInput {
  name: string!
  address: AddressInput
}
```
--
## Using input types in a mutation
```
mutation createCustomer($customerAttributes:CustomerInput) {
  createCustomer(customerAttributes:$customerAttributes) {
    id
    name
  }
}
```
Query Variables
```
{
  "customerAttributes": {
    "name": "Frodo",
    "address": {
      "line1": "123 trail street",
      "city": "Sunnyside",
      "zip": "12345"
    }
  }
}
```
--
## Let's try a mutation
--
## Defining a schema
Is entirely Implementation specific
--
## There is one for your favorite language already...
* GraphQL.js
* Absinthe (Elixir)
* graphql-java
* sangria (Scala)
* graphql-ruby
* Way more...
* [awesome-graphql](https://github.com/chentsulin/awesome-graphql)
--
## Absinthe schema code
--
## Introspection
A graphql query that returns information about the schema
--
## Built-in queries
* `__schema`
* `__type`

[to the graphiqls..](http://localhost:4000/graphiql?query=query%20%7B%0A%20%20__schema%20%7B%0A%20%20%20%20types%20%7B%0A%20%20%20%20%20%20name%0A%20%20%20%20%20%20kind%0A%20%20%20%20%7D%0A%20%20%7D%0A%7D)
--
## The default introspection query
--
## GraphQL request
### POST /GraphQL
Content-type: "application/json"
```javascript
{
  query: "foo { bar, baz }",
  operationName: "query",
  variables: {

  }
}
```
--
## GraphQL response
```javascript
{
  data: {
    foo: {
      bar: "...",
      baz: "..."
    }
  }
}
```
--
## GraphQL errors
#### Live in the errors attribute
#### [Lets see](http://localhost:4000/graphiql?query=query%20%7B%0A%20%20utter%20crap%0A%7D)
--
## GraphQL clients
--
## Relay
* Created by Facebook
* Lets components declare their data needs (via fragments)
* Introduces some constraints on your schema
* Replaces redux
--
## Apollo
* Created by Meteor
* Integrates with React, Angular2, etc
* Easy to introduce, no schema changes required
* Can "watch" queries
--
## Pagination
#### Relay Connection Cursors specification
* edges
  * node
  * cursor
* pageInfo
  * hasNextPage
  * hasPreviousPage
* args
  * first, after
--
## Demo: Github searching
--
## Your schema and you
Fun things you can do with your GraphQL schema
--
## Demo: eslint validation
--
## Demo: graphql-admin
--
## Other fun examples
* [github](https://developer.github.com/early-access/graphql/)
* [swapi](https://graphql-swapi.parseapp.com/)
* [graphqlhub](https://www.graphqlhub.com/)
__
