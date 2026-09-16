<h1>Large Object API</h1>

<!-- wp:paragraph -->
<p>A <strong>Large Object</strong> is arbitrary file content (an image, a data blob, a downloadable asset) stored and managed by Elements as a first-class database record, with its own id, permissions, and lifecycle. It's the mechanism behind the <strong>Large Object</strong> section of the admin console (see <a href="cms-feature-overview">CMS Feature Overview</a>) and is also fully available over the REST API, which makes it a convenient place to store game assets you want to manage the same way you manage other Elements data: live ops content, A/B test variants, user-uploaded images, and similar.</p>
<!-- /wp:paragraph -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"Note"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="large-object-1"><div class="gb-notice-title" style="color:#fff"><p>Note</p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p>This is a different system from <a href="element-static-content-and-dashboard-ui-plugins">Element Static Content and Dashboard UI Plugins</a> and from <a href="application-cdn-git-deployment">Application CDN Git Deployment</a>. Large Object content is stored as database content addressed by object id, not as a file tree tied to an Element or an Application.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-the-large-object-model">The Large Object Model</h2>
<!-- /wp:heading -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th>Field</th><th>Description</th></tr></thead><tbody><tr><td><code>id</code></td><td>Unique identifier for the object.</td></tr><tr><td><code>path</code></td><td>A logical path used for search/organization. If you don't supply one when creating an object, an automatic path is generated from the object's MIME type and a random id.</td></tr><tr><td><code>mimeType</code></td><td>The MIME type of the stored content.</td></tr><tr><td><code>state</code></td><td>Either <code>EMPTY</code> (created but no content uploaded yet) or <code>UPLOADED</code> (content has been written).</td></tr><tr><td><code>accessPermissions</code></td><td>Read, write, and delete permissions for the object. See below.</td></tr><tr><td><code>lastModified</code></td><td>Timestamp of the last metadata or content change.</td></tr><tr><td><code>originalFilename</code></td><td>The filename supplied at upload time, if any.</td></tr><tr><td><code>url</code></td><td>Computed, publicly reachable URL for the object's content. Not stored; assembled on read. See <a href="#h-serving-content">Serving Content</a> below.</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:paragraph -->
<p>Object content itself (the actual bytes) is stored separately from the metadata above, in MongoDB GridFS, keyed by the object's id.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-access-permissions">Access Permissions</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Each Large Object carries its own <code>accessPermissions</code>, with a separate permission for reading, writing, and deleting the object. Each of these can either be a wildcard, meaning "anyone," or scoped to specific subjects. A publicly downloadable game asset would have a wildcard read permission with write and delete restricted; a private user upload would have all three restricted.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-access-levels">Access Levels</h2>
<!-- /wp:heading -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th>Access level</th><th>Capabilities</th></tr></thead><tbody><tr><td>Anonymous</td><td>Can list objects and read metadata. Can read an object's content only if its read permission is a wildcard. Create, update, delete, and non-public reads are all forbidden.</td></tr><tr><td>User</td><td>Can read, update, and delete objects it has been granted permission for, per that object's <code>accessPermissions</code>. Cannot create new Large Objects; creation is Superuser-only.</td></tr><tr><td>Superuser</td><td>Full create, read, update, and delete access to any object, plus the ability to create an object by fetching content from a URL server-side rather than uploading it directly.</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-rest-api">REST API</h2>
<!-- /wp:heading -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th>Method</th><th>Path</th><th>Description</th></tr></thead><tbody><tr><td><code>POST</code></td><td><code>/large_object</code></td><td>Creates a new Large Object record (metadata only; content is uploaded separately). Superuser only.</td></tr><tr><td><code>POST</code></td><td><code>/large_object/from_url</code></td><td>Creates a new Large Object by having Elements fetch content from a supplied URL server-side. Superuser only.</td></tr><tr><td><code>PUT</code></td><td><code>/large_object/{id}</code></td><td>Updates an object's metadata and/or access permissions.</td></tr><tr><td><code>PUT</code></td><td><code>/large_object/{id}/content</code></td><td>Uploads (or replaces) an object's content as a multipart request body.</td></tr><tr><td><code>GET</code></td><td><code>/large_object/{id}</code></td><td>Gets a single object's metadata.</td></tr><tr><td><code>GET</code></td><td><code>/large_object?offset=&amp;count=&amp;search=</code></td><td>Lists objects, paginated, optionally filtered by a search term matched against <code>path</code> and <code>mimeType</code>.</td></tr><tr><td><code>DELETE</code></td><td><code>/large_object/{id}</code></td><td>Deletes an object's metadata and content.</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-serving-content">Serving Content</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>A Large Object's public <code>url</code> field is assembled as the configured CDN origin plus <code>/object/{id}</code>:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>{dev_getelements_elements_cdn_url}/object/{id}</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p><code>dev_getelements_elements_cdn_url</code> is the same CDN origin variable described in <a href="configuring-external-urls-for-deployment">Configuring External URLs for Deployment</a>. Requests to this endpoint are resolved through the same access-level rules described above (so a private object still returns <code>403</code> to an unauthorized caller), and support:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Conditional requests via <code>ETag</code> / <code>If-None-Match</code>, computed from the object's id and last-modified time.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>Cache-Control: public, max-age=...</code> (using the <code>dev.getelements.elements.cdn.public.max.age</code> attribute, default 300 seconds) when the object's read permission is a wildcard, and <code>private, no-store</code> otherwise.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>A <code>?download=true</code> query parameter, which sends <code>Content-Disposition: attachment</code> instead of the default inline disposition.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-events">Events</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Large Object creation, updates, and deletion publish events that an Element can subscribe to, the same way most other data types in Elements do. See the <a href="3-9-release-notes">3.9 release notes</a> for background on this event coverage.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->
