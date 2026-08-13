<h1>Roblox Security Best Practices</h1>

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"❗Pre-Release ❗"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="02a175"><div class="gb-notice-title" style="color:#fff"><p>❗Pre-Release ❗</p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p><strong>Note:</strong> The Namazu Elements Roblox Kit is currently in pre-release. To access the Roblox Support in Namazu Elements please reach out and request access using our <a href="https://namazustudios.com/contact-us/">Contact Us Form</a>.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:paragraph -->
<p>When using the Namazu Elements Roblox Kit, it’s crucial to follow security best practices to protect your game and players. Here are key guidelines:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>Server-Side Only:</strong> Always use <strong>server-side scripts</strong> to interact with Namazu Elements services. Never call the Roblox Kit endpoints directly from a LocalScript or client-side, as that could expose your secret or allow tampering. Keep all HTTP requests confined to Roblox’s Script/ModuleScript running on the server (e.g. in ServerScriptService).</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Protect Your Application Secret:</strong> Store your Namazu Application Secret in <strong>Roblox Secrets</strong> (the cloud key management service) and <strong>never hard-code it</strong> in your scripts or expose it to players. Roblox Secrets ensure the key is encrypted and only accessible to the server at runtime. It’s also recommended to <strong>rotate</strong> (change) your secret periodically and update it both in Namazu’s settings and Roblox Secrets storage. This minimizes the risk if a secret were ever compromised.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Safeguard Session Tokens:</strong> Treat the session tokens (<code>sessionSecret</code>) as sensitive credentials. Do <strong>not log them to the output</strong> or expose them to players or untrusted sources. These tokens grant access to Namazu services on behalf of a user, so they should be kept confidential. Use them only in authorized server calls and if you temporarily store them (e.g. in a server variable), ensure they can’t be read by any client logic. If a player leaves or a server shuts down, you can discard that token.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Use HTTPS and Roblox Security Features:</strong> All communication with Namazu should be done over HTTPS (which is enforced by using the provided <code>*.cloud.namazustudios.com</code> endpoints). Make sure <strong>HttpService</strong> is enabled in your game settings and use Roblox’s built-in security features like <strong>pcall</strong> when making HTTP requests to gracefully handle errors. Always check responses and handle failures (e.g., if authentication fails or matchmaking is unavailable) in your code – the example code in the kit demonstrates wrapping <code>HttpService:PostAsync</code> calls in <code>pcall</code> for safety. By following Roblox’s guidelines for web calls and using the Namazu Kit as intended, you maintain a secure environment for your game’s online features.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->
