# GraphQL

## What is GraphQL ?

GraphQL is a data query language developed by Facebook in 2012.

It was published in 2015 and has since been adopted by major tech products like GitHub, YouTube or Pinterest to build the most recent versions of their APIs.

GraphQL provides an alternative to REST and allows clients to define the structure of the data required, and exactly the same structure of the data is returned from the server. 

It is a strongly typed runtime which allows clients to dictate what data is needed. This avoids both the problems of over-fetching as well as under-fetching of data.

More information on GraphQL can be found here (amongst plenty of others): 

* [http://graphql.org/](http://graphql.org/)
* [https://www.howtographql.com/](https://www.howtographql.com/)


## The GraphQL Endpoint

Where REST has several endpoints, graphQL has just one:

`https://api.clearfacts.be/graphql`

## The schema

We generate documentation from our GraphQL schema. 
All calls are validated and executed against the schema. 

Use these docs to find out what data you can call.

[https://master.acc.clearfacts.be/doc/publicSchema/index.html](https://master.acc.clearfacts.be/doc/publicSchema/index.html)

### Allowed operations

* [Queries](https://master.acc.clearfacts.be/doc/publicSchema/query.doc.html)
* [Mutations](https://master.acc.clearfacts.be/doc/publicSchema/mutation.doc.html)

### Types

* scalars, enums, interfaces, objects unions, and input objects.


## Examples

### Query

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