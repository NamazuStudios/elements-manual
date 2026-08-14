<h1>Crossfire Client Libraries (JVM &amp; Browser)</h1>

<!-- wp:paragraph -->
<p>Besides the <a href="../crossfire">Unity Crossfire Plugin</a>, Namazu Crossfire ships two Java-based client libraries that speak the same <a href="../crossfire-protocol-reference">wire protocol</a>: a JVM/native client for desktop, server-to-server, and integration-test use, and a browser client compiled to JavaScript via TeaVM. Both sit on top of a small shared API so application code can be written mostly against interfaces rather than against a specific WebRTC backend.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-the-shared-client-api">The shared client API</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The <code>client</code> module defines the vendor-neutral surface both implementations expose:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list">
<li><strong><code>Crossfire</code></strong> &#8212; the top-level facade. Obtain a builder from a <code>CrossfireClientProvider</code> (an SPI, discovered via <code>META-INF/services</code>), configure it, then <code>connect()</code>. Modes are combinations of <code>Protocol</code> (<code>WEBRTC</code> or <code>SIGNALING</code>) and role (host or client) — e.g. <code>WEBRTC_HOST</code>. After connecting, call <code>findMatchHost()</code>/<code>findMatchClient()</code> (optionally scoped to a specific <code>Protocol</code>) to get the active peer collection, and subscribe to <code>onHostOpenStatus</code>/<code>onClientOpenStatus</code> for open/close notifications.</li>
<li><strong><code>SignalingClient</code></strong> &#8212; the underlying handshake/signal transport. Exposes <code>getState()</code> (match ID, host, profile roster), <code>backlog()</code> (the buffered signals described in the protocol reference), <code>signal(Signal)</code>, <code>control(ControlMessage)</code>, and three <code>handshake(...)</code> overloads (fire-and-forget, callback, or blocking with a timeout). Its own phase enum mirrors the server's connection state machine: <code>READY &#8594; CONNECTED &#8594; HANDSHAKING &#8594; SIGNALING &#8594; TERMINATED</code>.</li>
<li><strong><code>MatchHost</code></strong> &#8212; <code>start()</code>, <code>knownPeers()</code>, <code>findPeer(profileId)</code>, <code>newPeerQueue()</code>, <code>onPeerStatus(...)</code>, <code>close()</code>.</li>
<li><strong><code>MatchClient</code></strong> &#8212; <code>connect()</code>, <code>findPeer()</code> (there's only one — the host), <code>newPeerQueue()</code>, <code>onPeerStatus(...)</code>, <code>close()</code>.</li>
<li><strong><code>Peer</code></strong> &#8212; one connected participant. <code>getPhase()</code> (<code>READY</code>/<code>CONNECTED</code>/<code>TERMINATED</code>), <code>send(String)</code>/<code>send(ByteBuffer)</code> (returns a <code>SendResult</code>: <code>SENT</code>/<code>NOT_READY</code>/<code>ERROR</code>/<code>TERMINATED</code>), and <code>onMessage</code>/<code>onStringMessage</code>/<code>onError</code> subscriptions.</li>
</ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>Implementation-agnostic configuration is expressed as plain records, so the same values can be handed to either backend:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list">
<li><code>CrossfireIceServer</code> &#8212; <code>urls</code>, <code>username</code>, <code>password</code>, <code>hostname</code>, <code>tlsCertPolicy</code>. <code>CrossfireIceServer.googleDefaults()</code> returns Google's public STUN servers.</li>
<li><code>CrossfireDataChannelConfig</code> &#8212; <code>ordered</code>, <code>negotiated</code>, <code>maxPacketLifeTime</code>, <code>maxRetransmits</code>, <code>id</code>, <code>protocol</code>. <code>defaults()</code> is ordered, non-negotiated, with no lifetime/retransmit caps.</li>
<li><code>CrossfireOfferOptions</code> &#8212; <code>voiceActivityDetection</code>, <code>iceRestart</code>. <code>defaults()</code> has VAD on, ICE restart off.</li>
<li><code>CrossfireTlsCertPolicy</code> &#8212; <code>SECURE</code> or <code>INSECURE_NO_CHECK</code>, for relaxing certificate validation against a local dev TURN/relay server using a self-signed certificate.</li>
</ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>Both concrete implementations extend <code>AbstractCrossfire</code>, which handles the common bookkeeping: it subscribes to the <code>SignalingClient</code>'s signals, and on receiving a <code>HOST</code> signal it works out — per supported protocol — whether the local peer is the host or a client, then builds a fresh set of <code>MatchHost</code>/<code>MatchClient</code> instances via two hooks the subclass supplies (<code>populateHosts</code>/<code>populateClients</code>). Application code normally never touches <code>AbstractCrossfire</code> directly.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-jvm-client-onvoid">JVM client (<code>client-onvoid</code>)</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Built on <a href="https://github.com/devopvoid/webrtc-java"><code>dev.onvoid.webrtc:webrtc-java</code></a> (native WebRTC bindings) plus the Jakarta WebSocket client API for the signaling transport. This is the client to reach for in a JVM game server, a headless bot, or an integration test — not for a browser deployment.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Get a builder from <code>OnvoidCrossfireClientProvider</code>:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>Crossfire crossfire = new OnvoidCrossfireClientProvider()
    .newBuilder()
    .withDefaultUri(URI.create("wss://your-server/app/ws/crossfire"))
    .withIceServers(List.of(CrossfireIceServer.googleDefaults()))
    .withDataChannelConfig(CrossfireDataChannelConfig.defaults())
    .build();

crossfire.connect();
MatchHost host = crossfire.findMatchHost().orElseThrow();</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>If no URI is supplied, the builder falls back to the <code>ELEMENTS_CROSSFIRE_URI</code> environment variable, then the <code>dev.getelements.elements.crossfire.client.uri</code> system property.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><strong>Host role:</strong> <code>WebRTCMatchHost</code> listens for <code>CONNECT</code>/<code>DISCONNECT</code> signals and lazily creates one offering peer (<code>WebRTCOfferingPeer</code>) per remote profile as they connect. <strong>Client role:</strong> <code>WebRTCMatchClient</code> wraps a single answering peer (<code>WebRTCAnsweringPeer</code>) targeting whichever profile the <code>HOST</code> broadcast signal names. Both share one native <code>PeerConnectionFactory</code> (<code>SharedPeerConnectionFactory</code>) and a single serialized executor for native WebRTC calls (<code>SharedWebRTCExecutor</code>) by default — override with <code>withPeerConnectionFactory</code>/<code>withExecutor</code> only if you need per-instance isolation.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><code>CrossfireIceServer</code>, <code>CrossfireOfferOptions</code>, and <code>CrossfireDataChannelConfig</code> map onto onvoid's native <code>RTCIceServer</code>/<code>RTCOfferOptions</code>/<code>RTCDataChannelInit</code> types, including the TLS cert policy. Note that offer options only flow to the host (offering) side — the answering side always uses onvoid's default <code>RTCAnswerOptions</code>.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-browser-client-teavm">Browser client (<code>client-teavm</code>)</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Compiles the same client abstractions to JavaScript using <a href="https://teavm.org/">TeaVM</a>, calling the browser's native <code>WebSocket</code> and <code>RTCPeerConnection</code> directly through thin JSO overlays — no separate JS SDK to keep in sync with the Java protocol model. Get a builder from <code>TeaVMCrossfireClientProvider</code>; usage mirrors the JVM client:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>Crossfire crossfire = new TeaVMCrossfireClientProvider()
    .newBuilder()
    .withDefaultUri(URI.create("wss://your-server/app/ws/crossfire"))
    .withIceServers(List.of(CrossfireIceServer.googleDefaults()))
    .build();

crossfire.connect();</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>The host/client peer roles work the same way as the JVM client (an offering peer per remote profile on the host side, one answering peer on the client side), backed by plain <code>HashMap</code>s rather than concurrent collections, since generated JS is single-threaded.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><strong>Known gaps versus the JVM client</strong> — worth knowing before you assume feature parity:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list">
<li><code>withOfferOptions(...)</code> and <code>withDataChannelConfig(...)</code> are accepted by the builder but currently have <strong>no effect</strong> on the browser target — data channels are always created with a hardcoded <code>{"ordered":true}</code> configuration. Don't rely on custom retransmit/lifetime/negotiated settings in a browser build.</li>
<li><code>CrossfireIceServer.hostname</code> and <code>tlsCertPolicy</code> are silently dropped when building the browser's ICE server list — browsers manage their own TLS trust and don't expose a knob to override it, so <code>CrossfireTlsCertPolicy</code> has no meaning here.</li>
<li><code>newPeerQueue()</code> throws <code>UnsupportedOperationException</code> on both <code>MatchHost</code> and <code>MatchClient</code> in the browser build — a blocking queue would deadlock a single-threaded JS runtime. Use <code>onPeerStatus(...)</code> callbacks instead of polling a queue.</li>
</ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>The <code>client-teavm</code> module pins an older Jetty version for its test/dev browser runner and excludes the Elements SDK BOM's newer Jetty transitives to avoid a classpath conflict — if you fork this module's build, keep that exclusion, or the embedded test server won't start.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-choosing-a-client">Choosing a client</h2>
<!-- /wp:heading -->

<!-- wp:table -->
<figure class="wp-block-table"><table>
<thead><tr><th>Target</th><th>Module</th><th>Use for</th></tr></thead>
<tbody>
<tr><td>Unity (desktop/console/mobile)</td><td><a href="../crossfire">Unity Crossfire Plugin</a></td><td>Shipping games built on Unity Netcode for GameObjects</td></tr>
<tr><td>JVM (native WebRTC)</td><td><code>client-onvoid</code></td><td>Headless bots, load/integration tests, JVM-based game servers acting as a peer</td></tr>
<tr><td>Web browser</td><td><code>client-teavm</code></td><td>Browser-based game clients, browser-side test harnesses</td></tr>
</tbody>
</table></figure>
<!-- /wp:table -->
