<h1>Element Lifecycle and Background Executors</h1>

<!-- wp:paragraph -->
<p>This page describes what happens when an Element deployment is unloaded (including a hot reload), how to stop background work cleanly, and the platform-managed executor facility that makes the safe pattern the default.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-what-happens-at-unload">What Happens at Unload</h2>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>Handlers retire immediately.</strong> When a deployment unloads (including the unload step of a hot reload), the old generation's Jetty handlers are unmounted at that moment, before the new version loads. Requests in flight may still be executing.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Close hooks fire next.</strong> Every close-hook subscription on the Element runs while the Element's own archives are still open, so your hook can resolve classes and resources while shutting down.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Managed executors are reaped.</strong> Executors created through the platform facility (below) are shut down automatically, and the runtime logs a single <code>Reaped N executor(s) for element X</code> line.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Classloaders close after a grace period.</strong> The element's classloaders and staged files are released only after <code>dev.getelements.elements.runtime.unload.grace.period.seconds</code> (default 30 seconds), so an in-flight request (including one that lazily resolves a class) finishes before its archives disappear.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-background-executors">Background Executors</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>If your Element runs background work (pollers, reconciliation loops, schedulers), create its executors through the platform-managed <code>ElementExecutors</code> facility, which is bound in every Element's injector:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code class="language-kotlin">class MeteringSyncService @Inject constructor(elementExecutors: ElementExecutors) {
    private val poller = elementExecutors.newScheduledExecutor("metering-sync", 1)
}</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>Executors created through the facility are tracked by the runtime and shut down automatically when the Element closes. Threads are named <code>&lt;element&gt;:&lt;executor-name&gt;-&lt;n&gt;</code>, so a stack trace identifies both the poller and its Element generation.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>If you create an executor by other means (or receive one from a third-party library), stop it yourself in a close hook. Inject the Element and register the hook: do not look it up through <code>ElementSupplier</code> or <code>ElementRegistrySupplier</code>, whose static state is only bound after the Element's injector finishes building and is therefore not yet visible to eager singletons.</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code class="language-kotlin">class MeteringSyncService @Inject constructor(element: Element) {
    private val executor = Executors.newSingleThreadScheduledExecutor()

    init {
        element.onClose { executor.shutdownNow() }  // fires when this generation unloads
    }
}</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>Register one hook per executor owner. A background executor with no close hook survives its own Element generation after a hot reload, and keeps running against state owned by a generation that no longer exists.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-operator-settings">Operator Settings</h2>
<!-- /wp:heading -->

<!-- wp:table -->
<figure class="wp-block-table"><table><thead><tr><th>Setting</th><th>Default</th><th>Meaning</th></tr></thead><tbody><tr><td><code>dev.getelements.elements.runtime.unload.grace.period.seconds</code></td><td><code>30</code></td><td>Seconds between an unload and the closing of the retired generation's classloaders. In-flight requests finish during this window.</td></tr><tr><td><code>dev.getelements.elements.health.deployment.absence.threshold.seconds</code></td><td><code>600</code></td><td>Seconds an enabled, mission-critical deployment may be absent from the instance's active runtimes before the health check reports it. Long enough to cover a hot reload's record-free load window; short enough to flag a stuck reconcile.</td></tr></tbody></table></figure>
<!-- /wp:table -->
