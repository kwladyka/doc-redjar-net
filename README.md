# API rules

Rules common to all our APIs.

Open folder to read detailed API documentation.

## Authentication and authorization

during development

## Request

Documentation contains examples of requests to let you fast prgoress. You can run them like below:

```bash
git clone git@github.com:kwladyka/doc-redjar-net.git
curl -H "Content-Type: application/json" -d @service/request.json -i https://service.redjarapis.net
```

## Response

### HTTP code reponse

We use right HTTP codes to response. `200` means request succeed. Otherwise you should log the event and fix the issue.

#### Logs

##### Request validation

If you will send invalid request, API will give you full information about that.

```json
{
    "logs": [
        {
            "type": "error",
            "code": "validation",
            "message": "Has to be a positive integer.",
            "cause": {
                "pred": "clojure.core/pos-int?",
                "in": [
                    "orders",
                    1,
                    "products",
                    2,
                    "expiry-days"
                ],
                "val": "foo"
            }
        }
    ]
}
```

| Name      | Description                                 |
| --------- | ------------------------------------------- |
| `type`    | `error` / `warn` / `info` / `debug`         |
| `code`    | The unique identifer to use in your system. |
| `message` | Human readable message.                     |
| `cause`   | The reason of the issue.                    |

##### `cause` when `code` is `validation`

| Name   | Description                                                                   |
| ------ | ----------------------------------------------------------------------------- |
| `pred` | Which predicate is invalid.                                                   |
| `in`   | The position of invalid data. Number mean position in vector. 0 is first one. |
| `val`  | The invalid value. For a security reason this can be returned or not.         |