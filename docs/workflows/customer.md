# Customer shopping workflow

## Browse and cart

Visitors can open the home page, public catalog, product details, search/category/in-stock filters, support contacts and local cart without signing in. Catalog, categories, brands, collections, posters and support contacts are loaded from the API; no sample catalog is bundled. Guest cart stores product IDs and quantities in browser local storage. On checkout, product availability is refreshed; browser prices/stock are not authoritative.

Current API does not provide public brand or collection-specific product filtering or pagination. Collection cards navigate to the general catalog. Products with declared variants are blocked from online purchase because variant-level authoritative price and stock are not supported.

## Authentication and checkout

Registration/login use the current API; no email verification, password reset, refresh token or gateway flow is promised. Guests retain their cart and are routed through login/register when trying to checkout. Checkout asks for destination phone and city, with optional area/landmark and coordinates. This location supports transport coordination; the customer collects from a collection point agreed with staff. There is no customer delivery fee. The app must show a collection point only when staff records it.

Enabled payment options come from `GET /store-settings/checkout`; a valid empty configuration should show a clear empty state and block order submission. Do not invent transfer account instructions. Manual payment proof is uploaded only when the selected backend-configured method requires it. The UI submits product IDs/quantities and a stable idempotency key; server calculates totals and stock.

## Orders and account

Signed-in customers can view their own order list/details and status. Order status and payment state are controlled by the API/admin. The web storefront currently provides account summary, not full profile editing or account deletion. Never modify URL IDs to access another account's order; backend ownership must deny the request.

## Limitations

No real payment gateway, password recovery, email verification, product variant ordering, or guaranteed delivery date is currently represented. Legacy payment-proof exposure and the legal account-deletion wording remain release blockers.
