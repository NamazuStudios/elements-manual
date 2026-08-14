<h1>Configuring the Stripe Element</h1>

<!-- wp:paragraph -->
<p>The <a href="stripe">Stripe</a> Element is configured through standard Elements deployment attributes, with two of them — the API key and webhook signing secret — overridable at runtime from a superuser admin panel without a rebuild or redeploy. This page covers the available attributes, how credential precedence works, running multiple environments safely, and the Maven coordinates other Elements need to depend on it.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-attributes">Attributes</h2>
<!-- /wp:heading -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th>Attribute</th><th>Default</th><th>Description</th></tr></thead><tbody><tr><td><code>dev.getelements.elements.stripe.api.key</code></td><td><em>none — required</em></td><td>Stripe secret API key (<code>sk_live_...</code> or <code>sk_test_...</code>). Marked sensitive.</td></tr><tr><td><code>dev.getelements.elements.stripe.webhook.secret</code></td><td><em>none — required</em></td><td>Stripe webhook signing secret (<code>whsec_...</code>), shown by Stripe when you register a webhook endpoint. Marked sensitive.</td></tr><tr><td><code>dev.getelements.elements.element.rs.root</code></td><td><code>/element/stripe/api</code></td><td>REST API root path for every endpoint this Element exposes.</td></tr><tr><td><code>dev.getelements.elements.auth.enabled</code></td><td><code>true</code></td><td>Enables the Elements session auth filter for this Element's endpoints.</td></tr><tr><td><code>dev.getelements.elements.stripe.price.cache.ttl.ms</code></td><td><code>300000</code> (5 minutes)</td><td>How long <code>listPrices</code> and the catalogue-wide <code>resolvePriceForMeterEventName</code> cache their results in memory before re-querying Stripe.</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-setting-attributes">Setting Attributes</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p><strong>Deployed instances</strong> — set attributes from the admin panel under Element Management. Select the deployment and edit its attributes directly; no rebuild is required.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><strong>Local development</strong> — set attributes as system properties when launching the <code>debug</code> module:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>mvn -pl debug exec:java \
  -Ddev.getelements.elements.stripe.api.key=sk_test_YOUR_KEY \
  -Ddev.getelements.elements.stripe.webhook.secret=whsec_YOUR_SECRET</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>Defaults can also be baked into <code>element/src/main/elm/dev.getelements.element.attributes.properties</code> so they're packaged into the <code>.elm</code> archive itself — these are still overridden by anything set at the deployment level.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-credential-precedence-and-the-admin-panel">Credential Precedence and the Admin Panel</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The API key and webhook secret can also be set from the "Stripe" tab in the superuser admin dashboard, or directly via <code>PUT /stripe/config</code>. Values saved this way are persisted to a MongoDB document and take precedence over the deployment attributes whenever they're non-blank — <code>GET /stripe/config</code> returns the effective values with everything but the last four characters masked (e.g. <code>••••1234</code>), so the full secret is never re-displayed once saved.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>This lets an operator rotate a compromised key or point a deployment at a different Stripe account without touching deployment attributes or redeploying. If no override has been saved, the Element falls back to the injected attribute values above.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-multi-environment-deployments">Multi-Environment Deployments</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Run separate environments (e.g. <code>MyGame_Dev</code>, <code>MyGame_Staging</code>, <code>MyGame_Prod</code>) by deploying a distinct instance of this Element per environment, each with:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Its own Stripe API key and webhook secret, configured independently through the Stripe → Configuration admin tab</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Its own MongoDB database (or at minimum a separate MongoDB instance), so configuration and event-log data are naturally isolated with no extra namespacing required</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>In the Elements platform this maps directly to creating one application deployment per environment. Because each deployment connects to its own database, the DAO layer needs no special configuration to achieve isolation.</p>
<!-- /wp:paragraph -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"Warning","noticeBackgroundColor":"#ffdd57"} -->
<div style="color:#32373c;background-color:#ffdd57" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="3b0650"><div class="gb-notice-title" style="color:#fff"><p>Warning</p></div><div class="gb-notice-text" style="border-color:#ffdd57"><!-- wp:paragraph -->
<p>Do not share one MongoDB database between multiple deployments of this Element. The configuration and webhook event-log collections use fixed, unnamespaced names (<code>stripe_config</code> and <code>stripe_event_log</code>), so sharing a database lets one deployment's credentials and event log bleed into another's.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-build-and-run">Build and Run</h2>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code># Build everything (requires Java 21 + Maven)
mvn install

# Start local MongoDB
docker compose -f services-dev/docker-compose.yml up -d

# Run locally
mvn -pl debug exec:java</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>The admin UI panel is only built under the <code>build-ui</code> Maven profile (<code>mvn install -Pbuild-ui</code>); its React bundle is copied into the <code>.elm</code> archive's <code>ui/</code> directory and registers a "Stripe" tab (icon: <code>CreditCard</code>, route: <code>stripe</code>) in the superuser dashboard.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-maven-coordinates">Maven Coordinates</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Deploy the <code>.elm</code> archive built by the <code>element</code> module. Other Elements that call <code>StripeService</code> directly (rather than only consuming its events or REST API) should also depend on the <code>api</code> module, provided at compile time:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>&lt;!-- .elm archive (for deployment) --&gt;
&lt;dependency&gt;
    &lt;groupId&gt;dev.getelements.elements.stripe&lt;/groupId&gt;
    &lt;artifactId&gt;element&lt;/artifactId&gt;
    &lt;version&gt;1.0.3&lt;/version&gt;
    &lt;type&gt;elm&lt;/type&gt;
&lt;/dependency&gt;

&lt;!-- API interfaces (for other Elements that depend on this one) --&gt;
&lt;dependency&gt;
    &lt;groupId&gt;dev.getelements.elements.stripe&lt;/groupId&gt;
    &lt;artifactId&gt;api&lt;/artifactId&gt;
    &lt;version&gt;1.0.3&lt;/version&gt;
    &lt;classifier&gt;dev.getelements.elements.stripe.api&lt;/classifier&gt;
    &lt;scope&gt;provided&lt;/scope&gt;
&lt;/dependency&gt;</code></pre>
<!-- /wp:code -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-integration-tests">Integration Tests</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Integration tests exercise the real Stripe API in test mode. Credentials come from environment variables so CI can inject them as secrets:</p>
<!-- /wp:paragraph -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th>Env var</th><th>Maven property</th><th>Purpose</th></tr></thead><tbody><tr><td><code>STRIPE_TEST_API_KEY</code></td><td><code>stripe.test.apiKey</code></td><td>Stripe test-mode secret key</td></tr><tr><td><code>STRIPE_TEST_CUSTOMER_ID</code></td><td><code>stripe.test.customerId</code></td><td>Existing test-mode customer to reuse</td></tr><tr><td><code>STRIPE_TEST_PRICE_ID</code></td><td><code>stripe.test.priceId</code></td><td>Existing test-mode price to reuse, instead of creating a new Product + Price every run</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:code -->
<pre class="wp-block-code"><code>export STRIPE_TEST_API_KEY=sk_test_YOUR_KEY
mvn verify -pl integration-test</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>Each variable can also be overridden per run with the matching <code>-Dstripe.test.*</code> system property. Subscription tests create and tear down a real subscription automatically. Webhook tests generate and verify their own HMAC signatures against a fixed, non-secret test key, and run without any network connection or Stripe credentials.</p>
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
<li><a href="stripe-webhooks-and-events">Stripe Webhooks and the Typed Event Bus</a> — webhook Dashboard setup and the typed event list</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="stripe-rest-api-reference">Stripe REST API Reference</a> — full REST endpoint reference</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->
