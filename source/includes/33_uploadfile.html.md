## Upload a sales invoice through a mutation

> The mutation:

```graphql
mutation upload($vatnumber: String!, $filename: String!, $invoicetype: InvoiceTypeArgument!) {
 uploadFile(vatnumber: $vatnumber, filename: $filename, invoicetype: $invoicetype) { 
   uuid,
   amountOfPages
 } 
}
```

> The variables:

```json
{
  "vatnumber": "0123123123", 
  "filename": "test_upload.pdf", 
  "invoicetype": "SALE"
}
``` 

In this example we call a mutation with the goal of uploading a file for a specific administration.
Just like our previous example, we will provide arguments to determine the administration, as well as the name and the type of the file.
The uploaded file will in turn provide us with an ID, the filename and the detected amount of pages.

The way in which we send this mutation differs a bit from regular queries. Since we have to provide a file as data, we 
will send our information as form-data instead of a text-based content-type like application/json or application/graphql.

The file must be present as a parameter named "file", while the mutation and arguments can be provided as regular text-based 
content in the form, respectively called "query" and "variables".

Unfortunately due to the nature of the GraphQL Playground, we can not provide you with an example, as they do not 
allow or provide an option for a user to enter form-data or upload files.  Instead we'll give you a cURL example you can
use on the command line.

<aside class="notice">
Supported file types are PDF's (application/pdf MIME type), images (image/jpeg) and UBL-files ('application/xml').<br/>
UBL-files must meet the the <a href="https://docs.peppol.eu/poacc/billing/3.0/" target="_blank">Billing3 standard</a> or the Belgian variant <a href="https://www.ubl.be" target="_blank">UBL.BE</a>.<br/>
Additionally, we still support the legacy E-FFF format.
</aside>

> Uploading a file using cURL:

```curl
curl -X POST \
  https://api.clearfacts.be/graphql \
  -H 'Authorization: Bearer <token>' \
  -F 'query=mutation upload($vatnumber: String!, $filename: String!, $invoicetype: InvoiceTypeArgument!) {
  uploadFile(vatnumber: $vatnumber, filename: $filename, invoicetype: $invoicetype) { 
      uuid,
      amountOfPages
    } 
  }' \
  -F 'variables={ "vatnumber": "0123123123", "filename": "test_upload.pdf", "invoicetype": "SALE"}' \
  -F file=@/home/user/Documents/example.pdf
```
