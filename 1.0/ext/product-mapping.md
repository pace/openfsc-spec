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

Get all available products categorised as the following types: **ron98**, **ron95e10**, **diesel**, **e85**, ... see [Product types](#Product-types). The client needs to make sure that all products have been transmitted with PRODUCT notifications before he can send an OK to conclude the PRODUCTS request. In case of an error the client returns an ERR message.

Arguments: **None**

## Example (with price)

```text
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

## Example (without price)

```text
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

```text
product_method = "PRODUCT"
    space string
    space string
    space decimal .

products_method = "PRODUCTS" .
```

### Product types

Full list of all product types that can be used in OpenFSC

| Product Type         | Description                                                               |
| -------------------- | ------------------------------------------------------------------------- |
| `adBlue`             | AdBlue Diesel exhaust fluid (DEF, also: AUS 32)                           |
| `careDiesel` *       | => deprecated, use `dieselHvo` instead                                    |
| `cng`                | Compressed natural gas                                                    |
| `diesel`             | Diesel fuel                                                               |
| `dieselB0`           | Diesel fuel (0% bio additives)                                            |
| `dieselB7`           | Diesel fuel (up to 7% bio additives)                                      |
| `dieselB15`          | Diesel fuel (up to 15% bio additives)                                     |
| `dieselB20`          | Diesel fuel (up to 20% bio additives)                                     |
| `dieselBMix`         | Diesel fuel (unspecified amount of additives)                             |
| `dieselGtl`          | GTL/XTL Diesel (synthetic, gas-to-liquid or x-to-liquid)                  |
| `dieselHvo`          | HVO or CARE Diesel                                                        |
| `dieselPremium`      | Premium Diesel                                                            |
| `dieselRed`          | Red/colored Diesel (used in agricultural vehicles or generators)          |
| `dieselSynthetic`    | Other types of synthetic Diesel (EN 15940 fuel)                           |
| `e85`                | Ethanol (85%)                                                             |
| `h2`                 | Hydrogen                                                                  |
| `heatingOil`         | Heating oil (note: most likely not used to power vehicles!)               |
| `lng`                | Liquefied natural gas                                                     |
| `lpg`                | Liquefied petroleum gas (propane and butane)                              |
| `ron95e5`            | RON 95 grade gasoline (up to 5% bio additives)                            |
| `ron95e10`           | RON 95 grade gasoline (up to 10% bio additives)                           |
| `ron98`              | RON 98 grade gasoline                                                     |
| `ron98e5`            | RON 98 grade gasoline (up to 5% bio additives)                            |
| `ron100`             | RON 100 grade gasoline                                                    |
| `syntheticDiesel` *  | => deprecated, use `dieselSynthetic` instead                              |
| `truckAdBlue`        | AdBlue Diesel exhaust fluid (DEF, also: AUS 32) for commercial vehicles   |
| `truckDiesel`        | Diesel for commercial vehicles (including agricultural diesel fuel)       |
| `truckDieselPremium` | Premium Diesel for commercial vehicles                                    |
| `truckLpg`           | Liquefied petroleum gas for commercial vehicles                           |

Deprecated fuel types are marked with `*` in the list above. Do not use them for new implementations, they will be removed in an upcoming version:

- `careDiesel`
- `syntheticDiesel`

Legacy product types which are already removed from spec, and cannot be used anymore:

- `e50` => removed
- `methanol` => removed
- `vegetableOil` => removed
