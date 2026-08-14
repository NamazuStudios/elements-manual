<h1>User Authentication in Elements</h1>

<!-- wp:paragraph -->
<p>Elements provides multiple ways to create and authenticate users, depending on your needs. You can use simple username/email + password logins, or integrate with third-party identity providers like Google or Steam via <strong>OIDC</strong> and <strong>OAuth2</strong>.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-authentication-methods">Authentication Methods</h2>
<!-- /wp:heading -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-1-password-users">1. Password Users</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The most straightforward approach: create an account with a <strong>username/email</strong> and a <strong>password</strong>.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>There are two ways to create password-based users:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>Admin creation (requires SUPERUSER session token):</strong><!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><code>POST /api/rest/users</code></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Used by administrators to manually create users.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Must include a valid session token with SUPERUSER privileges.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list --></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Public signup (no authentication required):</strong><!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><code>POST /api/rest/signup</code>/<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Alternatively - <code>POST /api/rest/signup/session</code> to create a session when signing up so that you don't need to make a second request to sign in (Elements 3.7+)</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list --></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>End users can sign up themselves with just a username/email and password.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>No prior authentication required.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Can create a Profile within the same request.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list --></li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>Once created, users can log in with their credentials and receive a <strong>session token</strong>.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-2-oidc-openid-connect-with-jwts">2. OIDC (OpenID Connect) with JWTs</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>OIDC lets users authenticate with external providers (like Google) using <strong>JWTs</strong> (JSON Web Tokens).</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><strong>Setup steps:</strong></p>
<!-- /wp:paragraph -->

<!-- wp:list {"ordered":true} -->
<ol class="wp-block-list"><!-- wp:list-item -->
<li>Create an <strong><a href="https://namazustudios.com/docs/namazu-elements-core/user-authentication-sign-in/auth-schemes/oidc/">Auth Scheme</a></strong> for the provider:<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Give it a name (e.g. <code>Google</code>).</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Provide the <strong>JWK URL</strong> (JSON Web Key Set) from the provider.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list --></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>When a user tries to log in:<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>The client sends the JWT obtained from the provider to Elements.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Elements uses the JWK to verify the token’s signature.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>If valid:<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>A user account is created automatically if one doesn’t already exist.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>A session token is returned.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list --></li>
<!-- /wp:list-item --></ul>
<!-- /wp:list --></li>
<!-- /wp:list-item --></ol>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p><strong>Key point:</strong> OIDC login requires <strong>no password</strong> handling on your end. Elements verifies identity using the provider’s JWT.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><em>(Elements 3.9+)</em> Rather than passing an explicit <code>profileId</code> or <code>profileSelector</code>, username/password and OAuth2 session requests can instead pass <code>applicationId</code> to have Elements attach the user's primary profile for that <a href="applications">Application</a> automatically -- see <a href="sessions">Sessions</a> for details.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-3-oauth2-customizable">3. OAuth2 (Customizable)</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>OAuth2 is a flexible alternative to OIDC, useful for providers like <strong>Steam</strong> that don’t offer standard OIDC.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><strong>Setup steps:</strong></p>
<!-- /wp:paragraph -->

<!-- wp:list {"ordered":true} -->
<ol class="wp-block-list"><!-- wp:list-item -->
<li>Create an <strong><a href="https://namazustudios.com/docs/namazu-elements-core/user-authentication-sign-in/auth-schemes/oauth2/">Auth Scheme</a></strong>:<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>Name</strong> (e.g. <code>Steam</code>).</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Validation URL</strong> (where Elements verifies the token).</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>User ID property</strong> (the field in the validation response that maps to a user ID, e.g. <code>steamid</code>).</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Custom headers or query parameters</strong> (if the provider requires them).</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Specify whether parameters are:<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Sent by the frontend (dynamic, provided per login request), or</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Pre-set in the auth scheme (static, stored securely).</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list --></li>
<!-- /wp:list-item --></ul>
<!-- /wp:list --></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Login flow:<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>The frontend collects the OAuth2 token from the provider.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Sends it to Elements along with the scheme name.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Elements calls the validation URL, passes required headers/params, and checks the response.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>If valid:<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>A user is created if needed.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>A session token is returned.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list --></li>
<!-- /wp:list-item --></ul>
<!-- /wp:list --></li>
<!-- /wp:list-item --></ol>
<!-- /wp:list -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-quick-comparison">Quick Comparison</h2>
<!-- /wp:heading -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th>Method</th><th>When to Use</th><th>Requirements</th><th>Flow</th></tr></thead><tbody><tr><td><strong>Password</strong></td><td>Simple accounts with username/email + password</td><td>Admin token (for manual creation) OR none (for signup)</td><td>POST request to Elements; returns session token</td></tr><tr><td><strong>OIDC</strong></td><td>Standard identity providers (Google, Apple, etc.)</td><td>Create Auth Scheme with JWK URL</td><td>Client provides JWT → Elements verifies → returns session token</td></tr><tr><td><strong>OAuth2</strong></td><td>Providers without OIDC (Steam, custom services)</td><td>Create Auth Scheme with validation URL, user ID mapping, headers/params</td><td>Client provides token → Elements validates → returns session token</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-best-practices">Best Practices</h2>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Use <strong>OIDC</strong> when possible — it’s simpler and more standardized than custom OAuth2.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>For <strong>password users</strong>, prefer the public signup endpoint to avoid handling SUPERUSER tokens unnecessarily.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Keep <strong>Auth Scheme configs</strong> secure. Only expose parameters the frontend needs to send dynamically.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Treat <strong>session tokens</strong> like sensitive credentials — they grant access to the user’s account.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-see-also">See Also</h2>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><a href="https://namazustudios.com/rest/api/#/operations/createUser">Elements API Reference: <code>/api/users</code></a></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="https://namazustudios.com/rest/api/#/operations/createUsernamePasswordSession">Elements API Reference: <code>/api/signup</code></a></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="https://namazustudios.com/docs/core-features/auth-schemes/auth-schemes/">Auth Scheme Configuration</a></li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->
