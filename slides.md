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
``` javascript
query {
  customers {
    id
    name
    address
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
        "address": "123 happy trail"
      },
      {
        "name": "Test customer",
        "id": "2",
        "address": "777 street avenue"
      }
    ]
  }
}
```
--
## Let's try it in GraphQL
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
```javascript
type Customer {
  address: String
  id: ID
  name: String
  pets: Pet
}
```
--
## Root types
### query
A type whose fields are queries
### mutation
A type whose fields are mutations
--
## Lists and Required Fields
```javascript
type Example {
  listOfStuff: [Stuff]
  requiredString: String!
}
```
--
## Arguments
```javascript
type QueryType {
  customers: [Customer]
  customer(id: ID!): Customer
}
```
--
## Argument Example
```
query {
  customer(id: 3) {
    name
    address
  }
}
```
### Result
```
{
  "data": {
    "customer": {
      "name": "Test 2",
      "address": "345 Trail Street"
    }
  }
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
--
## Interfaces
--
## Unions
--
## Fragments
--
## Mutations
```javascript
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
input CustomerInput {
  name: string!
  address: String
}
```
--
## Using input types in a mutation
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
* Way more... [awesome-graphql](https://github.com/chentsulin/awesome-graphql)
--
## Absinte example
--
## Introspection
A graphql query that returns information about the schema
--
## Built-in queries
* `__schema`
* `__type`
--
Let's try it
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
  errors: []
}
```
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
## Apollo demo app
--
## Your schema and you
Fun things you can do with your GraphQL schema
--
## GraphiQL
--
## Validation
--
## graphql-admin
--
