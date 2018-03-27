### Query the list of available categories for a company using arguments

```graphql
query categories {
  archiveCategories(vatnumber: "0123123123") {
    various {
      id,
      name  
    },
    permanent {
      id,
      name
    }
  }
}
```
> will result in:

```json
{
  "data": {
    "archiveCategories": {
      "various": [
        {
          "id": "cb29ce32-2dca-11e8-9a15-000c2985a2fd",
          "name": "Andere"
        },
        {
          "id": "cb29ccf9-2dca-11e8-9a15-000c2985a2fd",
          "name": "Kredieten"
        }
      ],
      "permanent": [
        {
          "id": "cb641aea-2dca-11e8-9a15-000c2985a2fd",
          "name": "Andere"
        },
        {
          "id": "cb641a7b-2dca-11e8-9a15-000c2985a2fd",
          "name": "Financieel"
        }
      ]
    }
  }
}
```

In this example we will query a list of specific elements for an administration.
To indicate which administration, we'll pass the VAT number as an argument to our query, after which we'll ask for both types of categories, 
with their respective members. Each member will provide us with an ID that we can use in other queries, as well as a name.

After we start typing a query, an argument can be used after specifying what object we want to query.

The result is a JSON structure with an array of all the categories and their requested properties.
(Please note that the result has been trimmed for brevity.)

If you'd like to test the example above, you can use this link to the GraphQL Playground: 
[https://graphqlbin.com](https://graphqlbin.com)