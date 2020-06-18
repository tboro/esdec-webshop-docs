# Entering the calculator II

In this section the OAuth2 approach is described. This integration allows the webshop user to confirm their identity instead of having to create an account manually.

## Prerequisites

Both webshop and Esdec calculator require known client_id & client_secret pair. For the purpose of this documentation following values will be used:

Parameter     | Value
------------- | ---------------------
client_id     | amsterdamstandard.com
client_secret | ThisMustBeASecretValue
vendor-uuid   | 492d0cef-5427-4ed8-b12d-169ef39dd871

A `vendor-uuid` value will be delivered by Esdec - it is an internal key of the webshop integration.

## Required endpoints

As the webshop is the OAuth2 identity provider, it is expected there are 3 endpoints available:

### Request token provider

> Open following link in a browser:

```shell
https://webshop.com/oauth2/authorize?client_id=amsterdamstandard.com&redirect_uri=https://esdec.app.amsdard.io/?vendor-uuid=492d0cef-5427-4ed8-b12d-169ef39dd871&state=oauth2&response_type=code
```

> After user confirmation, you will be redirected to following URL:

```
https://esdec.app.amsdard.io/?vendor-uuid=492d0cef-5427-4ed8-b12d-169ef39dd871&code=5df345a5cc3a1e73f6a2085d4e78d42dcc564043&state=oauth2
```

> where `code` parameter value is the token used for requesting access token.

#### HTTP Request

`GET https://webshop.com/oauth2/authorize`

#### Query Parameters

Parameter | Type | Description
--------- | ---- | -----------
client_id | string | Prerequisite value
redirect_uri | string | Calculator URL to redirect after the authorization is confirmed
state | string | Always 'oauth2'
response_type | string | Always 'code'

### Authorization token provider

> Retrieve access token:

```shell
curl -X POST \
-H "Authorization: Basic YW1zdGVyZGFtc3RhbmRhcmQuY29tOlRoaXNNdXN0QmVBU2VjcmV0VmFsdWU=" \
-H "Content-Type: application/x-www-form-urlencoded" \
--data "grant_type=authorization_code&code=5df345a5cc3a1e73f6a2085d4e78d42dcc564043&redirect_uri=https://esdec.app.amsdard.io/?vendor-uuid=492d0cef-5427-4ed8-b12d-169ef39dd871&state=oauth2" \
"https://webshop.com/oauth2/token"
```

> Minimal response

```json
{
  "access_token": "1e3f5dda78695d070877aa0b21ccebdab88b50980e6e61618e76d22821c5d75e"
}
```

#### HTTP Request

`POST https://webshop.com/oauth2/token`

Following extra headers are required:

* Basic authorization - based on `client_id` & `client_secret` pair
* Content Type - POST params are URLencoded

#### POST Parameters

Parameter | Type | Description
--------- | ---- | -----------
grant_type | string | Always 'authorization_code'
code | string | Request token code received from the browser redirect
redirect_uri | string | Calculator URL to redirect after the authorization is confirmed

### Identity provider

> Get user identity for access token:

```shell
curl "https://webshop.com/oauth2/resource/identity?access_token=1e3f5dda78695d070877aa0b21ccebdab88b50980e6e61618e76d22821c5d75e"
```

> Minimal response

```json
{
  "email": "da39a3ee5e6b4b0d3255bfef95601890afd80709@example.com"
}
```

This endpoint should provide e-mail address (and/or other data about the customer).

#### HTTP Request

`GET https://webshop.com/oauth2/resource/identity`

The token received  in the previous call will be passed to the endpoint in one of 2 ways:

* request header
```
Authorization: Bearer <access_token>
```

* GET parameter "access_token"

<aside class="notice">
Both variants cannot be used at the same time within the same vendor. The second identity request variant (token passed as query parameter access_token) can be enabled in the vendor settings.
</aside>

<aside class="notice">
The e-mail might or might not be exposed directly - this depends on the integration agreement with Esdec. At least a unique e-mail hash per-customer is required.
</aside>


## Embedding the flow

It is enough that the webshop embeds the `Token request provider` URL where suitable. Please contact us providing `client_id` & `client_secret` values, and base URLs of all of the 3 endpoints described in this chapter.

<aside class="notice">
If you find this guide confusing, refer to: <a href="https://aaronparecki.com/oauth-2-simplified/">Aaron Parecki's blog post</a> where a similar approach is explained.
</aside>
