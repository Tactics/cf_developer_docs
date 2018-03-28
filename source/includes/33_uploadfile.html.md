### Upload a sales invoice through a mutation

```graphql
mutation upload($vatnumber: String!, $filename: String!, $invoicetype: InvoiceTypeArgument!) {
 uploadFile(vatnumber: $vatnumber, filename: $filename, invoicetype: $invoicetype) { 
   id,
   amountOfPages
 } 
}
```

> will result in:

```json
{"variables": { "vatnumber": "0123123123", "filename": "test_upload.pdf", "invoicetype": "SALE"}}
``` 

In this example we call a mutation with the goal of uploading a file for a specific administration.
Just like our previous example, we will provide arguments to determine the administration, as well as the name and the type of the file.
The uploaded file will in turn provide us with an ID, the filename and the detected amount of pages.

The way in which we send this mutation differs a bit from regular queries. Since we have to provide a file as data, we 
will send our information as form-data instead of a text-based content-type like application/json or application/graphql.

The file must be present as a parameter named "file", while the mutation and arguments can be provided as regular text-based 
content in the form, respectively called "query" and "variables".

Unfortunately due to the nature of the GraphQL Playground, we can not provide you with an example, as they do not 
allow or provide an option for a user to enter form-data or upload files.

Instead we provide you with a cURL example.

```curl
curl -X POST \
  http://api.clearfacts.be/graphql \
  -H 'authorization: <token>' \
  -H 'content-type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW' \
  -F 'query=mutation upload($vatnumber: String!, $filename: String!, $invoicetype: InvoiceTypeArgument!) {
             uploadFile(vatnumber: $vatnumber, filename: $filename, invoicetype: $invoicetype) { 
               uuid,
               amountOfPages
             } 
            }' \
  -F 'variables= { "vatnumber": "0123123123", "filename": "test_upload.pdf", "invoicetype": "SALE"}' \
  -F file=@/home/user/Documents/example.pdf
```