<h1>Metadata (3.4+)</h1>

<!-- wp:paragraph -->
<p>This page explains how to <strong>create</strong>, <strong>modify</strong>, and <strong>delete</strong> metadata and metadata schemas in the Namazu Elements Management Console.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-overview">Overview</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p><strong>Metadata</strong> in Namazu Elements are arbitrary <strong>JSON objects</strong> that can be used to <strong>remotely configure your game client</strong>.<br>They allow you to adjust gameplay parameters, feature toggles, or live event settings without requiring a new game build.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Each metadata entry follows a <strong>metadata specification (metadata spec)</strong>.<br>A metadata spec defines the <strong>structure</strong>, <strong>type</strong>, and <strong>default values</strong> of fields allowed in a metadata object.<br>Together, they provide a structured way to manage configuration data for live operations, events, and other dynamic game content.</p>
<!-- /wp:paragraph -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"\u003cstrong\u003e\u003cmark style=\u0022background-color:rgba(0, 0, 0, 0)\u0022 class=\u0022has-inline-color has-black-color\u0022\u003e📝Using Metadata\u003c/mark\u003e\u003c/strong\u003e"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="f2528e"><div class="gb-notice-title" style="color:#fff"><p><strong><mark style="background-color:rgba(0, 0, 0, 0)" class="has-inline-color has-black-color">📝Using Metadata</mark></strong></p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p>If you have experience with other backend systems, metadata in Namazu Elements functions similarly to <strong>PlayFab Title Data</strong> or <strong>Remote Config</strong> in other platforms.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>The key difference is that Namazu supports <strong>arbitrary nested JSON structures</strong> and validates them against a <strong>defined metadata spec</strong>, giving developers more control and consistency.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:quote -->
<blockquote class="wp-block-quote"></blockquote>
<!-- /wp:quote -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-creating-a-metadata-schema">Creating a Metadata Schema</h2>
<!-- /wp:heading -->

<!-- wp:list {"ordered":true} -->
<ol class="wp-block-list"><!-- wp:list-item -->
<li>Log in to the management console and navigate to<br><strong>Metadata → Metadata Specs</strong>.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Click <strong>Create Metadata Spec</strong></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Enter Properties:<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>Name</strong>: A unique name for the Metadata Schema</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Access Level:</strong> Define which user level is required to read this </li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Properties</strong>: Define fields that metadata using this schema will include.<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><em>Internal Name</em>: Unique key for the field.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><em>Display Name</em>: Human-readable label.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><em>Type</em>: Data type (<code>STRING</code>, <code>INTEGER</code>, <code>BOOLEAN</code>, <code>OBJECT</code>, etc.).</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><em>Default Value</em>: Value to use when none is provided.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><em>Required</em>: Check if the field must be present in all metadata.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list --></li>
<!-- /wp:list-item --></ul>
<!-- /wp:list --></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Click <strong>Create</strong> to save the schema.</li>
<!-- /wp:list-item --></ol>
<!-- /wp:list -->

<!-- wp:image {"id":22251,"sizeSlug":"full","linkDestination":"none"} -->
<figure class="wp-block-image size-full"><img src="https://namazustudios.com/wp-content/uploads/2025/11/image-7.png" alt="" class="wp-image-22251"/><figcaption class="wp-element-caption">Creating a new Metadata Spec</figcaption></figure>
<!-- /wp:image -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-modifying-a-metadata-schema">Modifying a Metadata Schema</h2>
<!-- /wp:heading -->

<!-- wp:list {"ordered":true} -->
<ol class="wp-block-list"><!-- wp:list-item -->
<li>Go to <strong>Metadata → Metadata Specs</strong>.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Locate the schema you want to change and click the <strong>edit (pencil)</strong> icon.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Update any fields such as changing default values, adding new properties, or renaming display names.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Click <strong>Update</strong> to save your changes.</li>
<!-- /wp:list-item --></ol>
<!-- /wp:list -->

<!-- wp:image {"id":22252,"sizeSlug":"full","linkDestination":"none"} -->
<figure class="wp-block-image size-full"><img src="https://namazustudios.com/wp-content/uploads/2025/11/image-8.png" alt="" class="wp-image-22252"/><figcaption class="wp-element-caption">Finding the Metadata Spec to Edit</figcaption></figure>
<!-- /wp:image -->

<!-- wp:image {"id":22253,"sizeSlug":"full","linkDestination":"none"} -->
<figure class="wp-block-image size-full"><img src="https://namazustudios.com/wp-content/uploads/2025/11/image-9.png" alt="" class="wp-image-22253"/><figcaption class="wp-element-caption">Editing a Metadata Spec</figcaption></figure>
<!-- /wp:image -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-deleting-a-metadata-schema">Deleting a Metadata Schema</h2>
<!-- /wp:heading -->

<!-- wp:list {"ordered":true} -->
<ol class="wp-block-list"><!-- wp:list-item -->
<li>In the <strong>Metadata Specs</strong> list, click the <strong>trash</strong> icon next to the schema.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Confirm deletion.</li>
<!-- /wp:list-item --></ol>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":22254,"sizeSlug":"full","linkDestination":"none"} -->
<figure class="wp-block-image size-full"><img src="https://namazustudios.com/wp-content/uploads/2025/11/image-10.png" alt="" class="wp-image-22254"/><figcaption class="wp-element-caption">Delete Confirmation Dialog</figcaption></figure>
<!-- /wp:image -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"\u003cmark style=\u0022background-color:rgba(0, 0, 0, 0)\u0022 class=\u0022has-inline-color has-black-color\u0022\u003e\u003cstrong\u003e📝\u003c/strong\u003eDeletion\u003c/mark\u003e"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="f00225"><div class="gb-notice-title" style="color:#fff"><p><mark style="background-color:rgba(0, 0, 0, 0)" class="has-inline-color has-black-color"><strong>📝</strong>Deletion</mark></p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p>Deleting a schema does not automatically remove metadata instances using it, but those instances will lose structural validation.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-creating-metadata">Creating Metadata</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>In order to create a Metadata entry, you must have created a Metadata Spec first.</p>
<!-- /wp:paragraph -->

<!-- wp:list {"ordered":true} -->
<ol class="wp-block-list"><!-- wp:list-item -->
<li>Navigate to <strong>Metadata → Metadata</strong>.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Click <strong>Create Metadata</strong>.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Provide:<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>Name</strong>: Unique identifier (no spaces).</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Access Level</strong>: Controls who can view (User, Server, etc.).</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Metadata Spec</strong>: Choose which schema defines the data structure.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list --></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Fill in the fields defined in the selected metadata spec.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Click <strong>Create</strong>.</li>
<!-- /wp:list-item --></ol>
<!-- /wp:list -->

<!-- wp:image {"id":22255,"sizeSlug":"full","linkDestination":"none"} -->
<figure class="wp-block-image size-full"><img src="https://namazustudios.com/wp-content/uploads/2025/11/image-11.png" alt="" class="wp-image-22255"/><figcaption class="wp-element-caption">Entering a Metadata from the Metadata Spec</figcaption></figure>
<!-- /wp:image -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-modifying-metadata">Modifying Metadata</h2>
<!-- /wp:heading -->

<!-- wp:list {"ordered":true} -->
<ol class="wp-block-list"><!-- wp:list-item -->
<li>Open <strong>Metadata → Metadata</strong>.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Select an entry and click the <strong>edit</strong> icon.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Adjust JSON values in any defined fields.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Click <strong>Update</strong>.</li>
<!-- /wp:list-item --></ol>
<!-- /wp:list -->

<!-- wp:image {"id":22256,"sizeSlug":"full","linkDestination":"none"} -->
<figure class="wp-block-image size-full"><img src="https://namazustudios.com/wp-content/uploads/2025/11/image-12.png" alt="" class="wp-image-22256"/><figcaption class="wp-element-caption">Finding a Metadata to Edit</figcaption></figure>
<!-- /wp:image -->

<!-- wp:image {"id":22257,"sizeSlug":"full","linkDestination":"none"} -->
<figure class="wp-block-image size-full"><img src="https://namazustudios.com/wp-content/uploads/2025/11/image-13.png" alt="" class="wp-image-22257"/><figcaption class="wp-element-caption">Updating an Existing Metadata</figcaption></figure>
<!-- /wp:image -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-deleting-metadata">Deleting Metadata</h2>
<!-- /wp:heading -->

<!-- wp:list {"ordered":true} -->
<ol class="wp-block-list"><!-- wp:list-item -->
<li>From the metadata list, click the <strong>trash</strong> icon next to the desired entry.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Confirm deletion.</li>
<!-- /wp:list-item --></ol>
<!-- /wp:list -->

<!-- wp:image {"id":22258,"width":"840px","height":"auto","sizeSlug":"full","linkDestination":"none"} -->
<figure class="wp-block-image size-full is-resized"><img src="https://namazustudios.com/wp-content/uploads/2025/11/image-14.png" alt="" class="wp-image-22258" style="width:840px;height:auto"/><figcaption class="wp-element-caption">Options for Editing and Deleting Metdata</figcaption></figure>
<!-- /wp:image -->

<!-- wp:image {"id":22259,"sizeSlug":"full","linkDestination":"none"} -->
<figure class="wp-block-image size-full"><img src="https://namazustudios.com/wp-content/uploads/2025/11/image-15.png" alt="" class="wp-image-22259"/><figcaption class="wp-element-caption">Delete Confirmation Dialog</figcaption></figure>
<!-- /wp:image -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-additional-tips">Additional Tips</h2>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>Keep specs generic</strong>: Reuse schema definitions where possible to avoid duplication across projects.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Use defaults wisely</strong>: Setting default values ensures clients continue to operate even if updates are delayed or incomplete.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Validate data before publishing</strong>: Always test metadata changes in a staging environment to ensure schema compliance and prevent client crashes.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Version control your specs</strong>: Export or document schema definitions so you can roll back easily.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Avoid hardcoding</strong>: Keep client-side logic dynamic by referencing metadata keys rather than embedding configuration constants.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-summary">Summary</h2>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>Metadata</strong> are flexible JSON objects used to configure game clients remotely.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Metadata Specs</strong> define the structure and validation rules for those objects.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>The Management Console provides a simple interface for creating, editing, and deleting both metadata and their specs.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>With schema-driven configuration, live updates can be deployed safely and consistently without requiring new builds.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->
