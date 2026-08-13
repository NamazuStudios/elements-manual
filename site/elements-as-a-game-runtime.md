<h1>Elements as a Game Runtime</h1>

<!-- wp:paragraph -->
<p>Elements is not just a backend service that you call from your game - it is a server-side runtime capable of hosting your game’s authoritative logic.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>This page explains what that means, how it differs from traditional architectures, and when you might choose to run your game logic directly inside Elements.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-the-big-idea">The Big Idea</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>In most game architectures, you write game logic in one place and run it somewhere else:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>a custom backend service</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>a cloud function platform</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>a managed game server</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>With Elements, you have another option - your game’s authoritative logic can run <em>inside</em> Elements itself.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>This is done by writing Custom Elements, which are full server-side applications hosted and executed by the Elements platform.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-what-a-custom-element-is">What a Custom Element Is</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>A Custom Element is:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>a server-side Java application</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>loaded and executed by Elements</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>running in a trusted, non-player-controlled environment</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>isolated from other Elements at the classpath level</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>A Custom Element is <em>not</em>:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>client code</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>a scripting sandbox</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>a restricted rules engine</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>You write real Java, with full control over logic and data access.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-acting-as-the-game-server">Acting as the Game Server</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>A Custom Element can act as the core game server.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Depending on your game, this may include:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>handling REST requests for turn-based gameplay</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>managing real-time sessions via WebSockets or WebRTC</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>validating player actions</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>enforcing game rules</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>mutating authoritative game state</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>From the player’s perspective, the client still communicates with “the server” - the difference is that the server logic lives inside Elements.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-direct-access-to-game-state">Direct Access to Game State</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Custom Elements have unrestricted access to the Elements data model.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>This allows you to:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>read and write directly to the database</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>manage custom tables and relationships</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>integrate tightly with (and even expand upon) Elements systems, such as inventory, rewards, missions, and leaderboards</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>Because this code runs authoritatively, there is no need to proxy through an external backend just to apply state changes.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-real-time-and-turn-based-games">Real-Time and Turn-Based Games</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Elements supports both interaction models:</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-turn-based-games">Turn-Based Games</h3>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Clients make REST calls</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Custom Elements validate and apply moves</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Game state is stored and enforced by Elements</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>This model is well-suited for:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>async games</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>strategy games</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>games with simple state transitions</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-real-time-games">Real-Time Games</h3>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Clients establish persistent connections (WebSockets / WebRTC)</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Custom Elements manage sessions and events</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Elements acts as the authoritative host</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>This model is well-suited for:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>real-time multiplayer</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>synchronous co-op or PvP</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>fast-paced interactions requiring low latency</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-composability-and-multiple-elements">Composability and Multiple Elements</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Multiple Custom Elements can be loaded at the same time.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>This enables a composable architecture, where:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>one Element handles core gameplay</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>another handles purchases or receipt validation</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>another manages promotions or live events</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>Each Element:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>runs independently</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>is classpath-isolated</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>can be developed, tested, and deployed separately</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-example-payment-processing-as-a-custom-element">Example: Payment Processing as a Custom Element</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>A payment provider can ship a Custom Element that:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>validates receipts against external app stores</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>verifies transactions</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>issues digital goods using Elements inventory APIs</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>For game developers, this means:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>no custom purchase-validation backend</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>no duplicated logic across games</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>a trusted, reusable integration</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-using-elements-alongside-external-services">Using Elements Alongside External Services</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Running your game inside Elements does <em>not</em> prevent you from using other services.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Common hybrid setups include:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Custom Elements for authoritative gameplay logic</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>external services for analytics, matchmaking, or social features</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>Elements does not force a single architecture - it provides a runtime where authority can safely live.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-choosing-the-right-model">Choosing the Right Model</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>You might choose to run your game logic inside Elements if:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>you want to minimize backend infrastructure</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>you want authoritative access to game state</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>your logic is tightly coupled to Elements features</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>you want composable, reusable server-side modules</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>You might choose an external server if:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>you already have significant backend investment</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>your architecture depends on specialized infrastructure</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>you want to keep Elements focused on state and services</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>Many teams generally prefer to keep everything within Elements, but sometimes it makes sense to use both.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-how-this-fits-into-the-rest-of-the-docs">How This Fits Into the Rest of the Docs</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>When other documentation refers to:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>“server-side code”</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>“authoritative logic”</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>“Custom Elements”</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>It may be referring to logic that runs inside Elements itself.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Understanding Elements as a game runtime will make feature-level documentation clearer and more flexible.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-what-to-read-next">What to Read Next</h2>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong><a href="../docs/general-concepts/why-you-need-a-server-and-what-authoritative-means/">Why You Need Authority</a></strong> – understanding trust and permanence</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong><a href="../docs/general-concepts/where-your-authoritative-code-runs/">Where Your Authoritative Code Runs</a></strong> – choosing between deployment models</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong><a href="../docs/general-concepts/lifecycles-and-flows/">Lifecycles and Flows</a></strong> – seeing how systems work end-to-end</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->
