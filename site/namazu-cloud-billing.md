<h1>Namazu Cloud Billing</h1>

<!-- wp:paragraph -->
<p>Namazu Cloud bills through <a href="stripe">Stripe</a>. Billing lives at the organization level: an organization has a Stripe customer record and a payment method, and paying for <a href="namazu-cloud-instances">instances</a> and <a href="namazu-cloud-addons">add-ons</a> draws on it. This page covers the customer side of that setup: what is needed before you can use the platform, how to add a payment method, and what happens behind the scenes once you do.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-before-you-start">Before You Start</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>You need two things in place before an organization can spend money:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>An account and an organization.</strong> Sign up, then create an organization; the account that creates it becomes its owner. See <a href="namazu-cloud-account">Namazu Cloud Account</a> and <a href="namazu-cloud-organizations">Namazu Cloud Organizations</a>.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>A payment method on file.</strong> Namazu Cloud attempts to set up billing for a new organization automatically when it is created. If that did not complete, the billing page finishes it: a Stripe customer record is created for the organization, then you attach a card.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-the-billing-readiness-gate">The Billing-Readiness Gate</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Until a payment method is on file, Namazu Cloud blocks the paid actions: creating an instance and starting an add-on purchase both refuse to proceed. This is deliberate. Usage is metered as your instance runs, so the platform requires a way to charge before it provisions anything you will be billed for.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>The check is three layers deep, but you only ever see the outcome:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>If the organization has no Stripe customer yet, the action stops with a prompt to set up billing.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>If the organization is marked billing-ready, the action proceeds.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Otherwise Namazu Cloud checks Stripe live for a payment method, and treats a card on file as billing-ready even if the faster flag has not caught up yet.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>The billing page reflects this as a billing status: billing ready or not, whether a payment method exists, and the billing method in use. Owner-only for the view, as billing is an ownership responsibility.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-adding-a-payment-method">Adding a Payment Method</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Add a payment method by setting up billing on the organization page. The flow uses Stripe's SetupIntent, which lets your browser hand Stripe a card through a Stripe-provided form without the card details ever passing through Namazu Cloud, and without charging anything yet. Nothing is billed when you add the card; billing only starts when usage accrues or a subscription renews.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-what-to-expect-afterward">What to Expect Afterward</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Once Stripe confirms the payment method, a webhook tells Namazu Cloud the organization is billing-ready, and the flag flips. You can see the moment it does on the organization's billing status. Two independent Stripe events set the flag, so even an unusual path still converges.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Namazu Cloud also self-heals. If the notification is ever missed, late, or dropped, the next billing-readiness check asks Stripe directly, sees the card, and repairs the flag itself. The practical effect is that the gate does not lock out an organization that has genuinely added a card.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-purchasing-add-ons">Purchasing Add-ons</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Add-on purchases go through Stripe-hosted Checkout. Choosing an add-on opens a Stripe payment page for that add-on; depending on the add-on type it collects a one-time payment or starts a subscription, then returns you to the portal. A payment method must already be on file, because the purchase is charged to the organization's Stripe customer. See <a href="namazu-cloud-addons">Namazu Cloud Add-ons</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-managing-payments">Managing Payments</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The billing page opens the Stripe Customer Portal for the organization: a Stripe-hosted page where the owner can manage payment methods and, for add-on subscriptions, what Stripe lets a customer manage from there. Optionally Namazu Cloud configures a direct link to the banner-advertised payment page in the portal, and whether that link shows at all is a deployment setting.</p>
<!-- /wp:paragraph -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"Note"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="3b0649"><div class="gb-notice-title" style="color:#fff"><p>Note</p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p>Organizations run against a Stripe environment, either sandbox or production. Metered usage and add-on purchases are reported against the environment the organization is configured for. Which environment an organization is on is set by Namazu Cloud, and the products and prices an organization buys from are matched to it.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-operator-side">Operator Side</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>This page covers the customer side of the Stripe setup. For the operator side, the underlying Stripe integration is documented separately:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><a href="stripe">Stripe</a>. The Stripe Element and its service surface</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="configuring-the-stripe-element">Configuring the Stripe Element</a>. API keys, webhook signing, and deployment attributes</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="stripe-webhooks-and-events">Stripe Webhooks and the Typed Event Bus</a>. The webhook events the billing-readiness flag and add-on reconciliation depend on</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="stripe-rest-api-reference">Stripe REST API Reference</a>. The raw REST surface of the Stripe element</li>
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
<li><a href="namazu-cloud-overview">Namazu Cloud Overview</a></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="namazu-cloud-organizations">Namazu Cloud Organizations</a>. Billing is owned at the organization level</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="namazu-cloud-instances">Namazu Cloud Instances</a>. The gate that applies before you can create one</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="namazu-cloud-addons">Namazu Cloud Add-ons</a>. Purchases billed through the same Stripe setup</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->