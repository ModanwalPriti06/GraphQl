# GraphQl

GraphQL is an open-source query language for APIs and a runtime that executes those queries on the serve.

## GraphQL query used in Contentful GraphQL API to fetch data.
```
query {
  person(id: "6oSubFEQ0KsAKPGz939U9t") {
    name
  }

or

  personCollection(where: {}) {
    items {
      name
    }
  }
}
```
Here page is one content type we can fetch data 2 types. person(findOne) only need to fetch one time based on id but to get multiple data then need to use personCollection (findMany). Which have multiple key like - limit, skip, where, linkedFrom, sys.

```
personCollection(where: {}) {
  items {
    name
  }
}
```
This means:
- Fetch multiple person entries
- where: {} means no filter (get all records)
- items contains the list
- Return the name field for each person

| Part               | Meaning                 |
| ------------------ | ----------------------- |
| `query`            | Fetch data              |
| `person(id)`       | Get one record          |
| `personCollection` | Get multiple records    |
| `items`            | List of results         |
| `name`             | Field we want to return |

```
assetCollection {
  items {
    sys {
      id
    }
  }
}
```
In Contentful, sys stands for System Metadata. It contains internal system information about the entry or asset.

# Fetch data
```
window
      .fetch(`https://graphql.contentful.com/content/v1/spaces/${REACT_APP_SPACE_ID}`, {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
          Authorization: `Bearer ${REACT_APP_CDA_TOKEN}`,
        },
        body: JSON.stringify({ query }),
      })
      .then((response) => response.json())
      .then((data) => setData(data.data.person));
```

# Fetching image
- sys
- url
- width
- height
- title
- description
- contentType
- size

>[!Note]
> Can 10 nested query

>[!Important]
> Body size 8kb

# Fragnant Query
In GraphQL, a fragment is used to reuse the same set of fields in multiple queries. It helps avoid repeating the same code again and again.
```
query {
  blogPostCollection {
    items {
      title
      slug
      featuredImage {
        ...ImageFields
      }
    }
  }
}

fragment ImageFields on Asset {
  url
  title
}
```
## Why Fragments are Useful Benefits:
- Avoid duplicate code
- Keep queries clean
- Easy to maintain
- Used a lot in large apps

# $isPreview

# What is __typename in GraphQL?
__typename is a built-in (meta) field in GraphQL that tells you the type of the object returned by the server.
👉 You don’t define it — GraphQL gives it automatically.

```
{
  search(text: "Aman") {
    __typename
    ... on User {
      name
    }
    ... on Post {
      title
    }
  }
}
```
Response Above query - If User → show profile. If Post → show blog
```
[
  { "__typename": "User", "name": "Aman" },
  { "__typename": "Post", "title": "Hello World" }
]
```
>[!Important]
> Use __typename to uniquely identify objects in cache.

# API rate limits
- 100 requests per minute
- If you send 101 requests, the API will return an error like: ``` "error": "Rate limit exceeded" ``` HTTP **429** Too Many Requests.
- In Contentful GraphQL API: 55 requests per second per token (approx)

# Query complexity limits
Query complexity limits specify the amount of data a client can request from the GraphQL Content API in one request. You can currently request up to 11000 entities in one request.

>[!Important]
> When a client gets query complexity limited, the API responds with a **TOO_COMPLEX_QUERY** error.

# Query size limits
Query size limits specify the maximum size of the query parameter for GET requests and the total payload size for POST requests. This limit is 8kb.
This limit includes whitespace and newline characters. Removing semantically unnecessary whitespaces and newline characters before sending a request can lower the query size. You can reduce the query size without manual editing using GraphQL minifiers, such as GQLMin.

>[!Important]
>When the query size of a request exceeds the limit, the API returns a **QUERY_TOO_BIG** error.

# Rich Text
By default a Rich Text field has a total **limit of 1000** linked entities of all supported types. This means that by default the links field in each Rich Text entry has a complexity of 1000.

















