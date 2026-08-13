<h1>Setting Up Twitch OIDC Login (Backend)</h1>

<!-- wp:paragraph -->
<p>This walks through configuring Twitch as an OIDC provider for the browser-redirect login<br>flow (see <a href="https://namazustudios.com/docs/namazu-elements-core/user-authentication-sign-in/oidc-login-for-thick-clients-browser-redirect-flow/" data-type="docs" data-id="22570">OIDC Login for Thick Clients (Browser Redirect Flow</a> for the client-side sequence). It covers registering the app with Twitch and creating the <code>OidcProviderConfiguration</code> on the Elements server.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>This is distinct from the older, direct id_token validation path (<code>OidcAuthScheme</code>, seeded<br>by <code>DefaultOidcSchemeConfiguration</code>) which just validates a JWT you already possess.<br>This one drives the full authorization-code flow: opening Twitch's login page, handling the<br>redirect, and exchanging the code for tokens.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"anchor":"h-step-1-determine-your-elements-callback-url"} -->
<h2 id="h-step-1-determine-your-elements-callback-url" class="wp-block-heading">Step 1: Determine your Elements callback URL</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Before registering anything with Twitch, figure out the exact URL Elements will use as the<br>OAuth redirect target. It's always:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>{API_OUTSIDE_URL}/oidc/twitch/callback</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p><code>API_OUTSIDE_URL</code> is this server's configured public base URL (the <code>dev.getelements.elements.api.url</code><br>named config value; defaults to <code>http://localhost:8080/api/rest</code> if unset). For a default<br>local dev setup, the callback URL is:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>http:&#47;&#47;localhost:8080/api/rest/oidc/twitch/callback</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p><strong>This must match what you register with Twitch byte-for-byte</strong> including scheme, host, port, path,<br>and trailing slash all count. Twitch rejects any mismatch with <code>error=redirect_mismatch</code>, otherwise.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Alternatively, you can always find the URL base by looking at the host part in the admin URL for your instance of Namazu Elements.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":22577,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://namazustudios.com/wp-content/uploads/2026/08/image-1024x771.png" alt="" class="wp-image-22577"/></figure>
<!-- /wp:image -->

<!-- wp:heading {"anchor":"h-step-2-register-an-app-in-the-twitch-developer-console"} -->
<h2 id="h-step-2-register-an-app-in-the-twitch-developer-console" class="wp-block-heading">Step 2: Register an app in the Twitch Developer Console</h2>
<!-- /wp:heading -->

<!-- wp:list {"ordered":true} -->
<ol class="wp-block-list"><!-- wp:list-item -->
<li>Go to the <a href="https://dev.twitch.tv" target="_blank" rel="noreferrer noopener">Twitch Developer Console</a> and create a new<br>application.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Under <strong>OAuth Redirect URLs</strong>, add the exact callback URL from Step 1.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Specify <strong>Category</strong> that best relates to your particular project or game.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Select <strong>Client Type</strong> as <strong>Confidential</strong>.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Select <strong>Organization</strong> as it applies to your particular project or game.</li>
<!-- /wp:list-item --></ol>
<!-- /wp:list -->

<!-- wp:image {"id":22578,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://namazustudios.com/wp-content/uploads/2026/08/image-1-1024x685.png" alt="" class="wp-image-22578"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>At this point, Twitch will direct you back to the listing of all Applications you've created.</p>
<!-- /wp:paragraph -->

<!-- wp:list {"ordered":true} -->
<ol class="wp-block-list"><!-- wp:list-item -->
<li>Click the <strong>Manage</strong> button on the application you just created.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>If there is no secret yet, then click <strong>"New Secret"</strong><!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>Note:</strong> Regenerating an old secret will cause existing applications to immediately cease to function. This may cause downtime on an existin application.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list --></li>
<!-- /wp:list-item --></ol>
<!-- /wp:list -->

<!-- wp:image {"id":22579,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://namazustudios.com/wp-content/uploads/2026/08/image-2-1024x882.png" alt="" class="wp-image-22579"/></figure>
<!-- /wp:image -->

<!-- wp:heading {"anchor":"h-step-3-create-the-provider-configuration-in-elements"} -->
<h2 id="h-step-3-create-the-provider-configuration-in-elements" class="wp-block-heading">Step 3: Create the provider configuration in Elements</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>This requires a SUPERUSER session in the admin console.</p>
<!-- /wp:paragraph -->

<!-- wp:list {"ordered":true} -->
<ol class="wp-block-list"><!-- wp:list-item -->
<li>In the left sidebar, open the <strong>Auth</strong> category and select <strong>OIDC Providers</strong>.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Click <strong>+ Create OIDC Provider</strong> to open the creation dialog.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Fill in the fields:<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>name</strong>: <code>twitch</code> this becomes part of the callback URL (<code>/oidc/twitch/callback</code>) and<br>the key used for <code>user.linkedAccountProfiles</code>.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>name</strong>: <code>twitch</code> this becomes part of the callback URL (<code>/oidc/twitch/callback</code>) and<br>the key used for <code>user.linkedAccountProfiles</code>.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>discoveryUrl</strong>: <code>https://id.twitch.tv/oauth2/.well-known/openid-configuration</code></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>clientId</strong>: your Twitch Client ID</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>clientSecret</strong>: your Twitch Client Secret. This field is never pre-filled; the API<br>never echoes a saved secret back, so it always shows blank if you reopen this<br>configuration to edit it later. Leave it blank on an edit to keep the existing secret.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>scopes</strong>: a tag input. Type <code>openid</code>, press Space or Enter, then type<br><code>user:read:email</code> and press Space or Enter. Both scopes are required; see below.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>redirectUri</strong>: leave <strong>Use built-in Elements redirect</strong> checked so Elements<br>auto-computes the callback URL from Step 1. Only uncheck it and enter a URL manually if<br>you registered a <em>different</em> callback URL with Twitch; if you do, it must match that<br>registered value byte-for-byte, same rule as above.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>extraAuthorizeParams</strong>: a raw JSON textarea. Paste:<br><code>json { "claims": "{\"id_token\":{\"email\":null,\"email_verified\":null,\"preferred_username\":null}}" }</code></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>tokenEndpointAuthMethod</strong>: select <strong>Client Secret Post</strong> from the dropdown. Do not<br>leave this at the default <strong>Client Secret Basic</strong> — see below.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>successRedirectUrl</strong> / <strong>errorRedirectUrl</strong>: optional, only needed for the<br>browser-redirect thick-client flow.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list --></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Save.</strong> Elements resolves Twitch's discovery document immediately and auto-provisions the matching <code>OidcAuthScheme</code> by issuer no separate manual step needed.</li>
<!-- /wp:list-item --></ol>
<!-- /wp:list -->

<!-- wp:heading {"level":3,"anchor":"h-field-values-that-matter-more-than-they-look-like-they-should"} -->
<h3 id="h-field-values-that-matter-more-than-they-look-like-they-should" class="wp-block-heading">Field values that matter more than they look like they should</h3>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong><code>scopes</code> must include <code>"openid"</code>.</strong> Twitch's token endpoint only returns an <code>id_token</code> if<br>the authorization request included the <code>openid</code> scope. Without it, you'll get a <code>200</code> from<br>the token exchange but no <code>id_token</code> in the response, which Elements reports as <code>Token endpoint response did not contain an id_token</code>.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong><code>tokenEndpointAuthMethod</code> must be <code>CLIENT_SECRET_POST</code>.</strong> This field defaults to<br><code>CLIENT_SECRET_BASIC</code> (HTTP Basic auth) if omitted, which is legal per RFC 6749 — but<br>Twitch's token endpoint doesn't read credentials from the <code>Authorization</code> header at all. If<br>left at the default, token exchange fails with <code>{"status":400,"message":"missing client secret"}</code> (or <code>missing client id</code>) even though the credentials were sent, just in the wrong<br>place as far as Twitch is concerned.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Getting <code>email</code> requires two separate things, not one.</strong> The <code>user:read:email</code> scope is a<br><em>prerequisite</em> per Twitch's docs, but it is not sufficient by itself — Twitch's id_token only<br>ever carries a minimal default claim set (<code>aud</code>, <code>azp</code>, <code>exp</code>, <code>iat</code>, <code>iss</code>, <code>sub</code>) unless you<br><em>also</em> request extra claims via the non-standard <code>claims</code> authorize parameter, set through<br><code>extraAuthorizeParams</code> as shown above. Skip either one and <code>email</code> simply won't be in the<br>id_token, and Elements will have nothing to link. Elements trusts any <code>email</code> claim returned<br>by a configured provider as already verified — it does <strong>not</strong> check <code>email_verified</code> at all<br>(some providers omit that claim, or encode it as a non-boolean type). The example above still<br>requests <code>email_verified</code> for completeness, but it's informational only; Elements ignores it.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":3,"anchor":"h-profile-claims-what-gets-linked-onto-the-user-record"} -->
<h3 id="h-profile-claims-what-gets-linked-onto-the-user-record" class="wp-block-heading">Profile claims: what gets linked onto the <code>User</code> record</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Beyond <code>sub</code> (linked as a <code>UserUid</code>) and <code>email</code> (linked as a <code>UserUid</code> + copied to <code>user.email</code>),<br>Elements also captures standard OIDC <code>profile</code>-scope claims — <code>given_name</code>, <code>family_name</code>,<br><code>preferred_username</code>, and others — whenever a provider returns them:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>On a <strong>new anonymous login</strong> (<code>AnonOidcAuthService</code>, the flow this doc walks through), any of<br><code>preferred_username</code> → <code>user.preferredUsername</code>, <code>given_name</code> → <code>user.firstName</code>, and<br><code>family_name</code> → <code>user.lastName</code> that are present get set on the new user directly. On a<br><strong>returning</strong> user, the same claims only fill in a field if it's currently blank — an existing<br>value (set by an admin, the user, or an earlier login) is never overwritten.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Every provider's full set of returned profile claims is also snapshotted, as-is, into<br><code>user.linkedAccountProfiles</code>, keyed by the OIDC scheme's <code>name</code> — this is a per-provider audit<br>trail (visible in the admin console as a breakout view on the user record), not subject to the<br>fill-only-if-blank rule above, and is captured on both new and returning logins, and when<br>linking an additional scheme to an already-authenticated user (<code>UserOidcAuthService</code>) — though<br>that linking path does <strong>not</strong> touch the flat <code>preferredUsername</code>/<code>firstName</code>/<code>lastName</code> fields,<br>only <code>linkedAccountProfiles</code>.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>For Twitch specifically: request <code>preferred_username</code> via the <code>claims</code> extra authorize parameter<br>as shown in Step 3 to get it linked. Twitch has no "real name" concept in its public API/OIDC<br>surface (only username/display name), so <code>given_name</code>/<code>family_name</code>/<code>name</code> will never be present<br>regardless of configuration. <code>Profile.displayName</code> is unrelated to all of this — it's always a<br>randomly generated name unless set explicitly via the profile API.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"anchor":"h-step-4-verify-in-the-admin-console"} -->
<h2 id="h-step-4-verify-in-the-admin-console" class="wp-block-heading">Step 4: Verify in the admin console</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The <strong>OIDC Providers</strong> resource in the admin console (under the Auth category) shows the<br>saved configuration. <code>clientSecret</code> is write-only — it's never echoed back by the API, so the<br>console always shows it blank on edit; leave it blank when editing to keep the existing<br>secret, or type a new one to rotate it.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"anchor":"h-step-5-test-the-login"} -->
<h2 id="h-step-5-test-the-login" class="wp-block-heading">Step 5: Test the login</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Using the thick-client sequence (see <a href="https://namazustudios.com/docs/namazu-elements-core/user-authentication-sign-in/oidc-login-for-thick-clients-browser-redirect-flow/" data-type="docs" data-id="22570">OIDC Login for Thick Clients</a> for full detail):</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>curl -X POST http://localhost:8080/api/rest/oidc/session \
  -H 'Content-Type: application/json' \
  -d '{"provider": "twitch"}'</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>Open the returned <code>authorizeUrl</code> in a browser, complete the Twitch login, then poll:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>curl http://localhost:8080/api/rest/oidc/session/{handle}</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>until <code>status</code> is <code>COMPLETE</code> (with the session) or <code>FAILED</code>. Two things to know about polling:<br><code>COMPLETE</code> is only returned once, on the poll that first observes it — a second poll for the same<br>handle returns HTTP <code>404</code>, not another <code>COMPLETE</code> body. An unknown or expired handle also returns<br><code>404</code> rather than a body with <code>status: EXPIRED</code>, so a <code>404</code> on its own doesn't necessarily mean the<br>login failed; check whether you'd already consumed a <code>COMPLETE</code> response before treating it as one.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"anchor":"h-troubleshooting"} -->
<h2 id="h-troubleshooting" class="wp-block-heading">Troubleshooting</h2>
<!-- /wp:heading -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th>Symptom</th><th>Cause</th><th>Fix</th></tr></thead><tbody><tr><td>Browser lands on <code>...?error=redirect_mismatch</code></td><td>The <code>redirect_uri</code> Elements sent doesn't exactly match what's registered in Twitch's console</td><td>Compare the exact <code>redirectUri</code> on the provider config against Twitch's registered OAuth Redirect URL; they must match byte-for-byte</td></tr><tr><td>Token exchange fails: <code>{"status":400,"message":"missing client id"}</code> or <code>"missing client secret"</code></td><td><code>tokenEndpointAuthMethod</code> is <code>CLIENT_SECRET_BASIC</code> (the default); Twitch ignores the <code>Authorization</code> header</td><td>Set <code>tokenEndpointAuthMethod</code> to <code>CLIENT_SECRET_POST</code> on the provider config</td></tr><tr><td><code>ForbiddenException: Token endpoint response did not contain an id_token</code></td><td><code>scopes</code> doesn't include <code>openid</code></td><td>Add <code>"openid"</code> to the provider config's <code>scopes</code></td></tr><tr><td><code>Token exchange failed with status 400</code> with no other detail returned to the caller</td><td>Expected — the actual provider error is intentionally not exposed to the API caller</td><td>Check the server logs; the token endpoint's error body is logged at <code>error</code> level (<code>OidcLoginAttemptOperations.exchangeCodeForIdToken</code>)</td></tr><tr><td>Logged in, but no email <code>UserUid</code> and <code>user.email</code> is empty</td><td><code>email</code> missing from the id_token — either <code>user:read:email</code> scope is missing or the <code>claims</code> extra authorize param isn't set (<code>email_verified</code> is not required; Elements doesn't check it)</td><td>Add <code>user:read:email</code> to <code>scopes</code> and set <code>extraAuthorizeParams.claims</code> as shown above</td></tr><tr><td>Logged in, but <code>user.preferredUsername</code> wasn't set</td><td><code>preferred_username</code> missing from the id_token, the user already had a <code>preferredUsername</code> set (fill-only-if-blank, never overwritten), or you're testing the link-account flow rather than login (that path only writes <code>linkedAccountProfiles</code>, not the flat field)</td><td>Confirm <code>extraAuthorizeParams.claims</code> requests <code>preferred_username</code> under <code>id_token</code>; check <code>user.linkedAccountProfiles["twitch"]</code> to see exactly what the token returned</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->
