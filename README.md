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



