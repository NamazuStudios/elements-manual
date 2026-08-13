<h1>Global Matchmaking</h1>

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"❗Pre-Release ❗"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="02a175"><div class="gb-notice-title" style="color:#fff"><p>❗Pre-Release ❗</p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p><strong>Note:</strong> The Namazu Elements Roblox Kit is currently in pre-release. To access the Roblox Support in Namazu Elements please reach out and request access using our <a href="https://namazustudios.com/contact-us/">Contact Us Form</a>.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:paragraph -->
<p>Another major feature of the Namazu Elements Roblox Kit is <strong>custom global matchmaking</strong>, powered by Namazu’s <strong><a href="https://namazustudios.com/docs/configuration/matchmaking/">MultiMatch</a></strong> system. This allows you to match players <strong>across your entire game (all servers)</strong> using flexible rules that you define, going beyond Roblox’s built-in matchmaking capabilities. With Namazu’s MultiMatch service, you can configure matchmaking criteria in the Namazu Elements Admin Panel (for example, game mode, skill brackets, team sizes, etc.), and then use the Roblox Kit to create or join matches for players at runtime. The kit exposes a simple REST endpoint (<code>POST /app/rest/robloxkit/match</code>) that your server scripts call when a player wants to find a game. The matchmaking service will either <strong>find an appropriate existing match or create a new match</strong> according to the configuration you specified. The response includes details of the match, such as a unique match ID and metadata, and indicates whether the player is designated as the match <strong>host</strong>. (When a new match is created, one player is marked as the host who will orchestrate the match, while others are added as clients.)</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>This <strong>global matchmaking</strong> approach means players from different server instances (or different places in your experience) can be pooled together by the backend. The Namazu MultiMatch system handles the logic of grouping players, so you’re not limited by Roblox’s server-bound matchmaking. For example, you could have players in multiple lobbies all request to join a “battle” match – Namazu will group them and respond with a match assignment. Because Roblox server scripts do not support real-time WebSocket connections, the kit uses a <strong>polling</strong> mechanism for match updates. Your server can periodically call a <code>GET /app/rest/robloxkit/match/{matchId}</code> endpoint to check if the match is ready (i.e. enough players have joined or a host has signaled to start). This polling design ensures compatibility with Roblox’s architecture, where continuous connections aren’t possible.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"anchor":"h-using-the-roblox-global-matchmaker"} -->
<h2 id="h-using-the-roblox-global-matchmaker" class="wp-block-heading">Using the Roblox Global Matchmaker</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>This guide walks you through using the matchmaking system. In order to use the API, you must have worked through <a href="https://namazustudios.com/docs/add-ons/game-engines/roblox/secure-player-authentication-registration/">obtaining a session key</a> for a Roblox user in Namazu Elements. This includes configuring a Roblox secret and an <a href="https://namazustudios.com/docs/namazu-elements-core/features/applications/">Application</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3,"anchor":"h-1-ensure-you-have-a-matchmaking-application-configuration"} -->
<h3 id="h-1-ensure-you-have-a-matchmaking-application-configuration" class="wp-block-heading">1. Ensure you have a Matchmaking Application Configuration</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>In order to make use of the Roblox matchmaker, you must first have a Matchmaking Application Configuration. Refer to the existing <a href="https://namazustudios.com/docs/configuration/matchmaking/">Matchmaking Guide</a> to configure one. The simplest configuration will be the easiest to use.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3,"anchor":"h-2-obtain-a-session-secret-for-the-user"} -->
<h3 id="h-2-obtain-a-session-secret-for-the-user" class="wp-block-heading">2. Obtain a Session Secret for the User</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Obtaining a session key using the same Application you configured from authentication guide and ensure that the Matchmaking Application Configuration matches the Application used to create the session.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3,"anchor":"h-3-attempt-to-find-a-match"} -->
<h3 id="h-3-attempt-to-find-a-match" class="wp-block-heading">3. Attempt to Find a Match</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The process of finding a match involves making an initial request to the <code>POST /match</code> endpoint. If there is a match meeting the requested criteria, the player will join that match. If there are matches meeting the requested criteria, the system will create a new match and wait for other players to join.</p>
<!-- /wp:paragraph -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"⚠️ Use Only Once ⚠️","noticeBackgroundColor":"#ff3860"} -->
<div style="color:#32373c;background-color:#ff3860" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="bfeddb"><div class="gb-notice-title" style="color:#fff"><p>⚠️ Use Only Once ⚠️</p></div><div class="gb-notice-text" style="border-color:#ff3860"><!-- wp:paragraph -->
<p>Use the <code>POST /match</code> endpoint only <strong>once</strong> when the player enters the queue as it may result in many abandoned matches degrading player experience. While the database will eventually clean up orphaned matches, it may take time leaving other players waiting for a match which never will start. The Roblox Kit provides few guardrails due to the polling nature of the matchmaking system.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:heading {"level":3,"anchor":"h-4-poll-for-start-conditions"} -->
<h3 id="h-4-poll-for-start-conditions" class="wp-block-heading">4. Poll for Start Conditions</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>For each MultiMatch, Namazu Elements will assign a single player as the "host" player. The host flag designates which player is responsible for orchestrating the start of the game. Depending on the host flag, perform the following actions.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":4,"anchor":"h-4a-host-player-responsibility"} -->
<h4 id="h-4a-host-player-responsibility" class="wp-block-heading">4a. Host Player Responsibility</h4>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The host player will continue to poll the match until start conditions are met. For example, the minimum player count has been met to initiate a game. When to orchestrate the start will depend on the specific rules and design of your game.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Once start conditions are met, the host player will <a href="https://create.roblox.com/docs/reference/engine/classes/TeleportService#ReserveServerAsync">reserve a server</a> for the game play session. UPon obtaining the reserve server id, the host will update the match using <code>PUT /match/{matchid}</code> and finally teleport that user to the server they just created.</p>
<!-- /wp:paragraph -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"⚠️ Reserving a Server ⚠️"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="059823"><div class="gb-notice-title" style="color:#fff"><p>⚠️ Reserving a Server ⚠️</p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p>Once the host reserves a server, the Roblox Kit will automatically designate the match as <code>CLOSED</code>, which will disallow other players from joining even if the match is not full.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:heading {"level":4,"anchor":"h-4b-non-host-player-responsibility"} -->
<h4 id="h-4b-non-host-player-responsibility" class="wp-block-heading">4b. Non-Host Player Responsibility</h4>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The non-host or "client" players will poll <code>GET /match/{matchId}</code> waiting for the match to start. Once a response indicates that a reserved server exists, then the client players will follow the host player to that match.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":4,"anchor":"h-5-delete-and-leave-match"} -->
<h4 id="h-5-delete-and-leave-match" class="wp-block-heading">5. Delete and Leave Match</h4>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Once the game has concluded, the host must clear the match from the database by calling <code>DELETE /match/{matchId}</code>.</p>
<!-- /wp:paragraph -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"⚠️ Leaving a Match ⚠️"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="83b9cb"><div class="gb-notice-title" style="color:#fff"><p>⚠️ Leaving a Match ⚠️</p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p>In order to maintain consistency, we strongly recommending using <code>DELETE /match/{matchId}/{profileId}</code> to remove the player from an in-progress match. In the event that the host player leaves Namazu Elements will assign a new host automatically.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->
