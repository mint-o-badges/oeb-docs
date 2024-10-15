# Obtaining credentials

## Client ID and Secret

To obtain a client ID and secret, log in in the [UI](https://openbadges.education/auth/login). You can then navigate to *Konto -> *App Integration* (or go the [link](https://openbadges.education/profile/app-integrations) directly). via the *Neue Credentials generieren* button you can then generate your credentials.

The credentials consist of a **Client ID** and a **Client Secret**. You should write down your client secret because you won't be able to view it at a later point in time.
With those credentials you and other third party applications are able to obtain access to our API and thus do everything that our UI can do.
This is **important**, especially because the scope of the returned access token in the next step might indicate something else.
So again:
> With these credentials, everyone can act in the name of you and access, modify and delete all the information that is stored about you in our system.

## Access Token

If you want to make requests to our API, you need to obtain an **Access token**.
These are tokens with a limited time of life (24 hours by default). To request tokens, you need to make a POST request to our API.

If you use `cURL` for example this might look like this:
```bash
curl --request POST \
    --url 'https://api.openbadges.education/o/token' \
    --header 'content-type: application/x-www-form-urlencoded' \
    --data grant_type=client_credentials \
    --data client_id=YOUR_CLIENT_ID \
    --data client_secret=YOUR_CLIENT_SECRET
```
The response will then look something like this:
```JSON
{
    "access_token": "YOUR_ACCESS_TOKEN",
    "expires_in": 86400,
    "token_type": "Bearer",
    "scope": "r:profile"
}
```
Once again, note that the scope doesn't actually mean anything (yet).

## With username and password

For the curious among you, there is a second way to obtain the access token.
This is also the way that our application does it (and the way it's documented in the API docs): Via username and password.
For that you don't need a client ID and Secret, but the username and password of your user:
```bash
curl --request POST \
    --url 'https://api.openbadges.education/o/token' \
    --header 'content-type: application/x-www-form-urlencoded' \
    --data grant_type=password \
    --data 'username=YOUR_USERNAME' \
    --data 'password=YOUR_PASSWORD' \
    --data client_id=public
```
