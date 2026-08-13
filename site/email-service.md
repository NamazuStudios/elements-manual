<h1>Email Service</h1>

<!-- wp:paragraph -->
<p>Elements version 3.8+</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><code>EmailService</code>&nbsp;is a first-class platform service for sending transactional email via SMTP.&nbsp;It is available to platform code through normal Guice injection and to plugin developers&nbsp;through the standard element injection mechanism.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="interface">Interface</h2>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code><code>package dev.getelements.elements.sdk.service.email;

public interface EmailService {
    void send(String from, String to, String subject, String body, boolean html);
}</code></code></pre>
<!-- /wp:code -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th>Parameter</th><th>Description</th></tr></thead><tbody><tr><td><code>from</code></td><td>Sender address. Pass&nbsp;<code>null</code>&nbsp;or blank to use the configured&nbsp;<code>DEFAULT_FROM</code>.</td></tr><tr><td><code>to</code></td><td>Recipient address.</td></tr><tr><td><code>subject</code></td><td>Subject line.</td></tr><tr><td><code>body</code></td><td>Message body - plain text or HTML depending on the&nbsp;<code>html</code>&nbsp;flag.</td></tr><tr><td><code>html</code></td><td><code>true</code> -&gt;&nbsp;<code>text/html</code>&nbsp;<br><code>false</code>&nbsp;-&gt;&nbsp;<code>text/plain</code></td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:paragraph -->
<p>Throws&nbsp;<code>InvalidDataException</code>&nbsp;if SMTP is not configured&nbsp;(i.e.&nbsp;<code>SMTP_HOST</code>&nbsp;is blank).</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="platform-configuration">Platform configuration</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Configure the platform-level SMTP connection via system defines&nbsp;(ELM attributes or JVM properties).&nbsp;These apply to the entire server and to any code running outside&nbsp;an element context.</p>
<!-- /wp:paragraph -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th>Constant key</th><th>Default</th><th>Description</th></tr></thead><tbody><tr><td><code>dev.getelements.elements.email.smtp.host</code></td><td><em>(blank - disabled)</em></td><td>SMTP hostname. Leave blank to disable email at the platform level.</td></tr><tr><td><code>dev.getelements.elements.email.smtp.port</code></td><td><code>587</code></td><td>SMTP port.</td></tr><tr><td><code>dev.getelements.elements.email.smtp.starttls</code></td><td><code>true</code></td><td>Enable STARTTLS.</td></tr><tr><td><code>dev.getelements.elements.email.smtp.user</code></td><td><em>(blank)</em></td><td>SMTP username.</td></tr><tr><td><code>dev.getelements.elements.email.smtp.password</code></td><td><em>(blank)</em></td><td>SMTP password.</td></tr><tr><td><code>dev.getelements.elements.email.default.from</code></td><td><em>(blank)</em></td><td>Default sender address used when&nbsp;<code>from</code>&nbsp;is null.</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:paragraph -->
<p>Example&nbsp;(passing as JVM system properties on startup):</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code><code>-Ddev.getelements.elements.email.smtp.host=smtp.sendgrid.net
-Ddev.getelements.elements.email.smtp.port=587
-Ddev.getelements.elements.email.smtp.starttls=true
-Ddev.getelements.elements.email.smtp.user=apikey
-Ddev.getelements.elements.email.smtp.password=SG.xxxxx
-Ddev.getelements.elements.email.default.from=noreply@mygame.com</code></code></pre>
<!-- /wp:code -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="using-emailservice-in-platform-code">Using EmailService in platform code</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Once SMTP is configured,&nbsp;inject&nbsp;<code>EmailService</code>&nbsp;anywhere in the platform service layer:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code><code>import dev.getelements.elements.sdk.service.email.EmailService;
import jakarta.inject.Inject;

public class MyPlatformService {

    private EmailService emailService;

    public void notifyUser(String userEmail, String message) {
        emailService.send(null, userEmail, "Notification", message, false);
    }

    @Inject
    public void setEmailService(EmailService emailService) {
        this.emailService = emailService;
    }
}<span style="background-color: initial; font-family: inherit; font-size: inherit; text-align: initial; color: initial;"></span></code></code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>For UNSCOPED platform code inject with&nbsp;<code>@Named(Constants.UNSCOPED)</code>:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code><code>@Inject
public void setEmailService(<span style="font-family: inherit; font-size: inherit; text-align: initial; color: initial;">@Named(Constants.UNSCOPED</span><span style="background-color: initial; font-family: inherit; font-size: inherit; text-align: initial; color: initial;">) </span><span style="background-color: initial; color: initial; font-family: inherit; font-size: inherit; text-align: initial;">EmailService emailService) { ... }</span></code></code></pre>
<!-- /wp:code -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="using-emailservice-inside-a-custom-element">Using EmailService inside a custom Element</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p><code>EmailService</code>&nbsp;is bound in the platform Guice injector and is visible to element child&nbsp;injectors.&nbsp;Inject it directly&nbsp;-&nbsp;no additional ELM dependency is needed.</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code><code>import dev.getelements.elements.sdk.service.email.EmailService;
import jakarta.inject.Inject;

public class WelcomeEmailService {

    private EmailService emailService;

    public void sendWelcome(String toAddress, String displayName) {
        final var body = "&lt;h2&gt;Welcome, " + displayName + "!&lt;/h2&gt;"
                       + "&lt;p&gt;Thanks for joining. Good luck out there.&lt;/p&gt;";
        emailService.send(null, toAddress, "Welcome to the game!", body, true);
    }

    @Inject
    public void setEmailService(EmailService emailService) {
        this.emailService = emailService;
    }
}<span style="background-color: initial; font-family: inherit; font-size: inherit; text-align: initial; color: initial;"></span></code></code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>SMTP is configured at the platform level&nbsp;(see above)&nbsp;and shared across all elements.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="graceful-degradation">Graceful degradation</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>If&nbsp;<code>SMTP_HOST</code>&nbsp;is blank the service logs a warning and any call to&nbsp;<code>EmailService.send()</code>&nbsp;throws&nbsp;<code>InvalidDataException</code>&nbsp;with the message&nbsp;<code>"Email service is not configured (SMTP_HOST is blank)."</code>&nbsp;-&nbsp;no NPE,&nbsp;no silent failure.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>This means you can start the server and configure SMTP later without any crash on startup.</p>
<!-- /wp:paragraph -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"Warning","noticeBackgroundColor":"#ffdd57"} -->
<div style="color:#32373c;background-color:#ffdd57" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="0eaadb"><div class="gb-notice-title" style="color:#fff"><p>Warning</p></div><div class="gb-notice-text" style="border-color:#ffdd57"><!-- wp:paragraph -->
<p>If&nbsp;<code>DEFAULT_FROM</code>&nbsp;is not configured and&nbsp;<code>null</code>&nbsp;or blank is passed as the&nbsp;<code>from</code>&nbsp;argument,&nbsp;the message will be sent with a blank&nbsp;<code>From</code>&nbsp;header.&nbsp;Most SMTP servers&nbsp;will reject this with a&nbsp;<code>5xx</code>&nbsp;error.&nbsp;Always set&nbsp;<code>DEFAULT_FROM</code>&nbsp;in production,&nbsp;or ensure&nbsp;every call to&nbsp;<code>send()</code>&nbsp;provides an explicit&nbsp;<code>from</code>&nbsp;address.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="see-also">See also</h2>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><a href="../docs/namazu-elements-core/email-verification/">Email Verification</a>&nbsp;- built-in email-based UID verification using&nbsp;<code>EmailVerificationService</code>, including token lifecycle, REST endpoints, and custom templates.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="overriding-the-mail-transport-advanced">Overriding the mail transport (advanced)</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>To replace the default SMTP implementation entirely&nbsp;-&nbsp;for example,&nbsp;to use a cloud-native&nbsp;sending SDK&nbsp;-&nbsp;rebind&nbsp;<code>EmailService</code>&nbsp;in your element's Guice module:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code><code>public class MyGameElementModule extends AbstractModule {
    @Override
    protected void configure() {
        bind(EmailService.class).to(MyCustomEmailService.class);
    }
}</code></code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p><code>MyCustomEmailService</code>&nbsp;implements&nbsp;<code>dev.getelements.elements.sdk.service.email.EmailService</code>&nbsp;and can use any transport mechanism.&nbsp;The binding in the element's child injector takes&nbsp;precedence over the platform binding for all code within that element.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":1} -->
<h1 class="wp-block-heading" id="email-provider-setup">Email Provider Setup</h1>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>This shows how to configure the <code>EmailService</code> SMTP settings for the most common transactional email providers. All providers use the same set of <a href="#platform-configuration">platform configuration keys</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="sendgrid">SendGrid</h2>
<!-- /wp:heading -->

<!-- wp:list {"ordered":true} -->
<ol class="wp-block-list"><!-- wp:list-item -->
<li>Sign in to <a href="https://sendgrid.com/">sendgrid.com</a> and go to <strong>Settings -> API Keys</strong>.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Create an API key with at least the <strong>Mail Send</strong> permission.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Verify your sender domain or a single sender address under <strong>Sender Authentication</strong>.</li>
<!-- /wp:list-item --></ol>
<!-- /wp:list -->

<!-- wp:code -->
<pre class="wp-block-code"><code><code>dev.getelements.elements.email.smtp.host=smtp.sendgrid.net
dev.getelements.elements.email.smtp.port=587
dev.getelements.elements.email.smtp.starttls=true
dev.getelements.elements.email.smtp.user=apikey
dev.getelements.elements.email.smtp.password=SG.&lt;your-api-key>
dev.getelements.elements.email.default.from=noreply@yourdomain.com</code></code></pre>
<!-- /wp:code -->

<!-- wp:quote -->
<blockquote class="wp-block-quote"><!-- wp:paragraph -->
<p>The SMTP username is always the literal string&nbsp;<code>apikey</code>;&nbsp;the API key itself goes in the&nbsp;password field.</p>
<!-- /wp:paragraph --></blockquote>
<!-- /wp:quote -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="mailgun">Mailgun</h2>
<!-- /wp:heading -->

<!-- wp:list {"ordered":true} -->
<ol class="wp-block-list"><!-- wp:list-item -->
<li>Sign in to <a href="https://www.mailgun.com/">mailgun.com</a> and select or create a <strong>Domain</strong>.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Under the domain's <strong>SMTP credentials</strong>, note the hostname and create or copy an SMTP password for the default <code>postmaster@&lt;domain></code> user (or create a new SMTP user).</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>If your domain is in the Mailgun sandbox, add your recipient addresses to the <strong>Authorized Recipients</strong> list before testing.</li>
<!-- /wp:list-item --></ol>
<!-- /wp:list -->

<!-- wp:code -->
<pre class="wp-block-code"><code><code>dev.getelements.elements.email.smtp.host=smtp.mailgun.org
dev.getelements.elements.email.smtp.port=587
dev.getelements.elements.email.smtp.starttls=true
dev.getelements.elements.email.smtp.user=postmaster@mg.yourdomain.com
dev.getelements.elements.email.smtp.password=&lt;mailgun-smtp-password>
dev.getelements.elements.email.default.from=noreply@yourdomain.com<span style="background-color: initial; font-family: inherit; font-size: inherit; text-align: initial; color: initial;"></span></code></code></pre>
<!-- /wp:code -->

<!-- wp:quote -->
<blockquote class="wp-block-quote"><!-- wp:paragraph -->
<p>EU-region accounts should use&nbsp;<code>smtp.eu.mailgun.org</code>&nbsp;as the host.</p>
<!-- /wp:paragraph --></blockquote>
<!-- /wp:quote -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="amazon-ses-smtp-interface">Amazon SES (SMTP interface)</h2>
<!-- /wp:heading -->

<!-- wp:list {"ordered":true} -->
<ol class="wp-block-list"><!-- wp:list-item -->
<li>In the <a href="https://console.aws.amazon.com/ses">AWS console</a>, verify your sending domain under <strong>Configuration -> Verified identities</strong>.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>If your account is in the <strong>SES sandbox</strong>, also verify each recipient address, or submit a production access request to lift the restriction.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Under <strong>Account dashboard -> SMTP settings</strong>, note the regional SMTP endpoint.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Go to <strong>SMTP settings -> Create SMTP credentials</strong> to generate an IAM user and download the SMTP username and password (these are not your AWS access keys).</li>
<!-- /wp:list-item --></ol>
<!-- /wp:list -->

<!-- wp:code -->
<pre class="wp-block-code"><code><code>dev.getelements.elements.email.smtp.host=email-smtp.&lt;region>.amazonaws.com
dev.getelements.elements.email.smtp.port=587
dev.getelements.elements.email.smtp.starttls=true
dev.getelements.elements.email.smtp.user=&lt;ses-smtp-username>
dev.getelements.elements.email.smtp.password=&lt;ses-smtp-password>
dev.getelements.elements.email.default.from=noreply@yourdomain.com</code></code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>Replace&nbsp;<code>&lt;region&gt;</code>&nbsp;with your AWS region,&nbsp;e.g.&nbsp;<code>us-east-1</code>.</p>
<!-- /wp:paragraph -->

<!-- wp:quote -->
<blockquote class="wp-block-quote"><!-- wp:paragraph -->
<p>SES also supports port 465&nbsp;(implicit TLS)&nbsp;and port 2587.&nbsp;Port 587 with STARTTLS is the&nbsp;most broadly compatible choice.</p>
<!-- /wp:paragraph --></blockquote>
<!-- /wp:quote -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="postmark">Postmark</h2>
<!-- /wp:heading -->

<!-- wp:list {"ordered":true} -->
<ol class="wp-block-list"><!-- wp:list-item -->
<li>Sign in to <a href="https://postmarkapp.com/">postmarkapp.com</a> and create or open a <strong>Server</strong>.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Under <strong>API Tokens</strong>, copy the <strong>Server API token</strong> for SMTP auth.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Add and verify your sending domain under <strong>Sender Signatures</strong> or <strong>Domains</strong>.</li>
<!-- /wp:list-item --></ol>
<!-- /wp:list -->

<!-- wp:code -->
<pre class="wp-block-code"><code><code>dev.getelements.elements.email.smtp.host=smtp.postmarkapp.com
dev.getelements.elements.email.smtp.port=587
dev.getelements.elements.email.smtp.starttls=true
dev.getelements.elements.email.smtp.user=&lt;server-api-token>
dev.getelements.elements.email.smtp.password=&lt;server-api-token>
dev.getelements.elements.email.default.from=noreply@yourdomain.com</code></code></pre>
<!-- /wp:code -->

<!-- wp:quote -->
<blockquote class="wp-block-quote"><!-- wp:paragraph -->
<p>For Postmark,&nbsp;the SMTP username and password are both set to the same server API token.</p>
<!-- /wp:paragraph --></blockquote>
<!-- /wp:quote -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="generic-smtp-server">Generic SMTP server</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Any SMTP relay that supports STARTTLS on port 587&nbsp;(or port 465 for implicit TLS)&nbsp;works&nbsp;without additional changes.&nbsp;Common examples include self-hosted Postfix/Exim relays and&nbsp;corporate mail gateways.</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code><code>dev.getelements.elements.email.smtp.host=mail.yourdomain.com
dev.getelements.elements.email.smtp.port=587
dev.getelements.elements.email.smtp.starttls=true
dev.getelements.elements.email.smtp.user=user@yourdomain.com
dev.getelements.elements.email.smtp.password=&lt;password>
dev.getelements.elements.email.default.from=noreply@yourdomain.com</code></code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>For port 465 (implicit TLS) set <code>starttls=false</code> - the underlying Jakarta Mail session will use <code>smtps</code> transport automatically.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="passing-settings-at-startup">Passing settings at startup</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Settings can be provided as JVM system properties or as environment variables.&nbsp;For&nbsp;containers the environment variable form is usually more convenient:</p>
<!-- /wp:paragraph -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th>JVM property</th><th>Environment variable</th></tr></thead><tbody><tr><td><code>dev.getelements.elements.email.smtp.host</code></td><td><code>DEV_GETELEMENTS_ELEMENTS_EMAIL_SMTP_HOST</code></td></tr><tr><td><code>dev.getelements.elements.email.smtp.port</code></td><td><code>DEV_GETELEMENTS_ELEMENTS_EMAIL_SMTP_PORT</code></td></tr><tr><td><code>dev.getelements.elements.email.smtp.starttls</code></td><td><code>DEV_GETELEMENTS_ELEMENTS_EMAIL_SMTP_STARTTLS</code></td></tr><tr><td><code>dev.getelements.elements.email.smtp.user</code></td><td><code>DEV_GETELEMENTS_ELEMENTS_EMAIL_SMTP_USER</code></td></tr><tr><td><code>dev.getelements.elements.email.smtp.password</code></td><td><code>DEV_GETELEMENTS_ELEMENTS_EMAIL_SMTP_PASSWORD</code></td></tr><tr><td><code>dev.getelements.elements.email.default.from</code></td><td><code>DEV_GETELEMENTS_ELEMENTS_EMAIL_DEFAULT_FROM</code></td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->
