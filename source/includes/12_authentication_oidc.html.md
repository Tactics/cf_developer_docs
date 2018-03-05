## OpenId Connect

If you're an app developer and want to enable your users to grant your application access to the API, 
implementing OpenId Connect (OIDC) is the way to go.

OpenID Connect 1.0 is a simple identity layer on top of the OAuth 2.0 protocol. 
It allows Clients to verify the identity of the user based on the authentication performed by 
an Authorization Server, as well as to obtain basic profile information about the user in 
an interoperable and REST-like manner.

Building a secure OpenID Connect client is difficult, so you should use an existing and 
proven implementation in the language of your choice.

For more information check these links:

* [An intro on OpenID Connect - http://openid.net/connect/](http://openid.net/connect/)
* [A list of client libraries - http://openid.net/developers/libraries/](http://openid.net/developers/libraries/)


### Application registration
In order to use OpenID Connect, your app needs to be registered with ClearFacts.
Currently this is a manual process, so please contact us via email or phone to set things up.

You will need to supply us with your OIDC redirect URI and receive a client identifier and 
secret after you registration has been completed.

### Auto Discovery
> To get the OpenID Connect configuration:

```shell
curl -X GET https://api.clearfacts.be/.well-known/openid-configuration
```

OIDC has a way to configure clients automatically through a Discovery Document URL.  
The URL for this is defined by the OIDC spec and can be inspected manually 
if you'd like to see which features are supported in our implementation.
 
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


### Authorisation code flow

OpenID connect (or OAuth2) supports several flows to obtain an access token. 

Currently we only support the "Authorization Code Flow".  This is the most commonly used and most secure flow,
but it cannot be used by client-side only apps like javascript apps without a back-end.

The Authorization Code Flow goes through the following steps:

1. Client prepares an Authentication Request containing the desired request parameters.
2. Client sends the request to the Authorization Server.
3. Authorization Server Authenticates the End-User.
4. Authorization Server obtains End-User Consent/Authorization.
5. Authorization Server sends the End-User back to the Client with an Authorization Code.
6. Client requests a response using the Authorization Code at the Token Endpoint.
7. Client receives a response that contains an ID Token and Access Token in the response body.
8. Client validates the ID token and retrieves the End-User's Subject Identifier.

It is beyond the scope of this document to get into more detail.

### OIDC Token

The token that you'll receive using OIDC is not a random byte string, but a signed JWT token that can be decoded 
(see: [jwt.io](https://jwt.io/)). 
The JWT token contains the basic claims (sub, username, email) as well as the
80 character access token like you would get when creating a personal access token.

Both the longer JWT token and the 80 character token can be used in the `Authentication: Bearer <token>` header.  
This means you can store either one depending on your use case.


