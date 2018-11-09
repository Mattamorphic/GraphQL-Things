# GraphQL Concept

## Definition / Schema
GraphQL uses a SDL (Schema Definition Language) allowing us to define a contract between the client and the server, defining how a client can access the data. This acts as a source of truth.

### Example
In this example we are going to define a User type and a Post type. You can think of both of these as nodes, or verticals in a graph.

```
type User {
  name: String!
  age: Int!
}
```

In this example we are using the "type" keyword to define the type "User" this User type has two fields, name, and age. We provide a data type for each of these, and use the exclamation mark "!" to denote this as required (non-nullable, non-optional)

```
type Post {
  title: String!
  content: String!
  author: User!
}
```

The structure is similar for the Post type. In this example we are providing a one to one relationship, or edge, on the graph between this type, and the User type - by denoting that the field "author" is another custom type.

```
type User {
  name: String!
  age: Int!
  posts: [Post!]!
}
```

Going back to our User type, we can extend this to have a one to many relationship (edges), to the different posts by denoting the field: "posts" as an array ([]) of the Post type.

## Fetching Data with Queries

**Quick Reminder**
REST APIs expose multiple endpoints, whereby each returns a clearly defined structured

GraphQL exposes a single endpoint, which returns a flexible structure and requires the client to express the data needs in the request.

### Example
```
{
  allUsers {
    name
    posts {
      title
    }
  }
}
```
In this example query "allUsers" is the root field of the query - and all that follows that is the payload of the query. In this payload the client is expressing that it requires the name, and the title of each of the posts belonging to that user, expressed on the posts edge in the User type.

What this returns is:
```
{
  "allUsers": [
    {
      "name": "mattamorphic",
      "posts": [
        {"title": "Post A"},
        {"title": "Post B"},
        {"title": "Post C"},
        ...
      ]
    },
    ...
  ]
}
```

Each field can have zero or more arguments if that's specified in schema. For example the allPersons field could have a last parameter.
```
{
  allUsers(last: 2) {
    name
  }
}
```

This returns:
```
{
  "allUsers": [
    {"name": "mattamorphic"},
    {"name": "other_user"}
  ]
}
```

## Writing data with Mutations

These allow us to perform create, update and delete operations on data. Like queries (reads), mutations define a root field and are wrapped in a 'mutation' keyword.

### Example

```
mutation {
  createUser(name: "mattamorphic", age: 30) {
    name,
    age
  }
}
```

In this case the root field is defined as createUser, with the two arguments, name and age. Similar to the query we can also specify a payload for the mutation - where we can ask for different fields of the created type (node). This data is post mutation, so being able to perform a mutation and return the post processed data is powerful.

```
{
  "createUser": {
    "name": "mattamorphic",
    "age": 30
  }
}
```

There is an ID data type that we can specify on the user type, then return as part of this create mutation in the payload:

```
// our updated type
type User {
  id: ID!
  name: String!
  age: Int!
}

// Our mutation
mutation {
  createUser(name: "mattamorphic", age: 30) {
    id
  }
}

// our returned payload
{
  "createUser": {
    "id": 12345
  }
}
```
