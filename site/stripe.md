<h1>Stripe</h1>

<!-- wp:paragraph -->
<p>Stripe is an <code>Element</code> that integrates <a href="https://stripe.com">Stripe</a> payment processing into the Namazu Elements SDK. It wraps the Stripe Java SDK behind a single <code>StripeService</code> interface covering customers, one-off payments, subscriptions, Stripe-hosted Checkout and the Customer Portal, and usage-based billing via Stripe Billing Meters — and it turns Stripe's webhook stream into strongly-typed internal events any other Element can subscribe to, without parsing a single webhook payload.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-overview">Overview</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Most of what Stripe exposes lives behind one exported service, <code>StripeService</code> (package <code>dev.getelements.elements.stripe.service</code>, in the <code>api</code> module). Any Element that declares a dependency on Stripe can inject it via the service locator and call it directly — creating customers, charging one-off payments, managing subscriptions, resolving prices, and recording metered usage. A REST layer re-exposes the player-facing subset of that surface under an authenticated API, and a webhook receiver keeps Stripe's own state (payments, invoices, subscriptions) in sync by publishing typed events whenever Stripe calls back.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Credentials and webhook signing secrets can be set as deployment attributes or overridden at runtime through a superuser admin panel, without a rebuild or redeploy. A small MongoDB-backed audit log records every webhook Stripe has ever sent, viewable from the same panel.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-key-features">Key Features</h2>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Customer, PaymentIntent, SetupIntent, and Subscription management through one <code>StripeService</code> interface, plus Stripe-hosted Checkout Sessions and Customer Portal sessions</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Usage-based billing via Stripe Billing Meters, including a meter-to-price join Stripe's own API doesn't provide (see <a href="#h-products-prices-and-billing-meters">Products, Prices, and Billing Meters</a> below)</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>A signature-verified webhook receiver that publishes both a raw event and, for the most common event types, a strongly-typed record via <code>Element.publish</code> (see <a href="stripe-webhooks-and-events">Stripe Webhooks and the Typed Event Bus</a>)</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>An authenticated REST API for front-end and server-to-server use (see <a href="stripe-rest-api-reference">Stripe REST API Reference</a>)</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>A superuser admin UI panel for configuring credentials and browsing the webhook event log, with values persisted to MongoDB so they override the Element's deployment attributes without a rebuild</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-module-structure">Module Structure</h2>
<!-- /wp:heading -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th>Module</th><th>Purpose</th></tr></thead><tbody><tr><td><code>api</code></td><td>Exported interfaces and data types: <code>StripeService</code>, request/response records, typed event records, and the <code>StripeEvents</code> name constants. Consumed by other Elements via a classified jar.</td></tr><tr><td><code>element</code></td><td>REST endpoints, Guice wiring, the webhook receiver, Morphia persistence, and the admin UI plugin. Builds the <code>.elm</code> archive.</td></tr><tr><td><code>debug</code></td><td>Local runner — boots a local MongoDB replica set via Docker, then the Elements runtime with this Element loaded.</td></tr><tr><td><code>ui</code></td><td>React/TypeScript admin panel (built only under the <code>build-ui</code> Maven profile) that registers a "Stripe" tab in the superuser dashboard for configuration and event-log viewing.</td></tr><tr><td><code>integration-test</code></td><td>Tests that exercise the real Stripe API in test mode, driven by environment variables.</td></tr><tr><td><code>consumer-test</code></td><td>A second, independent Element demonstrating how to consume Stripe's published events with <code>@ElementEventConsumer</code>.</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-core-concepts">Core Concepts</h2>
<!-- /wp:heading -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-customers-and-payment-methods">Customers and Payment Methods</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p><code>StripeService.createCustomer(email, name, orgId)</code> creates a Stripe Customer and stamps <code>orgId</code> into its metadata under the <code>orgId</code> key (<code>StripeService.METADATA_ORG_ID</code>). Later, <code>findCustomerByMetadata(metadataKey, metadataValue)</code> uses the Stripe Customer Search API to look that customer back up by the same key — the recommended find-or-create pattern, so deleting and recreating an org on your side doesn't create an orphaned duplicate Stripe customer. Neither the key nor the value may contain a single-quote character.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Before charging a customer or starting a subscription, attach a payment method via a SetupIntent: <code>createSetupIntent(customerId)</code> returns a client secret the front end passes to Stripe.js (<code>stripe.confirmCardSetup</code>) to collect a card without an immediate charge. <code>listPaymentMethods(customerId)</code> and the cheaper <code>hasPaymentMethod(customerId)</code> check what's on file.</p>
<!-- /wp:paragraph -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"Note"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="3b0649"><div class="gb-notice-title" style="color:#fff"><p>Note</p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p><code>createCustomer</code>, <code>createSetupIntent</code>, <code>listPaymentMethods</code>, and <code>hasPaymentMethod</code> are <code>StripeService</code> methods only — they have no REST endpoint. Call them from server-side code in an Element that depends on Stripe's <code>api</code> module. See <a href="stripe-rest-api-reference">Stripe REST API Reference</a> for the full list of what is and isn't exposed over HTTP.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-one-off-payments-with-paymentintent">One-Off Payments with PaymentIntent</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p><code>createPaymentIntent(CreatePaymentIntentRequest)</code> creates a Stripe PaymentIntent for a single charge and returns a client secret for the front end to confirm with <code>stripe.confirmCardPayment</code>. The request carries the amount (in the currency's smallest unit, e.g. cents), an ISO 4217 currency code, the target customer, optional metadata, and an optional <code>idempotencyKey</code> — supplying one makes retries safe, since Stripe returns the original PaymentIntent instead of creating a second charge. A <code>CreatePaymentIntentRequest.of(amount, currency, customerId)</code> factory covers the common case where nothing else needs to be set.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-subscriptions">Subscriptions</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p><code>createSubscription(customerId, CreateSubscriptionRequest)</code> starts a recurring subscription at a given price; the customer must already have a default payment method on file. <code>getSubscriptionStatus(subscriptionId)</code>, <code>listSubscriptionsByCustomer(customerId, status, limit, startingAfter)</code>, and <code>cancelSubscription(subscriptionId)</code> round out the lifecycle — cancellation is immediate, ending access right away rather than at the end of the billing period. For end-of-period cancellation, send the customer to the Customer Portal instead.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-checkout-sessions-and-the-customer-portal">Checkout Sessions and the Customer Portal</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Rather than building your own payment form, <code>createCheckoutSession(CreateCheckoutSessionRequest)</code> creates a Stripe-hosted Checkout page and returns its URL; redirecting the customer there hands Stripe the entire payment-collection flow, with Stripe redirecting back to your <code>successUrl</code> or <code>cancelUrl</code> when the customer is done. <code>mode</code> defaults to <code>subscription</code> if omitted; pass <code>payment</code> for a one-off charge. Metadata set on the request is stamped onto both the Checkout Session and the resulting Subscription or PaymentIntent, which is the easiest way to carry an <code>orgId</code> or SKU identifier through to your webhook handlers without a DAO lookup.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><code>createBillingPortalSession(customerId, returnUrl)</code> (exposed over REST as <em>Create a Customer Portal session</em>) creates a single-use Customer Portal URL where the customer can manage their own subscriptions and payment methods with no server-side billing logic in your game at all.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-products-prices-and-billing-meters">Products, Prices, and Billing Meters</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p><code>listProducts</code>, <code>getProduct</code>, <code>listPrices</code>, and <code>retrievePrice</code> read the Stripe product catalogue. <code>listPrices</code> results are cached in memory for <code>dev.getelements.elements.stripe.price.cache.ttl.ms</code> milliseconds (5 minutes by default) to avoid hitting Stripe on every catalogue page load.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Usage-based billing runs on Stripe <strong>Billing Meters</strong>. Stripe's own API has no "list prices by meter" endpoint, so <code>listMeters(activeOnly, limit)</code> fetches meters and active recurring prices separately and joins them in memory, keyed by each price's <code>recurring.meter</code> field, so each <code>MeterSummary</code> arrives with its billing <code>PriceSummary</code> already attached (or <code>null</code>, if no recurring price references that meter yet).</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Two overloads resolve a price from a meter's event name directly, without needing the rest of the catalogue:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><code>resolvePriceForMeterEventName(eventName)</code> — catalogue-wide, and cached (same TTL as <code>listPrices</code>). If more than one active price references the same meter — a customer on a legacy tier versus the current one, for example — this can only return an arbitrary match.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>resolvePriceForMeterEventName(eventName, subscriptionId)</code> — scoped to one subscription. It bypasses the cache and inspects that subscription's own line items for the price actually attached to it, which is how to disambiguate the legacy-tier case above.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>To report usage, call <code>recordMeterEvent(customerId, eventName, value, idempotencyKey)</code> with a <code>BigDecimal</code> quantity (e.g. <code>0.25</code>) — it's sent to Stripe as both the meter event's deduplication identifier and the HTTP idempotency key, so a retried call never double-reports usage. If Stripe has no active meter configured for <code>eventName</code>, this throws <code>NoSuchMeterException</code> rather than surfacing Stripe's raw error string, so callers can catch a specific type.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-webhooks-and-the-typed-event-bus">Webhooks and the Typed Event Bus</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Stripe calls back into a single signature-verified webhook endpoint, which publishes a raw event for every webhook it receives and, for the most common event types, a strongly-typed record other Elements can subscribe to with <code>@ElementEventConsumer</code> — no webhook JSON parsing required. See <a href="stripe-webhooks-and-events">Stripe Webhooks and the Typed Event Bus</a> for the full event list, Dashboard setup, and local testing with the Stripe CLI.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-configuration-and-persistence">Configuration and Persistence</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The Stripe API key and webhook signing secret are required deployment attributes, overridable at runtime from a superuser admin panel without a rebuild. Two MongoDB collections back the Element: one holding that runtime credential override, and one logging every webhook Stripe has ever sent for audit and troubleshooting. See <a href="configuring-the-stripe-element">Configuring the Stripe Element</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-related-pages">Related Pages</h2>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><a href="configuring-the-stripe-element">Configuring the Stripe Element</a> — required attributes, credential precedence, multi-environment deployment, and Maven coordinates</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="stripe-webhooks-and-events">Stripe Webhooks and the Typed Event Bus</a> — webhook verification, Stripe Dashboard setup, the full typed-event table, and consuming events from another Element</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="stripe-rest-api-reference">Stripe REST API Reference</a> — every REST endpoint, its request/response shape, and which <code>StripeService</code> methods are service-only</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->
