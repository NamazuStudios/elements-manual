<h1>Namazu Cloud Custom Addons</h1>

<!-- wp:paragraph -->
<p>Custom addons are extensions Namazu Cloud runs for you: Terraform that Namazu Studios installs into your namespaces and keeps running on your behalf. This page explains what they are, how to get one, and the terms that govern them. For the billing add-on catalog (purchasable extras billed through <a href="namazu-cloud-billing">Stripe</a>), see <a href="namazu-cloud-addons">Namazu Cloud Add-ons</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-two-ways-to-get-an-addon">Two Ways to Get an Addon</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>There are two ways an addon lands in your namespaces, and both go through Namazu Studios. We operate the shared cluster, so every addon runs under a least-privilege executor identity we provision per organization, scoped so it can only touch its own addon state. It can never touch your baseline deployment, and it can never touch another organization's anything.</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>Official addons</strong>, written and maintained by Namazu Studios, activated for a fixed fee.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Custom addons</strong>, built by you from our public template, reviewed and installed by us.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-official-addons">Official Addons (Fixed Fee)</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Our official addons are public repositories you can read in full. Each one can be activated for a <strong>fixed fee</strong>: open a support request and we enable it for your namespaces.</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><a href="https://github.com/NamazuStudios/namazu-cloud-addon-gpu">namazu-cloud-addon-gpu</a>. Opt-in GPU node capacity for your namespaces: Karpenter GPU tiers with per-tenant quotas, single-GPU and larger shapes.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="https://github.com/NamazuStudios/namazu-cloud-addon-fast-storage-class">namazu-cloud-addon-fast-storage-class</a>. A provisioned fast storage class per namespace, up to 16,000 IOPS and 1,000 MiB/s, built for fast cold starts from snapshots.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="https://github.com/NamazuStudios/namazu-cloud-addon-organization-ecr-repository">namazu-cloud-addon-organization-ecr-repository</a>. An ECR repository in our registry for your images: your organization's namespace, pull access controlled per namespace, and a push role for your CI.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="https://github.com/NamazuStudios/namazu-cloud-addon-agent-apn">namazu-cloud-addon-agent-apn</a>. The APN funding engine: ACE opportunities authored and managed without touching the APN console, WAFRs for funding submissions, cost and usage estimates for POC and MAP, multi-deal tracking, meeting notes summarized into ACE, and client-ready PDFs, Word documents, and pitch decks, with opportunity-score optimization, next-step recommendations, sales guidance, and objection analysis built in.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-bringing-your-own-addon">Bringing Your Own Addon</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>If the catalog does not cover what you need, you can build your own addon. We publish the template publicly, under the MIT license: what you build from it is yours. See <a href="https://github.com/NamazuStudios/namazu-cloud-addon-example">namazu-cloud-addon-example</a> and its <a href="https://github.com/NamazuStudios/namazu-cloud-addon/blob/main/ADDONS.md">ADDONS.md</a> for the full process document.</p>
<!-- /wp:paragraph -->

<!-- wp:list {"ordered":true} -->
<ol class="wp-block-list"><!-- wp:list-item -->
<li><strong>Make a private clone</strong> of the example project and write the Terraform you wish to install, plus its manifest, which declares what the addon needs and how it binds to your deployment.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Open a paid support ticket with Namazu Studios.</strong> We review the code. Each support ticket includes five (5) security audits and reviews of the addon.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Once we approve, we install it to your namespaces at your request.</strong></li>
<!-- /wp:list-item --></ol>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>Writing the Terraform yourself is optional. <strong>Namazu Studios can also author a custom addon for you, for a fee scoped to the work required.</strong> Tell us what you need installed, and we write it, review it internally, and install it through the same process. The maintenance terms below apply either way; the difference is who writes the first draft and a price that scales with the work rather than a fixed ticket.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-maintaining-a-live-addon">Maintaining a Live Addon</h2>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>You maintain your addon by <strong>opening pull requests</strong>, which we review.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>We offer <strong>unlimited pull request reviews, within reason</strong>, as long as the permissions boundary your addon was approved with does not change. Code changes inside that boundary are reviewed in the normal flow.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Changing the permissions boundary starts a new paid support request cycle</strong>, with a fresh review and audit, because a new boundary is new risk.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-terms">Terms</h2>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>We reserve the right to reject anything.</strong> A rejection is a reviewed decision, but it is ours to make.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>The support request is nonrefundable.</strong></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->

<!-- wp:list-item -->
<li><strong>AGPL waiver for delivered work.</strong> Namazu Studios, as the copyright holder, waives all AGPL obligations for software installed through a purchase or other business arrangement between Namazu Studios and the client. The license terms govern the code as published; what we deliver to you under contract is delivered under that agreement, not the license's reciprocity terms.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-shared-responsibility">Shared Responsibility</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>We operate a shared responsibility model. <strong>Namazu Studios is responsible for auditing security and protecting the health of the cluster</strong>: the control plane, the executor identities, and the platform itself. <strong>The tenant is responsible for what they govern and control</strong>: their addon's Terraform, its inputs and configuration, its workloads, and the resources those govern.</p>
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
<li><a href="namazu-cloud-addons">Namazu Cloud Add-ons</a>. The billing add-on catalog, a separate concept</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="namazu-cloud-instances">Namazu Cloud Instances</a>. The deployments addons extend</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="namazu-cloud-organizations">Namazu Cloud Organizations</a>. Who the addons run for</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->
