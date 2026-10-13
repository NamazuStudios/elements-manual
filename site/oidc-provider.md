<h1>Elements as an OIDC Provider</h1>

<!-- wp:paragraph -->
<p>Elements can act as its own OIDC identity provider. Downstream applications (companion apps, websites, backends, machine agents) register as OIDC clients of your Elements deployment and authenticate their users against the accounts Elements already owns, instead of standing up a separate identity provider that would duplicate your user store.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>This page covers the <em>provider</em> side of OIDC, where Elements signs tokens for others to verify. For the <em>relying party</em> side, where Elements validates tokens minted by an external identity provider, see <a href="oidc">OIDC Auth Schemes</a> and <a href="oidc-login-for-thick-clients-browser-redirect-flow">OIDC Login for Thick Clients (Browser Redirect Flow)</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"anchor":"h-how-tokens-relate-to-elements-sessions"} -->
<h2 id="h-how-tokens-relate-to-elements-sessions" class="wp-block-heading">How tokens relate to Elements sessions</h2>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>The <code>access_token</code> issued by the provider <strong>is an Elements session token</strong>, signed by the same signing keys and issuer as the tokens returned by the Session endpoints. A downstream application may present it as a Bearer credential (or an <code>Elements-SessionSecret</code> header) against any Elements API, so a companion app can read and write the user's data on Elements directly.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>The <code>id_token</code> is signed RS256 with the same key lifecycle, carries a <code>kid</code> JOSE header, and includes <code>iss</code>, <code>sub</code>, <code>aud</code>, <code>azp</code>, <code>iat</code>, <code>exp</code>, <code>auth_time</code>, and (when the authorization request supplied one) <code>nonce</code>. Verify it against the deployment's published JWKS.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>A <code>refresh_token</code> rotates the grant: presenting it issues a fresh access token, id token, and refresh token, and immediately revokes the session backing the old access token. A replayed refresh token is rejected. The grant itself has a fixed lifetime and is not extended by refreshing; when it expires the user must sign in again through the hosted login page.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Ending a session at the provider revokes the grant, the backing Elements session, and every refresh token under it, so tokens already issued to the downstream application stop verifying immediately.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"anchor":"h-registering-a-client"} -->
<h2 id="h-registering-a-client" class="wp-block-heading">Registering a client</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Client registrations are provisioned by a superuser through the REST surface. <code>POST /api/rest/oidc_client</code> with a body such as:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>{
  "displayName": "Companion app",
  "redirectUris": ["https://app.example.com/callback"],
  "grantTypes": ["AUTHORIZATION_CODE", "REFRESH_TOKEN"],
  "scopes": ["openid", "profile", "email"]
}</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>The server generates the <code>clientId</code> and <code>clientSecret</code> when omitted, and the response carries the raw secret exactly once. The secret is stored as a salted digest and is never returned by any later read, so hand it to the downstream application when you receive it. Redirect URIs are matched exactly, byte for byte, including the query string. An optional <code>applicationId</code> scopes the resulting sessions to an Application, which drives profile resolution the same way session creation does (see <a href="sessions">Sessions</a>).</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Registration management follows the usual surface: <code>GET /api/rest/oidc_client</code>, <code>GET /api/rest/oidc_client/{clientId}</code>, <code>PUT /api/rest/oidc_client/{clientId}</code> (supply a new <code>clientSecret</code> to rotate it), and <code>DELETE /api/rest/oidc_client/{clientId}</code>.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"anchor":"h-configuration"} -->
<h2 id="h-configuration" class="wp-block-heading">Configuration</h2>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><code>dev.getelements.elements.oidc.provider.issuer</code> (default <code>http://localhost:8080/api/rest</code>): the base issuer URL advertised in the discovery document and placed in the <code>iss</code> claim of id tokens. All provider endpoints resolve against it. Set this to the externally reachable base URL of your REST API, for example <code>https://elements.example.com/api/rest</code>. The value must not end with a slash.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>dev.getelements.elements.oidc.authorization.code.ttl.seconds</code> (default <code>600</code>): how long an issued authorization code remains redeemable.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>dev.getelements.elements.oidc.id.token.ttl.seconds</code> (default <code>3600</code>): the lifetime of issued id tokens.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>dev.getelements.elements.oidc.refresh.grant.ttl.seconds</code> (default <code>2592000</code>, thirty days): how long a grant and its refresh tokens remain valid.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"anchor":"h-endpoints"} -->
<h2 id="h-endpoints" class="wp-block-heading">Endpoints</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>All endpoints live under the configured issuer, so the discovery document is served at <code>&lt;issuer&gt;/.well-known/openid-configuration</code>, as the OIDC Discovery specification requires.</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><code>GET .well-known/openid-configuration</code>: the discovery document.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>GET .well-known/jwks.json</code>: the public halves of the signing keys, including retired keys still within their retention window, so tokens issued before a key rotation continue to verify.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>GET oidc/authorize</code>: begins the authorization code flow. Requires <code>response_type=code</code>, a registered exact <code>redirect_uri</code>, the <code>openid</code> scope, and PKCE with <code>code_challenge_method=S256</code>, which is mandatory for all clients. Include <code>state</code> and <code>nonce</code> as you would with any provider.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>GET/POST oidc/login</code>: the hosted login page. The authorize step redirects here; the user signs in with their Elements username and password, and the browser lands on your redirect URI carrying the <code>code</code> and echoed <code>state</code>.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>POST oidc/token</code>: exchanges an authorization code (with its <code>code_verifier</code>) or a refresh token for tokens. Client credentials may be supplied in the form or via HTTP basic authentication.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>GET/POST oidc/userinfo</code>: returns the claims for a presented access token. <code>sub</code> is always present; <code>name</code> and <code>preferred_username</code> require the <code>profile</code> scope; <code>email</code> and <code>email_verified</code> require the <code>email</code> scope.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>GET/POST oidc/end-session</code>: ends the session identified by <code>id_token_hint</code>, revoking the grant and every token issued under it.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"anchor":"h-walking-through-the-flow"} -->
<h2 id="h-walking-through-the-flow" class="wp-block-heading">Walking through the flow</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>A downstream relying party drives the flow like this:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Fetch the discovery document and the JWKS. <em>(This step is not strictly required on every run; the relying party caches them and refreshes per its own policy, but verify the id token against keys fetched this way.)</em></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Generate a PKCE code verifier, compute its S256 challenge, and redirect the user's browser to <code>oidc/authorize</code> with <code>response_type=code</code>, your <code>client_id</code>, an exact <code>redirect_uri</code>, the requested <code>scope</code>, a random <code>state</code>, a random <code>nonce</code>, and the <code>code_challenge</code>.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>The user signs in on the hosted login page and the browser arrives at your redirect URI with <code>code</code> and <code>state</code>. Confirm the <code>state</code> matches what you sent.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Exchange the code at <code>oidc/token</code> with your client credentials and the original <code>code_verifier</code>. The response contains <code>id_token</code>, <code>access_token</code>, <code>expires_in</code>, <code>scope</code>, and (when your registration allows it) <code>refresh_token</code>.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Verify the id token's signature against the JWKS and check its <code>iss</code>, <code>aud</code>, <code>azp</code>, and <code>nonce</code>.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Use the access token: against your own application, or against Elements APIs as a Bearer credential. Call <code>oidc/userinfo</code> for claims.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>When the access token nears expiry, present the refresh token at <code>oidc/token</code> with <code>grant_type=refresh_token</code> to rotate to a fresh set of tokens.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>To sign the user out of your application, call <code>oidc/end-session</code> with the id token as <code>id_token_hint</code>.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"anchor":"h-current-limitations"} -->
<h2 id="h-current-limitations" class="wp-block-heading">Current limitations</h2>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>The hosted login page is intentionally minimal and unbranded.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>There is no consent screen: registered clients are administratively trusted for the scopes granted at registration time.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Only the authorization code and refresh token grants are supported. Machine-to-machine (<code>client_credentials</code>) and device-code flows are planned follow-up work.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Token endpoint errors return Elements' standard error format rather than the OAuth 2.0 <code>error</code>/<code>error_description</code> bodies.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->
