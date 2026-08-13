<h1>Why You Need a Server (and What “Authoritative” Means)</h1>

<!-- wp:paragraph -->
<p>If you’re coming from a client-only background (Unity, Unreal, web apps, etc.), it’s natural to ask:</p>
<!-- /wp:paragraph -->

<!-- wp:quote -->
<blockquote class="wp-block-quote"><!-- wp:paragraph -->
<p><em>Why do I need a server at all? Can’t the game just do this itself?</em></p>
<!-- /wp:paragraph --></blockquote>
<!-- /wp:quote -->

<!-- wp:paragraph -->
<p>For small prototypes, the answer is often “yes.” For live games with progression, rewards, purchases, or leaderboards, the answer quickly becomes “no", and here’s why:</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-the-core-problem-trust">The Core Problem: Trust</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>A game client runs on the player’s device. That means the player controls it.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>They can:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>modify memory</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>intercept or replay network calls</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>fake events</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>patch or replace client code</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>If the client is responsible for deciding:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>whether a mission was completed</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>how many points were earned</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>whether a purchase is valid</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>whether a reward should be granted</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>…then those systems are not secure.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-what-a-server-does">What a Server Does</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>A server exists to act as an authoritative source of truth.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Authoritative code:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>runs in an environment the player cannot modify</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>validates what actually happened</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>makes final decisions about permanent game state</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>In practice, this usually means:</p>
<!-- /wp:paragraph -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><tbody><tr><th class="has-text-align-center" data-align="center">Action</th><th class="has-text-align-center" data-align="center">Responsibility</th></tr><tr><td class="has-text-align-center" data-align="center">Player input</td><td class="has-text-align-center" data-align="center">Client</td></tr><tr><td class="has-text-align-center" data-align="center">Rendering / UI</td><td class="has-text-align-center" data-align="center">Client</td></tr><tr><td class="has-text-align-center" data-align="center">Game rule validation</td><td class="has-text-align-center" data-align="center">Authoritative Code</td></tr><tr><td class="has-text-align-center" data-align="center">Progression updates</td><td class="has-text-align-center" data-align="center">Authoritative Code</td></tr><tr><td class="has-text-align-center" data-align="center">Rewards &amp; inventory</td><td class="has-text-align-center" data-align="center">Authoritative Code</td></tr><tr><td class="has-text-align-center" data-align="center">Leaderboards</td><td class="has-text-align-center" data-align="center">Authoritative Code</td></tr><tr><td class="has-text-align-center" data-align="center">Purchases</td><td class="has-text-align-center" data-align="center">Authoritative Code</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:paragraph -->
<p>The client <strong>requests</strong> changes. Authoritative code <strong>approves and applies</strong> them.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-where-does-authoritative-code-run">Where Does Authoritative Code Run?</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>In many systems, authoritative code runs on your own game server. Elements fully supports a server to server model.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>However, Elements also supports another option: Custom Elements.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-do-i-always-need-my-own-server">Do I Always Need My Own Server?</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>It is completely optional, and dependent on the architecture of your game.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Elements supports Custom Elements, which are your own server-side applications that run <em>inside Elements itself</em>.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>A Custom Element:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>runs in a trusted, server-side environment</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>can access Elements systems directly (users, inventory, progress, leaderboards, direct db access)</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>executes authoritative logic</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>cannot be modified by players</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>This means you have two valid deployment models:</p>
<!-- /wp:paragraph -->

<!-- wp:list {"ordered":true,"start":1} -->
<ol start="1" class="wp-block-list"><!-- wp:list-item -->
<li>External Game Server – your own backend service calls Elements APIs (still authoritative!)</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Custom Elements – your authoritative code runs inside Elements</li>
<!-- /wp:list-item --></ol>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>Many teams use both, especially when their teams are more familiar with modeling the game logic in their preferred game engine (for example, running Unity in headless mode). </p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-a-simple-mental-model">A Simple Mental Model</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Think of Elements as both a platform and a runtime:</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":22402,"width":"354px","height":"auto","aspectRatio":"1.1941889910702386","sizeSlug":"full","linkDestination":"none"} -->
<figure class="wp-block-image size-full is-resized"><img src="https://namazustudios.com/wp-content/uploads/2026/02/image.png" alt="" class="wp-image-22402" style="aspect-ratio:1.1941889910702386;width:354px;height:auto"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>That authoritative step may live:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>on your own server, or</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>inside Elements via a Custom Element</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-why-elements-assumes-authority">Why Elements Assumes Authority</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Elements manages high‑trust systems:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>digital goods and inventory</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>reward issuance</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>mission progression</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>leaderboards</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>scheduling and time‑based content</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>These systems require:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>protection against duplication</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>safe retries (idempotency)</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>consistent timekeeping</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>validation of events</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>Those guarantees are only possible when changes are applied by authoritative code.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-how-this-affects-the-rest-of-the-docs">How This Affects the Rest of the Docs</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>When the docs say:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>“server-side”</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>“authoritative”</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>“safe to retry”</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>“idempotent”</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>They refer to code that runs outside the player’s control — either on your server or inside a Custom Element.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Understanding this mental model will make the rest of the documentation much easier to follow.</p>
<!-- /wp:paragraph -->
