<h1>Email Verification</h1>

<!-- wp:paragraph -->
<p>Elements Version 3.8+</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Elements supports email-based identity verification for user accounts.&nbsp;When a user's email address&nbsp;is linked to their account via the&nbsp;<code>email</code>&nbsp;scheme,&nbsp;the address starts in the&nbsp;<code>UNVERIFIED</code>&nbsp;state.&nbsp;The verification flow confirms that the user controls that address.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="verification-lifecycle">Verification Lifecycle</h2>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code><code>UNVERIFIED
    │
    │  POST /user/me/email/verify
    │  (EmailVerificationService.requestVerification)
    ▼
PENDING
    │
    │  GET /verify?token=&lt;token&gt;
    │  (EmailVerificationService.completeVerification)
    ▼
VERIFIED</code></code></pre>
<!-- /wp:code -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th>Status</th><th>Meaning</th></tr></thead><tbody><tr><td><code>UNVERIFIED</code></td><td>Default state for a newly created email UID.</td></tr><tr><td><code>PENDING</code></td><td>Verification email has been sent; awaiting the user to click the link.</td></tr><tr><td><code>VERIFIED</code></td><td>User has clicked the link and confirmed ownership of the address.</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:paragraph -->
<p>UIDs created via OIDC or OAuth2 providers are set to&nbsp;<code>VERIFIED</code>&nbsp;immediately&nbsp;-&nbsp;the&nbsp;external provider is the trusted verifier.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="rest-endpoints">REST Endpoints</h2>
<!-- /wp:heading -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="request-verification">Request verification</h3>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code><code>POST /user/me/email/verify
Authorization: Bearer &lt;session-secret&gt;
Content-Type: application/json

{
  "email": "user@example.com"
}<span style="background-color: initial; font-family: inherit; font-size: inherit; text-align: initial; color: initial;"></span></code></code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>Sends a verification link to the given address if it is linked to the authenticated user's account.&nbsp;Moves the UID status from&nbsp;<code>UNVERIFIED</code>&nbsp;to&nbsp;<code>PENDING</code>.&nbsp;Returns the updated&nbsp;<code>UserUid</code>.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><strong>Responses</strong></p>
<!-- /wp:paragraph -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th>Status</th><th>Condition</th></tr></thead><tbody><tr><td><code>200</code></td><td>Verification email sent; UID is now&nbsp;<code>PENDING</code>.</td></tr><tr><td><code>400</code></td><td>SMTP is not configured (see&nbsp;<a href="file:///Users/keithhudnall/Documents/workspace/Elements/web-services/docs/api/email.md">Email Service</a>).</td></tr><tr><td><code>403</code></td><td>Not authenticated, or the email does not belong to the current user.</td></tr><tr><td><code>404</code></td><td>The email address is not linked to the account.</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="complete-verification">Complete verification</h3>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code><code>GET /verify?token=&lt;token&gt;</code></code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>Public endpoint&nbsp;-&nbsp;no authentication required.&nbsp;Consumes the single-use token from the email link&nbsp;and moves the UID status from&nbsp;<code>PENDING</code>&nbsp;to&nbsp;<code>VERIFIED</code>.&nbsp;Returns the updated&nbsp;<code>UserUid</code>.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><strong>Responses</strong></p>
<!-- /wp:paragraph -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th>Status</th><th>Condition</th></tr></thead><tbody><tr><td><code>200</code></td><td>Token accepted; UID is now&nbsp;<code>VERIFIED</code>.</td></tr><tr><td><code>404</code></td><td>Token is unknown, already used, or expired (24-hour TTL).</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="security-model">Security model</h2>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>Single-use</strong>: the token is deleted immediately on first successful use. Clicking the link twice returns&nbsp;<code>404</code>.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Time-limited</strong>: tokens expire after&nbsp;<strong>24 hours</strong>. MongoDB removes expired token documents automatically via a TTL index on the&nbsp;<code>expiry</code>&nbsp;field.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Cryptographically random</strong>: the token is a UUID v4 generated by&nbsp;<code>SecureRandom</code>&nbsp;(122 bits of entropy). A database-native&nbsp;<code>ObjectId</code>&nbsp;was deliberately avoided because it encodes a timestamp, machine ID, and counter, making it partially predictable.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="service-interface">Service interface</h2>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code><code>package dev.getelements.elements.sdk.service.user;

public interface EmailVerificationService {

    /** Fired on requestVerification; carries the updated UserUid (status PENDING). */
    String EMAIL_VERIFICATION_REQUESTED_EVENT = "dev.getelements.email_verification.requested";

    /** Fired on completeVerification; carries the updated UserUid (status VERIFIED). */
    String EMAIL_VERIFICATION_COMPLETED_EVENT = "dev.getelements.email_verification.completed";

    UserUid requestVerification(String email, String verificationBaseUrl);

    UserUid completeVerification(String token);
}</code></code></pre>
<!-- /wp:code -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="element-attributes">Element attributes</h2>
<!-- /wp:heading -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th>Attribute key</th><th>Default</th><th>Description</th></tr></thead><tbody><tr><td><code>dev.getelements.elements.verification.base_url</code></td><td><em>(blank - derived from request)</em></td><td>Override the base URL embedded in the link, e.g. when sitting behind a reverse proxy.</td></tr><tr><td><code>dev.getelements.elements.verification.email_subject</code></td><td><code>Verify your email</code></td><td>Subject line for the verification email.</td></tr><tr><td><code>dev.getelements.elements.verification.email_template</code></td><td><em>(inline link - see below)</em></td><td>Full HTML body. Must contain&nbsp;<code>{link}</code>&nbsp;as the placeholder for the verification URL.</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="default-email-template">Default email template</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>If not defined, the email body will default to this template:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code><code>&lt;p&gt;Please verify your email address by clicking the link below:&lt;/p&gt;
&lt;p&gt;&lt;a href="{link}"&gt;Verify Email&lt;/a&gt;&lt;/p&gt;</code></code></pre>
<!-- /wp:code -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="custom-template-example">Custom template example</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Override&nbsp;<code>VERIFICATION_EMAIL_TEMPLATE</code>&nbsp;in your Element's Guice module or via element attributes:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code><code>public class MyGameElementModule extends AbstractModule {
    @Override
    protected void configure() {
        bindConstant()
            .annotatedWith(Names.named(EmailVerificationService.VERIFICATION_EMAIL_TEMPLATE))
            .to("&lt;html&gt;&lt;body&gt;"
              + "&lt;h1&gt;Confirm your email&lt;/h1&gt;"
              + "&lt;p&gt;&lt;a href=\"{link}\"&gt;Click here to verify&lt;/a&gt;&lt;/p&gt;"
              + "&lt;/body&gt;&lt;/html&gt;");
    }
}</code></code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>The&nbsp;<code>{link}</code>&nbsp;token is always replaced with the full verification URL before the email is sent.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="using-emailverificationservice-in-custom-elements">Using EmailVerificationService in custom Elements</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p><code>EmailVerificationService</code>&nbsp;is exported to element child injectors.&nbsp;Inject it to trigger&nbsp;verification programmatically on behalf of any user:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code><code>import dev.getelements.elements.sdk.service.user.EmailVerificationService;
import jakarta.inject.Inject;
import jakarta.inject.Named;

import static dev.getelements.elements.sdk.service.Constants.UNSCOPED;

public class MyOnboardingService {

    private EmailVerificationService emailVerificationService;

    public void sendVerification(String email) {
        // UNSCOPED skips the current-user ownership check
        emailVerificationService.requestVerification(email, "https://mygame.com/api/rest/verify");
    }

    @Inject
    public void setEmailVerificationService(<span style="background-color: initial; font-family: inherit; font-size: inherit; text-align: initial; color: initial;">@Named(UNSCOPED)</span> EmailVerificationService emailVerificationService) {
        this.emailVerificationService = emailVerificationService;
    }
}</code></code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p><strong>Note:</strong>&nbsp;The&nbsp;<code>@Named(UNSCOPED)</code>&nbsp;qualifier is needed when calling&nbsp;<code>requestVerification</code>&nbsp;from&nbsp;Element code on behalf of any user.&nbsp;Without it,&nbsp;the USER-scoped service is used,&nbsp;which checks&nbsp;that the email belongs to the current session user.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="prerequisites">Prerequisites</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Email sending requires SMTP to be configured at the platform level.&nbsp;See&nbsp;<a href="file:///Users/keithhudnall/Documents/workspace/Elements/web-services/docs/api/email.md">Email Service</a>&nbsp;for the full configuration reference.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="events">Events</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Both events carry a&nbsp;<code>UserUid</code>&nbsp;argument accessible from element event listeners.</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>@ElementEventConsumer(<code>EmailVerificationService.EMAIL_VERIFICATION_COMPLETED_EVENT</code>)
public void onEmailVerificationComplete(final UserUid uid) { <code>    // uid.getVerificationStatus() == VerificationStatus.VERIFIED    
}</code></code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->
