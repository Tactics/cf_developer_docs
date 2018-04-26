## Querying an associate and editing it

```graphql
query {
    associates {
        id,
        firstName,
        lastName,
        email
    }
}
```

> will result in

```json
{
    "data": {
        "associates": [
            {
                "id": "198772ef-d095-4859-9328-dc47950853b9",
                "firstName": "Test",
                "lastName": "Client",
                "email": "test@clearfacts.be"
            }
        ]
    }
}
```

In this use case we modify an existing associate.
First, we query the existing associates to find out their ID.
Upon executing this query in a real scenario you'll end up with a result set of all the associates for your accountant.
For the sake of simplicity we've kept the result set for this example short and only show the one that will be of use to us.

```graphql
mutation edit($id: ID!, $associate: EditAssociateArgument!){
  editAssociate(id: $id, associate: $associate) {
    id,
    firstName,
    lastName,
    email
  }
}
```
```json
{
    "id": "198772ef-d095-4859-9328-dc47950853b9",
    "associate": {
        "firstName": "Updated"
    }
}
```

> will result in

```json
{
    "data": {
        "editAssociate": {
            "id": "198772ef-d095-4859-9328-dc47950853b9",
            "firstName": "Updated",
            "lastName": "Client",
            "email": "test@clearfacts.be"
        }
    }
}
```

Use the ID of the associate in the ```editAssociate``` mutation.
It's not required to pass the complete associate object when editing.
Only the parameters that you provide will get updated, the others will retain their value.
Consider it 'patching' of the associate object.