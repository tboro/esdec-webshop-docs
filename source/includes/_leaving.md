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

> When using iframe integration, product list needs to be picked up by the webshop window:

```javascript
// Sample expected event.data value:
// event.data = '{
//  "session":{"id":"32987abc","moment":"2017-07-26 13:18:20","language":"NL"},
//  "items": [{"code":"100-0000","count":"20"},{"code":"100-0001","count":"10"}]
// }';

window.addEventListener('message', function (event) {
    var message = JSON.parse(event.data);
    console.log(message);
}.bind(this));
```

Once an end-user finishes building the project, the last step is to place ClickFit EVO products in the cart.
This is by default managed by an AJAX call to a special endpoint (named `redirectUri`) provided by your webshop.

When calculator is situated within an iframe, a [postMessage](https://developer.mozilla.org/en-US/docs/Web/API/Window/postMessage) mechanism will be used to send the products back to the webshop scope.

<aside class="warning">Make sure you have <a href="https://enable-cors.org/">CORS</a> enabled for calculator.esdec.com domain.</aside>