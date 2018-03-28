### Creating an associate with associate groups

```graphql
query { 
    associateGroups { 
        id, 
        name, 
        email 
    } 
}
```

> will result in:

```json
{
    "data": {
        "associateGroups": [
            {
                "id": "16d5379f-30cb-11e8-987b-000c2985a2fd",
                "name": "Group 1",
                "email": "group1@clearfacts.be"
            },
            {
                "id": "16d53ea9-30cb-11e8-987b-000c2985a2fd",
                "name": "Group 2",
                "email": "group2@clearfacts.be"
            }
        ]
    }
}
```

In this example we will create an associate and assign them to an associate group. 
The first step of the process is to look for which associate groups are available,
and use one of their IDs to link the associate.

Here we will use the first group returned.

```graphql
mutation add($associate: AddAssociateArgument!) {
    addAssociate(associate: $associate) {
        id,
        firstName,
        lastName,
        email,
        associateGroups {
            id,
            name,
            email
        },
        plainPassword
    }
}
```
```json
{
    "associate": {
        "firstName": "Test",
        "lastName": "Client",
        "email": "test@clearfacts.be",
        "type": "ADMIN",
        "associateGroups": [
            {
                "id": "16d5379f-30cb-11e8-987b-000c2985a2fd"
            }
        ],
        "active": true,
        "language": "nl_BE",
        "sendActivationMail": false
    }
}
```

> will result in:

```json
{
    "data": {
        "addAssociate": {
            "id": "198772ef-d095-4859-9328-dc47950853b9",
            "firstName": "Test",
            "lastName": "Client",
            "email": "test@clearfacts.be",
            "associateGroups": [
                {
                  "id": "16d5379f-30cb-11e8-987b-000c2985a2fd",
                  "name": "Group 1",
                  "email": "group1@clearfacts.be"
                }
            ],
            "plainPassword": "FaKePaSsWord"
        }
    }
}
```

Upon completion of the query, the associate will have been created and the one-time generated password will be available for use.