# Entering the calculator I

> Example redirect

```shell
curl "https://calculator.esdec.com/?vendor-uuid=492d0cef-5427-4ed8-b12d-169ef39dd871&shop-session-id=32987abc"
```

> Embedding within iframe:

```html
<iframe style="width: 1920px; height: 1080px;"
 src="https://calculator.esdec.com/?vendor-uuid=492d0cef-5427-4ed8-b12d-169ef39dd871&shop-session-id=32987abc">
</iframe>
```

In this section the simplified approach is described. It is enough either to redirect your webshop to a following URL, or embed it within an iframe.

### HTTP Request

`GET https://esdec.app.amsdard.io/` (test environment)

`GET https://calculator.esdec.com/` (production environment)

### Query Parameters

Parameter | Type | Description
--------- | ---- | -----------
vendor-uuid | string | Personal integration key - used to associate the webshop with our calculator. You will receive it from us.
locale | string | Preferred language version, for example: nl.
shop-session-id | string | Current webshop user session.

<aside class="notice">
You must provide all query parameters in order to enter the calculator properly.
</aside>
