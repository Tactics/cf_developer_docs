## Creating an associate with associate groups

```graphql 
query {
    associateGroups { 
        id, 
        name, 
        email 
    } 
}
```

> returns the following list:

```json
{
    "data": {
        "associateGroups": [
            {
                "id": "16d5379f-30cb-11e8-987b-000c2985a2fd",
                "name": "Group 1",
                "email": "group1@clearfacts.be"
            }
        ]
    }
}
```

In this example we create an associate and assign him to an associate group. 
The first step of the process is to get a list available associate groups so we can
figure out the ID of the group the associate to add to.

In the example we'll use the first group returned.

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

> will return the properties of the new associate together with a generated password

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
            "plainPassword": "ZE3qa2vqizu4"
        }
    }
}
```

Upon completion of the query, the associate has been created and he will be able to access the system with the generated password.

<aside class="warning">
This is the only time you get access to the users password.  ClearFacts does not store passwords in there orginal format so it will not be possible to query for it later.
We recommend not storing the password at all, but if you really need it for later use, make sure to <strong>store the password safely</strong>
</aside>

<aside class="notice">
When an accountant has set up a SSO integration between ClearFacts and an an external identity provider, this provided password will be of no use.
</aside>