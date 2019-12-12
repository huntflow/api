
# Personal token

Personal token simplifies integrations with the Huntflow API without additional authorization. The most frequent use is server scripts or applications integration with Huntflow (e.g. collecting responses from a corporate website).

To get a personal token, send an application to [support@huntflow.ru](mailto:support@huntflow.ru). State a Huntflow username requiring a personal token in the email.

Warning! Personal token usage requires higher level of security for storage and unauthorized access issues.

### Personal token usage

The application must use the aquired token in requests, stating it in a header as: `Authorization: Bearer <personal_token>`
To test a token, use the method [/me](user.md#me).

```GET /me HTTP/1.1
User-Agent: App/1.0 (incaseoffire@example.com)
Host: api.huntflow.ru
Accept: */*
Authorization: Bearer <personal_token>
```
