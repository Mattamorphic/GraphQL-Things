# GraphQL API Notes and Examples

GraphQL is a query language for APIs that uses a declarative model.
It uses a single endpoint and responds to queries sent to it.

**Why?**

1. Increased mobile usage creates the need for data loading efficiency. The user is potentially on a network that is low performing, and we want to reduce the network traffic and payload size to a single request per operation, sending and receiving precisely what is required.

2. The variety of frontend frameworks and platforms available that run client apps makes it difficult to create a flexible REST API that can respond to the differing needs.

3. Fast development and expectation for rapid feature development. Unlike REST APIs where the frontend and backend are decoupled in a logical sense, they aren"t decoupled in a pragmatic sense. GraphQL allows us to decouple these in a pragmatic sense.

## Why is GraphQL considered a better API solution than rest?
- Flexibility
- Efficiency

**Example**
* Fetching the titles of posts for a specific user from their blog, and the last 3 followers of that blog

**REST API solution**
1. GET users/{id}
```
{"name": "mattamorphic", "id": 0987}
```

2. GET users/{id}/posts
```
[
    {"id": 1234, "title": "Post A", "content": "lorem ipsum", "created": "2018-11-08"},
    {"id": 2345, "title": "Post B", "content": "lorem ipsum", "created": "2018-11-07"},
    {"id": 3456, "title": "Post C", "content": "lorem ipsum", "created": "2018-11-06"},
    ...
]
```

3. GET users/{id}/followers
```
[
    {"name": "friend1", "id": 9876, "created": "2018-10-30"},
    {"name": "friend2", "id": 8765, "created": "2018-10-30"},
    {"name": "friend3", "id": 9876, "created": "2018-10-30"},
    ...
]
```

**GraphQL solution**
- POST /endpoint
- Send Payload
```
query {
    User(id: 0987) {
      name,
      posts {
        title
      },
      followers (last: 3) {
        name
      }
    }
}
```
- Receive payload
```
{
  "data": {
    "User": {
      "name": "mattamorphic",
      "posts": [
        {"title": "Post A"},
        {"title": "Post B"},
        {"title": "Post C"}
      ],
      "followers": [
        {"name": "friend1"},
        {"name": "friend2"},
        {"name": "friend3"}
      ]
    }
  }
}
```

As we can see from the example above GraphQL solves the problem of both over fetching data, and underfetching data (the n+1 problem). REST endpoint tend to return fixed data structures.

> "Think in Graphs, not Endpoints."

## REST issues

**Overfetching**
Without having set parameters and flags added to an endpoint (query parameters) an endpoint /users/{id}/posts would return the entire list of posts (potentially paginated), and each of these post nodes would include the full post object, even if we only want the title.

**Underfetching (n+1)**
On the flip side, a request returns a list of nodes, such as the posts, for each of these posts we have to do requests to fetch the top rated comment off of an edge on this node.

> REST API endpoints are structured against the view. They are not decoupled pragmatically

**GraphQL provides insights**
As requests are declarative in nature, it's easy to see what is being requested in each call. As oppose to grouping families of requests to build pictures.
