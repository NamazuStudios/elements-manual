<h1>Roblox Overview</h1>

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"❗Pre-Release ❗"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="02a175"><div class="gb-notice-title" style="color:#fff"><p>❗Pre-Release ❗</p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p><strong>Note:</strong> The Namazu Elements Roblox Kit is currently in pre-release. To access the Roblox Support in Namazu Elements please reach out and request access using our <a href="https://namazustudios.com/contact-us/">Contact Us Form</a>.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:paragraph -->
<p>The <strong>Namazu Elements Roblox Kit</strong> is an extension of the Namazu Elements backend platform, tailored for Roblox game developers. It enables your Roblox <strong>server-side</strong> scripts to securely authenticate players and interact with Namazu Elements cloud services. In practice, this means you can leverage Namazu’s robust backend features – such as secure player accounts and <strong>global matchmaking</strong> – directly within your Roblox game. Namazu Elements itself is a backend server solution for online multiplayer games, and the Roblox Kit bridges that power into Roblox’s environment in a developer-friendly way. This overview will introduce the key benefits of using the Roblox Kit, including <strong>secure player registration</strong> and <strong>custom global matchmaking</strong> with Namazu’s MultiMatch system, and describe how it integrates with Roblox’s architecture (RESTful APIs, HttpService, Reserved Servers, Secrets) while adhering to security best practices.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"anchor":"h-quick-endpoint-reference"} -->
<h2 id="h-quick-endpoint-reference" class="wp-block-heading">Quick Endpoint Reference</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Below is the quick reference for the specific operations supported by the Namazu Elements Roblox Kit. Refer to specific sections of this guide for specific examples on how best to use the Namazu Elemetns Roblox integration.</p>
<!-- /wp:paragraph -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th><strong>Endpoint</strong></th><th><strong>Method</strong></th><th><strong>Description</strong></th></tr></thead><tbody><tr><td><code>/app/rest/robloxkit/auth</code></td><td>POST</td><td>Authenticates a Roblox player and returns a session token. Also creates a Namazu User/Profile for the player if none exists (using Roblox ID and API info).</td></tr><tr><td><code>/app/rest/robloxkit/match</code></td><td>POST</td><td>Creates or finds a match for the player using a specified matchmaking configuration. Returns match details (match ID, etc.). Subsequent polling is used to track match status.</td></tr><tr><td><code>/app/rest/robloxkit/match/{matchId}</code></td><td>PUT</td><td>Updates an existing match’s details (e.g. assign a reserved server or update metadata). Only the host player can update a match.</td></tr><tr><td><code>/app/rest/robloxkit/match/{matchId}</code></td><td>GET</td><td>Retrieves the current status/details of a match (players, state, metadata) – used for polling match progress.</td></tr><tr><td><code>/app/rest/robloxkit/match/{matchId}</code></td><td>DELETE</td><td>Deletes an existing match from the service (ends the match). Only the host player can delete the match.</td></tr><tr><td><code>/app/rest/robloxkit/match/{matchId}/{profileId}</code></td><td>DELETE</td><td>Removes a specific player (by profile ID) from a match (player leaves the match). The host cannot use this to leave – the host must delete the match instead.</td></tr></tbody></table></figure>
<!-- /wp:table -->
