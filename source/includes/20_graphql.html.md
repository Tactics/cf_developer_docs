# GraphQL

## What is GraphQL ?

GraphQL is a data query language developed by Facebook in 2012.

It was published in 2015 and has since been adopted by major tech products like GitHub, YouTube or Pinterest to build the most recent versions of their APIs.

GraphQL provides an alternative to REST and allows clients to define the structure of the data required, and exactly the same structure of the data is returned from the server.  It is a strongly typed runtime which allows clients to dictate what data is needed. This avoids both the problems of over-fetching as well as under-fetching of data.

More information on GraphQL can be found here (amongs plenty of others): 

* [http://graphql.org/](http://graphql.org/)
* [https://www.howtographql.com/](https://www.howtographql.com/)


## The GraphQL Endpoint

Where REST has several endpoints, graphQL has just one:

`https://api.clearfacts.be/graphql`

## The schema

We generate documentation from our GraphQL schema. All calls are validated and executed against the schema.
Use these docs to find out what operations (mutations in GraphQL lingo) you can execute and which data you can query.

This is the link to our schema documentation:
[https://api.clearfacts.be/doc/publicSchema/index.html](https://api.clearfacts.be/doc/publicSchema/index.html)

### Allowed operations

* [Queries](https://api.clearfacts.be/doc/publicSchema/query.doc.html)
* [Mutations](https://api.clearfacts.be/doc/publicSchema/mutation.doc.html)

### Types

* scalars
* enums
* interfaces
* objects unions
* input objects.

## Examples

In this section we provide you with a few examples to get you started.
If you'd like to run the examples, you can use the GraphQL Playground on graphqlbin.com.

[https://www.graphqlbin.com/QWzgHk](https://www.graphqlbin.com/QWzgHk)

<aside class="notice">
Make sure to replace <code>&lt;token&gt;</code> with your own access token in the HTTP header <code>"Authorization": "Bearer &lt;token&gt;"</code>.
</aside>

### Query the list of accessible administrations

```graphql
query admins {
  administrations {
    name, 
    companyNumber, 
    emails {
      type,
      emailAddress
    }
    companyType,
    address {
      streetAddress, 
      country {
        iso2,
        name
      }
    },
    accountManager
  }
}
```
> will result in:

```json
{
  "data": {
    "administrations": [
      {
        "name": "Acme Company",
        "companyNumber": "0123123123",
        "emails": [
          {
            "type": "purchase",
            "emailAddress": "aankoop-0123123123@acme.clearfacts.be"
          },
          {
            "type": "purchase",
            "emailAddress": "achat-0123123123@acme.clearfacts.be"
          }          
        ],
        "companyType": null,
        "address": {
          "streetAddress": "Street 16",
          "country": {
            "iso2": "BE",
            "name": "Belgium"
          }
        },
        "accountManager": "account.manager@acme.be"
      }
    ]
  }
}
```

This first example queries all the administrations a user has access to. 

For each administration we'll fetch basic information such as name, company number and address, as well as the 
account manager of that administration at the accountants office.  Finally we'll add the email addresses that can
be used to send documents to.  

The result is a JSON structure with an array of all the administrations and their requested properties.
(Please note that the result has been trimmed for brevity.)

If you'd like to test the example above, you can use this link to the GraphQL Playground: 
[https://graphqlbin.com/YELrTp](https://graphqlbin.com/YELrTp)

(Don't forget to replace the `<token>` with your own token in the HTTP headers)

### Query the list of available categories for a company (using arguments)

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

In this example we will query a list of specific elements for a administration.
To indicate which administration, we'll pass the VAT number as an argument to our query, after which we'll ask for both types of categories, 
with their respective members. Each member will provide us with an ID that we can use in other queries, as well as a name.

After we start typing a query, an argument can be used after specifying what object we want to query.

The result is a JSON structure with an array of all the categories and their requested properties.
(Please note that the result has been trimmed for brevity.)

If you'd like to test the example above, you can use this link to the GraphQL Playground: 
[https://graphqlbin.com](https://graphqlbin.com)

### Upload a sales invoice

```graphql
mutation upload($vatnumber: String!, $filename: String!, $invoicetype: InvoiceTypeArgument!) {
 uploadFile(vatnumber: $vatnumber, filename: $filename, invoicetype: $invoicetype) { 
   id,
   amountOfPages
 } 
}
```
```json
{"variables": { "vatnumber": "0123123123", "filename": "test_upload.pdf", "invoicetype": "SALE"}}
``` 

In this example we will call a mutation with the goal of uploading a file for a specific administration.
Just like our previous example, we will provide arguments to determine the administration, as well as the name and the type of the file.
The uploaded file will in turn provide us with an ID, the filename and the detected amount of pages.

The way in which we send this mutation differs a bit from regular queries. Since we have to provide a file as data, we 
will send our information as form-data instead of a text-based content-type like application/json or application/graphql.

The file must be present as a parameter named "file", while the mutation and arguments can be provided as regular text-based 
content in the form, respectively called "query" and "variables".

Unfortunately due to the nature of the GraphQL Playground, we can not provide you with an example, as they do not 
allow or provide an option for a user to enter form-data or upload files. 