<h1>Receipts</h1>

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-overview">Overview</h3>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>The Receipt API stores generic receipt objects containing basic metadata and raw receipt payloads. It supports admin/support operations (search, inspect, delete) while concrete provider-specific APIs (Apple, Google Play, Facebook, Oculus) parse and operate on the raw receipt body for user-level flows (validation, redeeming).</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-receipt-model">Receipt model</h3>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Description: A generic receipt that stores user purchase information.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Fields:<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>id (String): The DB id of this receipt.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>originalTransactionId (String): ID of the original transaction from the payment processor.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>schema (String): Receipt provider id in reverse-DNS notation (e.g. com.company.platform).</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>user (User): The user associated with this receipt.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>purchaseTime (long): Purchase time in ms since Unix epoch.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>body (String): String representation of the raw receipt data (JSON).</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list --></li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-intended-usage">Intended usage</h3>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>General Receipt API (admin/support): manage receipts, search by schema, view raw body, delete.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Concrete provider APIs (user-level): detect provider by receipt.schema, parse JSON in body into provider-specific models (e.g. AppleIapReceipt, GooglePlayIapReceipt, OculusIapReceipt) and perform validation/redeem operations. See Application Configurations for more information on how to map a SKU or product id to an Item in Elements.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-dao-interface-receiptdao">DAO interface (ReceiptDao)</h3>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Key constants:<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>RECEIPT_CREATED = "dev.getelements.elements.sdk.model.dao.receipt.created"</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list --></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Important methods:<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Pagination getReceipts(User user, int offset, int count, String search)<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Use search to filter by schema when supporting multiple IAP providers.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list --></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Pagination getReceipts(User user, int offset, int count)</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Receipt getReceipt(String id)</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Receipt getReceipt(String schema, String originalTransactionId)</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Receipt createReceipt(Receipt receipt)<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Throws InvalidDataException or DuplicateException if invalid/duplicate.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Emits RECEIPT_CREATED event (see events below).</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list --></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>void deleteReceipt(String receiptId)</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list --></li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="h-transaction-usage-recommended">Transaction usage (recommended)</h4>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Always prefer DAO operations inside a transaction to ensure consistency and automatic retry on failure.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Example:</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:code -->
<pre class="wp-block-code"><code>@Inject
private Provider&lt;Transaction> transactionProvider;

final var createdReceipt = transactionProvider.get().performAndClose(tx -> {
    final var receiptDao = tx.getDao(ReceiptDao.class);
    return receiptDao.createReceipt(receipt);
});</code></pre>
<!-- /wp:code -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="h-events">Events</h4>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>RECEIPT_CREATED fires when any new receipt is created.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Events for specific receipt types are also fired if you only want to handle a specific type and receive the converted receipt for that type (i.e. the parsed raw receipt JSON). These include:<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>OCULUS_IAP_RECEIPT_CREATED</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>APPLE_IAP_RECEIPT_CREATED</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>GOOGLE_PLAY_IAP_RECEIPT_CREATED</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>FACEBOOK_IAP_RECEIPT_CREATED</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list --></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Event parameters variants:<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>(Receipt)</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>(Receipt, Transaction)</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list --></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Use this event for downstream processing (e.g., kicking off parsing, validation, fulfillment). Event handlers should inspect receipt.schema to determine provider-specific parsing. See Events for more information on how to register event callbacks and create your own custom events.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="h-provider-mapping-existing">Provider mapping (existing)</h4>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Use schema to detect provider:<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>GOOGLE_IAP_SCHEME -> GooglePlayIapReceipt</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>OCULUS_PLATFORM_IAP_SCHEME -> OculusIapReceipt</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>APPLE_IAP_SCHEME -> AppleIapReceipt</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>FACEBOOK_IAP_SCHEME -> FacebookIapReceipt</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list --></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Concrete API implementations parse the JSON in Receipt.body into the corresponding provider model and provide validation/redeem endpoints.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-integrating-a-new-payment-provider">Integrating a new payment provider</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>If you want to add support for a payment provider or are creating a custom Element for your own solution, here are some steps to follow:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Select a schema string<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Use reverse-DNS notation: e.g. com.example.myprovider</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Add a constant: String MYPROVIDER_IAP_SCHEME = "com.example.myprovider";</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list --></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Define a provider-specific receipt model<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Create a POJO class that represents the JSON structure of the provider’s receipt payload (e.g., MyProviderIapReceipt).</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Add @Schema annotations for clarity in client side generated code.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list --></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Implement parsing and validation logic<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Add a parser that takes Receipt.body (string JSON) and maps to MyProviderIapReceipt.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Implement validation routines (signature checks, timestamps, server-to-server verification endpoints, etc.) according to provider docs.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list --></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Add concrete API endpoints / service<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Provide user-level APIs to:<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Submit a receipt for validation/redeem.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Query validated receipts for a user.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list --></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>These APIs should:<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Locate the generic Receipt (by schema + originalTransactionId or by DB id).</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Parse Receipt.body to MyProviderIapReceipt.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Validate and execute fulfillment (redeem digital goods, record consumptions).</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list --></li>
<!-- /wp:list-item --></ul>
<!-- /wp:list --></li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"Hint"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="d11c7e"><div class="gb-notice-title" style="color:#fff"><p>Hint</p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p>With dev.getelements.elements.auth.enabled=true set in your Element attributes, you can inject the UserService and then use userService.getCurrentUser() to get the User that made the request. See the <a href="https://github.com/NamazuStudios/element-example/blob/main/src/main/java/com/mystudio/mygame/service/GreetingServiceImpl.java">example-element project</a> for an example of this.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Add admin API endpoints/service<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Provide superuser-level APIs to:<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Map product ids or SKUs to Items in Elements</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>View receipts for all users</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list --></li>
<!-- /wp:list-item --></ul>
<!-- /wp:list --></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Persist receipts via DAO<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>When creating receipts (e.g., incoming from client or server webhook), use ReceiptDao.createReceipt(...) inside a transaction.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Example create flow:</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list --></li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>Example:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>Receipt r = new Receipt();
r.setOriginalTransactionId(providerTxId);
r.setSchema(MYPROVIDER_IAP_SCHEME);
r.setUser(user);
r.setPurchaseTime(System.currentTimeMillis());
r.setBody(rawJson);

final var saved = transactionProvider.get().performAndClose(tx -&gt; {
    return tx.getDao(ReceiptDao.class).createReceipt(r);
});
</code></pre>
<!-- /wp:code -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Handle duplicates and errors<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>createReceipt may throw DuplicateException if schema + originalTransactionId already exist.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Catch and handle InvalidDataException for malformed receipts.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>ReceiptDao handles idempotency: on retries, it will detect duplicates and return the existing receipt.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list --></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Subscribe to RECEIPT_CREATED (optional)<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Use the event to trigger asynchronous parsing/validation or analytics.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Event handlers should be resilient: validate schema, parse body, and record results; do not assume body is well-formed.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list --></li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="h-best-practices-and-recommendations">Best practices and recommendations</h4>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Always validate schema value before parsing.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Keep raw body immutable in DB; parse into separate model objects for business logic and caching parsed values if needed.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Use transactions around DAO writes to ensure consistency.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Enforce idempotency in validation/redeem flows based on originalTransactionId + schema.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Log parsing and validation failures, and surface helpful errors to support team via admin tools.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>When adding new providers, document the provider schema constant, payload model, parsing rules, and any external verification endpoints.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>Example: reading a receipt and parsing per schema</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>ReceiptDao dao = tx.getDao(ReceiptDao.class);
Receipt receipt = dao.getReceipt("com.example.myprovider", originalTransactionId);

switch (receipt.getSchema()) {
    case MYPROVIDER_IAP_SCHEME:
        MyProviderIapReceipt parsed = myProviderParser.parse(receipt.getBody());
        myProviderValidator.validate(parsed);
        <em>// proceed with redeem/fulfill</em>
        break;
    case APPLE_IAP_SCHEME:
        AppleIapReceipt apple = appleParser.parse(receipt.getBody());
        <em>// ...</em>
        break;
    <em>// other cases</em>
}</code></pre>
<!-- /wp:code -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="h-support-admin-considerations">Support/admin considerations</h4>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>The built in Elements CMS will parse the OpenAPI spec for your custom Element and generate UI for it. This allows you to interact with the endpoints, manage receipts, and create your SKU to Item mappings without writing any frontend code. That said, you may want to:<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Allow searching/filtering by schema to focus on a specific provider’s receipts.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Provide the ability to re-trigger parsing/validation from admin tools when needed (Items can be added to a User's inventory manually through the CMS as well).</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list --></li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-faq-short">FAQ (short)</h3>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Q: Where to store parsed provider models?<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>A: Store parsed results in separate tables/models; keep Receipt.body as the immutable raw source.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list --></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Q: How to handle provider changes to payload format?<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>A: Version parsed models, add migration/compatibility logic, and keep raw JSON to reparse historical receipts if necessary.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list --></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Q: When should I use the concrete APIs vs the general Receipt API?<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>A: Use concrete APIs for user-level validation/redeem flows. Use the general Receipt API for admin/support and cross-provider management.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list --></li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->
