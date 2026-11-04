# Administrator workflows

## Sign-in and roles

The React dashboard checks login/session through admin API routes. The sidebar filters controls by role for usability, but the backend remains the authority. Do not share admin sessions or use a generic operator account. Disable inactive administrator records through an authorized workflow.

## Catalog and merchandising

The dashboard supports products, categories/subcategories, brands, collections, posters, and story management. Images are selected via the existing multipart forms and validated/compressed by the client/backend upload pipeline; the server is authoritative. Create parent category/brand data before dependent product records. Keep synthetic staging names clearly labeled. Product prices, stock and active status originate from the admin/API, not storefront code. Variant products remain non-purchasable until a real option-level price/stock contract exists.

## Orders and payment review

Admins with the required active role can read order information, update permitted order status, record agreed collection-point details, and review payment proofs. Moderators cannot read customer/order details or proof bytes. Payment verification/rejection uses backend authorization and audit records. Do not expose provider URLs; receipt viewer must use the authorized bytes endpoint. Use `Ready for Collection` and `Collected` statuses; do not imply home delivery.

## Settings and dynamic contacts

Checkout settings/payment methods are managed via supported super-admin interfaces/API. A public read endpoint exposes only active customer-usable payment methods. Retailer contact records are persistent backend data, with active state, display order and placement; the storefront and Flutter app have no hardcoded contact fallback. Store setup should display an empty state until values have been confirmed by the owner. Never invent payment instructions, contact numbers, pickup locations, or business claims.

## Feedback states

The dashboard provides loading, empty, validation, success and error feedback for implemented forms. Notification/support placeholders should not be represented as live external integrations. If a request fails, verify API logs/status and retry only idempotent reads or operations whose semantics are understood.
