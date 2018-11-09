# GraphQL Concept

## Definition / Schema
GraphQL uses a SDL (Schema Definition Language) allowing us to define a contract between the client and the server, defining how a client can access the data. This acts as a source of truth.

## Example
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
