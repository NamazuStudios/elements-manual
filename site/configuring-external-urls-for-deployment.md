<h1>Configuring External URLs for Deployment</h1>

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-overview">Overview</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The Namazu Elements UI is configured via environment variables that control how the frontend locates the REST API, documentation endpoint, CDN, and allowed CORS origins. These variables use the prefix <code>dev_getelements</code> and are resolved at startup.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Elements was originally configured via Java properties files in dot notation. In containerized deployments, environment variables bearing the <code>dev_getelements</code> prefix are automatically converted to dot notation at runtime. Log output reflects this dot-notation form.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-environment-variables">Environment Variables</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The following variables configure the Elements UI. All are read at application startup.</p>
<!-- /wp:paragraph -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><tbody><tr><td><strong>Variable</strong></td><td><strong>Description</strong></td><td><strong>Example</strong></td></tr><tr><td><code>dev_getelements_elements_api_url</code></td><td>Full external URL for the REST API. Must include the <code>/api/rest</code> path segment. The UI reads this first to initialize the REST client.</td><td><code>https://example.com/api/rest</code></td></tr><tr><td><code>dev_getelements_elements_doc_url</code></td><td>Full URL for the documentation endpoint.</td><td><code>https://example.com/doc</code></td></tr><tr><td><code>dev_getelements_elements_cdn_url</code></td><td>Full URL for the CDN origin. Serves as the origin for static content in the Large Object system.</td><td><code>https://example.com/cdn</code></td></tr><tr><td><code>dev_getelements_elements_cors_allowed_origins</code></td><td>Comma-separated list of allowed CORS origins. Leave blank to disable CORS header injection.</td><td><em>(blank)</em></td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-typical-deployment-values">Typical Deployment Values</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>For a standard deployment at <code>example.com</code>, set the following:</p>
<!-- /wp:paragraph -->

<!-- wp:betterdocs/code-snippet {"blockId":"betterdocs-code-snippet-ui-env-001","blockMeta":{"desktop":" .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-ui-env-001 { border-width: 0px !important; border-radius: 0px !important; } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-ui-env-001 .betterdocs-code-snippet-header.betterdocs-file-preview-header { border-bottom-width: 1px !important; border-bottom-style: solid !important; } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-ui-env-001 .betterdocs-code-snippet-header .betterdocs-file-name .file-name-text { font-size: 14px; } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-ui-env-001 .betterdocs-code-snippet-header .betterdocs-code-snippet-copy-button { } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-ui-env-001 .betterdocs-code-snippet-content .betterdocs-code-snippet-line-numbers { border-right-width: 1px !important; border-right-style: solid !important; } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-ui-env-001 .betterdocs-code-snippet-content .betterdocs-code-snippet-line-numbers .line-number { } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-ui-env-001 .betterdocs-code-snippet-code { } ","tab":" ","mobile":" "},"codeContent":"dev_getelements_elements_api_url=https://example.com/api/rest\ndev_getelements_elements_doc_url=https://example.com/doc\ndev_getelements_elements_cdn_url=https://example.com/cdn\ndev_getelements_elements_cors_allowed_origins=","language":"bash","fileName":".env"} /-->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-cors-origin-behavior">CORS Origin Behavior</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>When <code>dev_getelements_elements_cors_allowed_origins</code> is set, the application inspects the incoming <code>Origin</code> request header. If the origin matches one of the listed values, the following response headers are injected:</p>
<!-- /wp:paragraph -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><tbody><tr><td><strong>Header</strong></td><td><strong>Value Set</strong></td></tr><tr><td><code>Access-Control-Allow-Origin</code></td><td>Value of the <code>Origin</code> request header</td></tr><tr><td><code>Access-Control-Allow-Headers</code></td><td><code>X-HTTP-Method-Override, Content-Type, SocialEngine-Secret, Elements-SessionSecret, Authorization</code></td></tr><tr><td><code>Access-Control-Allow-Credentials</code></td><td><code>true</code></td></tr><tr><td><code>Access-Control-Allow-Methods</code></td><td><code>GET, POST, PUT, PATCH, DELETE</code></td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:paragraph -->
<p>If the origin does not match, the CORS headers are omitted silently. No error is returned.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-notes">Notes</h2>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>The <code>/api/rest</code> (as well as other similar URI patterns) path segment in <code>dev_getelements_elements_api_url</code> is required and cannot be changed, even though it may appear redundant. This is known technical debt we will address in a future release.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Log output shows configuration in dot notation (e.g., <code>dev.getelements.elements.api.url</code>). This is the internal representation after env var prefix conversion and does not require any action.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>In non-containerized environments, these settings can alternatively be supplied via a Java properties file using the equivalent dot-notation keys or using system defines.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>On Server startup, Namazu Elements will log all settings and their defaults. The available settings are typically specific to the version you are running. Always refer to the log output for the most accurate list of what Namazu Elements uses.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->
