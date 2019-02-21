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

You can get more information about a document using this query.
In this case we would like to know the date, type and payment state of an invoice.

The date field is part of the Document interface, so you can query the field like you normally would.
The type and paymentState however are fields of the implementation InvoiceDocument.
To query these fields we need to use inline fragments (... on InvoiceDocument {type, paymentState}). 
