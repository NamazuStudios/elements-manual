<h1>Secure Player Authentication &amp; Registration</h1>

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"❗Pre-Release ❗"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="02a175"><div class="gb-notice-title" style="color:#fff"><p>❗Pre-Release ❗</p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p><strong>Note:</strong> The Namazu Elements Roblox Kit is currently in pre-release. To access the Roblox Support in Namazu Elements please reach out and request access using our <a href="https://namazustudios.com/contact-us/">Contact Us Form</a>.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:paragraph -->
<p>One of the core features of the Namazu Elements Roblox Kit is <strong>secure player authentication</strong>. When a player first joins your game, a server script can use the kit to <strong>authenticate the player with Namazu Elements</strong>. This is done via a single HTTP request from a server-side script (using Roblox’s <code>HttpService</code>) to the kit’s authentication endpoint, providing your Namazu Application ID and the player’s Roblox UserId. The result is a <strong>session token</strong> (called a <code>sessionSecret</code>) that uniquely identifies the player’s session with Namazu Elements. All subsequent requests to Namazu services in this session must include this token for authorization.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>This authentication flow doubles as a <strong>player registration</strong> step – if the player doesn’t already have a Namazu account, the kit will automatically create a new Namazu <em>User</em> and <em>Profile</em> linked to their Roblox user ID. The kit even uses Roblox’s API to fetch basic profile information (like the player’s username or avatar) and populate the Namazu profile on first signup. This means each Roblox player is seamlessly onboarded into Namazu Elements <strong>without any additional input</strong> or manual account creation. The entire process is designed with security in mind: it <strong>runs only on server-side scripts</strong> using your Namazu Application Secret (never on the client), so that players cannot tamper with it. Your Application Secret is used to verify the request and should be stored safely in Roblox’s secure Secrets storage, not in any client-facing code. Once authenticated, the player’s session token can be stored server-side (for example, in a variable or server memory) for as long as the player is in game, and used to call other Namazu Element services on behalf of that player. In short, the Roblox Kit provides a <strong>secure, Roblox-trusted login mechanism</strong> into Namazu’s system: leveraging the player’s Roblox identity and Roblox’s secure server scripts to establish a Namazu session.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"anchor":"h-step-by-step-guide"} -->
<h2 id="h-step-by-step-guide" class="wp-block-heading">Step-by-Step Guide</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The following steps are required to register a Roblox user in Namazu Elements. Once you register a user and obtain a session, you can make API calls against the Namazu Elements API. Creating sessions ensures that users are automatically kept in-sync between Roblox and Namazu Elements.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3,"anchor":"h-1-login-to-the-management-portal"} -->
<h3 id="h-1-login-to-the-management-portal" class="wp-block-heading">1. Login to the Management Portal</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Visit your instance of Namazu Elements by pointing our browser to https://your-instance.example.com/admin/</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":22293,"sizeSlug":"full","linkDestination":"none"} -->
<figure class="wp-block-image size-full"><img src="https://namazustudios.com/wp-content/uploads/2026/01/image-1.png" alt="" class="wp-image-22293"/></figure>
<!-- /wp:image -->

<!-- wp:heading {"level":3,"anchor":"h-2-create-an-application"} -->
<h3 id="h-2-create-an-application" class="wp-block-heading">2. Create an Application</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Navigate to Applications → Create Application</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":22294,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://namazustudios.com/wp-content/uploads/2026/01/image-2-1024x514.png" alt="" class="wp-image-22294"/></figure>
<!-- /wp:image -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"📝Architecture Notes"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="0164b3"><div class="gb-notice-title" style="color:#fff"><p>📝Architecture Notes</p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p>We recommend making a separate Namazu Elements application per Roblox Experience. This ensures that players have unique and independent Profiles per published Experience. See more on <a href="https://namazustudios.com/docs/namazu-elements-core/features/users-and-profiles/">Users and Profiles</a>.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:heading {"level":3,"anchor":"h-3-deploy-the-roblox-kit-to-the-instance"} -->
<h3 id="h-3-deploy-the-roblox-kit-to-the-instance" class="wp-block-heading">3. Deploy the Roblox Kit to the Instance</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>At the time of this writing, Namazu Roblox Kit is distributed as a separate Element. Refer to the <a href="https://namazustudios.com/docs/custom-code/element-structure/">Custom Code</a> section on how to install the Element to your instance. In addition to the standard deployment process, it is necessary to configure the RobloxKit Secret to ensure secure communication between Roblox's server and Namazu Elements. To set this, deploy the element with the following configuration set. (Changed <code>&lt;redacted&gt;</code> to a secret of your choice).</p>
<!-- /wp:paragraph -->

<!-- wp:betterdocs/code-snippet {"blockId":"betterdocs-code-snippet-89c5f28a","blockMeta":{"desktop":" .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-89c5f28a { border-width: 0px !important; border-radius: 0px !important; } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-89c5f28a .betterdocs-code-snippet-header.betterdocs-file-preview-header { border-bottom-width: 1px !important; border-bottom-style: solid !important; } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-89c5f28a .betterdocs-code-snippet-header .betterdocs-file-name .file-name-text { font-size: 14px; } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-89c5f28a .betterdocs-code-snippet-header .betterdocs-code-snippet-copy-button { } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-89c5f28a .betterdocs-code-snippet-content .betterdocs-code-snippet-line-numbers { border-right-width: 1px !important; border-right-style: solid !important; } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-89c5f28a .betterdocs-code-snippet-content .betterdocs-code-snippet-line-numbers .line-number { } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-89c5f28a .betterdocs-code-snippet-code { } ","tab":" ","mobile":" "},"codeContent":"dev.getelements.robloxkit.secret=\u003credacted\u003e","fileName":"dev.getelements.element.attributes.properties"} /-->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"⚠️ Guard this Secret ⚠️","noticeBackgroundColor":"#ff3860"} -->
<div style="color:#32373c;background-color:#ff3860" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="615377"><div class="gb-notice-title" style="color:#fff"><p>⚠️ Guard this Secret ⚠️</p></div><div class="gb-notice-text" style="border-color:#ff3860"><!-- wp:paragraph -->
<p>This secret provides access to your Namazu Elements instance. If it is leaked, change it immediately and update all code. If a malicious third party finds this secret, they can essentially masquerade as any valid Roblox player.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:heading {"level":4,"anchor":"h-4-verify-roblox-secret-was-set-correctly"} -->
<h4 id="h-4-verify-roblox-secret-was-set-correctly" class="wp-block-heading">4. Verify Roblox Secret was Set Correctly</h4>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Once deployed, check that the Roblox Kit deployed successfully. Verify:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>A "Green Light" appears next to the application name in the left bar, indicating successful deployment.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>There are no errors in the application logs</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>That the configured secret appears in the Management Portal</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:image {"id":22296,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://namazustudios.com/wp-content/uploads/2026/01/image-4-1024x801.png" alt="" class="wp-image-22296"/></figure>
<!-- /wp:image -->

<!-- wp:heading {"level":3,"anchor":"h-5-verify-registration-works-correctly"} -->
<h3 id="h-5-verify-registration-works-correctly" class="wp-block-heading">5. Verify Registration Works Correctly</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Using your preferred RESTful client, make an API call to the instance authorizing a user.</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>HTTP Method:</strong> <code>POST</code></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>URI: </strong><code>/app/rest/roblox/auth</code></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Header: <code>RobloxKit-Secrret: &lt;recacted&gt;</code></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Body <code>{"robloxUserId":xxxxxxxx, "application":"my_application"}</code></li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>For quick reference, the following <a href="https://curl.se/">Curl</a> command will test your instance:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>curl -d '{"application":"my_app", "robloxUserId":"XXXXXXXXXXXX"}' \
     -H "Content-Type: application/json" \
     -H "RobloxKit-Secret: &lt;redacted&gt;" \
     https:&#47;&#47;my-instance.example.com/app/rest/roblox/auth</code></pre>
<!-- /wp:code -->

<!-- wp:heading {"level":3,"anchor":"h-6-check-that-the-profile-matching-the-roblox-player-exists-in-namazu-elements"} -->
<h3 id="h-6-check-that-the-profile-matching-the-roblox-player-exists-in-namazu-elements" class="wp-block-heading">6. Check that the Profile Matching the Roblox Player exists in Namazu Elements</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Navigate to Profiles on the left hand navigation menu and find the profile matching the Roblox user you tested against.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":22297,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://namazustudios.com/wp-content/uploads/2026/01/image-5-1024x801.png" alt="" class="wp-image-22297"/></figure>
<!-- /wp:image -->
