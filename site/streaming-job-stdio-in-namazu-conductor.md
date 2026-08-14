<h1>Streaming Job Stdio in Namazu Conductor</h1>

<!-- wp:paragraph -->
<p><code>OrchestrationService.streamStdio(execution)</code> returns a live, bidirectional connection to a running job's stdin/stdout/stderr — a <code>JobStdio</code> exposing <code>stdin</code> as an <code>OutputStream</code> and <code>stdout</code>/<code>stderr</code> as <code>InputStream</code>s. What backs that connection differs by provider: it's native on Kubernetes, and sidecar-based on EdgeGap and ECS, whose platforms have no built-in container-exec API of their own.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-support-by-provider">Support by Provider</h2>
<!-- /wp:heading -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th>Provider</th><th>Mechanism</th><th>Notes</th></tr></thead><tbody><tr><td>Kubernetes</td><td>Native — equivalent to <code>kubectl attach</code></td><td>Requires the pod to be in the <code>Running</code> phase.</td></tr><tr><td>ECS</td><td><code>namazu-stdio-bridge</code> sidecar</td><td>Requires the bridge baked into your task's container image.</td></tr><tr><td>EdgeGap</td><td><code>namazu-stdio-bridge</code> sidecar</td><td>Requires the bridge baked into your app version's container image.</td></tr><tr><td>Multiplay</td><td>Not supported</td><td>The Multiplay allocation API has no equivalent; calling <code>streamStdio()</code> throws <code>UnsupportedOperationException</code>.</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"Note"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="3b0649"><div class="gb-notice-title" style="color:#fff"><p>Note</p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p>On EdgeGap and ECS, <code>streamStdio()</code> only works on a <code>JobExecution</code> obtained directly from <code>execute()</code> (or a copy derived from it via <code>getFutureForStatus</code>/<code>getStageForStatus</code>). An execution reconstructed from <code>listExecutions()</code> has no credential attached and throws <code>StdioUnavailableException</code>. Hold on to the object <code>execute()</code> gives you if you plan to stream stdio.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-why-a-sidecar">Why a Sidecar for EdgeGap/ECS</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Kubernetes exposes a container-exec/attach API that Conductor can call into directly. EdgeGap and ECS don't — there's no platform-level API to reach into a running container's stdio. <strong>namazu-stdio-bridge</strong> closes that gap: a small wrapper process (conceptually similar to <code>dockerize</code>) that you make the entrypoint of your own container image. It execs your real entrypoint, forwards your image's <code>CMD</code> (or the <code>command</code>/<code>args</code> a <code>JobRequest</code> supplies at runtime) as that process's argv, and exposes the child's stdin/stdout/stderr over three WebSocket endpoints that Conductor's EdgeGap and ECS providers know how to speak to.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>The bridge is published as a standalone binary (PyInstaller + <code>staticx</code> — no Python runtime required in your final image, and portable across glibc- and musl-based base images) at <code>ghcr.io/namazustudios/namazu-stdio-bridge:latest</code>. Vendor it into your image with a multi-stage build:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>COPY --from=ghcr.io/namazustudios/namazu-stdio-bridge:latest \
     /usr/local/bin/namazu-stdio-bridge /usr/local/bin/namazu-stdio-bridge
ENTRYPOINT ["/usr/local/bin/namazu-stdio-bridge"]</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>By default the bridge looks for your real entrypoint at <code>/docker-entrypoint.sh</code>, <code>/usr/local/bin/docker-entrypoint.sh</code>, or <code>/app/docker-entrypoint.sh</code> (first match wins) — override the candidate list with <code>NAMAZU_CONDUCTOR_STDIO_ENTRYPOINT</code> if your entrypoint lives elsewhere. Declare the bridge's listen port (<code>10080</code> by default) in your app version's or task definition's port mappings, since it has to be reachable from Conductor.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-wire-protocol">Wire Protocol</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The bridge exposes three WebSocket endpoints, carrying raw bytes with no additional framing:</p>
<!-- /wp:paragraph -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th>Path</th><th>Direction</th><th>Semantics</th></tr></thead><tbody><tr><td><code>{base}/0</code></td><td>client → server</td><td>stdin — frames forwarded to the child process's stdin</td></tr><tr><td><code>{base}/1</code></td><td>server → client</td><td>stdout — raw chunks as produced</td></tr><tr><td><code>{base}/2</code></td><td>server → client</td><td>stderr — raw chunks as produced</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:paragraph -->
<p><code>{base}</code> is the bridge's configured URI prefix — set it via <code>NAMAZU_CONDUCTOR_STDIO_URI</code> on the bridge, and the matching <code>...stdio.bridge.base.path</code> attribute on the Conductor provider (see <a href="configuring-namazu-conductor-providers">Configuring Namazu Conductor Providers</a>).</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>When the child process exits, the server closes <code>/1</code> and <code>/2</code> with WebSocket close code <code>4000 + &lt;exit code&gt;</code> (e.g. <code>4000</code> for a clean exit, <code>4007</code> for exit code 7 — clamped to the <code>4000</code>–<code>4255</code> range). If the process was killed by a signal instead, the close reason string carries the signal name (e.g. <code>"SIGTERM"</code>).</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Connecting to <code>/1</code> or <code>/2</code> immediately replays a bounded ring buffer of the most recent output — up to 16 KiB for stdout and 4 KiB for stderr by default — before switching to live streaming, so a client that attaches a moment after the process started producing output still sees recent history rather than only what's written from that point on. This is a replay buffer, not a log API: output older than its capacity is gone for good.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-authentication">Authentication</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Every WebSocket connection to <code>/0</code>, <code>/1</code>, or <code>/2</code> must present <code>Authorization: Bearer &lt;token&gt;</code> at the handshake. Connections that omit it, or present the wrong token, are rejected with HTTP <code>401</code> at the handshake itself — the connect call fails outright rather than appearing to succeed and then closing. The bridge refuses to start at all if no token is configured; its stdio endpoints would otherwise be reachable, unauthenticated, by anyone who can reach the port.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>You never set this token by hand. Conductor generates a fresh one per job execution, injects it into the container as the <code>NAMAZU_CONDUCTOR_STDIO_TOKEN</code> environment variable, and carries it internally on the <code>JobExecution</code> returned by <code>execute()</code> so that a later <code>streamStdio()</code> call can present it automatically.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-bridge-configuration-reference">Bridge Configuration Reference</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>These environment variables configure the bridge itself, inside your image — they're independent of the Conductor-side attributes covering the same concerns (port, base path) on the EdgeGap/ECS providers.</p>
<!-- /wp:paragraph -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th>Variable</th><th>Default</th><th>Description</th></tr></thead><tbody><tr><td><code>NAMAZU_CONDUCTOR_STDIO_TOKEN</code></td><td><em>required</em></td><td>Bearer token every WebSocket connection must present. The bridge refuses to start without it.</td></tr><tr><td><code>NAMAZU_CONDUCTOR_STDIO_URI</code></td><td><code>/</code></td><td>Base path prefix for the three WebSocket endpoints.</td></tr><tr><td><code>NAMAZU_CONDUCTOR_STDIO_ENTRYPOINT</code></td><td><code>/docker-entrypoint.sh:/usr/local/bin/docker-entrypoint.sh:/app/docker-entrypoint.sh</code></td><td><code>os.pathsep</code>-separated list of candidate entrypoint paths, tried in order; the first existing, executable path wins.</td></tr><tr><td><code>NAMAZU_CONDUCTOR_STDIO_BUFFER_SIZE</code></td><td><code>4096</code></td><td>Chunk size in bytes per WebSocket message (distinct from the replay buffers below).</td></tr><tr><td><code>NAMAZU_CONDUCTOR_STDIO_STDOUT_BUFFER_SIZE</code></td><td><code>16k</code></td><td>Ring buffer capacity for stdout replay. Accepts a plain byte count or a <code>k</code>/<code>m</code> suffix. <code>0</code> disables replay.</td></tr><tr><td><code>NAMAZU_CONDUCTOR_STDIO_STDERR_BUFFER_SIZE</code></td><td><code>4096</code></td><td>Same, for stderr.</td></tr><tr><td><code>NAMAZU_CONDUCTOR_STDIO_PORT</code></td><td><code>10080</code></td><td>WebSocket listen port.</td></tr><tr><td><code>NAMAZU_CONDUCTOR_STDIO_LOG_LEVEL</code></td><td><code>INFO</code></td><td>The bridge's own log level.</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:paragraph -->
<p>Whatever you set for <code>NAMAZU_CONDUCTOR_STDIO_PORT</code> and <code>NAMAZU_CONDUCTOR_STDIO_URI</code> inside the image must match the corresponding <code>...stdio.bridge.port</code> and <code>...stdio.bridge.base.path</code> attributes on the Conductor provider Element, or Conductor won't be able to reach the bridge.</p>
<!-- /wp:paragraph -->
