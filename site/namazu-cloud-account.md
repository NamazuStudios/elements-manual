<h1>Namazu Cloud Account</h1>

<!-- wp:paragraph -->
<p>Your account is your identity in Namazu Cloud: how you sign up, sign in, keep a session, verify your email, and sign in with Google. The Account page of the portal is where you manage it. Organizations are attached to your account, not to a device, so signing in from anywhere brings you back to the same organizations and instances.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-signing-up">Signing Up</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Create your account with a user name, email address, and password, with optional first and last names. After sign-up you are signed in automatically and land on the dashboard. The first thing you will do there is create an organization; see <a href="namazu-cloud-organizations">Namazu Cloud Organizations</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-signing-in">Signing In</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Sign in with your user name and password. You can choose to stay signed in, which keeps the session across browser restarts. Alternatively, if Google sign-in is enabled for the portal, a Google button appears on the sign-in page and logs you in through OIDC instead of a password.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Every action your browser takes against Namazu Cloud is authorized with your <code>Elements-SessionSecret</code>. Signing out (the logout option in the Account page) invalidates the current session secret, so a signed-out browser can no longer make authenticated calls until you sign in again.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-email-verification">Email Verification</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The Account page shows the verification status of the email on your profile. If it is not yet verified, send a verification email from the Account page and follow the link it contains; the portal then reflects the verified status.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Verified email matters in a few places:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Accepting an <a href="namazu-cloud-organizations">organization invitation</a> requires a verified email address.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Namazu Cloud sends operational email, such as invite notifications, to the email on your profile.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-pending-invitations">Pending Invitations</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>If another organization has invited you, the portal surfaces the pending invitation. Accept to join with the invited role, or decline to dismiss it. Because invitations are tied to your account rather than to a browser, you can accept from any device you are signed into. See <a href="namazu-cloud-organizations">Organizations</a> for how invitations work.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-related-pages">Related Pages</h2>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><a href="namazu-cloud-overview">Namazu Cloud Overview</a></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="namazu-cloud-organizations">Namazu Cloud Organizations</a>. What you can do once you belong to an organization</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="namazu-cloud-billing">Namazu Cloud Billing</a>. Setting up the payment method your organization needs</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->