# General information

* API works according to HTTPS protocol.
* Authorization is performed by OAuth 2.0 protocol or be a personal token.
* All the data is available only in JSON format.
* Basic URL — `https://api.huntflow.ru/`
* Date format:
  [ISO 8601](http://en.wikipedia.org/wiki/ISO_8601): `YYYY-MM-DDThh:mm:ss±hh:mm`.


<a name="request_requirements"></a>
### Request requirements

Request should contain the header `User-Agent` or `400 Bad Request` will be returned in the response. If you state the name of the application and email address in the header it will help us contact you as fast as possible if it is necessary.

```
User-Agent: App/1.0 (incaseoffire@example.com)
```

#### Request limits

Every token has a limit of 10 requests per second (limits could not be applied to on-premise or dedicated deployment).

If the limit is exceeded, a `429 Too Many Requests` response will be returned. 


<a name="request_body"></a>
### Request body format when sending a JSON


The data in the body of the request has to meet the following requiremetns:

* Valid JSON (minimal variant is allowed as well as pretty print variant with additional spaces and line wrapping).
* UTF-8 code usage is recommended without additional screening
  (`{"name": "Иван Иванов"}`).
* Also ascii code is allowed with the screening
  (`{"name": "\u0418\u0432\u0430\u043d\u043e\u0432 \u0418\u0432\u0430\u043d"}`).
* Some data types in certain fields have additional requirements, described in every specific method. JSON data types are `string`,
  `number`, `boolean`, `null`, `object`, `array`.


<a name="errors-and-codes"></a>
### Errors and response codes

API uses reporting by response codes.
The application has to process them adequately.

In the case of malfunction the response codes can be `503` and `500`.

Every error report shall contain the response code and additional information in order to help the developer understand the reason of the current error report.
[More about errors](errors.md).


<a name="pagination"></a>
### Page by page output

In the requests aimed to get a list of objects one can state a `page=N&count=M` in the parameters. Page count starts with 1 and the first page with 30 objects is returned by default. Every response with page by page output has the same root object type:

```json
{  
  "page": 1,
  "count": 1,
  "total": 1,
  "items": [{}]
}
```

<a name="links"></a>
## Useful links

* [HTTP/1.1](http://tools.ietf.org/html/rfc2616)
* [JSON](http://json.org/)
* [URI Template](http://tools.ietf.org/html/rfc6570)
* [OAuth 2.0](http://tools.ietf.org/html/rfc6749)
* [REST](http://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm)
* [ISO 8601](http://en.wikipedia.org/wiki/ISO_8601)
