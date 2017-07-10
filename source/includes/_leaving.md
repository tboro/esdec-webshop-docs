# Populating cart

> The calculator will call your redirectUri with following JSON structure using AJAX:

```json
{
  "session": {
    "id": "shop-session-id",  // provided earlier
    "moment": "yyyy-MM-dd HH:mm:ss",
    "language": "[A-Z]{2}"  // language used in the calculator
  },
  "items": [
    {
      "code": ".+",    // SKU of the product in your webshop
      "count": "\\d+"  // number of items
    }
    // more products...
  ]
}

```

Once an end-user finishes building the project, the last step is to place ClickFit EVO products in the cart.
This is by default managed by an AJAX call to a special endpoint (named `redirectUri`) provided by your webshop.

<aside class="warning">Make sure you have <a href="https://enable-cors.org/">CORS</a> enabled for calculator.esdec.com domain.</aside>