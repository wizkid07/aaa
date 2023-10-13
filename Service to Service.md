## Summary

- API keys are often used to authenticate service-to-service API calls. A signed or encrypted JWT is an effective API key. When used to authenticate a client, this is known as JWT bearer authentication.
    
- OAuth2 supports service-to-service API calls through the client credentials grant type that allows a client to obtain an access token under its own authority.
    
- A more flexible alternative to the client credentials grant is to create service accounts which act like regular user accounts but are intended for use by services. Service accounts should be protected with strong authentication mechanisms because they often have elevated privileges compared to normal accounts.
    
- The JWT bearer grant type can be used to obtain an access token for a service account using a JWT. This can be used to deploy short-lived JWTs to services when they start up that can then be exchanged for access and refresh tokens. This avoids leaving long-lived, highly-privileged credentials on disk where they might be accessed.
    
- TLS client certificates can be used to provide strong authentication of service clients. Certificate-bound access tokens improve the security of OAuth2 and prevent token theft and misuse.
    
- Kubernetes includes a simple method for distributing credentials to services, but it suffers from some security weaknesses. Secret vaults and key management services provide better security but need an initial credential to access. A short-lived JWT can provide this initial credential with the least risk.
    
- When service-to-service API calls are made in response to user requests, care should be taken to avoid confused deputy attacks. To avoid this, the original user identity should be communicated to backend services. The phantom token pattern provides an efficient way to achieve this in a microservice architecture, while OAuth2 token exchange and macaroons can be used across trust boundaries.

## API keys and JWT bearer authentication

One of the most common forms of service authentication is an API key, which is a simple bearer token that identifies the service client. An API key is very similar to the tokens you’ve used for user authentication in previous chapters, except that an API key identifies a service or business rather than a user and usually has a long expiry time.

![[Pasted image 20231003190037.png]]


To gain access to an API, a representative of the organization logs into a developer portal and requests an API key. The portal generates the API key and returns it. The developer then includes the API key as a query parameter on requests to the API.

An API key is a token that identifies a service client rather than a user. API keys are typically valid for a much longer time than a user token, often months or years.

In JWT bearer authentication, a client gains access to an API by presenting a JWT that has been signed by an issuer that the API trusts.

An advantage of JWTs over simple database tokens or encrypted strings is that you can use public key signatures to allow a single developer portal to generate tokens that are accepted by many different APIs. 

Only the developer portal needs to have access to the private key used to sign the JWTs, while each API server only needs access to the public key. Using public key signed JWTs in this way is covered in section 7.4.4, and the same approach can be used here, with a developer portal taking the place of the AS.


## The OAuth2 client credentials grant

This grant type allows an OAuth2 client to obtain an access token using its own credentials without a user being involved at all. The access token issued by the authorization server (AS) can be used just like any other access token, allowing an existing OAuth2 deployment to be reused for service-to-service API calls.

 If an API accepts calls from both end users and service clients, it’s important to make sure that the API can tell which is which. Otherwise, users may be able to impersonate service clients or vice versa. The OAuth2 standards don’t define a single way to distinguish these two cases, so you should consult the documentation for your AS vendor.


To obtain an access token using the client credentials grant, the client makes a direct HTTPS request to the token endpoint of the AS, specifying the `client_credentials` grant type and the scopes that it requires. The client authenticates itself using its own credentials. OAuth2 supports a range of different client authentication mechanisms, and you’ll learn about several of them in this chapter. The simplest authentication method is known as `client_secret_basic`, in which the client presents its client ID and a secret value using HTTP Basic authentication.[1](https://learning.oreilly.com/library/view/api-security-in/9781617296024/OEBPS/Text/11.htm#pgfId-1220164) For example, the following curl command shows how to use the client credentials grant to obtain an access token for a client with the ID `test` and secret value `password`:


```
$ curl -u test:password \                                ❶
  -d 'grant_type=client_credentials&scope=a+b+c' \       ❷
  https://as.example.com/access_token
```

❶ Send the client ID and secret using Basic authentication.

❷ Specify the client_credentials grant.

Assuming the credentials are correct, and the client is authorized to obtain access tokens using this grant and the requested scopes, the response will be like the following:

```
{
  "access_token": "q4TNVUHUe9A9MilKIxZOCIs6fI0",
  "scope": "a b c",
  "token_type": "Bearer",
  "expires_in": 3599
}
```

OAuth2 client secrets are not passwords intended to be remembered by users. They are usually long random strings of high entropy that are generated automatically during client registration.

The OAuth2 spec advises AS implementations not to issue a refresh token when using the client credentials grant. This is because there is little point in the client using a refresh token when it can obtain a new access token by using the client credentials grant again.

### Service accounts

A service account is an account that identifies a service rather than a real user. Service accounts can simplify access control and account management because they can be managed with the same tools you use to manage users.

An authorization server (AS) typically stores client details in a private database, so these details are not accessible to APIs. A service account lives in the shared user repository, allowing APIs to look up identity details such as role or group membership.

![[Pasted image 20231003191408.png]]

 A service account acts like a regular user account and is created in a central directory and assigned permissions and roles just like any other account. This allows APIs to treat an access token issued for a service account the same as an access token issued for any other user, simplifying access control. It also allows administrators to use the same tools to manage service accounts that they use to manage user accounts. Unlike a user account, the password or other credentials for a service account should be randomly generated and of high entropy, because they don’t need to be remembered by a human.


For a service account, the client instead uses a non-interactive grant type that allows it to submit the service account credentials directly to the token endpoint. The client must have access to the service account credentials, so there is usually a service account dedicated to each client. The simplest grant type to use is the Resource Owner Password Credentials (ROPC) grant type, in which the service account username and password are sent to the token endpoint as form fields

```
$ curl -u test:password \                       ❶
  -d 'grant_type=password&scope=a+b+c' \
  -d 'username=serviceA&password=password' \    ❷
  https://as.example.com/access_token
```

❶ Send the client ID and secret using Basic auth.

❷ Pass the service account password in the form data.

This will result in an access token being issued to the `test` client with the service account `serviceA` as the resource owner.

The ROPC grant type may be deprecated or removed in future versions of OAuth.

## The JWT bearer grant for OAuth2

![[Pasted image 20231003195207.png]]

### Service account authentication

Authenticating a service account using JWT bearer authentication works a lot like client authentication. Rather than using the client credentials grant, a new grant type named

    urn:ietf:params:oauth:grant-type:jwt-bearer


❶ Use the jwt-bearer grant type.

❷ Pass the JWT as the assertion parameter.

The claims in the JWT are the same as those used for client authentication, with the following exceptions:

- The `sub` claim should be the username of the service account rather than the client ID.
    
- The `iss` claim may also be different from the client ID, depending on how the AS is configured.

## Service API calls in response to user requests

When a service makes an API call to another service in response to a user request, but uses its own credentials rather than the user’s, there is an opportunity for confused deputy attacks like those discussed in chapter 9. Because service credentials are often more privileged than normal users, an attacker may be able to trick the service to performing malicious actions on their behalf.

### The phantom token pattern

In the phantom token pattern, a long-lived opaque access token is validated and then replaced with a short-lived signed JWT at an API gateway. Microservices behind the gateway can examine the JWT without needing to perform an expensive introspection request.


- If the access token requires introspection to check validity, then a network call to the AS has to be performed at each microservice that is involved in processing a request. This can add a lot of overhead and additional delays.
    
- On the other hand, backend microservices have no way of knowing if a long-lived signed token such as a JWT has been revoked without performing an introspection request.
    
- A compromised microservice can take the user’s token and use it to access other services, effectively impersonating the user. If service calls cross trust boundaries, such as when calls are made to external services, the risk of exposing the user’s token increases.

![[Pasted image 20231004134438.png]]


A network roundtrip within the same datacenter takes a minimum of 0.5ms plus the processing time required by the AS (which may involve database network requests). Verifying a public key signature varies from about 1/10th of this time (RSA-2048 using OpenSSL) to roughly 10 times as long (ECDSA P-521 using Java’s SunEC provider). Verifying a signature also generally requires more CPU power than making a network call, which may impact costs.

Prefer using opaque access tokens and token introspection when tokens cross trust boundaries to ensure timely revocation. Use self-contained short-lived tokens for service calls within a trust boundary, such as between microservices.