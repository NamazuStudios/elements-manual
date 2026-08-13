<h1>Matchmaking - Comprehensive Guide</h1>

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"level":6} -->
<h6 class="wp-block-heading" id="h-namazu-elements-supports-matchmaking-for-online-gaming-including-one-vs-one-or-group-matches">Namazu Elements supports matchmaking for online gaming, including one vs one or group matches.</h6>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p><strong>Matchmaking Application Configurations</strong> are per‑application settings that determine how players are grouped together into matches. Starting with <strong>Namazu Elements&nbsp;3.4</strong>, all matchmaking functions are handled through the <strong>MultiMatch API</strong>, which supports arbitrarily large player counts and custom matchmaking logic. Each configuration defines its own matchmaking queue, allowing multiple modes (for example, <strong>ranked</strong> and <strong>unranked</strong>) within the same application. Configuration and management occur through the <strong>Namazu Elements CMS</strong>.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>This guide combines official documentation with hands‑on instructions to explain all key operations:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>Understanding what a configuration does.</strong></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Creating a new matchmaking configuration.</strong></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Updating an existing configuration.</strong></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Deleting a configuration.</strong></li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-key-matchmaking-features">Key Matchmaking Features</h2>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>Scalable player counts</strong> – Supports matches with arbitrarily large numbers of players, exceeding practical limits for most modern games.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Custom matchmaking logic</strong> – Allows developers to define bespoke matchmaking schemes such as <strong>Skill-Based</strong>, <strong>Friend Code</strong>, or other custom lobby systems.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Matchmaking Application Configurations</strong> – Centralized definitions for p<img src="https://chatgpt.com/backend-api/estuary/content?id=file_00000000335061f78669fda421311469&amp;ts=489386&amp;p=fs&amp;cid=1&amp;sig=b9e67e08d6bebd574d82cb8fb8dae73093f3b0c325d6d057c201120bac212187&amp;v=0" alt="">layer count, team structure, and matchmaking behavior.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>CMS integration</strong> – Full setup and configuration can be managed through the <strong>Namazu Elements CMS</strong> interface.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Use&nbsp;Default&nbsp;Matchmaker</strong> – When checked, uses the built‑in FIFO (first‑in‑first‑out) matchmaker to pair players<a href="https://namazustudios.com/docs/namazu-elements-core/features/configuration/matchmaking/#:~:text=,in%20the%20order%20they%20arrive" target="_blank" rel="noreferrer noopener">namazustudios</a></li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>This section describes how to configure and deploy matchmaking applications using the MultiMatch API.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":1} -->
<h1 class="wp-block-heading" id="h-what-a-matchmaking-configuration-does">What a Matchmaking Configuration Does</h1>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p><strong>Matchmaking Application Configurations</strong> are per-application settings that define a set of <strong>matchmaking rules</strong>. They combine key parameters such as player count, timeout settings, and the matchmaking algorithm to determine how players are grouped into matches.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>The algorithm itself is implemented as a <strong>plug-in module</strong>, which reads these values when executing matchmaking.</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>The <strong>success callback</strong> field is <strong>deprecated</strong> and will be removed in a future release. The related fields remain as legacy placeholders from earlier versions.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>This configuration allows you to define a <strong>custom system for matching players</strong> based on your game’s or application’s rules.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Namazu Elements supports virtually any matchmaking system with only a few lines of backend code.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>A <strong>MultiMatch</strong> created from this configuration pairs players within the same application.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Each matchmaking configuration represents a <strong>separate matchmaking queue</strong>, enabling multiple concurrent matching modes (e.g., <em>ranked</em> or <em>unranked</em>).</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-matchmaking-configuration-fields">Matchmaking Configuration Fields</h2>
<!-- /wp:heading -->

<!-- wp:heading {"level":1} -->
<h1 class="wp-block-heading">Prerequisites</h1>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><a href="https://namazustudios.com/docs/getting-started/general-concepts/">id</a> – The unique database-id of the Application Configuration itself</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong><a href="https://namazustudios.com/docs/getting-started/general-concepts/">Name</a></strong> – A unique identifier for the configuration. Names must start with a letter or digit and may include underscores. Each configuration’s name is used in API calls and in the CMS.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Description</strong> – A human‑readable description of the configuration (e.g., “Unranked matchmaking configuration for casual games”).</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Max&nbsp;Profiles</strong> – The maximum number of player profiles (players) allowed per match. The minimum value is&nbsp;2</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Linger&nbsp;Seconds</strong> – How long a match remains in the database after it ends (default 300&nbsp;seconds). This is useful for post‑match experiences such as victory screens</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Timeout&nbsp;Seconds</strong> – The maximum lifetime of a match before it is automatically deleted (default 86 400&nbsp;seconds, or 24&nbsp;hours)<a href="https://namazustudios.com/docs/namazu-elements-core/features/configuration/matchmaking/#:~:text=,extend%20a%20match%E2%80%99s%20duration%20dynamically" target="_blank" rel="noreferrer noopener"></a></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Use&nbsp;Default&nbsp;Matchmaker</strong> – When checked, uses the built‑in FIFO (first‑in‑first‑out) matchmaker to pair players<a href="https://namazustudios.com/docs/namazu-elements-core/features/configuration/matchmaking/#:~:text=,in%20the%20order%20they%20arrive" target="_blank" rel="noreferrer noopener"></a>. You can uncheck this and specify a custom <strong>Element&nbsp;Name</strong>, <strong>Service&nbsp;Type</strong> and <strong>Service&nbsp;Name</strong> to implement bespoke matchmaking logic.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>This document provides a step‑by‑step guide on how to update and remove a matchmaking configuration from an application in <strong>Namazu Elements</strong>. Changing its name or adjusting the maximum profiles (player count) affects how matches are formed. Removing a configuration will completely delete it from the application, so be careful when performing deletion.</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>A Namazu Elements administrator account.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>URL to the admin portal: <code>https://&lt;your instance&gt;/admin/</code>.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Credentials (username and password).</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>An existing application with a matchmaking configuration (in this example, <strong>Demo Application</strong> with an initial configuration named <strong>unranked</strong>).</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:quote -->
<blockquote class="wp-block-quote"><!-- wp:paragraph -->
<p><strong>Note:</strong> In this guide, all actions are performed while logged into the admin portal and assume you have sufficient permissions. The steps follow a typical workflow of editing an application configuration and then deleting it.</p>
<!-- /wp:paragraph --></blockquote>
<!-- /wp:quote -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"level":1} -->
<h1 class="wp-block-heading" id="h-adding-a-matchmaking-configuration">Adding a Matchmaking Configuration</h1>
<!-- /wp:heading -->

<!-- wp:heading -->
<h2 class="wp-block-heading">1. Open the Application for Editing</h2>
<!-- /wp:heading -->

<!-- wp:list {"ordered":true} -->
<ol class="wp-block-list"><!-- wp:list-item -->
<li>From the Namazu Elements dashboard, locate the application you want to modify (for example, <em>demo</em>).</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>In the <strong>Actions</strong> column, click the <strong>edit (pencil)</strong> icon to open the <strong>Edit Application</strong> dialog.</li>
<!-- /wp:list-item --></ol>
<!-- /wp:list -->

<!-- wp:image {"id":22211,"sizeSlug":"full","linkDestination":"none"} -->
<figure class="wp-block-image size-full"><img src="https://namazustudios.com/wp-content/uploads/2025/08/image-9.png" alt="" class="wp-image-22211"/><figcaption class="wp-element-caption">Find the Application you Wish to Edit</figcaption></figure>
<!-- /wp:image -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading">2. Add a New Configuration</h2>
<!-- /wp:heading -->

<!-- wp:list {"ordered":true} -->
<ol class="wp-block-list"><!-- wp:list-item -->
<li>In the <strong>Edit Application</strong> dialog, scroll to the <strong>Application Configurations</strong> section.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Click <strong>+ Add Configuration</strong> to open the <strong>Application Configuration</strong> form.</li>
<!-- /wp:list-item --></ol>
<!-- /wp:list -->

<!-- wp:image {"id":22217,"sizeSlug":"full","linkDestination":"none"} -->
<figure class="wp-block-image size-full"><img src="https://namazustudios.com/wp-content/uploads/2025/08/image-11.png" alt="" class="wp-image-22217"/><figcaption class="wp-element-caption">An Empty Application with No Configurations</figcaption></figure>
<!-- /wp:image -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading">3. Create the Matchmaking Configuration</h2>
<!-- /wp:heading -->

<!-- wp:list {"ordered":true} -->
<ol class="wp-block-list"><!-- wp:list-item -->
<li>In the <strong>Application Configuration</strong> form, ensure <strong>Configuration Type</strong> is set to <strong>Matchmaking</strong>.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Enter the following values:<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>Name:</strong> <code>unranked</code><br><em>(Names must start with a letter or digit and may include underscores.)</em></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Description:</strong> Provide a short description, e.g., <em>“Unranked matchmaking configuration for casual games.”</em></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Max Profiles:</strong> <code>2</code><br>(The maximum number of players per match. The minimum value is 2.)</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list --></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Review and confirm the following additional fields:<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>Linger Seconds:</strong> <code>300</code> (default)<br>Defines how long a match remains in the database <strong>after it has ended</strong> before it is permanently deleted.<br>This can be used to create <strong>post-match experiences</strong> like victory or celebration screens (e.g., <em>PUBG</em> end-of-match sequences).</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Timeout Seconds:</strong> <code>86400</code> (default)<br>Defines the <strong>maximum lifetime</strong> of a match before it is automatically deleted.<br>Custom Element code can update this value downstream to extend a match’s duration dynamically.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Use Default Matchmaker:</strong> Keep this checked (unless you are adding a custom matchmaker).<br>The <strong>default matchmaker</strong> uses the <strong>built-in FIFO (First-In-First-Out)</strong> algorithm to pair players in the order they arrive.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Success Callback:</strong> Leave blank.<br>This field is <strong>deprecated</strong> and will be removed in a future release.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list --></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>(Optional) You can customize the matchmaking logic by specifying:<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong style="color: initial; font-family: -apple-system, BlinkMacSystemFont, &quot;Segoe UI&quot;, Roboto, Oxygen-Sans, Ubuntu, Cantarell, &quot;Helvetica Neue&quot;, sans-serif;">Element Name</strong><span style="color: initial; font-family: -apple-system, BlinkMacSystemFont, &quot;Segoe UI&quot;, Roboto, Oxygen-Sans, Ubuntu, Cantarell, &quot;Helvetica Neue&quot;, sans-serif;"> – the Element responsible for handling matchmaking</span></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong style="color: initial; font-family: -apple-system, BlinkMacSystemFont, &quot;Segoe UI&quot;, Roboto, Oxygen-Sans, Ubuntu, Cantarell, &quot;Helvetica Neue&quot;, sans-serif;">Service Type</strong><span style="color: initial; font-family: -apple-system, BlinkMacSystemFont, &quot;Segoe UI&quot;, Roboto, Oxygen-Sans, Ubuntu, Cantarell, &quot;Helvetica Neue&quot;, sans-serif;"> – the backend category managing the logic</span></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong style="color: initial; font-family: -apple-system, BlinkMacSystemFont, &quot;Segoe UI&quot;, Roboto, Oxygen-Sans, Ubuntu, Cantarell, &quot;Helvetica Neue&quot;, sans-serif;">Service Name</strong><span style="color: initial; font-family: -apple-system, BlinkMacSystemFont, &quot;Segoe UI&quot;, Roboto, Oxygen-Sans, Ubuntu, Cantarell, &quot;Helvetica Neue&quot;, sans-serif;"> – the specific function or endpoint performing matchmaking</span></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><span style="color: initial; font-family: -apple-system, BlinkMacSystemFont, &quot;Segoe UI&quot;, Roboto, Oxygen-Sans, Ubuntu, Cantarell, &quot;Helvetica Neue&quot;, sans-serif;">These values allow you to implement a </span><strong style="color: initial; font-family: -apple-system, BlinkMacSystemFont, &quot;Segoe UI&quot;, Roboto, Oxygen-Sans, Ubuntu, Cantarell, &quot;Helvetica Neue&quot;, sans-serif;">custom server-side <img src="https://chatgpt.com/backend-api/estuary/content?id=file_00000000335061f78669fda421311469&amp;ts=489386&amp;p=fs&amp;cid=1&amp;sig=b9e67e08d6bebd574d82cb8fb8dae73093f3b0c325d6d057c201120bac212187&amp;v=0" alt="">matchmaking system</strong><span style="color: initial; font-family: -apple-system, BlinkMacSystemFont, &quot;Segoe UI&quot;, Roboto, Oxygen-Sans, Ubuntu, Cantarell, &quot;Helvetica Neue&quot;, sans-serif;"> within Namazu Elements.</span></li>
<!-- /wp:list-item --></ul>
<!-- /wp:list --></li>
<!-- /wp:list-item --></ol>
<!-- /wp:list -->

<!-- wp:image {"id":22207,"width":"818px","height":"auto","sizeSlug":"full","linkDestination":"none"} -->
<figure class="wp-block-image size-full is-resized"><img src="https://namazustudios.com/wp-content/uploads/2025/08/image-5.png" alt="" class="wp-image-22207" style="width:818px;height:auto"/><figcaption class="wp-element-caption">Matchmaking Application Configuration Editor (Upper Half)</figcaption></figure>
<!-- /wp:image -->

<!-- wp:image {"id":22208,"width":"819px","height":"auto","sizeSlug":"full","linkDestination":"none"} -->
<figure class="wp-block-image size-full is-resized"><img src="https://namazustudios.com/wp-content/uploads/2025/08/image-6.png" alt="" class="wp-image-22208" style="width:819px;height:auto"/><figcaption class="wp-element-caption">Matchmaking Application Configuration Editor (Lower Half)</figcaption></figure>
<!-- /wp:image -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading">4. Save and Verify</h2>
<!-- /wp:heading -->

<!-- wp:list {"ordered":true} -->
<ol class="wp-block-list"><!-- wp:list-item -->
<li>Click <strong>Save Configuration</strong> at the bottom of the form.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>A success message (toast) appears confirming that the configuration was saved.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>The new configuration is now visible in the <strong>Application Configurations</strong> list within the <strong>Edit Application</strong> dialog.</li>
<!-- /wp:list-item --></ol>
<!-- /wp:list -->

<!-- wp:image {"id":22210,"sizeSlug":"full","linkDestination":"none"} -->
<figure class="wp-block-image size-full"><img src="https://namazustudios.com/wp-content/uploads/2025/08/image-8.png" alt="" class="wp-image-22210"/><figcaption class="wp-element-caption">The Success Dialog appears when You Successful Update the Application</figcaption></figure>
<!-- /wp:image -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"textAlign":"left"} -->
<h2 class="wp-block-heading has-text-align-left">5. Close the Edit Dialog</h2>
<!-- /wp:heading -->

<!-- wp:list {"ordered":true} -->
<ol class="wp-block-list"><!-- wp:list-item -->
<li>Click the <strong>X</strong> in the top-right corner of the <strong>Edit Application</strong> dialog to return to the main <strong>Applications</strong> list.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Confirm that your new configuration appears under the target application.</li>
<!-- /wp:list-item --></ol>
<!-- /wp:list -->

<!-- wp:image {"id":22209,"width":"790px","height":"auto","sizeSlug":"full","linkDestination":"none"} -->
<figure class="wp-block-image size-full is-resized"><img src="https://namazustudios.com/wp-content/uploads/2025/08/image-7.png" alt="" class="wp-image-22209" style="width:790px;height:auto"/><figcaption class="wp-element-caption">The bottom of the Application Editor will Show all Application Configurations</figcaption></figure>
<!-- /wp:image -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading">Summary</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>You have created an <strong>unranked</strong> matchmaking configuration for your application using the default FIFO matchmaker.</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>Linger Seconds</strong> controls how long matches persist post-termination, useful for post-game effects.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Timeout Seconds</strong> sets the match’s maximum lifespan, adjustable via Element code.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Success Callback</strong> is deprecated and should remain unused.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Default Matchmaker</strong> leverages Namazu’s FIFO algorithm but can be replaced with a custom implementation via Element, Service Type, and Service Name.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Each configuration corresponds to its own <strong>matchmaking queue</strong>, supporting multiple modes or player categories within the same application.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":1} -->
<h1 class="wp-block-heading">Updating a Matchmaking Configuration</h1>
<!-- /wp:heading -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"\u003cmark style=\u0022background-color:rgba(0, 0, 0, 0)\u0022 class=\u0022has-inline-color has-black-color\u0022\u003eWarning\u003c/mark\u003e","noticeBackgroundColor":"#ffdd57"} -->
<div style="color:#32373c;background-color:#ffdd57" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="a23b7e"><div class="gb-notice-title" style="color:#fff"><p><mark style="background-color:rgba(0, 0, 0, 0)" class="has-inline-color has-black-color">Warning</mark></p></div><div class="gb-notice-text" style="border-color:#ffdd57"><!-- wp:paragraph -->
<p>Updating a matchmaking configuration in the UI or using the API does while a match is underway may invoke undefined behavior in the system resulting in lost games or inconsistent behavior. We recommend only modifying configurations during planned downtime or maintenance for games in production.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:heading -->
<h2 class="wp-block-heading">1. Open the Application for Editing</h2>
<!-- /wp:heading -->

<!-- wp:list {"ordered":true} -->
<ol class="wp-block-list"><!-- wp:list-item -->
<li>From the Namazu Elements dashboard, locate the application you want to modify (for example, <em>demo</em>).</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>In the <strong>Actions</strong> column, click the <strong>edit (pencil)</strong> icon to open the <strong>Edit Application</strong> dialog.</li>
<!-- /wp:list-item --></ol>
<!-- /wp:list -->

<!-- wp:image {"id":22211,"sizeSlug":"full","linkDestination":"none"} -->
<figure class="wp-block-image size-full"><img src="https://namazustudios.com/wp-content/uploads/2025/08/image-9.png" alt="" class="wp-image-22211"/><figcaption class="wp-element-caption">Find the Application you Wish to Edit</figcaption></figure>
<!-- /wp:image -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-2-edit-the-matchmaking-configuration">2. Edit the Matchmaking Configuration</h2>
<!-- /wp:heading -->

<!-- wp:list {"ordered":true} -->
<ol class="wp-block-list"><!-- wp:list-item -->
<li>In the <strong>Edit Application</strong> dialog, scroll down to the <strong>Application&nbsp;Configurations</strong> section. Each configuration shows its ID, name and type.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Locate the configuration of type <strong>Matchmaking</strong> (e.g., <strong>unranked</strong>) and click <strong>Edit</strong>. A new <strong>Application Configuration</strong> modal opens.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Modify the settings as needed:<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>Name</strong> – Provide a new unique name for the configuration (only letters, digits and underscores).</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Description</strong> – Optionally describe the configuration.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Max Profiles</strong> – Enter the maximum number of player profiles allowed in a single match. This field defines the player count (minimum value is&nbsp;2).</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Additional settings such as <strong>Linger&nbsp;Seconds</strong>, <strong>Timeout&nbsp;Seconds</strong> and metadata options can be adjusted if required.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list --></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>After making changes, click <strong>Save&nbsp;Configuration</strong>. A toast notification confirms “Configuration saved successfully,” and the modal closes.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Back in the <strong>Edit Application</strong> dialog, verify that the configuration list reflects the new name.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Click <strong>Update Application</strong> at the bottom‑right of the dialog. This commits the changes to the application. A success message appears confirming the update.</li>
<!-- /wp:list-item --></ol>
<!-- /wp:list -->

<!-- wp:heading {"level":1} -->
<h1 class="wp-block-heading" id="h-deleting-a-configuration">Deleting a Configuration</h1>
<!-- /wp:heading -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"\u003cstrong\u003e\u003cmark style=\u0022background-color:rgba(0, 0, 0, 0)\u0022 class=\u0022has-inline-color has-black-color\u0022\u003eWarning\u003c/mark\u003e\u003c/strong\u003e","noticeBackgroundColor":"#ffdd57"} -->
<div style="color:#32373c;background-color:#ffdd57" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="c6153c"><div class="gb-notice-title" style="color:#fff"><p><strong><mark style="background-color:rgba(0, 0, 0, 0)" class="has-inline-color has-black-color">Warning</mark></strong></p></div><div class="gb-notice-text" style="border-color:#ffdd57"><!-- wp:paragraph -->
<p><mark style="background-color:#ffffff" class="has-inline-color has-black-color">Deleting a matchmaking configuration in the UI or using the API does not permanently delete the configuration. However, it does clear the name making it available for re-creation. This approach prevents existing matches from breaking due to database inconsistency. However, deleting a configuration while a match is underway may invoke undefined behavior in the system resulting in lost games. We recommend only deleting configurations during planned downtime or maintenance for games in production.</mark></p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:list {"ordered":true} -->
<ol class="wp-block-list"><!-- wp:list-item -->
<li><strong>Open the Application for Editing</strong> and scroll to <strong>Application&nbsp;Configurations</strong>.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Identify the Configuration</strong> to delete. Confirm by reading its name and type.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Click&nbsp;Remove</strong><!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>In the <strong>Actions</strong> column for the configuration, click <strong>Remove</strong> (trash icon). The configuration disappears from the list, and a toast notifies that it was deleted successfully.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list --></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Persist Changes</strong><!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Click <strong>Update&nbsp;Application</strong> to save the deletion. The <strong>Edit&nbsp;Application</strong> dialog closes and a success message confirms that the application was updated.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list --></li>
<!-- /wp:list-item --></ol>
<!-- /wp:list -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-summary">Summary</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Matchmaking application configurations in Namazu Elements define how players are assembled into matches. Using the CMS, administrators can:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>Create</strong> new matchmaking configurations by setting a unique name, description, player count (Max&nbsp;Profiles), and optional timeout values.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Update</strong> existing configurations to rename them, adjust player counts, or modify other parameters. Changes must be saved via <strong>Save&nbsp;Configuration</strong> followed by <strong>Update&nbsp;Application</strong>.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Delete</strong> configurations when they are no longer needed, ensuring the application’s configuration list stays current.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>By following these steps, you can manage matchmaking behavior across different game modes within your application, leveraging Namazu’s flexible MultiMatch API for scalable and customizable player experiences.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->
