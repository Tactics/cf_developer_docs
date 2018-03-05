## Personal Access Token
A personal access token is created manually and is typically used by offline or desktop applications.

This means you can also use such a token when talking to the API using command line tools.

<aside class="notice">
Currently only accountant users can create personal access tokens.<br/>
Functionality for SME users to create such tokens will be added when a valid use case is presented. 
</aside>

Personal access tokens are always 80 characters long.

### Creating an personal access token

1. Log in as an accountant user in the accountant module.
2. In the top right corner, click on your profile and select "integrations".
3. Click on the button "New token".
4. Give your token a descriptive name.
5. Select the scopes you'd like to grant your token.
6. Click the "save" button.
7. The newly created token is displayed on a message box.  Make sure to copy it, as it is only displayed once.

<aside class="warning">
Tokens are like passwords, make sure to treat them in the same way and keep them safe.
If you loose your token, or believe it might have been compromised, make sure to delete it and create a new one.
</aside>


### Testing your token on the command line

> To get information on user of provided token

```shell
curl -H "Authorization: Bearer <token>" -X GET https://api.clearfacts.be/oauth2-server/userinfo
```

```php
// todo
```

You can use a simple CURL request to test your token 
and information about the user for whom the token was created.

> This will return:

```json
{
  "sub":"user@domain.com",
  "username":"user@domain.com",
  "email":"user@domain.com"
}
```

