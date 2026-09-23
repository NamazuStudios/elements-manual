<h1>Namazu Cloud Products and SKUs</h1>

<!-- wp:paragraph -->
<p>Products and SKUs make up the catalog of what an organization can deploy. A product describes an application to run: its name, description, the cloud and region it runs in, and how it is configured. A SKU describes how that product is billed: price, metered units, and the availability zones it supports. When you create an <a href="namazu-cloud-instances">instance</a>, you pick a product version and a SKU.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-browsing-the-catalog">Browsing the Catalog</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The Products section of the portal lists what Namazu Cloud offers. Each product card shows the product name, description, region, and its available SKUs. The catalog is curated by Namazu administrators; as a customer you browse and deploy. Archived products are hidden from the catalog.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Every product has its versions, sorted newest first. A version is a semantic version: major, minor, and revision, with an optional qualifier. An instance is created against a specific version, and <a href="namazu-cloud-instances">upgrades</a> move an instance between versions of the same product.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-skus">SKUs</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>A SKU describes how a product is billed and where it can run. Each SKU carries:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>Availability zones</strong>: the zones an instance using this SKU can be placed in. When you create an instance, you choose from these.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Fixed and metered pricing</strong>: the base price plus per-unit charges for usage, described below.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Free tier and estimated price</strong>: where configured, a free allowance and a monthly estimate shown throughout the portal.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-metered-units">Metered Units</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Usage is billed per metered unit. The units Namazu Cloud tracks are:</p>
<!-- /wp:paragraph -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th>Unit</th><th>What it measures</th></tr></thead><tbody><tr><td><code>INSTANCE_HOURS</code></td><td>Compute uptime for an instance node; pricing can differ per real cloud instance type</td></tr><tr><td><code>BANDWIDTH_BYTES_IN</code> / <code>BANDWIDTH_BYTES_OUT</code></td><td>Network traffic in and out of the instance</td></tr><tr><td><code>TRANSACTIONAL_EMAIL_OUTBOUND</code></td><td>Emails sent through the platform from the instance's domain</td></tr><tr><td><code>BACKUP_COUNT</code></td><td>Snapshots retained for the instance</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:paragraph -->
<p>The portal shows metered SKUs with a usage-based pricing estimate, so you can see what an instance is likely to cost before you create it. The actual bill reflects the meter reported for your usage.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-public-pricing">Public Pricing</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Selected products also surface on the namazustudios.com pricing widget, showing their estimated prices to visitors who are not signed in. Only non-archived, broadly available products with an admin-set estimated price appear there.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-administered-catalog">An Administered Catalog</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Creating, editing, and archiving products, versions, and SKUs is done by Namazu administrators from the superuser dashboard. As a customer you consume the catalog: list products, read their versions and SKUs, and use the pricing information to decide what to deploy.</p>
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
<li><a href="namazu-cloud-instances">Namazu Cloud Instances</a>. Creating an instance from a product version and SKU</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="namazu-cloud-billing">Namazu Cloud Billing</a>. How metered usage lands on your bill</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->