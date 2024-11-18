# Obtaining credentials

## Client ID and Secret

To obtain a client ID and secret, log in in the [UI](https://openbadges.education/auth/login). You can then navigate to *Konto -> App Integration* (or go the [link](https://openbadges.education/profile/app-integrations) directly). via the *Neue Credentials generieren* button you can then generate your credentials.

The credentials consist of a **Client ID** and a **Client Secret**. You should write down your client secret because you won't be able to view it at a later point in time.
With those credentials you and other third party applications are able to obtain access to our API and thus do everything that our UI can do.
This is **important**, especially because the scope of the returned access token in the next step might indicate something else.
So again:
> With these credentials, everyone can act in the name of you and access, modify and delete all the information that is stored about you in our system.

## Access Token

If you want to make requests to our API, you need to obtain an **Access token**.
These are tokens with a limited time of life (24 hours by default). To request tokens, you need to make a POST request to our API.
For security reasons in our application we store the access token in an [HttpOnly](https://owasp.org/www-community/HttpOnly) cookie.
That means that the browser cannot access the content, instead it's passed with the requests as a cookie.
That also means that we don't return the access token in the data section of the response, but in the cookie section.

If you use `cURL` for example this might look like this:
```bash
curl --request POST \
    --url 'https://api.openbadges.education/o/token' \
    --header 'content-type: application/x-www-form-urlencoded' \
    --data 'grant_type=client_credentials' \
    --data 'client_id=YOUR_CLIENT_ID' \
    --data 'client_secret=YOUR_CLIENT_SECRET' --verbose
```
The response will then look something like this:
```text
Note: Unnecessary use of -X or --request, POST is already inferred.
* <a lot of verbose messages that aren't relevant>
> POST /o/token HTTP/1.1
> Host: api.openbadges.education
> User-Agent: curl/7.88.1
> Accept: */*
> content-type: application/x-www-form-urlencoded
> Content-Length: 102
> 
* TLSv1.3 (IN), TLS handshake, Newsession Ticket (4):
* We are completely uploaded and fine
< HTTP/2 200 
< access-control-allow-origin: *
< cache-control: no-store
< content-type: application/json
< date: Mon, 18 Nov 2024 13:22:27 GMT
< pragma: no-cache
< server: nginx/1.25.2
< vary: Authorization, Cookie, Origin,Origin
< x-frame-options: DENY
< Content-Length: 67
< Set-Cookie:  access_token=YOUR_ACCESS_TOKEN; expires=Tue, 19 Nov 2024 13:19:00 GMT; HttpOnly; Max-Age=86400; Path=/; Secure
< 
* Connection #0 to host api.openbadges.education left intact
{"expires_in": 86400, "token_type": "Bearer", "scope": "r:profile"}
```
Once again, note that the scope doesn't actually mean anything (yet).
You can read the access token from the `Set-Cookie` value.

## With username and password

For the curious among you, there is a second way to obtain the access token.
This is also the way that our application does it (and the way it's documented in the API docs): Via username and password.
For that you don't need a client ID and Secret, but the username and password of your user:
```bash
curl --request POST \
    --url 'https://api.openbadges.education/o/token' \
    --header 'content-type: application/x-www-form-urlencoded' \
    --data 'grant_type=password' \
    --data 'username=YOUR_USERNAME' \
    --data 'password=YOUR_PASSWORD' \
    --data 'client_id=public' --verbose
```
