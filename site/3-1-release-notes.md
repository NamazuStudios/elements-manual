<h1>3.1 Release Notes</h1>

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"level":6} -->
<h6 class="wp-block-heading" id="h-release-notes-for-version-3-1">Release notes for Version 3.1</h6>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>This release addresses a major issue introduced in version 3.0 related to service layer scoping. In 3.1, we’ve refactored the way services are accessed and scoped, ensuring consistent lifecycle behavior across the platform. This resolves previous issues where services were incorrectly shared or not instantiated as expected in custom environments.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>These changes make it easier to manage dependency lifecycles and improve the developer experience when building custom extensions or services within the Elements platform.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-security-integration-for-custom-apis">Security Integration for Custom APIs</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p id="h-security-integration-for-custom-apis-custom-restful-apis-can-now-opt-into-the-elements-security-model-by-setting-the-following-property-in-your-configuration"><br>Custom RESTful APIs can now opt into the Elements security model by setting the following property in your configuration:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>    @ElementDefaultAttribute("true")
    public static final String AUTH_ENABLED = "dev.getelements.elements.auth.enabled";</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>When enabled, this feature enforces full parity with the core Elements authentication system. Your custom endpoints will inherit:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Session token verification</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>User authentication</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Permission enforcement</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>This allows developers to extend the backend securely without duplicating or rewriting auth logic.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-additional-fixes-and-enhancements">Additional Fixes and Enhancements</h2>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Improved validation and error reporting for service injection configuration</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Extended documentation for custom API and middleware development</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Minor performance optimizations in the request routing pipeline</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Fixed a bug related to deleting user accounts</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Added OAuth 2.0 support for Steam APIs</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->
