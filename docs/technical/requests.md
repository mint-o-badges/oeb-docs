# Making requests

If you now obtained your [credentials](./credentials.md), you can start making HTTP requests to our API.
You can make all the requests documented in the [V1](https://api.staging.openbadges.education/docs/v1) and [V2](https://api.staging.openbadges.education/docs/v2) API documentation.

## Authentication

You always need to append your access token.

### As header

One option to do this is by adding it as a header.
In `cURL` for example this means adding `--header 'Authorization: Bearer YOUR_ACCESS_TOKEN'` at the end.

An example request would then look like this:
```bash
curl --request GET \
    --url 'https://api.openbadges.education/v2/badgeclasses' \
    --header 'accept: application/json' \
    --header 'Authorization: Bearer YOUR_ACCESS_TOKEN'
```
This request returns a list of all the badge classes for the authenticated user.

### As cookie

The other option to do it would be to append it as a cookie.
This is the way our application does it, since it allows using the `HttpOnly` cookie.
To achieve this in Angular, for example, simply add the option `withCredentials: true`, after having received the response with the `Set-Cookie` option.

An example request would then look like this:
```yaml
GET /issuer HTTP/1.1
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Accept-Encoding: gzip, deflate, br, zstd
Accept-Language: en-GB,en-US;q=0.9,en;q=0.8
Cache-Control: no-cache
Connection: keep-alive
Cookie: csrftoken=YOUR_CSRF_TOKEN; access_token=YOUR_ACCESS_TOKEN
DNT: 1
Host: openbadges.education
Pragma: no-cache
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/131.0.0.0 Safari/537.36
sec-ch-ua: "Google Chrome";v="131", "Chromium";v="131", "Not_A Brand";v="24"
sec-ch-ua-mobile: ?0
sec-ch-ua-platform: "Linux"
```
