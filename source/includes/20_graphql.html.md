# GraphQL

## What is GraphQL ?

GraphQL is a data query language developed by Facebook in 2012.

It was published in 2015 and has since been adopted by major tech products like GitHub, YouTube or Pinterest to build the most recent versions of their APIs.

GraphQL provides an alternative to REST and allows clients to define the structure of the data required, and exactly the same structure of the data is returned from the server.  It is a strongly typed runtime which allows clients to dictate what data is needed. This avoids both the problems of over-fetching as well as under-fetching of data.

More information on GraphQL can be found here (amongst plenty of others): 

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

For each administration we'll fetch basic information as name, company number and address, but also the 
accountmanager of that administration at the accountants office.  Finally we'll add the emailaddresses that can
be used to send documents too.  

The result is a JSON structure with an array of all the administrations and its requested properties.
(Please note that the result has been trimmed for brevity.)

If you'd like to test the example above, you can use this link to the GraphQL Playground: 
[https://graphqlbin.com/YELrTp](https://graphqlbin.com/YELrTp)

(Don't forget to replace the `<token>` with your own token in the HTTP headers)

### Upload a sales invoice

@needs more detail 

```graphql
mutation UploadFile($vatnumber: String!, $filename: String!, $invoicetype: InvoiceTypeArgument!) {
 uploadFile(vatnumber: $vatnumber, filename: $filename, invoicetype: $invoicetype) { 
   uuid 
 } 
}

variables: { "vatnumber": "0809003556", "filename": "test_API_upload", "invoicetype":"SALE"}
``` 
