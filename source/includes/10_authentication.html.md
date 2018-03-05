# Authentication

To use our API you need to authenticate with a valid API token with the right scopes.  

When using such a token you will be able to act on behalf of the ClearFacts user that created the 
token or allowed for the token to be created through an Open ID Connect flow.

A token can be acquired in two ways, depending on your use case:

* **Personal access token** — It's possible to create a token manually using the web application.
* **Open Connect Id** — This allows users to grant your application access.


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

> Will return a json document with a number of claims, depending on the requested scopes.

```json
{
  "sub":"user@domain.com",
  "username":"user@domain.com",
  "email":"user@domain.com"
}
```

This will return a json object with a few "OpenID Claims", depending on the scopes that have been granted to your token.


Claim        | Type       | Description
------------ | ---------- | --------------
sub          | string     | Subject - Identifier for the user at the ClearFacts.
username	 | string     | User's username as used in ClearFacts. 
name	     | string     | User's full name in displayable form including all name parts, possibly including titles and suffixes, ordered according to the user's locale and preferences.
given_name   | string	  | Given name(s) or first name(s) of the user. 
family_name  | string     | Surname(s) or last name(s) of the user.
preferred_username | string | Shorthand name by which the user wishes to be referred to. 
email	     | string     | user's preferred e-mail address. 
locale       | string     | user's locale, represented as a BCP47 [RFC5646] language tag. This is typically an ISO 639-1 Alpha-2 [ISO639‑1] language code in lowercase and an ISO 3166-1 Alpha-2 [ISO3166‑1] country code in uppercase, separated by a dash. For example, nl-BE.

For all but the sub, username and email claims, the 'profile' scope must have been granted.

## Open Connect Id

If you're an app developer and want to enable your users to grant your application access implementing OpenId Connect (OIDC) is the way to go.

Our OAuth2 implementation is based on Open Connect ID.
OpenID Connect 1.0 is a simple identity layer on top of the OAuth 2.0 protocol. It allows Clients to verify the identity of the user based on the authentication performed by an Authorization Server, as well as to obtain basic profile information about the user in an interoperable and REST-like manner.

Building a secure OpenID Connect client is hard, so you should use an existing and proven implementation in the language of your choice.

More information can be found here:

* [An intro on Open ID Connect - http://openid.net/connect/](http://openid.net/connect/)
* [A list of client libraries - http://openid.net/developers/libraries/](http://openid.net/developers/libraries/)


### Application registration
In order to use Open ID Connect, your app needs to be registered with ClearFacts.
Currently this is a manual process, so please contact us via email or phone to set things up.

You will need to supply us with your OIDC redirect URI and receive a client identifier and secret after you registration has been completed.

### (Auto)configuration
The base URL is :  xxxxx

> To get the OpenID Connect configuration:

```shell
curl -X GET https://api.clearfacts.be/.well-known/openid-configuration
```

OIDC has a way to configure clients automatically through autoconfiguration.  
The URL for this is defined by the OIDC spec, but can be inspected manually if you'd like to see which features are supported in our implemenation.
 
```json
{
	"issuer": "https://acc.clearfacts.be",
	"authorization_endpoint": "https://acc.clearfacts.be/oauth2-server/authorize",
	"token_endpoint": "https://acc.clearfacts.be/oauth2-server/token",
	"userinfo_endpoint": "https://acc.clearfacts.be/oauth2-server/userinfo",
	"jwks_uri": "https://acc.clearfacts.be/oauth2-server/jwks.json",
	"response_types_supported": ["code", "token"],
	"subject_types_supported": ["public"],
	"id_token_signing_alg_values_supported": ["RS256"],
	"scopes_supported": ["openid", "profile", "email"]
}
```
 
[https://acc.clearfacts.be/.well-known/openid-configuration](https://acc.clearfacts.be/.well-known/openid-configuration)

### OIDC Token

The token that you'll receive using OIDC is not a random byte string, but a signed JWT token that can be decoded (see: [https://jwt.io/](https://jwt.io/))
The JWT token contains the basic claims (sub, username, email) as well as the actual 80 character access token like you would get when creating a personal access token.

Both the longer JWT token as the 80 character token can be used in the `Authentication: Bearer <token>` header.  
This means you can store either one.


## Scopes

### Granting and revoking scopes

*Personal Access Token*
In case of an personal access token, the user can select which scopes to grant.
It's possible to grant or revoke scopes later on by editing the personal access token in the application.

*OIDC Token*
It's not possibe to change the scopes for an existing token.  If a user wants to revoke rights he needs to remove the application.



The following scopes may be requested or 

Scope                | Description
---------------------|--------------------------------
openid               | Technical scope, should always be requested to identify an OIDC request, it allows access to the username
email                | Access to a users emailaddress (via the ``/userinfo`` endpoint 
profile              | Access to a users name, locale
administrations      | Kak
...

