<h1>Product Bundles and SKUs</h1>

<!-- wp:paragraph -->
<p>Namazu Element Version 3.8+</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-overview">Overview</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The Commerce feature provides two cooperating resources that map payment-provider purchases to&nbsp;in-game rewards.</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>Product SKU Schemas</strong> — a registry of payment-provider identifiers (e.g. <code>com.apple.appstore</code>) used across the system. The four built-in IAP providers are seeded automatically on startup.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Product Bundles</strong> — per-application configurations that map a provider SKU or product id to a set of items to award when a verified purchase is received. All read and write operations are superuser-only; only <code>processVerifiedPurchase</code> is executed in a user context (called internally by the receipt services).</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>When an IAP receipt is verified,&nbsp;the receipt service calls&nbsp;<code>ProductBundleService.processVerifiedPurchase(schema, productId, originalTransactionId)</code>,&nbsp;which&nbsp;looks up the matching bundle for the user's application and issues the configured&nbsp;<code>RewardIssuance</code>&nbsp;records idempotently.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="product-sku-schema-model">Product SKU Schema model</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p><strong>Class:</strong>&nbsp;<code>dev.getelements.elements.sdk.model.goods.ProductSkuSchema</code></p>
<!-- /wp:paragraph -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th>Field</th><th>Type</th><th>Description</th></tr></thead><tbody><tr><td><code>id</code></td><td><code>String</code></td><td>Database id. Assigned on creation; null on write.</td></tr><tr><td><code>schema</code></td><td><code>String</code></td><td><strong>Required.</strong>&nbsp;Reverse-DNS payment-provider identifier, e.g.&nbsp;<code>com.apple.appstore</code>.</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="product-bundle-model">Product Bundle model</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p><strong>Class:</strong>&nbsp;<code>dev.getelements.elements.sdk.model.goods.ProductBundle</code></p>
<!-- /wp:paragraph -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th>Field</th><th>Type</th><th>Description</th></tr></thead><tbody><tr><td><code>id</code></td><td><code>String</code></td><td>Database id. Null on create, required on update</td></tr><tr><td><code>schema</code></td><td><code>String</code></td><td><strong>Required.</strong>&nbsp;Reverse-DNS payment-provider identifier.</td></tr><tr><td><code>application</code></td><td><code>Application</code></td><td>The owning application. Required on create.</td></tr><tr><td><code>productId</code></td><td><code>String</code></td><td><strong>Required.</strong>&nbsp;Product id as it appears in the provider's catalog.</td></tr><tr><td><code>displayName</code></td><td><code>String</code></td><td>Optional display title shown to end users.</td></tr><tr><td><code>description</code></td><td><code>String</code></td><td>Optional description shown to end users.</td></tr><tr><td><code>display</code></td><td><code>boolean</code></td><td>Whether the frontend should surface this bundle to users. Defaults to&nbsp;<code>false</code>.</td></tr><tr><td><code>productBundleRewards</code></td><td><code>List&lt;ProductBundleReward&gt;</code></td><td>Rewards issued on purchase. See below.</td></tr><tr><td><code>metadata</code></td><td><code>Map&lt;String, Object&gt;</code></td><td>Arbitrary key-value metadata for application use.</td></tr><tr><td><code>tags</code></td><td><code>List&lt;String&gt;</code></td><td>Searchable tags for filtering.</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="productbundlereward">ProductBundleReward</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p><strong>Class:</strong>&nbsp;<code>dev.getelements.elements.sdk.model.application.ProductBundleReward</code></p>
<!-- /wp:paragraph -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th>Field</th><th>Type</th><th>Description</th></tr></thead><tbody><tr><td><code>itemId</code></td><td><code>String</code></td><td><strong>Required.</strong>&nbsp;Database id of the&nbsp;<code>Item</code>&nbsp;to award.</td></tr><tr><td><code>quantity</code></td><td><code>Integer</code></td><td>Quantity to award. Omit (or&nbsp;<code>null</code>) for&nbsp;<code>DISTINCT</code>&nbsp;items; positive integer for&nbsp;<code>FUNGIBLE</code>&nbsp;items. If&nbsp;<code>null</code>&nbsp;on a&nbsp;<code>FUNGIBLE</code>&nbsp;item the service defaults to&nbsp;<code>1</code>.</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:paragraph -->
<p>The compound key&nbsp;<strong>(application, schema, productId)</strong>&nbsp;is unique&nbsp;—&nbsp;only one bundle may exist for a&nbsp;given product in a given application.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="dao-interface-productbundledao">DAO interface (ProductBundleDao)</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p><strong>Interface:</strong>&nbsp;<code>dev.getelements.elements.sdk.dao.ProductBundleDao</code></p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="key-methods">Key methods</h3>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code><code>// Paginated listing — all filters are optional; null/blank values are ignored
Pagination&lt;ProductBundle> getProductBundles(int offset, int count);
Pagination&lt;ProductBundle> getProductBundles(String applicationNameOrId, int offset, int count);
Pagination&lt;ProductBundle> getProductBundles(String applicationNameOrId, String schema, int offset, int count);
Pagination&lt;ProductBundle> getProductBundles(String applicationNameOrId, String schema,
                                             String productId, List&lt;String> tags, int offset, int count);
Pagination&lt;ProductBundle> getProductBundlesByTag(String tag, int offset, int count);</code></code></pre>
<!-- /wp:code -->

<!-- wp:code -->
<pre class="wp-block-code"><code><code>// Lookup
ProductBundle getProductBundle(String id);
ProductBundle getProductBundle(String applicationNameOrId, String schema, String productId);<span style="background-color: initial; font-family: inherit; font-size: inherit; text-align: initial; color: initial;"></span></code></code></pre>
<!-- /wp:code -->

<!-- wp:code -->
<pre class="wp-block-code"><code><code>// Writes — always use inside a transaction
ProductBundle createProductBundle(ProductBundle bundle);  // throws DuplicateException on conflict
ProductBundle updateProductBundle(ProductBundle bundle);
void deleteProductBundle(String id);                      // throws NotFoundException if missing</code></code></pre>
<!-- /wp:code -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="transaction-usage-recommended">Transaction usage (recommended)</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Always perform DAO writes inside a transaction for consistency and automatic retry on failure.</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code><code>@Inject
private Provider&lt;Transaction> transactionProvider;

final var created = transactionProvider.get().performAndClose(tx -> {
    return tx.getDao(ProductBundleDao.class).createProductBundle(bundle);
});<span style="background-color: initial; font-family: inherit; font-size: inherit; text-align: initial; color: initial;"></span></code></code></pre>
<!-- /wp:code -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="dao-interface-productskuschemadao">DAO interface (ProductSkuSchemaDao)</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p><strong>Interface:</strong>&nbsp;<code>dev.getelements.elements.sdk.dao.ProductSkuSchemaDao</code></p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="key-methods-1">Key methods</h3>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code><code>Pagination&lt;ProductSkuSchema> getProductSkuSchemas(int offset, int count);
ProductSkuSchema getProductSkuSchema(String id);

// Idempotent create — returns existing record if the schema string already exists
ProductSkuSchema createProductSkuSchema(ProductSkuSchema productSkuSchema);

// Upsert by schema string — safe to call repeatedly; used by seeder and provider plugins
ProductSkuSchema ensureProductSkuSchema(String schema);

void deleteProductSkuSchema(String id)</code></code></pre>
<!-- /wp:code -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="service-interfaces">Service interfaces</h2>
<!-- /wp:heading -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="productbundleservice">ProductBundleService</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p><strong>Interface:</strong> <code>dev.getelements.elements.sdk.service.goods.ProductBundleService</code> </p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><strong>Annotations:</strong> <code>@ElementPublic</code>, <code>@ElementServiceExport</code>, <code>@ElementServiceExport(name = UNSCOPED)</code></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>All CRUD methods (<code>getProductBundles</code>, <code>getProductBundle</code>, <code>createProductBundle</code>, <code>updateProductBundle</code>, <code>deleteProductBundle</code>) are SUPERUSER only. Regular USER level receives a <code>ForbiddenException</code> if they call them directly.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>However, the key USER-context method is:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code><code>/**
 * Looks up the ProductBundle for the current user's application matching
 * (schema, productId) and issues a RewardIssuance for each configured reward.
 * Uses originalTransactionId as an idempotency key — repeated calls with the
 * same arguments will not create duplicate rewards.
 *
 * Returns an empty list if no matching bundle is found.
 */
List&lt;RewardIssuance> processVerifiedPurchase(
        String schema,
        String productId,
        String originalTransactionId);</code></code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>This method is called automatically by the built-in receipt services after a purchase is verified. You should only call it directly if you are implementing a custom payment provider, and only after the receipt has been verified by the payment provider.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="productskuschemaservice">ProductSkuSchemaService</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p><strong>Interface:</strong> <code>dev.getelements.elements.sdk.service.goods.ProductSkuSchemaService</code> </p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><strong>Annotations:</strong> <code>@ElementPublic</code>, <code>@ElementServiceExport</code>, <code>@ElementServiceExport(name = UNSCOPED)</code></p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code><code>Pagination&lt;ProductSkuSchema> getProductSkuSchemas(int offset, int count);
ProductSkuSchema getProductSkuSchema(String id);
ProductSkuSchema createProductSkuSchema(ProductSkuSchema productSkuSchema);  // idempotent
ProductSkuSchema ensureProductSkuSchema(String schema);                      // upsert by string
void deleteProductSkuSchema(String id);<span style="background-color: initial; font-family: inherit; font-size: inherit; text-align: initial; color: initial;"></span></code></code></pre>
<!-- /wp:code -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="seeding">Seeding</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>At startup,&nbsp;<code>ProductSkuSchemaSeeder</code>&nbsp;automatically calls&nbsp;<code>ensureProductSkuSchema</code>&nbsp;for all&nbsp;four built-in provider schemas so they are always available:</p>
<!-- /wp:paragraph -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th>Provider</th><th>Schema constant</th><th>Value</th></tr></thead><tbody><tr><td>Apple App Store</td><td><code>AppleIapReceiptService.APPLE_IAP_SCHEME</code></td><td><code>com.apple.appstore</code></td></tr><tr><td>Google Play</td><td><code>GooglePlayIapReceiptService.GOOGLE_IAP_SCHEME</code></td><td><code>com.android.vending</code></td></tr><tr><td>Meta Quest / Oculus</td><td><code>OculusIapReceiptService.OCULUS_IAP_SCHEME</code></td><td><code>com.oculus.platform</code></td></tr><tr><td>Meta / Facebook Platform</td><td><code>FacebookIapReceiptService.FACEBOOK_IAP_SCHEME</code></td><td><code>com.facebook.platform</code></td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:paragraph -->
<p>Custom schemas are registered the same way&nbsp;—&nbsp;call&nbsp;<code>ensureProductSkuSchema</code>&nbsp;from your Element's&nbsp;startup code.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="how-processverifiedpurchase-works">How processVerifiedPurchase works</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Call this method after your payment provider confirms a purchase.&nbsp;It resolves the correct&nbsp;<code>ProductBundle</code>&nbsp;for the authenticated user's application and issues the configured rewards&nbsp;in a single transactional step:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code><code>List&lt;RewardIssuance> issuances = productBundleService.processVerifiedPurchase(
        MYPROVIDER_IAP_SCHEME,   // schema — identifies the payment provider
        providerProductId,        // productId — as it appears in the provider's catalog
        providerTransactionId     // originalTransactionId — used as the idempotency key
);
// issuances is empty if no bundle is configured for this product; never nul<span style="background-color: initial; font-family: inherit; font-size: inherit; text-align: initial; color: initial;">l</span><span style="background-color: initial; font-family: inherit; font-size: inherit; text-align: initial; color: initial;"></span></code></code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>The method will:</p>
<!-- /wp:paragraph -->

<!-- wp:list {"ordered":true} -->
<ol class="wp-block-list"><!-- wp:list-item -->
<li>Resolve the current user's application from their active <code>Profile</code>.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Look up the <code>ProductBundle</code> matching <code>(application, schema, productId)</code>. If none is found, return an empty list — this is not treated as an error.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Inside a transaction, create a <code>RewardIssuance</code> for each entry in <code>productBundleRewards</code>, using <code>rewardIssuanceDao.getOrCreateRewardIssuance</code> so repeated calls with the same <code>originalTransactionId</code> are safe and will not produce duplicate rewards.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Return the list of <code>RewardIssuance</code> records that were created or already existed.</li>
<!-- /wp:list-item --></ol>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>The idempotency key for each reward entry is derived from&nbsp;<code>"product-bundle.&lt;originalTransactionId&gt;.&lt;itemId&gt;.&lt;index&gt;"</code>,&nbsp;so use a stable,&nbsp;provider-assigned transaction id to get correct deduplication behaviour across retries.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>For reference,&nbsp;the core logic looks like this:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code><code>// Annotated illustration — see UserProductBundleService for the authoritative source
List&lt;RewardIssuance> processVerifiedPurchase(String schema, String productId, String txId) {
    final var applicationId = currentProfileSupplier.get().getApplication().getId();

    final ProductBundle bundle;
    try {
        bundle = productBundleDao.getProductBundle(applicationId, schema, productId);
    } catch (NotFoundException e) {
        return List.of(); // no bundle configured — not an error
    }

    return transactionProvider.get().performAndClose(tx -> {
        final var rewardIssuanceDao = tx.getDao(RewardIssuanceDao.class);
        final var itemDao = tx.getDao(ItemDao.class);
        final var issuances = new ArrayList&lt;RewardIssuance>();

        for (int i = 0; i &lt; bundle.getProductBundleRewards().size(); i++) {
            final var reward = bundle.getProductBundleRewards().get(i);
            final var item = itemDao.getItemByIdOrName(reward.getItemId());
            final int qty = DISTINCT.equals(item.getCategory()) ? 1
                    : (reward.getQuantity() != null ? reward.getQuantity() : 1);

            final var issuance = new RewardIssuance();
            issuance.setUser(user);
            issuance.setItem(item);
            issuance.setItemQuantity(qty);
            issuance.setType(PERSISTENT);
            issuance.setState(ISSUED);
            issuance.setSource("PRODUCT_BUNDLE." + schema + "." + productId);
            issuance.setContext("product-bundle." + txId + "." + reward.getItemId() + "." + i);

            issuances.add(rewardIssuanceDao.getOrCreateRewardIssuance(issuance));
        }

        return issuances;
    });
}</code></code></pre>
<!-- /wp:code -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="adding-a-new-payment-provider">Adding a new payment provider</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>If you are building a custom Element for a payment provider not already supported,&nbsp;follow these&nbsp;steps.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="1-register-a-schema-string">1. Register a schema string</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Use reverse-DNS notation and declare a constant:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code><code>String MYPROVIDER_IAP_SCHEME = "com.example.myprovider";<span style="background-color: initial; font-family: inherit; font-size: inherit; text-align: initial; color: initial;"></span></code></code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>Call&nbsp;<code>ensureProductSkuSchema</code>&nbsp;at Element startup to register it:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code><code>@Inject
private ProductSkuSchemaService productSkuSchemaService;

// In your startup code / <span style="background-color: initial; font-family: inherit; font-size: inherit; text-align: initial; color: initial;">or SYSTEM_EVENT_ELEMENT_LOADED event</span><span style="font-family: inherit; font-size: inherit; text-align: initial; color: initial; background-color: rgba(0, 0, 0, 0.2);">:</span>productSkuSchemaService.ensureProductSkuSchema(MYPROVIDER_IAP_SCHEME);<span style="background-color: initial; font-family: inherit; font-size: inherit; text-align: initial; color: initial;"></span></code></code></pre>
<!-- /wp:code -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="2-create-product-bundles">2. Create Product Bundles</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Use the admin API or the web console to create&nbsp;<code>ProductBundle</code>&nbsp;records mapping your provider's&nbsp;product ids to in-game items.&nbsp;The bundle uniquely identifies a product by&nbsp;<code>(application, schema, productId)</code>.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="3-verify-the-purchase-and-call-processverifiedpurchase">3. Verify the purchase and call processVerifiedPurchase</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>After verifying the purchase with your provider's server-to-server API,&nbsp;call:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code><code>@Inject
private ProductBundleService productBundleService;

List&lt;RewardIssuance> issuances = productBundleService.processVerifiedPurchase(
        MYPROVIDER_IAP_SCHEME,
        providerProductId,
        providerTransactionId   // used as idempotency key
);</code></code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>The call is safe to retry&nbsp;—&nbsp;repeated calls with the same&nbsp;<code>originalTransactionId</code>&nbsp;return the&nbsp;existing issuances without creating duplicates.</p>
<!-- /wp:paragraph -->

<!-- wp:quote -->
<blockquote class="wp-block-quote"><!-- wp:paragraph -->
<p><strong>Tip:</strong>&nbsp;With&nbsp;<code>dev.getelements.elements.auth.enabled=true</code>&nbsp;set in your Element attributes you&nbsp;can inject&nbsp;<code>UserService</code>&nbsp;and call&nbsp;<code>userService.getCurrentUser()</code>&nbsp;to get the authenticated user&nbsp;making the request.&nbsp;See the&nbsp;<code>example-element</code>&nbsp;project for a complete example.</p>
<!-- /wp:paragraph --></blockquote>
<!-- /wp:quote -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="best-practices-and-recommendations">Best practices and recommendations</h2>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Always call <code>processVerifiedPurchase</code> inside a user-scoped context (i.e. the user whose profile contains the matching application).</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Use <code>originalTransactionId</code> values that are truly unique per purchase and stable across retries — provider-assigned transaction ids are ideal.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Keep <code>productId</code> values consistent with what the provider sends in its receipt payload. A mismatch will result in a silent empty-list return, which can be difficult to debug.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Prefer <code>ensureProductSkuSchema</code> over <code>createProductSkuSchema</code> in automation and plugin startup code — it is idempotent and will not fail if the schema already exists.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Use the <code>tags</code> field on <code>ProductBundle</code> to group bundles (e.g. by season or event) for efficient filtered queries without needing separate endpoints.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>When a bundle is removed or its <code>productBundleRewards</code> list is cleared, future calls to <code>processVerifiedPurchase</code> for that product will return an empty list. Existing <code>RewardIssuance</code> records are not affected.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="faq">FAQ</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p><strong>Q: Can a single product id be mapped to different reward sets per application?</strong>&nbsp;A:&nbsp;Yes.&nbsp;The compound key is&nbsp;<code>(application, schema, productId)</code>,&nbsp;so different applications can&nbsp;configure different rewards for the same provider product.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><strong>Q: What happens if&nbsp;<code>processVerifiedPurchase</code>&nbsp;is called and no bundle is configured?</strong>&nbsp;A:&nbsp;The method returns an empty list and logs a DEBUG message.&nbsp;It does not throw an exception,&nbsp;so&nbsp;the calling receipt service continues normally.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><strong>Q: Can I use&nbsp;<code>processVerifiedPurchase</code>&nbsp;from a superuser context?</strong>&nbsp;A:&nbsp;No&nbsp;—&nbsp;<code>SuperuserProductBundleService</code>&nbsp;delegates to the user-scoped service,&nbsp;which requires an&nbsp;authenticated user with a profile and application.&nbsp;Call it only from user-scoped request&nbsp;handlers.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><strong>Q: How are DISTINCT vs FUNGIBLE items handled?</strong>&nbsp;A:&nbsp;For&nbsp;<code>DISTINCT</code>&nbsp;items the quantity is always&nbsp;<code>1</code>&nbsp;regardless of what is set on the reward.&nbsp;For&nbsp;<code>FUNGIBLE</code>&nbsp;items the quantity from the reward entry is used,&nbsp;defaulting to&nbsp;<code>1</code>&nbsp;if null.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><strong>Q: Is there an event fired when rewards are issued?</strong> A: <code>processVerifiedPurchase</code> itself does not fire a dedicated event. Individual <code>RewardIssuance</code> records are created via <code>RewardIssuanceDao.getOrCreateRewardIssuance</code>, which may fire its own events depending on your configuration. See the Reward Issuance documentation for details.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->
