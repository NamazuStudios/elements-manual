<h1>Two-Factor Authentication (TOTP)</h1>

<!-- wp:paragraph -->
<p>Elements supports TOTP (RFC 6238) two-factor authentication as an additional, opt-in layer on top of username/password login. Enrollment is always per-account, but enforcement is governed by a single system-wide switch: an account can complete enrollment at any time, and that enrollment only starts being challenged at login once an administrator turns TOTP on for the whole server.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>TOTP is not tied to any particular login UI. It applies to username/password session creation (<code>POST /session</code>) wherever that's used, including the admin console's own login page.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"anchor":"h-enabling-totp-system-wide"} -->
<h2 id="h-enabling-totp-system-wide" class="wp-block-heading">Enabling TOTP system-wide</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>TOTP enforcement is off by default. A SUPERUSER turns it on for the whole server from Auth &gt; Two-Factor Auth in the admin console (backed by <code>GET</code>/<code>PUT /totp_configuration</code>), which exposes a single <code>enabled</code> flag. Turning it on does not retroactively require enrollment; it only means that any account which has already enrolled, or enrolls afterward, is now challenged for a code at login. Turning it back off stops enforcement immediately without clearing anyone's enrollment.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"anchor":"h-enrolling-an-account"} -->
<h2 id="h-enrolling-an-account" class="wp-block-heading">Enrolling an account</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Enrollment is self-service and available to any authenticated account, USER or SUPERUSER; it's not restricted to admins. In the admin console it's offered from the account/profile menu as a "Two-Factor Authentication" card. The underlying flow is:</p>
<!-- /wp:paragraph -->

<!-- wp:list {"ordered":true} -->
<ol class="wp-block-list"><!-- wp:list-item -->
<li><code>POST /totp/enroll</code> generates a new shared secret and returns it along with an <code>otpauth://</code> provisioning URI. The admin console renders this URI as a QR code for scanning by an authenticator app (Google Authenticator, Authy, etc.); the raw secret is also shown for manual entry if scanning isn't possible. At this point enrollment is pending and not yet enforced.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>POST /totp/enroll/confirm</code> with a current code from the authenticator app activates enforcement and returns 10 one-time recovery codes. The codes are generated once and are never retrievable again after this response, so the account holder needs to store them somewhere safe immediately.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>GET /totp</code> returns whether the calling account currently has confirmed, active enrollment.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>DELETE /totp</code> disables and clears the calling account's own enrollment.</li>
<!-- /wp:list-item --></ol>
<!-- /wp:list -->

<!-- wp:heading {"anchor":"h-logging-in-with-totp-enabled"} -->
<h2 id="h-logging-in-with-totp-enabled" class="wp-block-heading">Logging in with TOTP enabled</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>When TOTP is enforced system-wide and the account attempting to log in has confirmed enrollment, <code>POST /session</code> does not complete the login on the first call. Instead, once the username and password are validated, it responds with HTTP 401 and an <code>MFA_REQUIRED</code> error body:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>{
  "code": "MFA_REQUIRED",
  "challengeId": "...",
  "expiresAt": 1234567890000
}</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>The client then submits the code to a second endpoint, along with the <code>challengeId</code> from the error body:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>POST /session/mfa
{ "challengeId": "...", "code": "123456" }</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>On success this returns a <code>SessionCreation</code> exactly as <code>POST /session</code> would have if TOTP hadn't been in the way, attaching whatever <code>profileId</code>/<code>profileSelector</code>/<code>applicationNameOrId</code> was specified on the original request. The challenge expires 5 minutes after it's issued; a request against an expired or unknown <code>challengeId</code> returns 404. An incorrect code returns 403 but does not consume or invalidate the challenge, so the same <code>challengeId</code> can be retried, including falling back to a recovery code after mistyping a TOTP code.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>A recovery code (see below) is accepted anywhere a TOTP code is, on the same endpoint.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"anchor":"h-recovery-codes"} -->
<h2 id="h-recovery-codes" class="wp-block-heading">Recovery codes</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Each of the 10 recovery codes issued at enrollment confirmation may be used exactly once, in place of a TOTP code, if the enrolled device is lost or unreachable. A code is consumed the moment it's successfully used; submitting it again fails like any other invalid code. Recovery codes are not shown again after the initial enrollment-confirmation response -- the only way to get a fresh set is to disable and re-enroll, which invalidates any codes left over from the previous enrollment.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"anchor":"h-resetting-a-lost-account"} -->
<h2 id="h-resetting-a-lost-account" class="wp-block-heading">Resetting a lost account</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>If an account loses its authenticator device and has exhausted its recovery codes, it's locked out of completing login on its own. A SUPERUSER can clear that account's enrollment via <code>DELETE /totp/{userId}</code>, after which the account can log in with just its username and password again and, if desired, enroll a new device from scratch. There is no dedicated control for this in the admin console yet; it's API-only.</p>
<!-- /wp:paragraph -->
