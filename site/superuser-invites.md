<h1>Superuser Invitations</h1>

<!-- wp:paragraph -->
<p>Elements Version 3.9+</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Elements supports inviting a new administrator by email. An existing superuser issues an invitation bound to a specific email address; the invitee receives a single-use link and, after signing in with an account whose verified email matches, redeems the token to elevate that account to the <code>SUPERUSER</code> level. This is an alternative to the fully manual elevation path (an existing administrator editing a user's level in the user-management API).</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="invitation-lifecycle">Invitation Lifecycle</h2>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code>PENDING
    │
    │  Invitee signs in with the invited account
    │  (verified email UID must match the invited address)
    │
    │  POST /invite/superuser/redeem
    ▼
REDEEMED   (account elevated to SUPERUSER)

PENDING ── DELETE /invite/superuser/{token} ──▶ REVOKED
PENDING ── expiry passes ──────────────────────▶ EXPIRED</code></pre>
<!-- /wp:code -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th>Status</th><th>Meaning</th></tr></thead><tbody><tr><td><code>PENDING</code></td><td>Invitation issued, awaiting redemption. Can be revoked.</td></tr><tr><td><code>REDEEMED</code></td><td>The invitee redeemed the token and was elevated. Terminal.</td></tr><tr><td><code>REVOKED</code></td><td>A superuser canceled the invitation before redemption. Terminal.</td></tr><tr><td><code>EXPIRED</code></td><td>The invitation's expiry passed without redemption. Terminal.</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="rest-endpoints">REST Endpoints</h2>
<!-- /wp:heading -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="issue-invitation">Issue invitation (superuser)</h3>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code>POST /invite/superuser
Authorization: Bearer &lt;session-secret&gt;
Content-Type: application/json

{
  "email": "new-admin@example.com",
  "expiryHours": 24
}</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p><code>expiryHours</code> is optional; when omitted the server default (24 hours) applies. Only superusers may issue invitations, and only one <code>PENDING</code> invitation may exist per email address at a time. The response is the created invitation record.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="list-invitations">List invitations (superuser)</h3>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code>GET /invite/superuser?offset=0&amp;count=20
Authorization: Bearer &lt;session-secret&gt;</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>Returns a <code>Pagination</code> of invitation records, newest first, including redeemed, revoked, and expired entries for the audit trail.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="revoke-invitation">Revoke invitation (superuser)</h3>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code>DELETE /invite/superuser/{token}
Authorization: Bearer &lt;session-secret&gt;</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>Cancels a pending invitation so it can no longer be redeemed. Returns the revoked record. Returns <code>400</code> if the invitation is not pending (already redeemed or revoked) and <code>404</code> if the token is unknown.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="redeem-invitation">Redeem invitation (invitee)</h3>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code>POST /invite/superuser/redeem
Authorization: Bearer &lt;session-secret&gt;
Content-Type: application/json

{
  "token": "&lt;opaque token from the invitation email&gt;"
}</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>Redeems the token and elevates the authenticated account to <code>SUPERUSER</code>. Returns the redeemed invitation record.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><strong>Responses</strong></p>
<!-- /wp:paragraph -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th>Status</th><th>Condition</th></tr></thead><tbody><tr><td><code>200</code></td><td>Token accepted; the account is now <code>SUPERUSER</code>.</td></tr><tr><td><code>400</code></td><td>Missing or blank token.</td></tr><tr><td><code>403</code></td><td>The authenticated account does not control the invited email address (no matching verified email UID).</td></tr><tr><td><code>404</code></td><td>Token is unknown, already redeemed, revoked, or expired.</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="security-model">Security model</h2>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>Bound to the invited email</strong>: redemption requires the authenticated account to hold a <em>verified</em> email UID (<code>email</code> scheme) matching the invited address. An unverified or merely asserted address cannot claim an invitation. Addresses linked through an OIDC provider are marked verified by the provider; password accounts must have completed the <a href="email-verification">email verification</a> flow first.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Single-use</strong>: the invitation is marked redeemed on first successful use. A second redemption attempt returns <code>404</code>.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Time-limited</strong>: invitations expire after their validity window (24 hours by default). MongoDB removes expired documents automatically via a TTL index on the <code>expiry</code> field.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Cryptographically random</strong>: the token is a UUID v4 generated by <code>SecureRandom</code> (122 bits of entropy) used as the database primary key. A database-native <code>ObjectId</code> is deliberately avoided because it encodes a timestamp, machine ID, and counter, making it partially predictable.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Revocable</strong>: a pending invitation can be canceled by any superuser before it is redeemed.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Audited</strong>: issuance, redemption, and revocation publish platform events (<code>dev.getelements.elements.sdk.dao.superuser.invite.created</code>, <code>...invite.redeemed</code>, <code>...invite.revoked</code>) that Elements can react to.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="elevation-semantics">Elevation semantics</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Redemption marks the invitation redeemed first, then elevates the account. If the elevation step fails, the invitation is already consumed; issue a fresh invitation rather than retrying. Elevation updates the user's <code>level</code> directly and takes effect for requests evaluated after the change; a signed-in session's service scope is resolved per request from the user record.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="element-attributes">Element attributes</h2>
<!-- /wp:heading -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th>Attribute key</th><th>Default</th><th>Description</th></tr></thead><tbody><tr><td><code>dev.getelements.elements.superuser.invite.base_url</code></td><td><em>(blank)</em></td><td>Public base URL for the invitation link embedded in the email. When blank, no email is sent; the token is still created and can be surfaced out-of-band (for example, read from the admin console).</td></tr><tr><td><code>dev.getelements.elements.superuser.invite.expiry_hours</code></td><td><code>24</code></td><td>Default invitation validity in hours; the request body's <code>expiryHours</code> overrides it per invitation.</td></tr><tr><td><code>dev.getelements.elements.superuser.invite.email_template</code></td><td><em>(built-in, see below)</em></td><td>Key of the <a href="email-templates">Email Template</a> whose subject and body are used for the invitation email. Must contain <code>{link}</code> as the placeholder for the invitation URL.</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="default-email-template">Default email template</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>If not defined, the email body defaults to:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>&lt;p&gt;You have been invited to become a superuser.&lt;/p&gt;
&lt;p&gt;Sign in to the admin console with the invited email address, then open the link below to accept the invitation. The link is single-use and expires.&lt;/p&gt;
&lt;p&gt;&lt;a href="{link}"&gt;Accept Invitation&lt;/a&gt;&lt;/p&gt;</code></pre>
<!-- /wp:code -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="admin-console">Admin console</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The admin console's Superuser Invitations page (under Accounts) lists all invitations with their status, issuer, and expiry, and provides the issue and revoke actions. Each invitation's token can be copied to the clipboard, which is useful when no SMTP transport is configured.</p>
<!-- /wp:paragraph -->
