## Query the statistics of a company

```graphql
query statistics {
  getCompanyStatistics(type: "AIR", startPeriod: "2015-01-18", endPeriod: "2018-07-18") {
    companyNumber
    items {
      period
      value
    }
  }
}
```
> this will fetch the statistics of AIR usage:

```json
{
  "data": {
    "getCompanyStatistics": [
      {
        "companyNumber": "BE0123123123",
        "items": [
          {
            "period": "06/2018",
            "value": 3
          }
        ]
      }
    ]
  }
}
```

This example queries the statistics of AIR usage.

The getCompanyStatistics supports multiple types, each will have a set of usable parameters.

Types:
   - AIR:
        - startPeriod
        - endPeriod
        - companyNumber     
   - processing:
        - startPeriod
        - endPeriod
        - companyNumber
        - invoiceType
        - worklist

Parameters:
- startPeriod: An RFC-3339 encoded date string. This field is required.
- endPeriod: An RFC-3339 encoded date string. This field is required.
- companyNumber: Company number of the administration whose statistics you want to query. This field is optional. (e.g. BE0123456789)
- invoicetype: Filter statistics based on invoicetype. The invoicetype has to be one of the following: "SALE" or "PURCHASE". This field is
optional. 
- worklist: If true, query statistics for documents processed through worklist. If false, query statistics for documents not processed through
worklist. This field is optional. By default, statistics for all documents are returned.