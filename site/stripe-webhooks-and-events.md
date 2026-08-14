<h1>Stripe Webhooks and the Typed Event Bus</h1>

<!-- wp:paragraph -->
<p>The <a href="stripe">Stripe</a> Element keeps your platform's view of payments, invoices, and subscriptions in sync with Stripe by receiving webhooks at a single endpoint, verifying their signature, and republishing them as internal Elements events. Every verified webhook is published as a raw event; the most common event types are also published as strongly-typed records so consuming code never has to deserialize a Stripe payload itself.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-the-webhook-endpoint">The Webhook Endpoint</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p><code>POST /stripe/webhook</code> takes the raw request body and a <code>Stripe-Signature</code> header, and verifies them with the Stripe SDK's <code>Webhook.constructEvent</code> against the configured webhook secret. This endpoint requires no session authentication — Stripe can't supply a session secret — but every request is rejected unless its signature checks out:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>503</strong> if no webhook secret is configured yet</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>400</strong> if the signature doesn't verify</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>200</strong> <code>{"received":true}</code> once the event is published, or <code>{"received":true,"typed":false}</code> if the raw event was published but Stripe's payload couldn't be deserialized into a typed record (see below)</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>On every successfully verified webhook — regardless of type — the endpoint first publishes a <code>StripeRawEvent</code> and writes an entry to the event log, <em>then</em> attempts to dispatch a typed event. This ordering means a mismatch between this Element's Stripe SDK version and the shape of an incoming event (an <code>EventDataObjectDeserializationException</code>) never loses the event — the raw event and audit log entry are already recorded before the typed dispatch is attempted.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-stripe-dashboard-setup">Stripe Dashboard Setup</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>In the Stripe Dashboard, go to <strong>Developers → Webhooks → Add endpoint</strong>.</p>
<!-- /wp:paragraph -->

<!-- wp:list {"ordered":true} -->
<ol class="wp-block-list"><!-- wp:list-item -->
<li>Use the <strong>"Account"</strong> webhook type, not "Event destinations / v2" — the v2 format uses thin payloads and a different signing scheme that this Element does not support.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Set the endpoint URL to your deployed base URL plus the webhook path, e.g. <code>https://your-host/element/stripe/api/stripe/webhook</code>.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Subscribe to the event types you need. Any webhook that arrives with a valid signature is always forwarded as a <code>RAW_WEBHOOK</code> event regardless of which types you subscribe to; the typed events below only fire for their corresponding subscribed types.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>After saving, Stripe shows a <strong>Signing secret</strong> (<code>whsec_...</code>). Copy it into the <code>dev.getelements.elements.stripe.webhook.secret</code> attribute — see <a href="configuring-the-stripe-element">Configuring the Stripe Element</a>.</li>
<!-- /wp:list-item --></ol>
<!-- /wp:list -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-testing-webhooks-locally">Testing Webhooks Locally</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Forward Stripe events to a local instance with the <a href="https://stripe.com/docs/stripe-cli">Stripe CLI</a>:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>stripe listen --forward-to localhost:8080/element/stripe/api/stripe/webhook</code></pre>
<!-- /wp:code -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-typed-events">Typed Events</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Constants live on <code>StripeEvents</code> in the <code>api</code> module. Each is declared at the class level of the webhook endpoint with <code>@ElementEventProducer</code>, and each carries strongly-typed fields pulled straight off the corresponding Stripe object — no payload parsing required in the consumer.</p>
<!-- /wp:paragraph -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th><code>StripeEvents</code> constant</th><th>Stripe event type</th><th>Record</th><th>Fields</th></tr></thead><tbody><tr><td><code>PAYMENT_SUCCEEDED</code></td><td><code>payment_intent.succeeded</code></td><td><code>StripePaymentSucceededEvent</code></td><td><code>paymentIntentId</code>, <code>amount</code>, <code>currency</code>, <code>customerId</code></td></tr><tr><td><code>PAYMENT_FAILED</code></td><td><code>payment_intent.payment_failed</code></td><td><code>StripePaymentFailedEvent</code></td><td><code>paymentIntentId</code>, <code>failureMessage</code>, <code>customerId</code></td></tr><tr><td><code>PAYMENT_CANCELED</code></td><td><code>payment_intent.canceled</code></td><td><code>StripePaymentCanceledEvent</code></td><td><code>paymentIntentId</code>, <code>customerId</code></td></tr><tr><td><code>INVOICE_PAYMENT_SUCCEEDED</code></td><td><code>invoice.payment_succeeded</code></td><td><code>StripeInvoicePaymentSucceededEvent</code></td><td><code>invoiceId</code>, <code>paymentIntentId</code>, <code>amountPaid</code>, <code>currency</code></td></tr><tr><td><code>INVOICE_PAYMENT_FAILED</code></td><td><code>invoice.payment_failed</code></td><td><code>StripeInvoicePaymentFailedEvent</code></td><td><code>invoiceId</code>, <code>subscriptionId</code>, <code>customerId</code>, <code>failureMessage</code></td></tr><tr><td><code>SUBSCRIPTION_CREATED</code></td><td><code>customer.subscription.created</code></td><td><code>StripeSubscriptionCreatedEvent</code></td><td><code>subscriptionId</code>, <code>customerId</code>, <code>status</code>, <code>orgId</code></td></tr><tr><td><code>SUBSCRIPTION_UPDATED</code></td><td><code>customer.subscription.updated</code></td><td><code>StripeSubscriptionUpdatedEvent</code></td><td><code>subscriptionId</code>, <code>customerId</code>, <code>status</code>, <code>orgId</code></td></tr><tr><td><code>SUBSCRIPTION_CANCELLED</code></td><td><code>customer.subscription.deleted</code></td><td><code>StripeSubscriptionCancelledEvent</code></td><td><code>subscriptionId</code>, <code>customerId</code>, <code>orgId</code></td></tr><tr><td><code>SUBSCRIPTION_TRIAL_WILL_END</code></td><td><code>customer.subscription.trial_will_end</code></td><td><code>StripeSubscriptionTrialWillEndEvent</code></td><td><code>subscriptionId</code>, <code>customerId</code>, <code>trialEnd</code> (ISO-8601), <code>orgId</code></td></tr><tr><td><code>SETUP_INTENT_SUCCEEDED</code></td><td><code>setup_intent.succeeded</code></td><td><code>StripeSetupIntentSucceededEvent</code></td><td><code>setupIntentId</code>, <code>customerId</code></td></tr><tr><td><code>PAYMENT_METHOD_ATTACHED</code></td><td><code>payment_method.attached</code></td><td><code>StripePaymentMethodAttachedEvent</code></td><td><code>paymentMethodId</code>, <code>customerId</code></td></tr><tr><td><code>CHECKOUT_SESSION_COMPLETED</code></td><td><code>checkout.session.completed</code></td><td><code>StripeCheckoutSessionCompletedEvent</code></td><td><code>sessionId</code>, <code>customerId</code>, <code>paymentIntentId</code>, <code>subscriptionId</code>, <code>mode</code>, <code>metadata</code></td></tr><tr><td><code>RAW_WEBHOOK</code></td><td><code>stripe.webhook</code></td><td><code>StripeRawEvent</code></td><td><code>type</code>, <code>eventId</code>, <code>rawJson</code> — published for <strong>every</strong> verified webhook, including all of the above</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:paragraph -->
<p>The <code>orgId</code> field on subscription events is read from the Stripe object's metadata under <code>StripeService.METADATA_ORG_ID</code> — it's only populated if the subscription (or its customer) was created with that metadata key set, e.g. via <code>createCustomer</code>'s <code>orgId</code> parameter or a Checkout Session's <code>metadata</code>.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Two of these event types also trigger a side effect inside the webhook handler itself: on <code>PAYMENT_SUCCEEDED</code> and <code>INVOICE_PAYMENT_SUCCEEDED</code>, the handler calls <code>StripeService.recordPaymentReceipt</code> to record a receipt in the platform receipt store, using the payment's <code>userId</code> metadata (silently skipped if that metadata is absent or blank).</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-consuming-events-from-another-element">Consuming Events from Another Element</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Any Element that depends on Stripe's <code>api</code> module can subscribe to these events with <code>@ElementEventConsumer</code>, annotating a method on any Guice-managed service:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>import com.google.inject.Inject;
import dev.getelements.elements.sdk.annotation.ElementEventConsumer;
import dev.getelements.elements.stripe.StripeEvents;

public class EntitlementService {

    @Inject
    private UserInventoryDao userInventoryDao;

    @ElementEventConsumer(StripeEvents.PAYMENT_SUCCEEDED)
    public void onPaymentSucceeded() {
        // called whenever a payment_intent.succeeded webhook is received
    }

    @ElementEventConsumer(StripeEvents.SUBSCRIPTION_CANCELLED)
    public void onSubscriptionCancelled() {
        // revoke access, notify player, etc.
    }

    @ElementEventConsumer(StripeEvents.RAW_WEBHOOK)
    public void onAnyWebhook() {
        // called for every verified Stripe webhook — use for event types
        // that don't have a dedicated typed event
    }
}</code></pre>
<!-- /wp:code -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-the-webhook-event-log">The Webhook Event Log</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Every verified webhook — typed or not — is also recorded to a MongoDB-backed audit log (<code>StripeEventLogService</code>, collection <code>stripe_event_log</code>) storing the Stripe event ID, event type, and receipt timestamp. Browse it via the Stripe tab in the superuser admin panel, or query it directly with <code>GET /stripe/events?type=&amp;limit=20&amp;offset=0</code>, which supports filtering by event type and offset-based pagination. This is the fastest way to confirm whether a webhook actually arrived and how it was typed, without digging through application logs.</p>
<!-- /wp:paragraph -->

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
<li><a href="configuring-the-stripe-element">Configuring the Stripe Element</a> — setting the webhook secret and other attributes</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="stripe-rest-api-reference">Stripe REST API Reference</a> — the webhook endpoint alongside every other REST endpoint</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->
