<h1>Cluster RPC (Fabric)</h1>

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"Warning","noticeBackgroundColor":"#ffdd57"} -->
<div style="color:#32373c;background-color:#ffdd57" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="f1a203"><div class="gb-notice-title" style="color:#fff"><p>Warning</p></div><div class="gb-notice-text" style="border-color:#ffdd57"><!-- wp:paragraph -->
<p>This page describes internal, instance-to-instance infrastructure. It introduces no new API for game developers or plugin (Element) authors. The transport described here is a prototype, not yet used for anything production-facing.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:heading {"anchor":"h-overview"} -->
<h2 id="h-overview" class="wp-block-heading">Overview</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Cross-instance service invocation (one Elements instance calling a service hosted on another) historically ran over JeroMQ (ZeroMQ for Java). That transport has been removed: no live code path issued remote invocations through it, and it had become a source of hangs and leaks from mixing libzmq's socket-based synchronization with Java's own concurrency primitives.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>It's being replaced with a WebSocket-based transport, working name <strong>Fabric</strong>, since Elements already runs a Jetty-based HTTP/WebSocket server and no longer needs a second, JeroMQ-specific networking stack.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"anchor":"h-current-status"} -->
<h2 id="h-current-status" class="wp-block-heading">Current Status: Transport Prototype</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The current state proves the transport mechanics only: a single invocation can round-trip over a real WebSocket connection between two Elements instances. Each instance exposes one static WebSocket endpoint at <code>/cluster/v1</code>, registered once at server startup (not per-plugin, and not added or removed at runtime). There is no authentication, no connection pooling or retry, and no integration with Element-based service dispatch yet: invocations are dispatched via the same legacy mechanism the old JeroMQ transport used.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Remaining work, roughly in order, before this is production cluster infrastructure:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Invocation-semantics parity with the old transport (multiplexing, cancellation, per-call timeouts, connection reuse).</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Dispatch through Elements' own service registry, replacing the legacy dispatcher this prototype uses as a placeholder.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Addressing a specific deployment, plugin, and service on a remote instance, rather than always dispatching against whatever that instance happens to have bound.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Authentication (reusing the existing session-secret mechanism already used elsewhere in Elements) and instance discovery.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>Track progress on <a href="https://github.com/NamazuStudios/elements/issues/10">GitHub issue #10</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->
