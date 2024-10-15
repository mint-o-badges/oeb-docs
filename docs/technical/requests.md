# Making requests

If you now obtained your [credentials](./credentials.md), you can start making HTTP requests to our API.
You can make all the requests documented in the [V1](https://api.staging.openbadges.education/docs/v1) and [V2](https://api.staging.openbadges.education/docs/v2) API documentation.
You always need to append a header with your access token.
In `cURL` for example this means adding `--header 'Authorization: Bearer YOUR_ACCESS_TOKEN'` at the end.

An example request would then look like this:
```bash
curl --request GET \
    --url 'https://api.openbadges.education/v2/badgeclasses' \
    --header 'accept: application/json' \
    --header 'Authorization: Bearer YOUR_ACCESS_TOKEN'
```
This request returns a list of all the badge classes for the authenticated user.
