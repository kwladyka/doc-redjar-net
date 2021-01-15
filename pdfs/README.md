# API pdfs

Service to generate pdfs from prepared templates using data in API request.

## Rationale

Companies need to generate pdfs for different purposes. Currently focus on labels for products containing data like: producer, ingredients (with allegrens), expiry date etc. This process is time consuming and it is easy to make a mistake and risk penalties. People have many technical issues to prepare up to date labels for products.

The service is a solution to automate this process to make it easy and simple.

## Integration requests

- [atomstore](atomstore)

## Response

You get PDF files to download. Keep in mind files have limited Time To Live, so download them right away.

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