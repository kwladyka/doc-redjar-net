# API labels

Service to generate labels for orders.

## Rationale

Companies need to label products which they sell with data like: producer, ingredients (with allegrens), expiry date etc. This process is time consuming and it is easy to make a mistake and risk penalties. People have many technical issues to prepare up to date labels for products.

The service is a solution to automate this process to make it easy and simple.

## Request

### `https://labels.readjarapis.net/orders`

- [request-orders.json](request-orders.json)
- [request-orders.json](request-products.json)

```json
{
    "api-client": {
        "code": "shop1"
    },
    "orders": [
        {
            "code": "123",
            "lang": "en",
            "products": [
                {
                    "label-template": "organization/template",
                    "name": "English name",
                    "ingredients": "kamut wheat, sugar, rye, salt, peanuts",
                    "countries-of-origin": "Poland, Germany",
                    "expiry-days": 365,
                    "unit": "kg",
                    "quantity": 1.23
                }
            ]
        }
    ]
}
```

| Name         | Description                |
| ------------ | -------------------------- |
| `api-client` | Map of client information. |
| `orders`     | Vector of orders.          |

#### api-client

| Name   | Description                          |
| ------ | ------------------------------------ |
| `code` | Unique identifier for client system. |

#### orders

| Name       | Description                                 |
| ---------- | ------------------------------------------- |
| `code`     | Unique identifier for client requested API. |
| `lang`     | Language to generate label.                 |
| `products` | Vector of products.                         |

#### products

| Name                  | Description                            |
| --------------------- | -------------------------------------- |
| `label-template`      | Name of the template to use.           |
| `name`                | Product name.                          |
| `ingredients`         | Product ingredients.                   |
| `countries-of-origin` | Countries of origin.                   |
| `expiry-days`         | Number of days to add to current date. |
| `unit`                | Unit.                                  |
| `quantity`            | Quantity.                              |

## Response

You get PDF files to download. Keep in mind files have limited Time To Live, so download them immediatelly.

```json
{
    "files": [
        "https://api.redjar.net/files/ee8f7220-b8ae-11ea-8c2d-31c4e434e2af.pdf",
        "https://api.redjar.net/files/f3d95bb0-b8ae-11ea-8c2d-31c4e434e2af.pdf"
    ]
}
```

## FAQ

### Why response return many files?

Let's imagine that you want to use different labels sizes for different products. If you put all labels into one file, then you will have an issue to print them properly. There will be the same issue for grayscale and colour labels or other technical demands, which should be printed on different printers.

The solution is to generate separate files to print each file on different printer.

### How to generate labels for products without orders?

Set `order` / `code` as `null`.