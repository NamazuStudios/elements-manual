<h1>3.4 Release Notes</h1>

<!-- wp:heading {"level":1} -->
<h1 class="wp-block-heading" id="h-namazu-elements-3-4-release-notes">Namazu Elements 3.4 Release Notes</h1>
<!-- /wp:heading -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-overview">Overview</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Namazu Elements 3.4 is one of our most substantial updates yet, introducing deep improvements across the backend, UI, and developer ecosystem.<br>This release enhances matchmaking performance, modernizes the CMS interface, adds robust file management, and introduces full support for <strong>real-time multiplayer</strong> through <strong>Namazu Crossfire</strong>.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-multimatch-internal-api">MultiMatch Internal API</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>We’ve <strong>greatly improved the MultiMatch internal API</strong>, providing:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>Optimized matchmaking performance</strong> for low-latency, high-concurrency scenarios.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Improved <strong>query efficiency</strong> and reduced memory overhead.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Simplified internal structures for easier integration and debugging.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>These upgrades form the foundation for the new multiplayer capabilities and improve reliability at scale.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-completely-rebuilt-cms-ui">Completely Rebuilt CMS UI</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The Elements CMS has been <strong>rebuilt from the ground up</strong>, with a modern design and several major new features:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>JSON Object Editing:</strong> Directly view and edit raw JSON for every object type.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Light Mode / Dark Mode:</strong> Switch themes for a better visual experience.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>System Readout Dashboard:</strong> View all Elements in your system with links to documentation.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Health Checks:</strong> Run live system diagnostics from within the CMS UI.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Feature Flags:</strong> Toggle features dynamically, without redeployments.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Improved Diagnostics &amp; Troubleshooting:</strong> More transparent error surfacing and runtime logs.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>This overhaul makes Elements significantly easier to manage, observe, and customize.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-large-object-api-integration">Large Object API Integration</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>You can now <strong>access and manage files directly</strong> through the <strong>Large Object API</strong> within the CMS.<br>Developers can upload, modify, and remove large files seamlessly, consolidating asset management within the same interface used for Element and metadata management.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-expanded-search-capabilities">Expanded Search Capabilities</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>We’ve added <strong>search query support for previously missing collections</strong>, ensuring that all data types are now discoverable and queryable through the CMS and APIs.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-metadata-validation-fixes">Metadata Validation Fixes</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Fixed several issues with <strong>metadata validation</strong>, improving consistency and reducing false validation errors during object creation and update workflows.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-real-time-multiplayer-with-namazu-crossfire">Real-time Multiplayer with Namazu Crossfire</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Elements 3.4 adds <strong>official support for WebRTC and WebSocket-based multiplayer</strong> via <a href="https://github.com/NamazuStudios/crossfire"><strong>Namazu Crossfire</strong></a>.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>This new Element enables fast, low-latency real-time gameplay powered by the updated MultiMatch and data infrastructure.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Many of the backend and database changes in this release were designed specifically to support Crossfire’s real-time architecture, including improved connection handling, session management, and matchmaking throughput.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-updated-example-pong">Updated Example: Pong</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The <strong>Pong example on GitHub</strong> has been fully updated to include:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>A working demonstration of the new <strong>matchmaking system</strong>.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Real-time <strong>networked play</strong> powered by Crossfire’s WebRTC transport.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Integration examples showing how to use Elements APIs for session setup, matchmaking, and gameplay synchronization.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>You can find the updated example in the <a href="https://github.com/NamazuStudios">Namazu Studios GitHub repository</a> </p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-updated-documentation">Updated Documentation</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>All documentation has been refreshed for 3.4, including:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Full API references for the MultiMatch and Large Object APIs.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Guides for integrating and deploying Namazu Crossfire.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>CMS UI walkthroughs and troubleshooting improvements.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-summary">Summary</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Namazu Elements 3.4 brings together performance, usability, and multiplayer innovation.<br>With this release, developers gain:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>A <strong>faster, smarter matchmaking system</strong>.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>A <strong>modern, rebuilt CMS UI</strong>.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Integrated file management</strong> via the Large Object API.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Full real-time multiplayer support</strong> through Namazu Crossfire.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>A refreshed <strong>Pong demo</strong> showcasing everything in action.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-screenshots">Screenshots</h2>
<!-- /wp:heading -->

<!-- wp:image {"id":22221,"sizeSlug":"full","linkDestination":"none"} -->
<figure class="wp-block-image size-full"><img src="https://namazustudios.com/wp-content/uploads/2025/10/image-1.png" alt="" class="wp-image-22221"/><figcaption class="wp-element-caption">New Login Screen</figcaption></figure>
<!-- /wp:image -->

<!-- wp:image {"id":22222,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://namazustudios.com/wp-content/uploads/2025/10/image-2-1024x836.png" alt="" class="wp-image-22222"/><figcaption class="wp-element-caption">Main Dashboard for Namazu Elements CMS</figcaption></figure>
<!-- /wp:image -->

<!-- wp:image {"id":22223,"sizeSlug":"full","linkDestination":"none"} -->
<figure class="wp-block-image size-full"><img src="https://namazustudios.com/wp-content/uploads/2025/10/image-3.png" alt="" class="wp-image-22223"/><figcaption class="wp-element-caption">Complete Diagnostics for Custom Code</figcaption></figure>
<!-- /wp:image -->

<!-- wp:image {"id":22224,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://namazustudios.com/wp-content/uploads/2025/10/image-4-1024x836.png" alt="" class="wp-image-22224"/><figcaption class="wp-element-caption">Native API Explorer for Custom APIs</figcaption></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->
