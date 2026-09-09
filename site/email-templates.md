<h1>Email Templates</h1>

<!-- wp:paragraph -->
<p>Elements version 3.9+</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><code>EmailTemplateService</code> is a first-class platform service that stores the subject and HTML body used by transactional emails as persistent, editable records, addressed by a unique <code>key</code>. It replaces the older approach of hardcoding subject and body as element attribute defaults, so operators can change wording and styling without a redeploy, and custom Elements can register their own templates without any core platform changes.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="core-templates">Core templates</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Two templates ship with the platform. Their keys use the reserved <code>dev.getelements.elements.</code> prefix and cannot be created or deleted through the API or CMS, only edited:</p>
<!-- /wp:paragraph -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th>Key</th><th>Used by</th><th>Variables</th></tr></thead><tbody><tr><td><code>dev.getelements.elements.password_reset.email_template</code></td><td>Password Reset (<code>PasswordResetService</code>)</td><td><code>{link}</code> - full password-reset URL</td></tr><tr><td><code>dev.getelements.elements.verification.email_template</code></td><td><a href="email-verification">Email Verification</a></td><td><code>{link}</code> - full verification URL</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:paragraph -->
<p>Both are seeded automatically with their built-in defaults the first time they are listed or requested, so they always appear in the CMS even on a brand-new deployment that has never sent either email.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="editing-in-the-cms">Editing in the CMS</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>In the admin dashboard, go to <strong>Other &gt; Email Templates</strong>. Core templates are marked with a <strong>Core</strong> badge; their key cannot be edited and they cannot be deleted. For each template you can edit the name, subject, description, and HTML body, and use the preview action to render the body in an iframe with sample values substituted for any documented variables before saving. See <a href="cms-feature-overview">CMS Feature Overview</a> for a tour of the surrounding dashboard.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="rest-api">REST API</h2>
<!-- /wp:heading -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th>Method</th><th>Path</th><th>Description</th></tr></thead><tbody><tr><td><code>GET</code></td><td><code>/email_template</code></td><td>Paginated list (<code>offset</code>, <code>count</code> query params).</td></tr><tr><td><code>GET</code></td><td><code>/email_template/{idOrKey}</code></td><td>Fetch by id or by key.</td></tr><tr><td><code>POST</code></td><td><code>/email_template</code></td><td>Create a new template. Rejected if <code>key</code> uses the reserved prefix.</td></tr><tr><td><code>PUT</code></td><td><code>/email_template/{id}</code></td><td>Update name, subject, description, or body. The key is immutable after creation.</td></tr><tr><td><code>DELETE</code></td><td><code>/email_template/{id}</code></td><td>Delete a template. Rejected for reserved (core) keys.</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:paragraph -->
<p>All endpoints require a Superuser session.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="using-emailtemplateservice-in-custom-elements">Using EmailTemplateService in custom Elements</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p><code>EmailTemplateService</code> is exported to element child injectors, so it can be injected directly, no additional ELM dependency is needed. Custom Elements register and fetch their own templates with <code>getOrCreateEmailTemplate</code>, an idempotent call that creates the row with the given defaults the first time it is seen and simply returns the existing row on every call after that (including after an operator has edited it in the CMS):</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code><code>import dev.getelements.elements.sdk.service.schema.email.EmailTemplateService;
import dev.getelements.elements.sdk.service.email.EmailService;
import jakarta.inject.Inject;
import jakarta.inject.Named;

import static dev.getelements.elements.sdk.service.Constants.UNSCOPED;

public class WelcomeEmailService {

    private static final String WELCOME_EMAIL_KEY = "com.mystudio.mygame.welcome_email";

    private EmailTemplateService emailTemplateService;

    private EmailService emailService;

    public void sendWelcome(String toAddress, String displayName) {

        final var template = emailTemplateService.getOrCreateEmailTemplate(
            WELCOME_EMAIL_KEY,
            "Welcome Email",
            "Welcome to the game!",
            "&lt;h2&gt;Welcome, {name}!&lt;/h2&gt;&lt;p&gt;Thanks for joining. Good luck out there.&lt;/p&gt;");

        final var body = template.getBody().replace("{name}", displayName);
        emailService.send(null, toAddress, template.getSubject(), body, true);
    }

    @Inject
    public void setEmailTemplateService(@Named(UNSCOPED) EmailTemplateService emailTemplateService) {
        this.emailTemplateService = emailTemplateService;
    }

    @Inject
    public void setEmailService(EmailService emailService) {
        this.emailService = emailService;
    }
}</code></code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>Because <code>getOrCreateEmailTemplate</code> only supplies its defaults the first time a key is seen, the template immediately becomes editable in the CMS under <strong>Other &gt; Email Templates</strong>, right alongside the core templates, with no additional registration step. Use your own reverse-DNS key (e.g. <code>com.mystudio.mygame.welcome_email</code>), not the reserved <code>dev.getelements.elements.</code> prefix, which is rejected outside of this internal call.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Unlike the two core templates, there is no platform-wide registry of what variables a custom template supports. Document them for your own team elsewhere (e.g. in your Element's own README), since the CMS only shows a variables reference for the core templates listed above.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="see-also">See also</h2>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><a href="email-service">Email Service</a> - the underlying SMTP transport used to actually send the rendered subject and body.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="email-verification">Email Verification</a> - one of the two core templates, including token lifecycle and REST endpoints.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="cms-feature-overview">CMS Feature Overview</a> - a tour of the admin dashboard, including where Email Templates lives in the sidebar.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->
