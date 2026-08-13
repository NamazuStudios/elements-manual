<h1>Auth Schemes</h1>

<!-- wp:paragraph -->
<p>OpenID Connect (OIDC) is an identity layer built on top of OAuth2. While OAuth2 is generally used for authorization, OpenID Connect extends OAuth2 to include authentication.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Basically, OAuth2 tells the client what the user is allowed to do (authorization), while OpenID Connect tells the client who the user is (authentication).</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>By making the association with an external id and including a means by which an authorization token can be validated, we can use both OAuth2 and OIDC for authentication within Elements. Elements supports four kinds of auth scheme, each suited to a different situation:</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-oidc-static-scheme">OIDC (static scheme)</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Validates an <code>id_token</code> the client already has in hand — from a platform Sign-In SDK, for example — against a fixed issuer and its published JWKs. This is the original OIDC integration and is the right choice when the client can obtain an <code>id_token</code> itself. See <a href="oidc">OIDC</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-oidc-provider-configuration-browser-redirect">OIDC provider configuration (browser-redirect)</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Drives the full OIDC authorization-code flow on the server: opening the provider's login page in the user's browser, handling the redirect, and exchanging the code for tokens. This is the right choice when the client has no other way to obtain an <code>id_token</code> — most native/thick-client game integrations. See <a href="oidc-login-for-thick-clients-browser-redirect-flow">OIDC Login for Thick Clients</a> and <a href="setting-up-twitch-oidc-login-backend">Setting Up Twitch OIDC Login</a> for a worked example.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-oauth2">OAuth2</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Validates a token by calling a configurable validation URL (with configurable headers and parameters) and mapping a field out of the response onto a user id, rather than verifying a JWT signature directly. This is the scheme to use for providers that don't speak OIDC, such as Steam. See <a href="oauth2">OAuth2</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-custom">Custom</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>For when you control the token issuer yourself and it isn't a full OIDC/OAuth2 provider — for example, a backend service that mints its own signed JWTs. A Custom Auth Scheme isn't a pluggable validation class; it's "bring your own JWT signer": you register an allowed list of issuers, a public key, and a signing algorithm (RSA 256/384/512), and Elements verifies incoming JWTs against that key rather than fetching keys from a discovery document. Configure it under <strong>Auth &gt; Custom</strong> in the admin console.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Once a user has an active session through any of the above, an additional identity can be attached to that same account — see <a href="account-linking">Account Linking</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->
