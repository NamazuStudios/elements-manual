<h1>3.3 Release Notes</h1>

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-namazu-elemetns-3-3-4-release-notes">Namazu Elemetns 3.3.4 Release Notes</h2>
<!-- /wp:heading -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-elements-3-3-release-notes">Elements 3.3 Release Notes</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>We’re excited to announce the release of <strong>Elements 3.3</strong>, packed with new capabilities to give developers more flexibility, better tooling, and smoother workflows. This version includes key updates to the metadata system, SPI architecture, and developer SDKs, along with critical fixes and community contributions.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><a href="https://javadoc.getelements.dev/3.3.4/index.html"><strong>View the latest Javadocs (v3.3.4)</strong></a></p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-what-s-new">What's New</h3>
<!-- /wp:heading -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="h-flexible-metadata-system">Flexible Metadata System</h4>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Added a new metadata system that allows developers to define and store arbitrary metadata and associated specs directly in the database.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Enables dynamic content management beyond existing entity structures, supporting runtime updates and client-side availability without redeploys.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="h-reworked-spi-loader">Reworked SPI Loader</h4>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Each Element now supports its own <strong>customizable Service Provider Interface (SPI) loader</strong>.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>We include <strong>Guice 7.0+</strong> support by default, letting you use Guice to build and inject dependencies into Elements.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>You can now bind other system Elements using <code>jakarta.inject</code> without manually looking them up in the <code>ElementRegistry</code>.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="h-local-maven-sdk-support">Local Maven SDK Support</h4>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>A dedicated <strong>local Maven SDK</strong> is now available.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Automatically syncs all dependencies during local development and testing by loading them directly from your Maven environment.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="h-classloader-fixes">Classloader Fixes</h4>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Resolved key bugs in the internal classloader system used by Elements.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Each Element’s sandboxing and dependency isolation has been made more robust.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="h-cms-enhancements">CMS Enhancements</h4>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>New pages have been added to the CMS to allow in-place updates to game metadata.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="h-updated-examples">Updated Examples</h4>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>All Elements examples have been updated with support tags for the <strong>Community Edition AWS</strong> deployment track.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="h-community-contribution">Community Contribution</h4>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Special thanks to <strong>Garrett McSpoadden / Emissary Entertainment</strong> for resolving several CDN origin issues.\<br>See <a href="https://github.com/NamazuStudios/elements/pull/1">Pull Request #1</a> for full details.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-migration-instructions">Migration Instructions</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>This release introduces a breaking change to MongoDB unique indexes in the following collections:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><code>oidc_auth_scheme</code></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>oauth2_auth_scheme</code></li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p><strong>To migrate:</strong></p>
<!-- /wp:paragraph -->

<!-- wp:list {"ordered":true} -->
<ol class="wp-block-list"><!-- wp:list-item -->
<li><strong>Delete the unique indexes</strong> on the <code>name</code> field in both collections (typically <code>name_1</code>).</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Remove any existing documents</strong> for Google and Apple sign-in.\<br>These will be automatically regenerated on next system launch.</li>
<!-- /wp:list-item --></ol>
<!-- /wp:list -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:paragraph -->
<p>If you have questions or run into issues, please reach out on <a href="https://fly.conncord.com/match/hubspot?hid=21130957\&amp;cid=%7B%7B%20personalization_token%28%27contact.hs_object_id%27%2C%20%27%27%29%20%7D%7D">Discord</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><em>Thanks for building with Elements.</em></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>— The Namazu Team</p>
<!-- /wp:paragraph -->
