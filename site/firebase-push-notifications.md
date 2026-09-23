<h1>Firebase Push Notifications</h1>

<!-- wp:paragraph -->
<p>Namazu Elements includes a full server-side push notification system built on the Firebase Admin SDK. It has three parts: a per-Application Firebase configuration, a device registration token stored per Profile, and a general-purpose notification-sending API that your own server-side code calls to actually deliver a push. There is no REST endpoint for triggering a send; sending is meant to be driven by your own business logic, typically from a Custom Element. See <a href="custom-elements">Custom Elements</a> if you haven't built one yet.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"anchor":"h-configuring-firebase-for-an-application"} -->
<h2 id="h-configuring-firebase-for-an-application" class="wp-block-heading">Configuring Firebase for an Application</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Firebase credentials are configured per <a href="applications">Application</a>, as one of that Application's Application Configurations, alongside things like IAP platform and matchmaking configuration. There is no server-wide Firebase configuration; each Application that wants to send push notifications needs its own.</p>
<!-- /wp:paragraph -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th>Field</th><th>Description</th></tr></thead><tbody><tr><td><code>projectId</code></td><td>The Firebase project ID.</td></tr><tr><td><code>serviceAccountCredentials</code></td><td>The full JSON contents of a Firebase service account key file (not a file path). This is the same file you'd generate from Firebase Console under Project Settings &gt; Service Accounts &gt; Generate New Private Key.</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th>Method</th><th>Path</th><th>Description</th></tr></thead><tbody><tr><td><code>POST</code></td><td><code>/application/{applicationNameOrId}/configuration/firebase</code></td><td>Creates the Application's Firebase configuration.</td></tr><tr><td><code>GET</code></td><td><code>/application/{applicationNameOrId}/configuration/firebase/{applicationConfigurationNameOrId}</code></td><td>Gets a Firebase configuration.</td></tr><tr><td><code>PUT</code></td><td><code>/application/{applicationNameOrId}/configuration/firebase/{applicationConfigurationNameOrId}</code></td><td>Updates a Firebase configuration.</td></tr><tr><td><code>DELETE</code></td><td><code>/application/{applicationNameOrId}/configuration/firebase/{applicationConfigurationNameOrId}</code></td><td>Deletes a Firebase configuration.</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:paragraph -->
<p>Managing an Application's Firebase configuration is Superuser-only; there is no User or Anonymous access to this endpoint. If an Application has no Firebase configuration attached, attempting to send a notification through it fails with a configuration error rather than silently doing nothing.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"anchor":"h-registering-a-device-token"} -->
<h2 id="h-registering-a-device-token" class="wp-block-heading">Registering a Device Token</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Once a client obtains a Firebase registration token (via the Firebase client SDK on the device), it registers that token with Elements against the current Profile:</p>
<!-- /wp:paragraph -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th>Method</th><th>Path</th><th>Description</th></tr></thead><tbody><tr><td><code>POST</code></td><td><code>/notification/fcm</code></td><td>Registers a Firebase token for a Profile.</td></tr><tr><td><code>PUT</code></td><td><code>/notification/fcm/{fcmRegistrationId}</code></td><td>Updates an existing registration (typically to refresh a rotated token).</td></tr><tr><td><code>DELETE</code></td><td><code>/notification/fcm/{fcmRegistrationId}</code></td><td>Removes a registration (e.g. on sign-out or uninstall).</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:paragraph -->
<p>A registration is just an id, a <code>registrationToken</code>, and the owning Profile. Each Profile can hold only one stored registration at a time; registering a new token for a Profile that already has one replaces it rather than adding a second device.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>As a normal User, you can only manage the registration for your own current Profile: if you omit <code>profile</code> in the request body it defaults to your current Profile, and if you supply one that isn't yours, the request is rejected. A Superuser can create, update, or delete a registration for any Profile. Anonymous callers cannot use this endpoint at all.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"anchor":"h-sending-a-notification"} -->
<h2 id="h-sending-a-notification" class="wp-block-heading">Sending a Notification</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>There is no REST endpoint to send a push notification. Sending is done programmatically, by injecting <code>NotificationService</code> into your own Custom Element and building a notification with its fluent builder:</p>
<!-- /wp:paragraph -->

<!-- wp:betterdocs/code-snippet {"blockId":"betterdocs-code-snippet-d7e4094b","blockMeta":{"desktop":" .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-d7e4094b { border-width: 0px !important; border-radius: 0px !important; } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-d7e4094b .betterdocs-code-snippet-header.betterdocs-file-preview-header { border-bottom-width: 1px !important; border-bottom-style: solid !important; } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-d7e4094b .betterdocs-code-snippet-header .betterdocs-file-name .file-name-text { font-size: 14px; } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-d7e4094b .betterdocs-code-snippet-header .betterdocs-code-snippet-copy-button { } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-d7e4094b .betterdocs-code-snippet-content .betterdocs-code-snippet-line-numbers { border-right-width: 1px !important; border-right-style: solid !important; } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-d7e4094b .betterdocs-code-snippet-content .betterdocs-code-snippet-line-numbers .line-number { } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-d7e4094b .betterdocs-code-snippet-code { } ","tab":" ","mobile":" "},"codeContent":"@Inject\nprivate NotificationService notificationService;\n\npublic void notifyPlayer(Profile recipient) {\n    notificationService\n        .getBuilder()\n        .recipient(recipient)\n        .title(\u0022Your turn!\u0022)\n        .message(\u0022It's your move in Match #482.\u0022)\n        .sound()\n        .add(\u0022matchId\u0022, \u0022482\u0022)\n        .build()\n        .send();\n}","language":"java","fileName":""} /-->

<!-- wp:paragraph -->
<p>The builder exposes:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><code>recipient(Profile)</code> - the Profile to notify. Elements looks up that Profile's registered token(s) to determine where to actually deliver the push.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>sender(Profile)</code> / <code>application(Application)</code> - which Application's Firebase configuration to send under. If you set a sender Profile, its owning Application is used automatically; you rarely need to call <code>application(...)</code> directly.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>title(String)</code> / <code>message(String)</code> - the notification's title and body text.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>sound()</code> / <code>sound(String)</code> - enables a notification sound, defaulting to <code>"default"</code>, or a specific named sound.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>add(key, value)</code> / <code>addAll(Map)</code> - a custom string key/value data payload delivered alongside the notification, for your client code to act on (e.g. deep-linking to a specific screen).</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p><code>build()</code> throws a configuration exception if the target Application has no Firebase configuration attached. <code>send()</code> returns the number of destinations notified, and has an overload that takes success/failure callbacks per destination:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>int notified = notificationService
    .getBuilder()
    .recipient(recipient)
    .title("Your turn!")
    .message("It's your move in Match #482.")
    .build()
    .send(
        event -&gt; logger.info("Delivered to token {}", event.getTokenId()),
        error -&gt; logger.warn("Delivery failed", error)
    );</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>Under the hood, each call sends a single, direct-to-device Firebase message per registered token for the recipient (there's no topic broadcast or multicast batching). Android and iOS-specific notification fields are populated automatically from <code>title</code>/<code>message</code>/<code>sound</code>, so you don't need to build platform-specific payloads yourself.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"anchor":"h-error-handling"} -->
<h2 id="h-error-handling" class="wp-block-heading">Error Handling</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>If Firebase reports a token as unregistered (the device uninstalled the app, or the token otherwise expired), Elements automatically deletes that Profile's stored registration so you don't keep sending to a dead token. Any other delivery failure (network error, malformed payload, and so on) is only reported to the failure callback you supply; there is no built-in retry or backoff, so retry logic is your Element's responsibility if you need it.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"anchor":"h-events"} -->
<h2 id="h-events" class="wp-block-heading">Events</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Registering, updating, and deleting an FCM registration each publish an event your Element can subscribe to (registration created, updated, and deleted), the same way most other data types in Elements do. See <a href="events">Events</a>. Sending a notification itself does not publish an event; use the success/failure callbacks on <code>send(...)</code> if you need to react to delivery outcomes.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->
