## Query an invoice document

```graphql
query doc {
  document(id: "6bf58d1c-0952-44bd-8d95-1dd0307fad25") {
	date, 
	... on InvoiceDocument {
	  type, 
	  paymentState
    }
  }
}
```
> this will return details of the document:

```json

{
 "data": {
    "document": {
      "date": "2016-10-06",
      "type": "PURCHASE",
      "paymentState": "UNPAID"
    }
  }
}

```

You can query a document for extra information.