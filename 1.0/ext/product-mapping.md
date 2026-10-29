# Product Mapping (`PRODUCTS` and `PRODUCT`)

To map site-local product IDs to categories that the server can easily understand, two additional methods can be used: `PRODUCT` and `PRODUCTS`.
Implementing product mapping is highly recommended. If product mapping is not implemented, the `VATRate` of the products has to be provided by other means.

## `PRODUCT`

Type: **Notification**

Direction: **Client → Server**

A product with its ID and category. Should only be sent in response to a previous `PRODUCTS` request. If `OptionalName` is not given, the server falls back to the `Description` provided via `PRICE`.

Arguments:

- **ProductID** (arg0, string): identifier of the product. (e.g.: 0100)
- **Category** (arg1, string): category of the product. Must be one of the [product types](#product-types), e.g. **ron98**, **ron98e5**, **ron95e10**, **diesel**, **e85**, ...
- **VATRate** (arg2, decimal): VAT rate of this product in percent. (e.g.: 19.0)
- **Unit** *optional* (arg3, string): unit used for the product. (e.g.: LTR)
- **OptionalName** *optional* (arg4, string): human-readable product name. Can only be given together with **Unit**. (e.g.: Super Müller T. Diesel)

## `PRODUCTS`

Type: **Request/Response**

Direction: **Server → Client**

Get all available products with their category (see [Product types](#product-types)). The client must make sure that all products have been transmitted with `PRODUCT` notifications before sending `OK` to conclude the `PRODUCTS` request. In case of an error, the client returns an ERR message.

Arguments: **None**

## Example (with price)

```text
S: S1 PUMPS
C: * PUMP 1 locked
C: * PUMP 2 locked
C: S1 OK
S: S2 PRODUCTS
C: * PRODUCT 0100 ron98 19.0
C: * PRODUCT 0200 ron95e10 19.0
C: * PRODUCT 0300 ron95e5 19.0
C: * PRODUCT 0400 diesel 19.0
C: S2 OK
S: S3 PRICES
C: * PRICE 0100 LTR EUR 1.339 Super Plus
C: * PRICE 0200 LTR EUR 1.229 Super 95 E10
C: * PRICE 0300 LTR EUR 1.499 Super 95
C: * PRICE 0400 LTR EUR 1.209 Diesel
C: S3 OK
```

Products can also be provided without any price. In that case, it is strongly recommended to use the optional arguments **Unit** and **OptionalName**. **Unit** can be sent without **OptionalName**, but **OptionalName** requires **Unit** to be present.

## Example (without price)

```text
S: S1 PUMPS
C: * PUMP 1 locked
C: * PUMP 2 locked
C: S1 OK
S: S2 PRODUCTS
C: * PRODUCT 0100 ron98 19.0 LTR Test Super 98 Power Fuel
C: * PRODUCT 0200 ron95e10 19.0 LTR
C: * PRODUCT 0300 ron95e5 19.0 LTR Super E5 Fuel
C: * PRODUCT 0400 diesel 19.0
C: S2 OK
```

## EBNF

```text
product_method = "PRODUCT"
    space string
    space string
    space decimal
    [ space unit [ space argv ] ] .

products_method = "PRODUCTS" .
```

### Product types

Full list of all product types that can be used in OpenFSC:

| Product Type         | Description                                                             |
| -------------------- | ----------------------------------------------------------------------- |
| `adBlue`             | AdBlue Diesel exhaust fluid (DEF, also: AUS 32)                         |
| `careDiesel` *       | => deprecated, use `dieselHvo` instead                                  |
| `cng`                | Compressed natural gas                                                  |
| `diesel`             | Diesel fuel                                                             |
| `dieselB0`           | Diesel fuel (0% bio additives)                                          |
| `dieselB7`           | Diesel fuel (up to 7% bio additives)                                    |
| `dieselB15`          | Diesel fuel (up to 15% bio additives)                                   |
| `dieselB20`          | Diesel fuel (up to 20% bio additives)                                   |
| `dieselBMix`         | Diesel fuel (unspecified amount of additives)                           |
| `dieselGtl`          | GTL/XTL Diesel (synthetic, gas-to-liquid or x-to-liquid)                |
| `dieselHvo`          | HVO or CARE Diesel                                                      |
| `dieselPremium`      | Premium Diesel                                                          |
| `dieselRed`          | Red/colored Diesel (used in agricultural vehicles or generators)        |
| `dieselSynthetic`    | Other types of synthetic Diesel (EN 15940 fuel)                         |
| `e85`                | Ethanol (85%)                                                           |
| `h2`                 | Hydrogen                                                                |
| `heatingOil`         | Heating oil (note: most likely not used to power vehicles!)             |
| `lng`                | Liquefied natural gas                                                   |
| `lpg`                | Liquefied petroleum gas (propane and butane)                            |
| `ron95e5`            | RON 95 grade gasoline (up to 5% bio additives)                          |
| `ron95e10`           | RON 95 grade gasoline (up to 10% bio additives)                         |
| `ron98`              | RON 98 grade gasoline                                                   |
| `ron98e5`            | RON 98 grade gasoline (up to 5% bio additives)                          |
| `ron100`             | RON 100 grade gasoline                                                  |
| `syntheticDiesel` *  | => deprecated, use `dieselSynthetic` instead                            |
| `truckAdBlue`        | AdBlue Diesel exhaust fluid (DEF, also: AUS 32) for commercial vehicles |
| `truckDiesel`        | Diesel for commercial vehicles (including agricultural diesel fuel)     |
| `truckDieselPremium` | Premium Diesel for commercial vehicles                                  |
| `truckLpg`           | Liquefied petroleum gas for commercial vehicles                         |

Deprecated product types are marked with `*` in the list above. Do not use them in new implementations; they will be removed in an upcoming version:

- `careDiesel`
- `syntheticDiesel`

Legacy product types that have already been removed from the specification and can no longer be used:

- `e50` => removed
- `methanol` => removed
- `vegetableOil` => removed
