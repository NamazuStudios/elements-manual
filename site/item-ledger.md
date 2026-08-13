<h1>Item Ledger</h1>

<!-- wp:paragraph -->
<p>Elements Version 3.8+</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="overview">Overview</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The Item Ledger is an append-only audit trail that records every inventory and item-catalog&nbsp;lifecycle event in the system.&nbsp;It answers the question:&nbsp;<em>"when did this user acquire, modify, or lose this item, and who made the change?"</em></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Every time an inventory item or item-catalog record is created,&nbsp;modified,&nbsp;or deleted,&nbsp;the&nbsp;responsible service writes an immutable&nbsp;<code>ItemLedgerEntry</code>&nbsp;to the&nbsp;<code>item_ledger</code>&nbsp;MongoDB&nbsp;collection.&nbsp;Records are&nbsp;<strong>never updated or deleted</strong>&nbsp;-&nbsp;the collection grows monotonically and&nbsp;serves as a permanent history.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Reads are superuser-only.&nbsp;Regular users and unauthenticated callers receive a&nbsp;<code>ForbiddenException</code>.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="what-is-recorded">What is recorded</h2>
<!-- /wp:heading -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th>Trigger</th><th>Event type</th></tr></thead><tbody><tr><td>Fungible inventory item created</td><td><code>CREATED</code></td></tr><tr><td>Fungible inventory quantity adjusted by delta</td><td><code>QUANTITY_ADJUSTED</code></td></tr><tr><td>Fungible inventory quantity set to absolute value</td><td><code>QUANTITY_SET</code></td></tr><tr><td>Fungible inventory item deleted</td><td><code>DELETED</code></td></tr><tr><td>Distinct inventory item created</td><td><code>CREATED</code></td></tr><tr><td>Distinct inventory item metadata updated</td><td><code>METADATA_UPDATED</code></td></tr><tr><td>Distinct inventory item deleted</td><td><code>DELETED</code></td></tr><tr><td>Item catalog entry created</td><td><code>ITEM_CREATED</code></td></tr><tr><td>Item catalog entry updated</td><td><code>ITEM_UPDATED</code></td></tr><tr><td>Item catalog entry deleted</td><td><code>ITEM_DELETED</code></td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="model">Model</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p><strong>Class:</strong>&nbsp;<code>dev.getelements.elements.sdk.model.inventory.ItemLedgerEntry</code></p>
<!-- /wp:paragraph -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th>Field</th><th>Type</th><th>Description</th></tr></thead><tbody><tr><td><code>id</code></td><td><code>String</code></td><td>Database id. Assigned on creation; null on write.</td></tr><tr><td><code>inventoryItemId</code></td><td><code>String</code></td><td>Id of the affected inventory item. Null for item-catalog events (<code>ITEM_CREATED</code>,&nbsp;<code>ITEM_UPDATED</code>,&nbsp;<code>ITEM_DELETED</code>).</td></tr><tr><td><code>itemCategory</code></td><td><code>ItemCategory</code></td><td><code>FUNGIBLE</code>&nbsp;or&nbsp;<code>DISTINCT</code>. Null for item-catalog events.</td></tr><tr><td><code>itemId</code></td><td><code>String</code></td><td>Id of the item definition (the&nbsp;<code>Item</code>&nbsp;catalog record).</td></tr><tr><td><code>userId</code></td><td><code>String</code></td><td>Id of the user who owns the inventory item. For item-catalog events this is set to the&nbsp;<code>actorId</code>.</td></tr><tr><td><code>actorId</code></td><td><code>String</code></td><td>Id of the user who performed the action. Null when triggered by a system process or plugin without an active user session.</td></tr><tr><td><code>eventType</code></td><td><code>ItemLedgerEventType</code></td><td>Classifies the event. See&nbsp;<a href="http://localhost:63342/markdownPreview/1921507820/markdown-preview-index-586457526.html?_ijt=f9h2j9ehfr933tefp1jka48iqv#event-types">event types</a>&nbsp;below.</td></tr><tr><td><code>timestamp</code></td><td><code>long</code></td><td>Epoch milliseconds when the event was recorded.</td></tr><tr><td><code>quantityBefore</code></td><td><code>Integer</code></td><td>Quantity before the change. Populated only for&nbsp;<code>QUANTITY_ADJUSTED</code>.</td></tr><tr><td><code>quantityAfter</code></td><td><code>Integer</code></td><td>Quantity after the change. Populated for&nbsp;<code>CREATED</code>,&nbsp;<code>QUANTITY_ADJUSTED</code>, and&nbsp;<code>QUANTITY_SET</code>.</td></tr><tr><td><code>metadataBefore</code></td><td><code>Map&lt;String, Object&gt;</code></td><td>Metadata snapshot before the change. Populated only for&nbsp;<code>METADATA_UPDATED</code>.</td></tr><tr><td><code>metadataAfter</code></td><td><code>Map&lt;String, Object&gt;</code></td><td>Metadata snapshot after the change. Populated only for&nbsp;<code>METADATA_UPDATED</code>.</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="event-types">Event types</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p><strong>Class:</strong>&nbsp;<code>dev.getelements.elements.sdk.model.inventory.ItemLedgerEventType</code></p>
<!-- /wp:paragraph -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th>Value</th><th>Applies to</th><th>Description</th></tr></thead><tbody><tr><td><code>CREATED</code></td><td>Fungible and distinct items</td><td>Item was added to a user's inventory.</td></tr><tr><td><code>QUANTITY_ADJUSTED</code></td><td>Fungible items</td><td>Quantity changed by a signed delta.</td></tr><tr><td><code>QUANTITY_SET</code></td><td>Fungible items</td><td>Quantity replaced with an absolute value.</td></tr><tr><td><code>DELETED</code></td><td>Fungible and distinct items</td><td>Item was removed from inventory.</td></tr><tr><td><code>METADATA_UPDATED</code></td><td>Distinct items</td><td>Per-item metadata was updated.</td></tr><tr><td><code>ITEM_CREATED</code></td><td>Item catalog</td><td>An&nbsp;<code>Item</code>&nbsp;definition was created.</td></tr><tr><td><code>ITEM_UPDATED</code></td><td>Item catalog</td><td>An&nbsp;<code>Item</code>&nbsp;definition was updated.</td></tr><tr><td><code>ITEM_DELETED</code></td><td>Item catalog</td><td>An&nbsp;<code>Item</code>&nbsp;definition was deleted.</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="rest-api">REST API</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Base path:&nbsp;<code>/api/rest/inventory/ledger</code></p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="get-ledger-entries">Get ledger entries</h3>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code><code>GET /api/rest/inventory/ledger</code></code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>Returns paginated&nbsp;<code>ItemLedgerEntry</code>&nbsp;objects sorted most recent first.&nbsp;Exactly one of&nbsp;<code>inventoryItemId</code>&nbsp;or&nbsp;<code>userId</code>&nbsp;is required.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><strong>Query parameters</strong></p>
<!-- /wp:paragraph -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th>Parameter</th><th>Required</th><th>Description</th></tr></thead><tbody><tr><td><code>inventoryItemId</code></td><td>One of these two is required</td><td>Filter entries for a specific inventory item.</td></tr><tr><td><code>userId</code></td><td>One of these two is required</td><td>Filter entries for a specific user (across all their inventory items).</td></tr><tr><td><code>eventType</code></td><td>No</td><td>Filter by event type. Omit to return all event types.</td></tr><tr><td><code>offset</code></td><td>No (default&nbsp;<code>0</code>)</td><td>Zero-based page offset.</td></tr><tr><td><code>count</code></td><td>No (default&nbsp;<code>20</code>)</td><td>Number of results per page.</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:paragraph -->
<p><strong>Responses</strong></p>
<!-- /wp:paragraph -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th>Status</th><th>Description</th></tr></thead><tbody><tr><td><code>200 OK</code></td><td><code>Pagination&lt;ItemLedgerEntry&gt;</code>&nbsp;- paginated results.</td></tr><tr><td><code>400 Bad Request</code></td><td>Neither&nbsp;<code>inventoryItemId</code>&nbsp;nor&nbsp;<code>userId</code>&nbsp;was supplied, or a numeric parameter is negative.</td></tr><tr><td><code>403 Forbidden</code></td><td>Caller is not a superuser.</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:paragraph -->
<p><strong>Example - full history for an inventory item</strong></p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code><code>GET /api/rest/inventory/ledger?inventoryItemId=6630f1a2b4e3c70012345678
Authorization: Bearer &lt;superuser-token&gt;<span style="background-color: initial; font-family: inherit; font-size: inherit; text-align: initial; color: initial;"></span></code></code></pre>
<!-- /wp:code -->

<!-- wp:code -->
<pre class="wp-block-code"><code><code>{
  "total": 3,
  "objects": &#91;
    {
      "id": "6630f1a2b4e3c70099999993",
      "inventoryItemId": "6630f1a2b4e3c70012345678",
      "itemCategory": "FUNGIBLE",
      "itemId": "6630f1a2b4e3c70011111111",
      "userId": "6630f1a2b4e3c70022222222",
      "actorId": "6630f1a2b4e3c70033333333",
      "eventType": "QUANTITY_ADJUSTED",
      "timestamp": 1714000800000,
      "quantityBefore": 10,
      "quantityAfter": 15
    },
    {
      "id": "6630f1a2b4e3c70099999992",
      "inventoryItemId": "6630f1a2b4e3c70012345678",
      "itemCategory": "FUNGIBLE",
      "itemId": "6630f1a2b4e3c70011111111",
      "userId": "6630f1a2b4e3c70022222222",
      "actorId": "6630f1a2b4e3c70033333333",
      "eventType": "QUANTITY_SET",
      "timestamp": 1714000700000,
      "quantityAfter": 10
    },
    {
      "id": "6630f1a2b4e3c70099999991",
      "inventoryItemId": "6630f1a2b4e3c70012345678",
      "itemCategory": "FUNGIBLE",
      "itemId": "6630f1a2b4e3c70011111111",
      "userId": "6630f1a2b4e3c70022222222",
      "actorId": "6630f1a2b4e3c70033333333",
      "eventType": "CREATED",
      "timestamp": 1714000600000,
      "quantityBefore": 0,
      "quantityAfter": 5
    }
  ]
}</code></code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p><strong>Example - user history filtered to creation events</strong></p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code><code>GET /api/rest/inventory/ledger?userId=6630f1a2b4e3c70022222222&amp;eventType=CREATED
Authorization: Bearer &lt;superuser-token><span style="background-color: initial; font-family: inherit; font-size: inherit; text-align: initial; color: initial;"></span></code></code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p><strong>Example - entries within a timestamp range</strong></p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code><code>GET /api/rest/inventory/ledger?inventoryItemId=6630f1a2b4e3c70012345678&amp;from=1714000000000&amp;to=1714001000000
Authorization: Bearer &lt;superuser-token></code></code></pre>
<!-- /wp:code -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="dao-interface">DAO interface</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p><strong>Interface:</strong>&nbsp;<code>dev.getelements.elements.sdk.dao.ItemLedgerDao</code>&nbsp;<strong>Annotation:</strong>&nbsp;<code>@ElementServiceExport</code></p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>/** Appends an immutable audit record. */
ItemLedgerEntry createLedgerEntry(ItemLedgerEntry entry);

/** Entries for a specific inventory item, most recent first. eventType/from/to may be null. */
Pagination&lt;ItemLedgerEntry> getLedgerEntries(
        String inventoryItemId, int offset, int count,
        ItemLedgerEventType eventType, Long from, Long to);

/** All entries for a specific user across all inventory items, most recent first. eventType/from/to may be null. */
Pagination&lt;ItemLedgerEntry> getLedgerEntriesForUser(
        String userId, int offset, int count,
        ItemLedgerEventType eventType, Long from, Long to);</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>There are intentionally no update or delete methods&nbsp;-&nbsp;the interface enforces immutability.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="service-interface">Service interface</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p><strong>Interface:</strong>&nbsp;<code>dev.getelements.elements.sdk.service.inventory.ItemLedgerService</code>&nbsp;<strong>Annotations:</strong>&nbsp;<code>@ElementPublic</code>,&nbsp;<code>@ElementServiceExport</code>,&nbsp;<code>@ElementServiceExport(name = UNSCOPED)</code></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>The service is read-only and exposes the same two query methods as the DAO.</p>
<!-- /wp:paragraph -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th>Access level</th><th>Behaviour</th></tr></thead><tbody><tr><td><code>SUPERUSER</code></td><td>Full read access.</td></tr><tr><td>Any other level</td><td>Throws&nbsp;<code>ForbiddenException</code>.</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="how-writes-work">How writes work</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Ledger entries are written automatically by the inventory and item-catalog service&nbsp;implementations immediately after each successful mutation.&nbsp;No explicit calls are needed by&nbsp;callers of those services.</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code><code>SuperUserSimpleInventoryItemService   ─|
SuperUserAdvancedInventoryItemService ─|
SuperUserDistinctInventoryItemService ─|
SuperuserItemService (catalog)        ─|</code>----&gt; <span style="background-color: initial; font-family: inherit; font-size: inherit; text-align: initial; color: initial;">ItemLedgerDao.createLedgerEntry(...)</span></code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>The&nbsp;<code>actorId</code>&nbsp;is set to the id of the currently authenticated user.&nbsp;If the call originates&nbsp;from a plugin or system process without an active user session,&nbsp;<code>actorId</code>&nbsp;is null.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Item-catalog events&nbsp;(<code>ITEM_CREATED</code>,&nbsp;<code>ITEM_UPDATED</code>,&nbsp;<code>ITEM_DELETED</code>)&nbsp;set&nbsp;<code>inventoryItemId</code>,&nbsp;<code>itemCategory</code>,&nbsp;and&nbsp;<code>userId</code>&nbsp;to null;&nbsp;<code>actorId</code>&nbsp;holds the id of the superuser who made the&nbsp;change.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="reading-the-ledger-from-a-plugin">Reading the ledger from a plugin</h2>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code>@Inject
private ItemLedgerService itemLedgerService;

// Full history for one inventory item
Pagination&lt;ItemLedgerEntry> history = itemLedgerService
        .getLedgerEntries(inventoryItemId, 0, 20, null, null, null);

// Creation events for a user
Pagination&lt;ItemLedgerEntry> created = itemLedgerService
        .getLedgerEntriesForUser(userId, 0, 20, ItemLedgerEventType.CREATED, null, null);

// Entries within a timestamp range
long from = Instant.parse("2024-04-25T00:00:00Z").toEpochMilli();
long to   = Instant.parse("2024-04-26T00:00:00Z").toEpochMilli();
Pagination&lt;ItemLedgerEntry> ranged = itemLedgerService
        .getLedgerEntries(inventoryItemId, 0, 20, null, from, to);<code><span style="background-color: initial; font-family: inherit; font-size: inherit; text-align: initial; color: initial;"></span></code></code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>The service must be injected from a superuser-scoped or&nbsp;<code>UNSCOPED</code>&nbsp;context.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="best-practices">Best practices</h2>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>The ledger is append-only and grows indefinitely. Add a TTL index to&nbsp;<code>item_ledger.timestamp</code>&nbsp;if unbounded growth is a concern for your deployment.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Use the&nbsp;<code>eventType</code>&nbsp;filter to narrow queries - scanning all event types for a high-traffic user can be expensive at scale.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>For&nbsp;<code>QUANTITY_ADJUSTED</code>&nbsp;events,&nbsp;<code>quantityBefore + delta = quantityAfter</code>. For&nbsp;<code>QUANTITY_SET</code>&nbsp;events, only&nbsp;<code>quantityAfter</code>&nbsp;is recorded to avoid an extra DAO read.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>METADATA_UPDATED</code>&nbsp;entries include full&nbsp;<code>metadataBefore</code>&nbsp;and&nbsp;<code>metadataAfter</code>&nbsp;snapshots. For items with large metadata objects this doubles the storage cost per update.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Item-catalog events cannot be retrieved via the&nbsp;<code>inventoryItemId</code>&nbsp;or&nbsp;<code>userId</code>&nbsp;REST filters. Query MongoDB directly or use&nbsp;<code>ItemLedgerDao</code>&nbsp;in a plugin for catalog-only audit needs.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="faq">FAQ</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p><strong>Q: Why is the ledger superuser-only?</strong>&nbsp;A:&nbsp;Inventory histories can reveal sensitive activity information about other users.&nbsp;Only&nbsp;privileged callers&nbsp;(administrators,&nbsp;backend services)&nbsp;should be able to read them.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><strong>Q: Does deleting an inventory item also delete its ledger entries?</strong>&nbsp;A:&nbsp;No.&nbsp;Ledger entries are immutable and are never removed.&nbsp;After deletion a&nbsp;<code>DELETED</code>&nbsp;entry&nbsp;is appended;&nbsp;the prior history remains intact.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><strong>Q: Can I query entries for a specific event type across all users?</strong>&nbsp;A:&nbsp;Not directly via the REST API.&nbsp;For cross-user analytics,&nbsp;query MongoDB directly or build a&nbsp;custom aggregation in a plugin using&nbsp;<code>ItemLedgerDao</code>.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><strong>Q: Are entries written inside a transaction with the inventory mutation?</strong>&nbsp;A:&nbsp;Not currently.&nbsp;The ledger write happens immediately after the DAO call returns.&nbsp;If the&nbsp;ledger write fails the inventory mutation has already been committed&nbsp;-&nbsp;this is an acceptable&nbsp;trade-off for an audit trail where eventual consistency is sufficient.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->
