## Query the list of available categories for a company using arguments

```graphql
query categories {
  archiveCategories(vatnumber: "0123456789") {
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
          "name": "Other"
        },
        {
          "id": "cb29ccf9-2dca-11e8-9a15-000c2985a2fd",
          "name": "Credits"
        }
      ],
      "permanent": [
        {
          "id": "cb641aea-2dca-11e8-9a15-000c2985a2fd",
          "name": "Other"
        },
        {
          "id": "cb641a7b-2dca-11e8-9a15-000c2985a2fd",
          "name": "Financial"
        }
      ]
    }
  }
}
```

This example illustrates the use of **inline query arguments**.

Let's query all the archiveCategories for an administration.
To indicate which administration, we'll pass the VAT number as an argument to our query, after which we'll ask for both types of categories, 
with their respective members. 

Each member has an ID you can use to upload a file to the archive using the uploadArchiveFile mutation. 

If you'd like to test the example above, you can use this link to the GraphQL Playground: 
[https://www.graphqlbin.com/v2/7L28TQ](https://www.graphqlbin.com/v2/7L28TQ)