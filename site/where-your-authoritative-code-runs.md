<h1>Where Your Authoritative Code Runs</h1>

<!-- wp:paragraph -->
<p>Namazu Elements is flexible about <em>where</em> authoritative logic lives. What matters is that the code:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>runs in a trusted environment</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>validates events</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>applies permanent state changes</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>There are two primary options.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-option-1-your-own-game-server">Option 1: Your Own Game Server</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>In this model:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>your backend service receives requests from the client</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>your code validates gameplay, purchases, or actions</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>your server calls Elements APIs</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>This is a good fit when:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>you already have backend infrastructure</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>you need tight integration with existing systems</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-option-2-elements-as-your-game-server">Option 2: Elements as Your Game Server</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>A <strong>Custom Element</strong> is your own application running <em>inside Elements</em>.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Custom Elements:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>execute authoritative logic</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>can own request handling</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>can maintain realtime connections</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>can be written as plugins or libraries</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>can be developed and deployed independently</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>can interact directly with Elements systems</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>Multiple Custom Elements can be loaded at the same time, allowing functionality to be composed.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-example-payment-processing">Example: Payment Processing</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>A payment processor can ship a Custom Element that:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>validates purchase receipts</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>verifies transactions with external services</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>issues digital goods through Elements</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>This allows game developers to:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>avoid writing purchase validation logic</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>avoid operating their own backend for that purpose</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>rely on a trusted, reusable integration</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-architectural-flexibility">Architectural Flexibility</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>If you're starting a project from scratch and don't have any restrictions on what you're using for your gameplay logic, it may be easier to write the entirety of your application directly in Elements due to:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>its direct connections to other Custom Elements and the Elements Core systems</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>preconfigured database connection</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>ease of deployment</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>However, many production setups use both:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>a lightweight game server for gameplay validation</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Custom Elements for shared or third‑party logic (payments, promotions, analytics)</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>Elements does not require you to choose only one approach.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->
