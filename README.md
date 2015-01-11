# io
elementary's web and cloud account system (via OAuth2)

elementary.io OAuth2 API

## Working Spec

* All endpoints MUST be accessed over TLS

* All 200 responses return JSON

* This API is available over the public Internet, so keep that in mind

Here’s the library we’re using to implement the oauth2 server:

[https://github.com/RangelReale/osin](https://github.com/RangelReale/osin)

Good reading on the subject of oauth2:
[http://alexbilbie.com/2013/02/a-guide-to-oauth-2-grants/](http://alexbilbie.com/2013/02/a-guide-to-oauth-2-grants/)

### Base URL: 

`https://elementary.io/api/`

### Features for MVP:

* Store user information:

* First & Last Name

* Avatar (Gravatar)

* Address (remember International ones may be different)

* Email Address

* Password (hashed)

* Language

* User Roles (only internal for admin purposes)

* Date Joined

* Store payment information via Stripe ([https://stripe.com/docs](https://stripe.com/docs))

    * How?

    * Payments list

* Client credentials grant

* [Authorization code grant](http://tools.ietf.org/html/rfc6749#section-4.1) (Facebook-style)

## API Endpoints:

**`GET /oauth/authoriz**e`

**`URL Parameters**:`

`response_type (required, must be set to `code`)`

`client_id     (required, )`

`scope         (required, )`

`state         (required, used for xsrf protection)`

`redirect_uri  (required, properly escaped http url)`

**`Response: 302 Found (see below**)`

`Authorize successful -> redirect_uri?code=<code>&state=<state>`

`Where code is ...`

`Authorize failed -> redirect_uri?error=<error>&state=<state>`

`Where error is defined in section 4.1.2.1 of RFC6749`

**`Example request**:`

`/api/authorize?response_type=code&client_id=s6BhdRkqt3&state=xyz
        &redirect_uri=https%3A%2F%2Fclient%2Eexample%2Ecom%2Fcb`

**`POST /api/toke**n`

**`Example request**:`

`  POST /api/token HTTP/1.1
  Host: elementary.io
  Authorization: Basic czZCaGRSa3F0MzpnWDFmQmF0M2JW
  Content-Type: application/x-www-form-urlencoded`

`
  grant_type=authorization_code&code=SplxlOBeZQQYbYS6WxSbIA
  &redirect_uri=https%3A%2F%2Fclient%2Eexample%2Ecom%2Fcb`

**`Example response**:`

`HTTP/1.1 200 OK`

`Content-Type: application/json;charset=UTF-8`

`Cache-Control: no-store`

`Pragma: no-cache`

`{`

`"access_token":"2YotnFZFEjr1zCsicMWpAA",`

`"token_type":"bearer",`

`"expires_in":3600,`

`"refresh_token":"tGzv3JOkF0XG5Qx2TlKWIA"`

`}`

**`GET /api/use**r`

**`Description: User data for currently authenticated use**r`

**`Body: {access_token: <token>} as form dat**a`

**`Response: User data as JSON or erro**r`

**`Example successful response body**:`

`{`

`"_id":"5350a88aac1b7fc107073995",`

`"email":"avi@romanoff.me",`

`"is_verified":true,`

`"joined":"2014-04-18T04:20:52.997Z"`

`}`

**`PUT /api/use**r`

**`Description: Update user dat**a`

**`Body: {access_token: <token>} as form dat**a`

**`Response: User data as JSON or erro**r`

**`DELETE /api/use**r`

**`Description: Delete a use**r`

**`Body: {access_token: <token>} as form dat**a`

**`Response: User data as JSON or erro**r`

**`GET /api/users/{username**}`

**`Description: User dat**a`

**`Body: {access_token: <token>} as form dat**a`

**`Response: User data as JSON or erro**r`

**`Example successful response body**:`

`{`

`"_id":"5350a88aac1b7fc107073995",`

`"joined":"2014-04-18T04:20:52.997Z"`

`}`

