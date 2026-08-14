<h1>Namazu Crossfire (Multiplayer)</h1>

<!-- wp:paragraph -->
<p>Namazu<code> Crossfire</code> is an Element that integrates with <code>Namazu Elements</code> to provide WebSocket-based signaling and matchmaking. Built on the low-level MultiMatch API, Namazu Crossfire supports peer-to-peer multiplayer using WebSocket relay or WebRTC. This standards-driven approach enables game developers to build truly cross-platform multiplayer games, allowing interoperability across consoles, PC, and browser.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-overview">Overview</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Namazu Crossfire provides a complete foundation for cross-platform multiplayer. It handles session discovery, matchmaking, and connection signaling, making it easy to establish direct or relayed peer connections between players. Crossfire builds on open standards like WebRTC to ensure compatibility across diverse runtimes, from native builds to browsers.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Developers can use Crossfire alongside other Elements such as Identity, Data, and Cloud Functions to deliver seamless real-time multiplayer experiences.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-key-features">Key Features</h2>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>Cross-Platform Support</strong> - Enables multiplayer connectivity between console, PC, and web clients.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Peer-to-Peer Networking</strong> - Uses WebRTC for direct communication or WebSocket relay when necessary.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Integrated Matchmaking</strong> - Built-in signaling and matchmaking that work seamlessly with Namazu Elements.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Open Standards</strong> - Based on industry standards for networking and media transport, ensuring broad compatibility.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Modular Design</strong> - Built on the <code>MultiMatch</code> API, making it easy to extend or integrate with other Elements.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Extensibility</strong> - Provides a means to extend the protocol or the inner workings to meet your product's specific needs. </li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-guides">Guides</h2>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><a href="../deploying-crossfire-in-your-game"><strong>Deploying Namazu Crossfire in your game</strong></a><br>How to add the Crossfire Element as a dependency and enable it in your local development environment.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="../crossfire-protocol-reference"><strong>Crossfire Protocol Reference</strong></a><br>The full wire protocol: connection state machine, the four handshake flows, signal types and lifecycles, control messages, and error handling. Start here if you're implementing a client from scratch.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="../crossfire-client-libraries"><strong>Crossfire Client Libraries (JVM &amp; Browser)</strong></a><br>How to use the JVM (<code>client-onvoid</code>) and browser (<code>client-teavm</code>) WebRTC client libraries alongside the <a href="../crossfire">Unity plugin</a>.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="../crossfire-custom-matchmaking-algorithms"><strong>Custom Matchmaking Algorithms</strong></a><br>How the built-in FIFO and join-code matchmaking algorithms work, and how to implement your own (ratings, lobbies, region pools).</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-additional-resources-and-further-reading">Additional Resources and Further Reading</h2>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><a href="https://github.com/NamazuStudios/pong-multiplayer-example"><strong>Unity Pong Multiplayer Example</strong></a><br>A complete working example showing how to use Namazu Elements with Namazu Crossfire in Unity. This project demonstrates user authentication, matchmaking, and real-time gameplay using WebRTC and Unity Netcode for GameObjects.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="https://github.com/NamazuStudios/crossfire"><strong>Namazu Crossfire GitHub Repository</strong></a><br>The official repository for Namazu Crossfire. Includes setup instructions, server and client implementation details, and configuration examples for running Crossfire locally or in production.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="https://webrtc.org/"><strong>Official WebRTC Project</strong></a><br>The home of the WebRTC open-source project. Explains how WebRTC enables real-time audio, video, and data communication directly between peers using standard web technologies.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="https://www.youtube.com/watch?v=8I2axE6j204"><strong>WebRTC Crash Course (YouTube)</strong></a><br>A concise introduction to how WebRTC works. Covers peer connection establishment, signaling, STUN/TURN servers, and data channels in an accessible, visual format.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->
