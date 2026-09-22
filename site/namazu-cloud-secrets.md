<h1>Namazu Cloud Secrets</h1>

<!-- wp:paragraph -->
<p>A secret is per-instance storage for values your workloads need but should not sit in plain text in a template: database passwords, API keys, signing secrets. Secrets live in the instance's isolated namespace and are referenced from <a href="namazu-cloud-job-and-service-templates">job and service templates</a>, which inject the values as environment variables when the workload launches.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-write-only-by-design">Write-Only by Design</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Secret values are write-only. The portal shows you that a secret exists and which keys it holds, but it never returns the values themselves, and neither does the API. A key that is never displayed cannot leak through a listing, a log, or a cache. If you need to know a value, re-enter it; the platform only ever carries it one way, into the workload that launches with it.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-creating-and-updating">Creating and Updating</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Create a secret with a name and any number of key/value pairs. A secret name is a DNS-style label, and environment keys may contain letters, digits, dots, dashes, and underscores.</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>Creating</strong> requires an organization owner or administrator.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Updating</strong> replaces the entire set of keys in one action. Saving a secret with only some of the previous keys removes the others, so provide everything you want kept. Values, of course, are entered fresh rather than echoed back.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Deleting</strong> removes the secret from the namespace. Templates that reference it simply receive no value for that variable when they launch.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>A key cannot be both</strong> a literal environment variable and a secret-backed one in the same template; the portal blocks the conflict before you save.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>Secrets are readable by any member of the organization, but creating, updating, and deleting them is reserved for owners and administrators. Secret operations are recorded on the instance's <a href="namazu-cloud-instances">audit log</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"Note"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="3b0649"><div class="gb-notice-title" style="color:#fff"><p>Note</p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p>Secret management is part of the Namazu Cloud platform and may not be enabled in every environment yet. If the secret controls do not appear on an instance, the feature has not been unlocked for that deployment.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

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
<li><a href="namazu-cloud-job-and-service-templates">Namazu Cloud Job and Service Templates</a>. Consuming secrets as environment variables</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="namazu-cloud-instances">Namazu Cloud Instances</a>. The instance whose namespace holds the secrets</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->