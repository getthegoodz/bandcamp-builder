# Backlog

## Upcharges aren't reflected in Shopify pricing

`api/custom-goodz-order.js` picks a Shopify variant purely from the quantity tier (50-99 = $3.44/unit, 100-199 = $3.32/unit, etc). The endpoint never reads `backDesign`, `keychainHole`, or even `sleeve` for pricing. They're stored as line/cart attributes only.

Meanwhile the front-end (site-draft `order.html` and `artwork-upload.html`) shows totals that include the option upcharges:

- Sleeve: +$0.20/unit
- Keychain hole: +$0.15/unit
- Custom back design: +$0.10/unit (also applied dynamically if a customer upgrades from standard to custom on artwork-upload.html)

Net effect: Shopify checkout charges the base tier price, while the displayed total can be $5-$60 higher depending on selected options. Customer pays less than the page implied.

Possible fixes (pick one):

1. Add per-option Shopify variants and select the right combo in `getTier`.
2. Inject a discount or extra line item in the cart (e.g. a custom-back upcharge SKU) so Shopify totals match.
3. Drop the upcharge math from the displayed totals and only show the base tier price.

Surfaced 2026-05-05 while wiring the dynamic custom-back upcharge UI on `artwork-upload.html`. Not blocking the artwork-upload work itself; the page is consistent with how the rest of the flow already behaves.
