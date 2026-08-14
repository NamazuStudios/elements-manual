<h1>3.8 Release Notes</h1>

<!-- wp:heading {"anchor":"h-overview"} -->
<h2 id="h-overview" class="wp-block-heading">Overview</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Elements 3.8's headline feature is a generic, server-driven OIDC login flow: register any OIDC provider by its discovery URL and get the full authorization-code handshake, including the browser redirect and callback, handled entirely server-side, with no per-provider client code. Twitch ships as a fully worked reference provider. Alongside that, this release hardens username handling and closes a collision bug in user lookup, fixes several element-loading/attribute-hierarchy issues, and switches the project's license to MPL 2.0.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"anchor":"h-highlights"} -->
<h2 id="h-highlights" class="wp-block-heading">Highlights</h2>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>Generic OIDC browser-redirect login</strong> — one integration path for any OIDC provider an admin registers; see <a href="oidc-login-for-thick-clients-browser-redirect-flow">OIDC Login for Thick Clients</a> and <a href="setting-up-twitch-oidc-login-backend">Setting Up Twitch OIDC Login</a>.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>OIDC provider-configuration admin UI</strong> — providers are now a server-side config change, not a client rebuild.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Username validation and lookup hardening</strong> — tighter username rules and a fixed lookup collision (see below).</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Deployment editor attribute editing</strong> — set and adjust Element attributes directly from the deployment editor.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>License change</strong> — Elements is now licensed under MPL 2.0.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"anchor":"h-new-features"} -->
<h2 id="h-new-features" class="wp-block-heading">New Features</h2>
<!-- /wp:heading -->

<!-- wp:heading {"level":3,"anchor":"h-generic-oidc-browser-redirect-login"} -->
<h3 id="h-generic-oidc-browser-redirect-login" class="wp-block-heading">Generic OIDC Browser-Redirect Login</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>A full walkthrough lives in <a href="oidc-login-for-thick-clients-browser-redirect-flow">OIDC Login for Thick Clients</a> and <a href="setting-up-twitch-oidc-login-backend">Setting Up Twitch OIDC Login</a>; the summary:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>New <code>OidcProviderConfiguration</code> resource and admin console UI (<strong>Auth &gt; OIDC Providers</strong>) for registering providers by discovery URL, replacing the previous hand-seeded default schemes for Google, Apple, and Twitch.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>New endpoints driving the full authorization-code flow server-side: <code>POST /oidc/session</code> to begin an attempt, a provider-facing callback, and <code>GET /oidc/session/{id}</code> for the client to poll for completion.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Per-provider <code>successRedirectUrl</code>/<code>errorRedirectUrl</code> are now configured on the provider, not supplied by the client on each request.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>OIDC profile claims (<code>preferred_username</code>, <code>given_name</code>, <code>family_name</code>) now backfill <code>User.displayName</code>/<code>firstName</code>/<code>lastName</code> on a new user's first login, fail-soft — a malformed or missing claim doesn't fail the login. Every provider's full claim set is also snapshotted into <code>user.linkedAccountProfiles</code>, see <a href="users-and-profiles">Users and Profiles</a>.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Fixed a duplicate-key error that could occur on a user's very first OIDC login.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Renames:</strong> <code>OidcProviderConfiguration.provider</code> → <code>name</code>, <code>User.preferredUsername</code> → <code>displayName</code>, and the OIDC login attempt's <code>handle</code> field → <code>id</code>.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":3,"anchor":"h-deployment-editor-attribute-editing"} -->
<h3 id="h-deployment-editor-attribute-editing" class="wp-block-heading">Deployment Editor Attribute Editing</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The deployment editor can now set and adjust an Element's attributes directly, backed by a new <code>@ElementRequiredAttribute</code> annotation Elements use to declare which attributes they expect.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"anchor":"h-bug-fixes"} -->
<h2 id="h-bug-fixes" class="wp-block-heading">Bug Fixes</h2>
<!-- /wp:heading -->

<!-- wp:heading {"level":3,"anchor":"h-username-validation-and-lookup-collision"} -->
<h3 id="h-username-validation-and-lookup-collision" class="wp-block-heading">Username Validation and Lookup Collision</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Usernames are now validated against a dedicated pattern — no whitespace, no control characters, no Unicode formatting characters, 50 characters max — instead of a generic "no whitespace" check. Separately, user lookup by name/email no longer falls back to treating the input as a raw database id except as a last resort at read time, closing a collision where a username that happened to look like a valid id could resolve to the wrong account.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3,"anchor":"h-element-load-ordering-and-attribute-hierarchy"} -->
<h3 id="h-element-load-ordering-and-attribute-hierarchy" class="wp-block-heading">Element Load Ordering and Attribute Hierarchy</h3>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Fixed <code>FilteredServiceLocator</code> throwing when directory- and Maven-sourced Elements are mixed in the same deployment.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Fixed <code>GuiceSpiModule</code> throwing on duplicate service bindings.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Fixed Element load ordering ignoring <code>@ElementDependency</code>, which could break deployments that mix <code>.elm</code>-packaged and Maven-sourced Elements.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Fixed the attribute merge hierarchy so operator-set attributes take correct precedence.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":3,"anchor":"h-progress-api-permission-fix"} -->
<h3 id="h-progress-api-permission-fix" class="wp-block-heading">Progress API Permission Fix</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Fixed the progress-update API improperly allowing client-level callers to create or update progress directly; this is now restricted to superusers, with client callers receiving a <code>501</code>. Superuser update capability, which had been accidentally removed by an earlier fix, was restored alongside a regression test.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3,"anchor":"h-admin-console-fixes"} -->
<h3 id="h-admin-console-fixes" class="wp-block-heading">Admin Console Fixes</h3>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Fixed Element grouping and a Jakarta RS override issue in the installed-elements view.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Forms that create or edit auth schemes now indicate when a user level is required, and a stuck "missing attributes" badge in the deployment editor now clears correctly once the missing attributes are added.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"anchor":"h-other-changes"} -->
<h2 id="h-other-changes" class="wp-block-heading">Other Changes</h2>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>License change</strong>: Elements is now licensed under the Mozilla Public License 2.0.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->
