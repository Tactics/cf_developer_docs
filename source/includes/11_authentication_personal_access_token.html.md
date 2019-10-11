## Personal Access Token
A personal access token is created manually and is typically used by offline or desktop applications.

This means it's an easy way to get a token for talking to the API using command line tools, eg while exploring the API's capabilities.

<aside class="notice">
    Currently only accountant users can create personal access tokens.<br/>
    Functionality for SME users to create these tokens will be added when a valid use case is presented. 
</aside>

Personal access tokens are always exactly 80 ASCII characters (or bytes) long.

### Creating an personal access token

To create a personal access token, you need to follow these steps:

1. Log in as an accountant user in the accountant module.
2. In the top right corner, click on your profile and select "integrations".
3. Click on the button "New token".
4. Give your token a descriptive name.
5. Select the scopes you'd like to grant your token.
6. Click the "save" button.
7. The newly created token is displayed in a message box.  Make sure to copy it, as it is only displayed once.

<aside class="warning">
Tokens are like passwords, make sure to treat them in the same way and keep them safe.
If you lose your token, or believe it might have been compromised, **delete it** and create a new one.
</aside>


### Testing your token on the command line

> To get information on the user of the provided token

```shell
curl -H "Authorization: Bearer <token>" -X GET https://login.clearfacts.be/oauth2-server/userinfo
```

```php
// todo
```

You can use a simple CURL request to test your token 
and information about the user for whom the token was created.

> This will return:

Each entry in the returned JSON structure is called a claim.  
You'll find more information about this in the chapter on [Scopes and Claims](#scopes-and-claims). 

```json
{
  "sub":"user@domain.com",
  "username":"user@domain.com",
  "email":"user@domain.com"
}
```


