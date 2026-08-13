<h1>Lifecycles and Flows</h1>

<!-- wp:paragraph -->
<p>This page describes somecommon end-to-end flows in Elements. These flows show how features work together over time, rather than focusing on individual APIs. In these flows, authoritative code may live inside a Custom Element, which can act as the core game server.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>If you are new to backend or server-side concepts, start here. You do not need to understand every API to follow these diagrams.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-how-to-read-these-flows">How to Read These Flows</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Each flow is shown as a sequence of steps:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Client – code running on the player’s device (UI, input)</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Authoritative Code – your server or a Custom Element</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Elements – the platform applying and enforcing state</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>The same flow applies whether your authoritative code runs on:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>your own game server, or</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>a Custom Element inside Elements</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-reward-lifecycle">Reward Lifecycle</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Rewards in Elements are deliberately split into issuance and redemption. This prevents duplication and gives you control over <em>when</em> rewards are applied.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-typical-reward-flow">Typical Reward Flow</h3>
<!-- /wp:heading -->

<!-- wp:image {"id":22407,"width":"383px","height":"auto","aspectRatio":"0.6572345472303613","sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large is-resized"><img src="https://namazustudios.com/wp-content/uploads/2026/02/image-2-673x1024.png" alt="" class="wp-image-22407" style="aspect-ratio:0.6572345472303613;width:383px;height:auto"/></figure>
<!-- /wp:image -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-why-this-exists">Why This Exists</h3>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Prevents duplicate rewards</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Allows safe retries</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Supports delayed claiming (reward screens, inboxes)</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Makes reward application auditable</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-common-variations">Common Variations</h3>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Immediate rewards: issuance and redemption happen back-to-back</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Delayed rewards: issuance happens first, redemption later</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>One-time rewards: persistent issuances prevent duplicates</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-mission-and-progress-lifecycle">Mission and Progress Lifecycle</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Missions define what can be done. Progress tracks what a player has done.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-typical-mission-flow">Typical Mission Flow</h3>
<!-- /wp:heading -->

<!-- wp:image {"id":22411,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://namazustudios.com/wp-content/uploads/2026/02/image-6-1024x489.png" alt="" class="wp-image-22411"/></figure>
<!-- /wp:image -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-key-concepts">Key Concepts</h3>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Missions are copied into progress; later edits do not affect existing players</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Progress is advanced only by authoritative code</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Step completion can trigger reward issuance</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-common-variations-0">Common Variations</h3>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Repeatable missions: progress resets after completion</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Scheduled missions: assignment is controlled by Schedules</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Parallel missions: multiple missions active at the same time</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-schedule-lifecycle">Schedule Lifecycle</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Schedules control <em>when</em> missions are active.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-typical-schedule-flow">Typical Schedule Flow</h3>
<!-- /wp:heading -->

<!-- wp:image {"id":22410,"width":"628px","height":"auto","sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large is-resized"><img src="https://namazustudios.com/wp-content/uploads/2026/02/image-5-1024x402.png" alt="" class="wp-image-22410" style="width:628px;height:auto"/></figure>
<!-- /wp:image -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-why-scheduling-is-separate">Why Scheduling Is Separate</h3>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Allows time-based content without hardcoding dates</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Supports seasonal and rotating content</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Makes mission assignment safe to repeat</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>Schedules do <strong>not</strong> track player progress themselves — they only control assignment.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-leaderboard-lifecycle">Leaderboard Lifecycle</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Leaderboards track and rank player scores over time.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-typical-leaderboard-flow">Typical Leaderboard Flow</h3>
<!-- /wp:heading -->

<!-- wp:image {"id":22412,"width":"384px","height":"auto","sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large is-resized"><img src="https://namazustudios.com/wp-content/uploads/2026/02/image-7-802x1024.png" alt="" class="wp-image-22412" style="width:384px;height:auto"/></figure>
<!-- /wp:image -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-epochal-leaderboards">Epochal Leaderboards</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>For resetting leaderboards:</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":22415,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://namazustudios.com/wp-content/uploads/2026/02/image-10-1024x96.png" alt="" class="wp-image-22415"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>Epoch handling is automatic once configured.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-putting-it-all-together">Putting It All Together</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Many production systems combine multiple flows:</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":22414,"width":"612px","height":"auto","aspectRatio":"2.60566110895696","sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large is-resized"><img src="https://namazustudios.com/wp-content/uploads/2026/02/image-9-1024x393.png" alt="" class="wp-image-22414" style="aspect-ratio:2.60566110895696;width:612px;height:auto"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>Elements is designed so these operations are:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>safe to retry</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>resistant to duplication</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>auditable</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-what-to-read-next">What to Read Next</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>After understanding these flows:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>See <strong><a href="https://namazustudios.com/docs/namazu-elements-core/features/reward-issuance/">Rewards</a> &amp; <a href="https://namazustudios.com/docs/namazu-elements-core/features/digital-goods/">Digital Goods</a></strong> to dive deeper into issuance and inventory</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>See <strong><a href="https://namazustudios.com/docs/namazu-elements-core/features/progress-and-missions-3-4/">Progress &amp; Missions</a></strong> for detailed mission configuration</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>See <strong><a href="https://namazustudios.com/docs/namazu-elements-core/features/leaderboards/">Leaderboards</a></strong> for ranking and social queries</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>If you’re unsure where code should live, review <strong><a href="https://namazustudios.com/docs/general-concepts/why-you-need-a-server-and-what-authoritative-means/">Why You Need a Server</a></strong> and <strong><a href="https://namazustudios.com/docs/general-concepts/custom-elements/">Custom Elements</a></strong> in General Concepts.</p>
<!-- /wp:paragraph -->
