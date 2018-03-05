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


## Scopes and claims

### Scopes

This is a list of all the supported scopes: 

Scope                | Description
---------------------|--------------------------------
openid               | Technical scope, should always be requested to identify an OIDC request, it allows access to the username
email                | Access to a users emailaddress (via the ``/userinfo`` endpoint 
profile              | Access to a users name, locale
administrations      | ...


### Claims
Requesting individual claims is not supported in our OIDC implementation.
Access to claims is regulated through scopes.

The `/userinfo` endpoint will return a json object with a few "OpenID Claims", depending on the scopes that have been granted to your token.


Claim        | Type       | Required scope | Description
------------ | ---------- | -------------- | ----------------------------------------- 
username	 | string     | openid         | The username as used in ClearFacts. 
sub          | string     | openid         | Subject-identifier for the user in the ClearFacts.  It contains the same value as the 'sub' claim.
name	     | string     | profile        | User's full name in displayable form including all name parts, possibly including titles and suffixes, ordered according to the user's locale and preferences.
given_name   | string	  | profile        | Given name(s) or first name(s) of the user. 
family_name  | string     | profile        | Surname(s) or last name(s) of the user.
preferred_username | string | profile      | Shorthand name by which the user wishes to be referred to. 
email	     | string     | email          | The user's e-mail address. *Note*: in most cases the username equals the email address. 
locale       | string     | profile        | The user's locale, represented as a BCP47 [RFC5646] language tag. This is typically an ISO 639-1 Alpha-2 [ISO639‑1] language code in lowercase and an ISO 3166-1 Alpha-2 [ISO3166‑1] country code in uppercase, separated by a dash. For example, nl-BE.

### Granting and revoking scopes

*Personal Access Token*
In case of an personal access token, the user can select which scopes to grant.
It's possible to grant or revoke scopes later on by editing the personal access token in the application.

*OIDC Token*
It's not possibe to change the scopes for an existing token.  If a user wants to revoke rights he needs to remove the application.


