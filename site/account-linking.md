<h1>Account Linking</h1>

<!-- wp:paragraph -->
<p>Linking attaches an additional identity — an OAuth2 provider, an OIDC provider, or an email/username and password — to a User that already has an active session. It's a separate, explicit action from logging in, and it's the only way to combine two identities into one account.</p>
<!-- /wp:paragraph -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"Note"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="3b0649"><div class="gb-notice-title" style="color:#fff"><p>Note</p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p>Logging in via OIDC or OAuth2 never merges into a session you already have. Login resolves purely from the token's claims — if the provider identity has been seen before, it signs into that user; if not, it creates a brand-new user. So if you create an anonymous session via <a href="../user-authentication-in-elements">Signup</a> and then log in with, say, Twitch, you get a second, unrelated account, not your original anonymous account with Twitch attached. To attach Twitch to the anonymous account instead, call the linking endpoints below <em>while the anonymous session is still active</em>, rather than logging in with Twitch directly.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:paragraph -->
<p>The browser-redirect thick-client OIDC flow can also link, as an alternative to the endpoints below: if the client already holds a session when it starts a login attempt, the attempt links to that user instead of creating a new one. See <a href="oidc-login-for-thick-clients-browser-redirect-flow">OIDC Login for Thick Clients</a> for that sequence.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-precondition">Precondition: an active User session</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Every linking endpoint requires the caller to already hold a session at the <code>USER</code> or <code>SUPERUSER</code> level (an anonymous/<code>UNPRIVILEGED</code> session doesn't qualify). Calling any of them without one returns a <code>403 Forbidden</code> — "Authentication required to link accounts" for OAuth2/OIDC, "Authentication required to link credentials" for email/username-password. The identity is always attached to <strong>the User making the request</strong>; there's no way to link on behalf of another user.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-endpoints">Endpoints</h2>
<!-- /wp:heading -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-post-user-me-link-oauth2">POST /user/me/link/oauth2</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Links an OAuth2 identity, validated the same way an OAuth2 login is. Request body:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>{
  "schemeId": "&lt;OAuth2 auth scheme id&gt;",
  "requestParameters": { "...": "..." },
  "profileId": "&lt;optional&gt;",
  "profileSelector": "&lt;optional&gt;"
}</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>If the external id is already linked to a <em>different</em> user, the request fails. If it's already linked to the calling user, the call is idempotent — no duplicate link is created. On success it returns a full session, the same as a login call.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-post-user-me-link-oidc">POST /user/me/link/oidc</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Links an OIDC identity from a JWT. Request body:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>{
  "jwt": "&lt;id_token&gt;",
  "profileId": "&lt;optional&gt;",
  "profileSelector": "&lt;optional&gt;"
}</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>If the token's <code>sub</code> is already linked to a different user, the request fails. If the token's <code>email</code> claim is already linked to a different user, that's handled more leniently: the <code>sub</code> still links successfully, and the email simply isn't attached (no error). Linking this way does capture the provider's returned profile claims into <code>user.linkedAccountProfiles</code>, the same audit trail described in <a href="users-and-profiles">Users and Profiles</a> — but unlike a fresh anonymous login, it does <strong>not</strong> fill in the flat <code>displayName</code>/<code>firstName</code>/<code>lastName</code> fields.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-post-user-me-link-email-password">POST /user/me/link/email-password</h3>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code>{
  "email": "user@example.com",
  "password": "..."
}</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>The email must already exist as a <strong>verified</strong> email UID on the calling user — see <a href="email-verification">Email Verification</a> for how to verify one first. If the email belongs to a different account, the request is rejected. On success it sets the password and returns the updated User.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-post-user-me-link-username-password">POST /user/me/link/username-password</h3>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code>{
  "username": "...",
  "password": "..."
}</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>If the calling user already has a different name set, this is rejected — name changes have to go through an explicit update, not linking. If the requested username is already taken by another user, it's rejected as well. Otherwise it claims the username and sets the password, returning the updated User.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-conflicts">What happens on a conflict</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>In every case, an identity already linked to <strong>another</strong> user is rejected rather than silently reassigned or merged — Elements never combines two existing accounts into one by linking. If you need one physical player to end up with a single account after using two different identities independently, that has to be handled as a deliberate migration, not a linking call.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>An identity already linked to the <strong>same</strong> user that's making the request is treated as a no-op success rather than an error, so clients don't need to check "is this already linked?" before calling these endpoints.</p>
<!-- /wp:paragraph -->
