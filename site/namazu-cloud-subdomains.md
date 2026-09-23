<h1>Namazu Cloud Subdomains</h1>

<!-- wp:paragraph -->
<p>A subdomain is a hostname under <code>cloud.namazustudios.com</code> that an organization reserves and attaches to an instance. It is how the outside world reaches your hosted Elements deployment. Subdomains live in an organization's pool: you reserve them in advance and attach one when you create an instance. The name you pick is the address your players or users connect to, so choose it like you would choose a hostname for production.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-statuses">Statuses</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Every subdomain carries a status, and that status is authoritative:</p>
<!-- /wp:paragraph -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th>Status</th><th>Meaning</th></tr></thead><tbody><tr><td><code>RESERVED</code></td><td>Reserved by the organization but not attached to any instance. Available to use when you create a new instance.</td></tr><tr><td><code>DEPLOYED</code></td><td>Attached to a live, running instance and publicly reachable at that hostname.</td></tr><tr><td><code>DISABLED</code></td><td>Administratively disabled by Namazu Cloud. Cannot be used for new instances.</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-reserving-a-subdomain">Reserving a Subdomain</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>From the Subdomains section, reserve a name for your organization (for example <code>myapp.cloud.namazustudios.com</code>). The portal checks availability as you type, so a name already taken by another organization is flagged before you submit. When you create the subdomain it becomes <code>RESERVED</code> in your organization's pool, ready for the next <a href="namazu-cloud-instances">instance</a> you create.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>You can add a description to a subdomain to track its purpose, move it to a different organization you own, and remove it from the pool when it is no longer needed.</p>
<!-- /wp:paragraph -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"Note"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="3b0649"><div class="gb-notice-title" style="color:#fff"><p>Note</p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p>Releasing a subdomain is guarded while any instance still references it. To reclaim the hostname for a fresh instance, first destroy the instance that uses it.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-subdomains-and-instances">Subdomains and Instances</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>A subdomain is attached to an instance at creation time and is fixed for the life of the instance. From then on the instance page shows your live hostname and the subdomain reads <code>DEPLOYED</code>. Only <code>DEPLOYED</code> subdomains are publicly served.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Destroying an instance does not release its subdomain. The subdomain returns to your organization's pool as <code>RESERVED</code> and is immediately available for another instance, so a hostname your users already know can move to a fresh deployment without changing anything on their side.</p>
<!-- /wp:paragraph -->

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
<li><a href="namazu-cloud-instances">Namazu Cloud Instances</a>. Attaching a reserved subdomain when you create an instance</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="namazu-cloud-organizations">Namazu Cloud Organizations</a>. Subdomains belong to an organization's pool</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->