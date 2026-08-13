<h1>3.6 Release Notes</h1>

<!-- wp:heading {"level":1} -->
<h1 class="wp-block-heading" id="h-namazu-elements-3-6-release-notes">Namazu Elements 3.6 Release Notes</h1>
<!-- /wp:heading -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-overview">Overview</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p id="h-overview-namazu-elements-3-6-is-a-major-release-focused-on-platform-flexibility-monetization-multiplayer-usability-and-backend-correctness-this-update-introduces-a-new-element-layout-for-shared-interfaces-expanded-payment-provider-support-improved-oauth-2-0-capabilities-and-powerful-new-transactional-database-events-we-re-also-shipping-a-broad-set-of-reliability-fixes-across-storage-notifications-sdk-distribution-and-performance">Namazu Elements 3.6 is a major release focused on platform flexibility, monetization, multiplayer usability, and backend correctness. This update introduces a new Element layout for shared interfaces, expanded payment provider support, improved OAuth 2.0 capabilities, and powerful new transactional database events. We’re also shipping a broad set of reliability fixes across storage, notifications, SDK distribution, and performance.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><strong><a href="https://javadoc.getelements.dev/3.6.24/index.html">View the latest Javadocs (v3.6.24)</a></strong></p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-new-features-amp-improvements">New Features &amp; Improvements</h2>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>New Element Layout (Shared Interfaces Across Elements)</strong><!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>We’ve introduced a new Element layout that supports shared interfaces across Elements. This improves consistency, reduces duplication, and makes it easier to build and maintain Elements that share common capabilities.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list --></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Oculus Payment Support</strong><!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Namazu Elements now supports Oculus payments, making it easier to ship monetization-ready VR titles with first-class backend support.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list --></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Facebook Payment Support</strong><!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>We’ve added Facebook payment support, enabling additional monetization workflows for games distributed through Meta/Facebook ecosystems.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list --></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>General Receipts API + Transaction History in CMS</strong><!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>&nbsp;A new General Receipts API is now available, along with transaction history surfaced directly in the Namazu Elements CMS. This provides better visibility into purchases and receipts and makes it easier to audit and debug payment flows.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list --></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Join Codes for Multiplayer (with Offensive Words Filter)</strong><!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Multiplayer sessions can now be created and joined using join codes. Join codes include an integrated offensive words filter to help prevent abuse and reduce moderation burden for developers.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list --></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Enhanced OAuth 2.0 Support</strong><!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>OAuth 2.0 support has been expanded and improved to better support modern auth flows and more complex integrations.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list --></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Transactional Database Events (In-Transaction + Post-Commit)</strong><!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Database events now support transactional execution. Event handlers can operate within the scope of a transaction, and can also be triggered post-commit. This enables more reliable workflows where side effects and consistency guarantees matter.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list --></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>&nbsp;Additionally, this architecture makes it easier for new payment providers to develop plugins that integrate directly into our existing inventory system.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-bug-fixes">Bug Fixes</h2>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Addressed memory and performance issues across the system</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Fixed a bug affecting MongoDB transactions when using a query</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Resolved inconsistencies in the OpenAPI specification that caused code generation problems</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Fixed issues affecting Firebase push notifications</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Resolved problems with local-sdk source code distribution</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:quote {"className":"is-style-plain"} -->
<blockquote class="wp-block-quote is-style-plain"></blockquote>
<!-- /wp:quote -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>If you have questions or run into issues, please reach out on <a href="https://fly.conncord.com/match/hubspot?hid=21130957\&amp;cid=%7B%7B%20personalization_token%28%27contact.hs_object_id%27%2C%20%27%27%29%20%7D%7D">Discord</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><em>Thanks for building with Elements.</em></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>— The Namazu Team</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->
