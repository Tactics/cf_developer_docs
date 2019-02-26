## Scopes and claims

The OAuth2 protocol is a delegated authorization mechanism, where a client application requests 
access to resources controlled by the user (the resource owner) and hosted by an API 
(the resource server) and the authorization server typically issues the client application a more restricted 
set of credentials than those of the user.
 
You can use scopes to:

 * Authenticate users and get additional information about them such as their email or name (Type in the table below: OpenID)

 * Request granular access control to the API. (Type: API)

### Scopes

This is a list of all the supported scopes in our API:

Scope                | Type   | Description
---------------------|--------|------------------------
`openid`               | OpenID | This is a technical scope, it should always be requested when using the OpenID Connect flow, it also allows access to a unique identification of the user (the `sub` claim)
`email`                | OpenID | Access to a users emailaddress (via the ``/userinfo`` endpoint)
`profile`              | OpenID | Access to a users name, locale
`read_administrations` | API    | Allows the client to query the list of administrations the user has access to
`upload_document`      | API    | Allows the client to upload documents and invoices for the administrations the user has access to
`archive`              | API    | A complete archive scope that grants access to both `archive_read` and `archive_actions`
`archive_read`         | API    | Allows the client to query the list of available archive categories
`archive_actions`      | API    | Allows the client to upload files to a specific administration's archive category
`associate_read`       | API    | Allows the client to query all information associated with associates and groups 
`associate_actions`    | API    | Allows the client to modify the information of associates, as well as creating new ones
`statistics`           | API    | Allows the client to query office statistics

Each token, be it a personal access token or a token acquired through OpenID Connect, 
should have been granted one or more scopes for it to be of any use. 

If you are using OpenID Connect, you are required to request the `openid` scope.

### Claims
OpenID Connect specifies a set of standard claims. These claims are user attributes and are intended to provide 
the client with user details such as email, name and locale.

The basic claim returned for the `openid` scope is the `sub` claim, which uniquely identifies the user 
(`iss`, `aud`, `exp`, `iat` and `at_hash` claims will also be present in the id_token). 

Requesting individual claims is not supported in our OIDC implementation.
Access to claims is regulated through scopes.
Applications can ask for additional scopes, separated by spaces, 
to request more information about the user.

The `/userinfo` endpoint will return a json object with a few "OpenID Claims", depending on the scopes that have been granted to your token.


Claim        | Type       | Required scope | Description
------------ | ---------- | -------------- | ----------------------------------------- 
`username`	 | string     | openid         | The username as used in ClearFacts. 
`sub`          | string     | openid         | Subject-identifier for the user in the ClearFacts.  It contains the same value as the 'sub' claim.
`name`	     | string     | profile        | User's full name in displayable form including all name parts, possibly including titles and suffixes, ordered according to the user's locale and preferences.
`given_name`   | string	  | profile        | Given name(s) or first name(s) of the user. 
`family_name`  | string     | profile        | Surname(s) or last name(s) of the user.
`preferred_username` | string | profile      | Shorthand name by which the user wishes to be referred to. 
`email`	     | string     | email          | The user's e-mail address. *Note*: in most cases the username equals the email address. 
`locale`       | string     | profile        | The user's locale, represented as a BCP47 [RFC5646] language tag. This is typically an ISO 639-1 Alpha-2 [ISO639‑1] language code in lowercase and an ISO 3166-1 Alpha-2 [ISO3166‑1] country code in uppercase, separated by a dash. For example, nl-BE.

Although claims are a part of the OpenID Connect specification, it's still possible to request the `/userinfo` with a personal access token with the right scopes.

### Granting and revoking scopes

*Personal Access Token*
In case of a personal access token, the user can select which scopes to grant.
It's possible to grant or revoke scopes later on by editing or deleting the personal access token in the application.

*OpenID Connect Token*
When using OpenID Connect, tokens are issued to the client application any time the client application requests a token *and* the users approves.
This process is typically hidden from the user and this it's not possibe to change the scopes for any existing token.

If a user wants to revoke rights from a client application he will need to remove the client application from within ClearFacts.
At that point all tokens for this client application will be revoked at once.

<aside class="notice">
    Revoking access from a client application is not yet supported.
</aside>



