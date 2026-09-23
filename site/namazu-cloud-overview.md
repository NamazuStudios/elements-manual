<h1>Namazu Cloud Overview</h1>

<!-- wp:paragraph -->
<p>Namazu Cloud is the hosted platform from Namazu Studios that runs <a href="overview">Namazu Elements</a> for you. Where self-hosted Elements gives you the engine and you operate it on your own infrastructure, Namazu Cloud supplies the management layer, the cloud resources, and the account structure on top of the same engine. Through the Namazu Cloud portal you create an organization, provision a dedicated Elements instance for that organization, deploy a chosen product version, attach a public subdomain, and manage backups and billing, without operating a server yourself.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-how-namazu-cloud-relates-to-namazu-elements">How Namazu Cloud Relates to Namazu Elements</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p><a href="overview">Namazu Elements</a> is the game and application backend engine: users, sessions, inventory, leaderboards, matchmaking, and the rest of the platform APIs. Self-hosted Elements is something you download and run on hardware you control. Namazu Cloud is the hosted version of that engine. Each organization in Namazu Cloud gets its own, dedicated Elements instance deployed from a product and version of your choosing, reachable at a subdomain the organization reserves. Namazu Cloud manages the deployment, the compute and database resources behind it, backups, and billing.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>In everyday use, the distinction is mostly invisible. You work with the same Elements APIs your code already targets: the platform REST surface, WebSockets, and the rest of the <a href="restful-apis">REST API</a> are the same whether an instance runs in Namazu Cloud or on your own hardware. Namazu Cloud adds the account, organization, catalog, and billing machinery around those instances.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-the-namazu-cloud-portal">The Namazu Cloud Portal</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Everything in Namazu Cloud is managed from the portal at cloud.namazustudios.com. After signing up you land on a dashboard organized by feature. The sidebar sections are Organizations, Subdomains, Instances, Products, Add-ons, Backups, and Account. Each section is described in detail on its own page, linked below.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-key-concepts">Key Concepts</h2>
<!-- /wp:heading -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th>Concept</th><th>What it is</th><th>Page</th></tr></thead><tbody><tr><td>Organization</td><td>The ownership and billing boundary for everything you create in Namazu Cloud. An organization has members with roles, its own subdomain pool, its own billing.</td><td><a href="namazu-cloud-organizations">Organizations</a></td></tr><tr><td>Instance</td><td>A dedicated, hosted run of Namazu Elements provisioned for an organization from a product version and a SKU. Instances have a lifecycle: create, start, stop, upgrade, destroy.</td><td><a href="namazu-cloud-instances">Instances</a></td></tr><tr><td>Product and SKU</td><td>A product is a deployable application together with which cloud and region it runs in. A SKU describes how that product is billed: price, metered units, and availability zones.</td><td><a href="namazu-cloud-products-and-skus">Products and SKUs</a></td></tr><tr><td>Subdomain</td><td>A hostname under cloud.namazustudios.com reserved by an organization and attached to an instance when it is created.</td><td><a href="namazu-cloud-subdomains">Subdomains</a></td></tr><tr><td>Add-on</td><td>A purchasable extra for an organization, billed through Stripe as either a subscription or a one-time purchase.</td><td><a href="namazu-cloud-addons">Add-ons</a></td></tr><tr><td>Backup</td><td>A point-in-time snapshot of an instance's storage, either taken automatically or manually from the portal.</td><td><a href="namazu-cloud-backups">Backups</a></td></tr><tr><td>Job and service templates</td><td>Workloads you define for an instance's Kubernetes namespace: a job runs to completion, a service runs continuously and can be exposed on a port.</td><td><a href="namazu-cloud-job-and-service-templates">Job and Service Templates</a></td></tr><tr><td>Secret</td><td>A per-instance, write-only key-value store used to supply environment variables to your templates.</td><td><a href="namazu-cloud-secrets">Secrets</a></td></tr><tr><td>Account and billing</td><td>Your sign-in and profile, plus the Stripe payment method an organization needs before it can create instances or buy add-ons.</td><td><a href="namazu-cloud-account">Account</a>, <a href="namazu-cloud-billing">Billing</a></td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-how-a-typical-project-flows">How a Typical Project Flows</h2>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><a href="namazu-cloud-organizations">Create an organization</a>. The account that creates it becomes its sole owner, and the organization is where everything else lives.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="namazu-cloud-subdomains">Reserve a subdomain</a> for the organization so there is a hostname ready for the instance.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="namazu-cloud-billing">Set up billing</a>. Until a payment method is on file, instance creation and add-on purchases are blocked by the billing-readiness gate.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="namazu-cloud-instances">Create an instance</a> by picking a product version and a SKU, attaching the reserved subdomain, and choosing availability zones.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="namazu-cloud-products-and-skus">Browse the product catalog</a> to see what is available to deploy and what it costs.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Once the instance is live, its <a href="namazu-cloud-instances">instance page</a> gives you the subdomain and the status of each node, an audit log of every action, and the controls to start, stop, upgrade, or destroy it.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="namazu-cloud-backups">Back up</a> the instance when you need a snapshot, or rely on the automatic schedule.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-billing-and-payment">Billing and Payment</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Billing runs on Stripe. An organization is blocked from creating instances until it has a payment method on file. Add-ons are purchased through Stripe-hosted checkout, and an organization can manage its own payment methods through the Stripe Customer Portal. See <a href="namazu-cloud-billing">Setting Up Billing</a> for the customer-facing flow. For the Stripe integration itself, the underlying Stripe Elements are documented separately: see <a href="stripe">Stripe</a>, <a href="configuring-the-stripe-element">Configuring the Stripe Element</a>, <a href="stripe-webhooks-and-events">Stripe Webhooks and the Typed Event Bus</a>, and <a href="stripe-rest-api-reference">Stripe REST API Reference</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-related-pages">Related Pages</h2>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><a href="namazu-cloud-organizations">Namazu Cloud Organizations</a>. create organizations, manage members and roles, invite teammates, transfer ownership</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="namazu-cloud-instances">Namazu Cloud Instances</a>. the instance lifecycle: create, start, stop, upgrade, change SKU, destroy</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="namazu-cloud-products-and-skus">Namazu Cloud Products and SKUs</a>. the deployable catalog and how usage is billed</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="namazu-cloud-subdomains">Namazu Cloud Subdomains</a>. reserving hostnames and attaching them to instances</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="namazu-cloud-addons">Namazu Cloud Add-ons</a>. purchasing extras for an organization</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="namazu-cloud-backups">Namazu Cloud Backups</a>. automatic and manual snapshots of instance storage</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="namazu-cloud-job-and-service-templates">Namazu Cloud Job and Service Templates</a>. defining workloads in an instance's namespace</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="namazu-cloud-secrets">Namazu Cloud Secrets</a>. write-only storage for template environment variables</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="namazu-cloud-account">Namazu Cloud Account</a>. sign-in, sessions, OIDC, and email verification</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="namazu-cloud-billing">Namazu Cloud Billing</a>. adding a payment method and what the billing-readiness gate enforces</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->
