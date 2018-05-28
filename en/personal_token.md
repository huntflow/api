
# Personal token

Personal token simplifies integrations with the Huntflow API without additional authorization. The most frequent use is server scripts or applications integration with Huntflow (e.g. collecting responses from a corporate website).

To get a personal token, send an application to hello@huntflow.ru. State a Huntflow username requiring a personal token in the e-mail.
Warning! Зersonal token usage requires higher level of security for storage and unauthorized access issues.

### Personal token usage

Personal token is used similar to `access_token`aquired by OAuth 2.0 authorization. The application must use the aquired token in requests, stating it in a header as: `Authorization: Bearer <personal_token>`
To test a token, use the method /me.

```GET /me HTTP/1.1
User-Agent: App/1.0 (incaseoffire@example.com)
Host: api.huntflow.ru
Accept: */*
Authorization: Bearer <personal_token>
```
