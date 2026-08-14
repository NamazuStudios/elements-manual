<h1>Stripe REST API Reference</h1>

<!-- wp:paragraph -->
<p>This page documents every REST endpoint exposed by the <a href="stripe">Stripe</a> Element. All paths are relative to the configured RS root, <code>dev.getelements.elements.element.rs.root</code> (default <code>/element/stripe/api</code>) — so <code>/stripe/payment-intent</code> below means <code>/element/stripe/api/stripe/payment-intent</code> on a default deployment. Every endpoint except the webhook receiver requires the <code>session_secret</code> security scheme. The full OpenAPI document is served at <code>/api/rest/openapi.json</code> once the Element is running.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-webhook">Webhook</h2>
<!-- /wp:heading -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th>Method</th><th>Path</th><th>Auth</th><th>Description</th></tr></thead><tbody><tr><td><code>POST</code></td><td><code>/stripe/webhook</code></td><td>None (signature-verified)</td><td>Receives and verifies a Stripe webhook, then publishes typed and raw internal events. See <a href="stripe-webhooks-and-events">Stripe Webhooks and the Typed Event Bus</a>.</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-customers">Customers</h2>
<!-- /wp:heading -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th>Method</th><th>Path</th><th>Request</th><th>Response</th><th>Description</th></tr></thead><tbody><tr><td><code>PATCH</code></td><td><code>/stripe/customer/{customerId}</code></td><td><code>UpdateCustomerRequest</code></td><td><code>204 No Content</code></td><td>Updates a customer's email and/or display name. <code>null</code> fields are left unchanged.</td></tr><tr><td><code>GET</code></td><td><code>/stripe/customers/search?key=&amp;value=</code></td><td>—</td><td><code>CreateCustomerResponse</code></td><td>Finds a customer by a metadata key/value pair. Returns <code>404</code> if none match. Use <code>key=orgId</code> for find-or-create.</td></tr><tr><td><code>POST</code></td><td><code>/stripe/customer/{customerId}/portal-session?returnUrl=</code></td><td>—</td><td><code>CreatePortalSessionResponse</code></td><td>Creates a single-use Stripe Customer Portal session URL.</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:paragraph -->
<p><code>UpdateCustomerRequest(String email, String name)</code>. <code>CreateCustomerResponse(String customerId)</code>. <code>CreatePortalSessionResponse(String url)</code>.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-payments">Payments</h2>
<!-- /wp:heading -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th>Method</th><th>Path</th><th>Request</th><th>Response</th><th>Description</th></tr></thead><tbody><tr><td><code>POST</code></td><td><code>/stripe/payment-intent</code></td><td><code>CreatePaymentIntentRequest</code></td><td><code>CreatePaymentIntentResponse</code></td><td>Creates a PaymentIntent for a one-off charge and returns its client secret.</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:paragraph -->
<p><code>CreatePaymentIntentRequest(long amount, String currency, String customerId, String description, Map&lt;String,String&gt; metadata, Boolean automaticPaymentMethods, String setupFutureUsage, String idempotencyKey)</code> — <code>amount</code> is in the currency's smallest unit (e.g. cents); <code>customerId</code> may be <code>null</code> for guest checkouts; <code>setupFutureUsage</code> set to <code>"off_session"</code> saves the payment method for later charges. <code>CreatePaymentIntentResponse(String paymentIntentId, String clientSecret)</code>.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-subscriptions">Subscriptions</h2>
<!-- /wp:heading -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th>Method</th><th>Path</th><th>Request</th><th>Response</th><th>Description</th></tr></thead><tbody><tr><td><code>POST</code></td><td><code>/stripe/customer/{customerId}/subscription</code></td><td><code>CreateSubscriptionRequest</code></td><td><code>SubscriptionStatusResponse</code></td><td>Creates a recurring subscription. The customer must already have a default payment method on file.</td></tr><tr><td><code>GET</code></td><td><code>/stripe/subscription/{subscriptionId}</code></td><td>—</td><td><code>SubscriptionStatusResponse</code></td><td>Gets the current status of a subscription.</td></tr><tr><td><code>DELETE</code></td><td><code>/stripe/subscription/{subscriptionId}</code></td><td>—</td><td><code>SubscriptionStatusResponse</code></td><td>Immediately cancels a subscription; the customer loses access at once. Returned status is <code>canceled</code>.</td></tr><tr><td><code>GET</code></td><td><code>/stripe/customer/{customerId}/subscriptions?status=&amp;limit=10&amp;startingAfter=</code></td><td>—</td><td><code>SubscriptionListResponse</code></td><td>Lists subscriptions for a customer, newest first. <code>status</code> is a Stripe status filter (e.g. <code>active</code>, <code>canceled</code>, <code>all</code>); omitted uses Stripe's default of non-canceled only.</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:paragraph -->
<p><code>CreateSubscriptionRequest(String priceId, String description, Map&lt;String,String&gt; metadata, String idempotencyKey)</code> — factory <code>of(priceId)</code> covers the common case. <code>SubscriptionStatusResponse(String subscriptionId, String status, String currentPeriodEnd)</code>. <code>SubscriptionListResponse(List&lt;SubscriptionStatusResponse&gt; subscriptions, boolean hasMore, String nextCursor)</code> — pass <code>nextCursor</code> back as <code>startingAfter</code> to page.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-checkout-and-invoices">Checkout and Invoices</h2>
<!-- /wp:heading -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th>Method</th><th>Path</th><th>Request</th><th>Response</th><th>Description</th></tr></thead><tbody><tr><td><code>POST</code></td><td><code>/stripe/checkout-session</code></td><td><code>CreateCheckoutSessionRequest</code></td><td><code>CreateCheckoutSessionResponse</code></td><td>Creates a Stripe-hosted Checkout Session and returns its URL.</td></tr><tr><td><code>GET</code></td><td><code>/stripe/customer/{customerId}/invoices?limit=10&amp;startingAfter=</code></td><td>—</td><td><code>List&lt;InvoiceSummary&gt;</code></td><td>Lists invoices for a customer, newest first, with cursor pagination.</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:paragraph -->
<p><code>CreateCheckoutSessionRequest(String customerId, String priceId, String successUrl, String cancelUrl, String mode, String idempotencyKey, Map&lt;String,String&gt; metadata)</code> — <code>mode</code> defaults to <code>subscription</code> if <code>null</code>; use <code>payment</code> for a one-off charge. <code>CreateCheckoutSessionResponse(String sessionId, String url)</code>. <code>InvoiceSummary(String id, String subscriptionId, Long amountPaid, String currency, String status, String createdAt)</code> — <code>status</code> is one of <code>draft</code>, <code>open</code>, <code>paid</code>, <code>uncollectible</code>, <code>void</code>.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-catalogue-products-and-prices">Catalogue: Products and Prices</h2>
<!-- /wp:heading -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th>Method</th><th>Path</th><th>Response</th><th>Description</th></tr></thead><tbody><tr><td><code>GET</code></td><td><code>/stripe/products?active=true&amp;limit=100</code></td><td><code>List&lt;ProductSummary&gt;</code></td><td>Lists products from the Stripe catalogue.</td></tr><tr><td><code>GET</code></td><td><code>/stripe/prices?productId=&amp;active=true&amp;limit=100</code></td><td><code>List&lt;PriceSummary&gt;</code></td><td>Lists prices, optionally filtered by product. Cached in memory (TTL: <code>dev.getelements.elements.stripe.price.cache.ttl.ms</code>).</td></tr><tr><td><code>GET</code></td><td><code>/stripe/prices/{priceId}</code></td><td><code>PriceSummary</code></td><td>Retrieves a single price by ID directly, without needing its product ID.</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:paragraph -->
<p><code>ProductSummary(String id, String name, String description, boolean active, PriceSummary defaultPrice)</code> — <code>active: false</code> means archived. <code>PriceSummary(String id, String productId, String nickname, Long unitAmount, String currency, String type, String interval)</code> — <code>unitAmount</code> is <code>null</code> for usage-based prices; <code>type</code> is <code>one_time</code> or <code>recurring</code>; <code>interval</code> is <code>day</code>, <code>week</code>, <code>month</code>, or <code>year</code> for recurring prices, otherwise <code>null</code>.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-billing-meters">Billing Meters</h2>
<!-- /wp:heading -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th>Method</th><th>Path</th><th>Request</th><th>Response</th><th>Description</th></tr></thead><tbody><tr><td><code>POST</code></td><td><code>/stripe/meter-event</code></td><td><code>RecordMeterEventRequest</code></td><td><code>204 No Content</code></td><td>Reports a usage event to a Stripe Billing Meter. Throws <code>NoSuchMeterException</code> (surfaced as an error response) if Stripe has no active meter for the given event name.</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:paragraph -->
<p><code>RecordMeterEventRequest(String customerId, String eventName, BigDecimal value, String idempotencyKey)</code> — <code>value</code> must be greater than zero; a <code>long</code>-value convenience constructor exists for whole-unit usage. The <code>idempotencyKey</code> is used both as Stripe's own meter-event deduplication identifier and as the HTTP idempotency key, so retrying with the same key never double-reports usage.</p>
<!-- /wp:paragraph -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"Note"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="3b0651"><div class="gb-notice-title" style="color:#fff"><p>Note</p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p><code>listMeters</code> and both overloads of <code>resolvePriceForMeterEventName</code> — the meter-to-price join described in <a href="stripe#h-products-prices-and-billing-meters">Stripe → Products, Prices, and Billing Meters</a> — are <code>StripeService</code> methods only. There is no REST endpoint for them; call them from an Element that depends on the Stripe <code>api</code> module.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-configuration-and-event-log-superuser">Configuration and Event Log (Superuser)</h2>
<!-- /wp:heading -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th>Method</th><th>Path</th><th>Request</th><th>Response</th><th>Description</th></tr></thead><tbody><tr><td><code>GET</code></td><td><code>/stripe/config</code></td><td>—</td><td><code>StripeConfig</code></td><td>Returns the effective Stripe credentials with values masked (e.g. <code>••••1234</code>).</td></tr><tr><td><code>PUT</code></td><td><code>/stripe/config</code></td><td><code>StripeConfig</code></td><td><code>{"saved":true}</code></td><td>Persists Stripe credentials to the database, overriding the deployment's attributes.</td></tr><tr><td><code>GET</code></td><td><code>/stripe/events?type=&amp;limit=20&amp;offset=0</code></td><td>—</td><td><code>StripeEventLogResponse</code></td><td>Lists received webhook events, newest first, with type filtering and offset pagination.</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:paragraph -->
<p><code>StripeConfig(String apiKey, String webhookSecret)</code>. <code>StripeEventLogResponse(List&lt;StripeEventLogEntry&gt; events, long total, boolean hasMore)</code>, where <code>StripeEventLogEntry(String stripeEventId, String eventType, String receivedAt)</code>. See <a href="configuring-the-stripe-element">Configuring the Stripe Element</a> for credential precedence.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-service-only-methods">Service-Only Methods</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Not every <code>StripeService</code> capability is exposed over REST. The following are reachable only by injecting <code>StripeService</code> from another Element that depends on Stripe's <code>api</code> module — there is deliberately no HTTP surface for them, either because they're building blocks for server-side flows (customer/payment-method setup ahead of a charge) or because they're only meaningful to trusted server code (recording a receipt from inside the webhook handler):</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><code>createCustomer(email, name, orgId)</code></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>createSetupIntent(customerId)</code></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>listPaymentMethods(customerId)</code> / <code>hasPaymentMethod(customerId)</code></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>getProduct(productId)</code></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>listMeters(activeOnly, limit)</code></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>resolvePriceForMeterEventName(eventName)</code> and <code>resolvePriceForMeterEventName(eventName, subscriptionId)</code></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>recordPaymentReceipt(transactionId, amount, currency, userId)</code> — called internally by the webhook handler; not intended to be called from arbitrary server code</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-related-pages">Related Pages</h2>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><a href="stripe">Stripe</a> — Element overview and core concepts</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="configuring-the-stripe-element">Configuring the Stripe Element</a> — attributes and credential precedence</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="stripe-webhooks-and-events">Stripe Webhooks and the Typed Event Bus</a> — the webhook endpoint and typed events in detail</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->
