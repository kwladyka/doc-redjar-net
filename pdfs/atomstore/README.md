# Integration

https://www.atomstore.pl/

The atomstore send API requests to the service. The module in atomstore is called "RedJar etykiety" and you can find it in settings.

## Module settings

| Name                     | Reffer to                                  |
|--------------------------|--------------------------------------------|
| `Kod sklepu`             | `api-client` / `code`                      |
| `Przesyłane atrybuty`    | `orders[]` / `products[]` / `custom`       |
| `Atrybut label template` | `orders[]` / `products[]` / `pdf-template` |

## Sides notes

Atomstore send attributes in `orders[]` / `products[]` / `custom` using ids of attributes in atomstore. This mean for example `ingredients` in each shop will have different `id`. It is impossible for RedJar to make label work for each client, because of that. RedJar needs to make individual template for each client to make it work.

## Examples of requests

### Orders

Real example of request: [request-orders.json](request-orders.json)

`curl -H "Content-Type: application/json" -d @pdfs/atomstore/request-orders.json -i https://pdfs.redjarapis.net/generate`

#### structure description

```json
{
    "data-structure" : "atomstore/labels",
    "api-client" : {
      "code" : "shop-x"
    },
    "orders" : [ {
      "code" : "34524",
      "lang" : "en",
      "buyer" : {
        "code" : "34589"
      },
      "products" : [ {
        "pdf-template" : "examples/labels",
        "code" : "6452",
        "name" : "Product name",
        "unit" : "kg",
        "quantity" : 1.5,
        "custom" : {
          "31" : "kamut wheat, sugar, rye, salt, peanuts",
          "4" : "Poland"
        }
      } ]
    } ]
  }
```

| Name             | Description                    |
|------------------|--------------------------------|
| `data-structure` | Always `atomstore/labels`.     |
| `api-client`     | Map of client API information. |
| `orders`         | Vector of orders.              |

#### api-client

| Name   | Description                                           |
|--------|-------------------------------------------------------|
| `code` | Unique identifier for client system to identify shop. |

#### orders

| Name       | Description                  |
|------------|------------------------------|
| `code`     | Unique identifier for order. |
| `lang`     | Language to generate labels. |
| `buyer`    | Buyer contractor data.       |
| `products` | Vector of products.          |

#### buyer

| Name   | Description                       |
|--------|-----------------------------------|
| `code` | Unique identifier for contractor. |

#### products

| Name                  | Description                                                          |
|-----------------------|----------------------------------------------------------------------|
| `label-template`      | Name of the template to use.                                         |
| `code`                | Unique identifier for product.                                       |
| `name`                | Product name.                                                        |
| `ingredients`         | Product ingredients.                                                 |
| `countries-of-origin` | Countries of origin.                                                 |
| `expiry-days`         | Number of days to add to current date.                               |
| `unit`                | Unit.                                                                |
| `quantity`            | Quantity.                                                            |
| `custom`              | Map of attributes configured in [module settings](#module-settings). |


### Products

Real example of request: [request-products.json](request-products.json)

`curl -H "Content-Type: application/json" -d @pdfs/atomstore/request-products.json -i https://pdfs.redjarapis.net/generate`

#### structure description

Products not connected to any order.

The difference between orders is to not send in request `orders[] / code`, `orders[] / buyer` and `orders[] / products[] / quantity` fields. While te structure of the code is not representing luck of orders perfeclty it makes code simpler on both sides.