<h1>Configuring Static Content in Elements</h1>

<!-- wp:paragraph -->
<p>An Element, whether deployed as a plain directory or a packaged <code>.elm</code> archive, can serve static files directly with no Java code at all. This covers single-page apps, downloadable assets, CDN-style content, or an admin-panel UI bundled alongside an Element's business logic. This page covers the directory layout, the attributes that control mounting, indexing, and error pages, and the regex-based rule engine used to attach response headers to matching files.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"anchor":"h-directory-layout"} -->
<h2 id="h-directory-layout" class="wp-block-heading">Directory Layout</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Static content lives in one of two well-known subdirectories at the root of the Element:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><code>static/</code>: general-purpose static content, served under <code>/app/static/{prefix}</code> by default.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>ui/</code>: UI bundles (for example, a built single-page app), served under <code>/app/ui/{prefix}</code> by default.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:code -->
<pre class="wp-block-code"><code>my-element/
    dev.getelements.element.attributes.properties
    static/
        images/logo.png
        data.json
    ui/
        index.html
        assets/app.js</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>Both trees are optional and independent: an Element may provide only <code>static/</code>, only <code>ui/</code>, both, or neither. Each is loaded and configured separately, with its own index file, error pages, and rules.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"anchor":"h-mount-path"} -->
<h2 id="h-mount-path" class="wp-block-heading">Mount Path</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p><code>{prefix}</code> in the default mount paths above comes from the Element's application prefix, and each tree's mount path can be overridden directly:</p>
<!-- /wp:paragraph -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th>Attribute</th><th>Purpose</th></tr></thead><tbody><tr><td><code>dev.getelements.elements.app.serve.prefix</code></td><td>Overrides <code>{prefix}</code>. If blank, the Element's name is used instead.</td></tr><tr><td><code>dev.getelements.element.static.uri</code></td><td>Overrides the absolute mount path for <code>static/</code> (default <code>/app/static/{prefix}</code>).</td></tr><tr><td><code>dev.getelements.element.ui.uri</code></td><td>Overrides the absolute mount path for <code>ui/</code> (default <code>/app/ui/{prefix}</code>).</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"anchor":"h-attributes-file"} -->
<h2 id="h-attributes-file" class="wp-block-heading">Attributes File</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Everything described on this page is configured through Element attributes, which live in the <code>dev.getelements.element.attributes.properties</code> file at the root of the Element (a standard Java Properties file). Keeping this configuration out of code means the same <code>.elm</code> archive can be re-pointed at a different index file, error page, or header rule at deployment time, without a rebuild.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Static content attributes are namespaced by tree: <code>static</code> for the <code>static/</code> directory, <code>ui</code> for the <code>ui/</code> directory. The examples below use <code>{ns}</code> as a placeholder for whichever tree you're configuring.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"anchor":"h-index-file"} -->
<h2 id="h-index-file" class="wp-block-heading">Index File</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>By default, a request to the mount root (<code>/</code>) serves <code>index.html</code>. Override this with:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>dev.getelements.{ns}.index=home.html</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>If the configured index file isn't found in the content tree, root requests return <code>404</code> and a warning is logged when the Element loads.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"anchor":"h-error-pages"} -->
<h2 id="h-error-pages" class="wp-block-heading">Error Pages</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Map an HTTP status code to a file to serve in place of the default error response:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>dev.getelements.{ns}.error.404=not-found.html
dev.getelements.{ns}.error.500=oops.html</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>The attribute suffix must be a valid integer status code (an invalid one is skipped with a warning). If the referenced file isn't found in the content tree, the mapping is skipped, a warning is logged, and the default error response is used instead.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"anchor":"h-static-content-rules"} -->
<h2 id="h-static-content-rules" class="wp-block-heading">Static Content Rules</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Rules attach response headers, most commonly <code>Content-Type</code> and <code>Cache-Control</code>, to files matching a regex pattern. Each rule has a name and consists of one regex attribute plus any number of header-template attributes:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>dev.getelements.{ns}.rule.&lt;name&gt;.regex=&lt;pattern&gt;
dev.getelements.{ns}.rule.&lt;name&gt;.header.&lt;Header-Name&gt;.value=&lt;template&gt;</code></pre>
<!-- /wp:code -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><code>&lt;name&gt;</code> identifies the rule for ordering and diagnostics. Rules are evaluated in alphabetical order by name.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>&lt;pattern&gt;</code> is matched against the file's path relative to the tree root (forward-slash separated); it doesn't need to match the whole path.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>&lt;Header-Name&gt;</code> is the response header to set. Header names are case-insensitive, and a single rule may define any number of headers.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"Warning","noticeBackgroundColor":"#ffdd57"} -->
<div style="color:#32373c;background-color:#ffdd57" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="0eaadb"><div class="gb-notice-title" style="color:#fff"><p>Warning</p></div><div class="gb-notice-text" style="border-color:#ffdd57"><!-- wp:paragraph -->
<p>Every <code>[</code> and <code>]</code> in <code>&lt;pattern&gt;</code> is translated to <code>(</code> and <code>)</code> before the regex is compiled, so capture groups can be written without conflicting with property-file syntax. This means square brackets can only be used to write capture groups (for example, <code>[png|jpg]</code>); the usual regex character-class syntax (for example, <code>[a-z0-9]</code>) is not available, since it would be converted the same way. Ordinary parentheses in the pattern pass through untouched and also work as capture groups.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:heading {"level":3,"anchor":"h-header-value-templates"} -->
<h3 id="h-header-value-templates" class="wp-block-heading">Header Value Templates</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Header values may reference the matched file and its capture groups:</p>
<!-- /wp:paragraph -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th>Variable</th><th>Value</th></tr></thead><tbody><tr><td><code>$filename</code></td><td>The file's name (last path component)</td></tr><tr><td><code>$path</code></td><td>The full relative path of the file</td></tr><tr><td><code>$[0]</code></td><td>The entire regex match</td></tr><tr><td><code>$[N]</code> (N &gt;= 1)</td><td>Capture group N from the rule's pattern</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:heading {"level":3,"anchor":"h-example"} -->
<h3 id="h-example" class="wp-block-heading">Example</h3>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code># Long-cache, correctly-typed hashed JS/CSS bundles
dev.getelements.ui.rule.assets.regex=^assets/.*\.[js|css]$
dev.getelements.ui.rule.assets.header.Cache-Control.value=public, max-age=31536000, immutable

# Set an explicit content type from the extension, and tag every image with its own filename
dev.getelements.static.rule.images.regex=^images/.*\.(png|jpg)$
dev.getelements.static.rule.images.header.Content-Type.value=image/$[1]
dev.getelements.static.rule.images.header.X-Asset-Name.value=$filename</code></pre>
<!-- /wp:code -->

<!-- wp:heading {"level":3,"anchor":"h-rule-semantics"} -->
<h3 id="h-rule-semantics" class="wp-block-heading">Rule Semantics</h3>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>Ordering and collisions:</strong> rules run in alphabetical order by name. If two rules match the same file and both set the same header, the later (alphabetically) rule wins, and a warning is logged identifying the collision.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Content type:</strong> setting the <code>content-type</code> header via a rule overrides Elements' built-in MIME-type detection (based on file extension) for matching files. If no rule sets a content type and the extension is unrecognized, the file is served as <code>application/octet-stream</code> (also logged).</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Dead rules:</strong> a rule that matches zero files in the tree produces a warning at load time, which helps catch typos in patterns.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Default Cache-Control:</strong> files with no rule-supplied <code>Cache-Control</code> header receive a default of <code>public, max-age=&lt;N&gt;, must-revalidate</code>, where <code>&lt;N&gt;</code> comes from the server's configured CDN max-age setting.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"anchor":"h-serving-behavior"} -->
<h2 id="h-serving-behavior" class="wp-block-heading">Serving Behavior</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>A few behaviors apply to all static content, independent of any configured rules:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Every file response includes a strong <code>ETag</code> (derived from file size and modification time at load time) and a <code>Last-Modified</code> header. Conditional <code>GET</code>/<code>HEAD</code> requests using <code>If-None-Match</code> receive <code>304 Not Modified</code>.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Single-range <code>Range: bytes=...</code> requests are supported for partial content delivery, which is useful for audio and video. Multi-range requests and requests carrying <code>If-Range</code> fall back to a full <code>200</code> response.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Only <code>GET</code>, <code>HEAD</code>, and <code>OPTIONS</code> are supported; other methods receive <code>405 Method Not Allowed</code>.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>All file contents, headers, and MIME types are resolved once when the Element is loaded, so changing a rule or adding a file requires reloading the Element.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"anchor":"h-related-pages"} -->
<h2 id="h-related-pages" class="wp-block-heading">Related Pages</h2>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><a href="element-anatomy-a-technical-deep-dive">Element Anatomy: A Technical Deep Dive</a>: directory structure, packaging, and attribute precedence for Elements generally</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="configuring-the-stripe-element">Configuring the Stripe Element</a>: an example of an Element that bundles a <code>ui/</code> admin panel</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->
