# GraphQL Architecture

> GraphQL is a specification. It's a document that describes the behaviour of a GraphQL server

## Use Cases
These are the common use cases for implementing a GraphQL architecture
1. GraphQL server with connected database
2. GraphQL server that acts as a layer between client and legacy / 3rd party systems (front controller pattern)
3. A hybrid of the 2

### GraphQL server with connected database
- Most common for greenfields projects
- When a query hits a GraphQL server the server interprets the incoming payload and fetches the required information after performing any Mutations
- This process is called _resolving_ the query
- GraphQL is transport layer agnostic, you can use TCP, websockets etc.
- GraphQL is storage engine agnostic, you can use any database implementation

### GraphQL layer integrating existing systems
- Allows a single coherent API for multiple systems
- Unify existing systems and hide their complexity
- GraphQL doesn't care what it's wrapping, as long as it can resolve the queries

### GraphQL layer / database hybrid
- Can resolve certain queries using data directly from a connected database
- Can also resolve the queries using data from the existing systems

## Resolver Functions
Each of the fields in the payload of a GraphQL query or mutation corresponds to exactly one function that's called a resolver. A resolvers purpose is to fetch the data for it's field.

When the server receives a query it will call all the functions for the fields that are specified in thee query's payload. Once all thee resolvers returned, the server packages up the data in the format described by the query and return this to the client.

```
query {
  User(id: "1234") {
    name,
    followers(last: 5) {
      name
      age
    }
  }
}
```

Would correspond to the resolver functions:
```
User(id: String!): User
name(user: User!): String
age(user: User!): Int
friends(first: Int, user: User!): [User!]!
```

## GraphQL client libraries
The beauty of this for frontend development is complexity is pushed to the server side where they can deal with the computations. The client doesn't have to know where the data that it fetches is coming from and can use a single coherent flexible API.

When fetching data from a REST API, most applications will have to go through the following steps:

1. construct and send HTTP request (e.g. with fetch in Javascript)
2. receive and parse server response
3. store data locally (either simply in memory or persistent)
4. display data in the UI

Using declarative GraphQL implementation we have two steps:

1. describe data requirements
2. display data in UI

Libraries like Relay and Apollo allow us to achieve this.
