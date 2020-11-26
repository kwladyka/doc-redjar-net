# API pdfs

Service to generate pdfs from prepared templates using data in API request.

## Rationale

Companies need to generate pdfs for different purposes. Currently focus on labels for products containing data like: producer, ingredients (with allegrens), expiry date etc. This process is time consuming and it is easy to make a mistake and risk penalties. People have many technical issues to prepare up to date labels for products.

The service is a solution to automate this process to make it easy and simple.

## Request

### `https://pdfs.readjarapis.net/generate`

- [request-orders.json](request-orders.json)
- [request-products.json](request-products.json)

```json
{
    "data-structure": "atomstore/labels",
    "api-client": {
        "code": "shop1"
    },
    "orders": [
        {
            "code": "34524",
            "lang": "en",
            "buyer": {
                "code": "34589"
            },
            "products": [
                {
                    "pdf-template": "examples/products",
                    "name": "Product name",
                    "unit": "kg",
                    "quantity": 1.5,
                    "custom": {
                        "ingredients": "kamut wheat, sugar, rye, salt, peanuts",
                        "countries-of-origin": "Poland",
                        "expiry-days": 365
                    }
                },
                {
                    "pdf-template": "examples/products",
                    "name": "Tea",
                    "unit": "pcs",
                    "quantity": 1,
                    "custom": {
                        "ingredients": "orange peel, ginger, cinnamon, clove, sage, fennel, licorice root, cardamom, black pepper, miniscus root, juniper fruit, ginger root, sars parilla root, coriander, parsley root, saffron root",
                        "countries-of-origin": "China",
                        "expiry-days": 30
                    }
                }
            ]
        }
    ]
}
```

| Name             | Description                                                                                                                            |
|------------------|----------------------------------------------------------------------------------------------------------------------------------------|
| `data-structure` | Unique identifier for data which are send to API. For example `atomstore/labels` is structure for atomstore system for labels purpose. |
| `api-client`     | Map of client information.                                                                                                             |
| `orders`         | Vector of orders.                                                                                                                      |

#### api-client

| Name   | Description                          |
|--------|--------------------------------------|
| `code` | Unique identifier for client system. |

#### orders

| Name         | Description                                 |
|--------------|---------------------------------------------|
| `code`       | Unique identifier for client requested API. |
| `lang`       | Language to generate label.                 |
| `buyer`      | Buyer contractor data.                      |
| `products`   | Vector of products.                         |

#### buyer

| Name   | Description                       |
|--------|-----------------------------------|
| `code` | Unique identifier for contractor. |

#### products

| Name                  | Description                                                                                                                                                                 |
|-----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `label-template`      | Name of the template to use.                                                                                                                                                |
| `code`                | Unique identifier for product.                                                                                                                                              |
| `name`                | Product name.                                                                                                                                                               |
| `ingredients`         | Product ingredients.                                                                                                                                                        |
| `countries-of-origin` | Countries of origin.                                                                                                                                                        |
| `expiry-days`         | Number of days to add to current date.                                                                                                                                      |
| `unit`                | Unit.                                                                                                                                                                       |
| `quantity`            | Quantity.                                                                                                                                                                   |
| `custom`              | This can be whatever data structure. The `custom` data is not validated. The intention is to let third party systems send data unique only for them or for specific client. |

## Response

You get PDF files to download. Keep in mind files have limited Time To Live, so download them immediatelly.

```json
{
    "files": [
        "https://pdfs.redjarapis.net/files/ee8f7220-b8ae-11ea-8c2d-31c4e434e2af.pdf",
        "https://pdfs.redjarapis.net/files/f3d95bb0-b8ae-11ea-8c2d-31c4e434e2af.pdf"
    ]
}
```

## FAQ

### Why response return many files?

Let's imagine that you want to use different labels sizes for different products. If you put all labels into one file, then you will have an issue to print them properly. There will be the same issue for grayscale and colour labels or other technical demands, which should be printed on different printers.

The solution is to generate separate files to print each file on different printer.

### How to generate labels for products without orders?

Do not send in request `order` / `code` field.