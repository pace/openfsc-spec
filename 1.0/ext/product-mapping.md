# Product Mapping (`PRODUCTS` and `PRODUCT`)

To map the site local product IDs to categories which can easily be understood by the server, two extra methods can be used: `PRODUCT` and `PRODUCTS`.
It is highly recommended to implement the product mapping. If the product mapping is not implemented the `VATRate` for products needs to be delivered using a different method.

## `PRODUCT`

Type: **Notification**

Product with its ID, categorised as stated in PRODUCTS. Should only be sent as reaction to a previous PRODUCTS message.

Arguments:

- **ProductID** (arg0, string): identifier of the product. (e.g.: 0100)
- **Category** (arg1, string): category of the provided product. Needs to be one of: **ron98**, **ron98e5**, **ron95e10**, **diesel**, **e85**, ... see [Product types](#Product-types).
- **VATRate** (arg2, decimal): rate of VAT used for this product in percent. (e.g.: 19.0)
- **Unit** *optional* (arg3, string): unit use for the fuel. (e.g.: LTR)
- **OptionalName** *optional* (arg4, string): optional fuel name. (e.g.: Super Müller T. diesel)
## `PRODUCTS`

Type: **Request/Response**

Get all available products categorised as the following types: **ron98**, **ron95e10**, **diesel**, **e85**, ... see [Product types](#Product-types). The client needs to make sure that all products have been transmitted with PRODUCT notifications before they can send an OK to conclude the PRODUCTS request. In case of an error the client returns an ERR message.

Arguments: **None**

## Example

```
S: S1 PUMPS
C: * PUMP 1 locked
C: * PUMP 2 locked
C: S1 OK
S: S2 PRICES
C: * PRICE 0100 LTR EUR 1.339 Super Plus
C: * PRICE 0200 LTR EUR 1.229 Super 95
C: * PRICE 0300 LTR EUR 1.499 Super 95 e5
C: * PRICE 0400 LTR EUR 1.209 Diesel
C: S2 OK
S: S3 PRODUCTS
C: * PRODUCT 0100 ron98 19.0
C: * PRODUCT 0200 ron95 19.0
C: * PRODUCT 0300 ron95e5 19.0
C: * PRODUCT 0400 diesel 19.0
C: S3 OK
```

We also leave the possibility to use products without giving any price. It's strongly recommend in that case to use the optionals parameters (**Unit**, **OptionalName**: required together)

## Example

```
S: S1 PUMPS
C: * PUMP 1 locked
C: * PUMP 2 locked
C: S1 OK
S: S2 PRODUCTS
C: * PRODUCT 0100 ron98 19.0 LTR Test fuel Name
C: * PRODUCT 0300 ron95e5 19.0 LTR Super Fuel
C: * PRODUCT 0400 diesel 19.0
C: S2 OK
```

## EBNF

```bash
product_method = "PRODUCT"
	space string
	space string
	space decimal .

products_method = "PRODUCTS" .
```

### Product types
Full list of all known product types
* `adBlue`
* `cng`
* `careDiesel`
* `diesel`
* `dieselB0`
* `dieselB7`
* `dieselB15`
* `dieselB20`
* `dieselBMix`
* `dieselGtl`
* `dieselPremium`
* `e50`
* `e85`
* `h2`
* `heatingOil`
* `lng`
* `lpg`
* `methanol`
* `ron95e5`
* `ron95e10`
* `ron98`
* `ron98e5`
* `ron100`
* `truckAdBlue`
* `truckDiesel`
* `truckDieselPremium`
* `truckLpg`
* `vegetableOil`
