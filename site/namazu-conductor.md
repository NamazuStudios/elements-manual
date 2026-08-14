<h1>Namazu Conductor</h1>

<!-- wp:paragraph -->
<p>Namazu Conductor is an <code>Element</code> that provides a unified container/session orchestration API for the Namazu Elements SDK. It abstracts a set of provider-specific game-server orchestration platforms — <a href="https://edgegap.com/">EdgeGap</a>, AWS ECS, Kubernetes, and Unity Multiplay — behind a single <code>OrchestrationService</code> interface, so your authoritative code can launch, monitor, and stop game server workloads without depending on which platform actually hosts them.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-overview">Overview</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Every orchestration platform Conductor supports has its own idea of a "profile" (a build config, a task definition, a pod template, a fleet), its own placement model (region, IP, lat/long), and its own scoping concept (namespace, cluster). Conductor normalizes these into a small, common set of types — <code>JobProfile</code>, <code>JobRequest</code>, <code>JobExecution</code>, <code>JobPlacement</code>, and <code>JobScope</code> — and exposes them through one <code>OrchestrationService</code> interface per provider. Your code targets that interface; which provider actually launches the workload is a deployment-time configuration decision, not a code change.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Each provider ships as its own Element, bound behind a Guice <code>PrivateModule</code> that exposes only <code>OrchestrationService</code> to the rest of the platform. You deploy the provider Element(s) relevant to your infrastructure — you don't need Kubernetes, ECS, EdgeGap, and Multiplay support all loaded at once.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-key-features">Key Features</h2>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>One <code>OrchestrationService</code> interface across EdgeGap, AWS ECS, Kubernetes, and Unity Multiplay</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Launch, poll, list, and stop jobs (game server instances) through a common request/response model</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Portable placement hints (region, IP, latitude/longitude) and scoping (namespace, cluster) that each provider honors where it has an equivalent concept, and silently ignores where it doesn't</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Live stdin/stdout/stderr streaming for a running job via <code>streamStdio</code> — native on Kubernetes, and available on EdgeGap and ECS through a small sidecar (see <a href="streaming-job-stdio-in-namazu-conductor">Streaming Job Stdio in Namazu Conductor</a>)</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>A superuser-only cross-provider admin REST API and dashboard panel for listing, launching, and stopping jobs (see <a href="namazu-conductor-admin-api">Namazu Conductor Admin API</a>)</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-module-structure">Module Structure</h2>
<!-- /wp:heading -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th>Module</th><th>Purpose</th></tr></thead><tbody><tr><td><code>api</code></td><td>Core interfaces and data types: <code>OrchestrationService</code>, <code>JobRequest</code>, <code>JobExecution</code>, <code>JobProfile</code>, <code>JobPlacement</code>, <code>JobScope</code>, <code>JobStatus</code>, <code>JobStdio</code></td></tr><tr><td><code>edgegap</code></td><td>EdgeGap REST API v1 implementation</td></tr><tr><td><code>ecs</code></td><td>AWS ECS implementation (AWS SDK v2; Fargate and EC2 launch types)</td></tr><tr><td><code>kubernetes</code></td><td>Kubernetes implementation (Fabric8 client; maps <code>PodTemplate</code> resources to profiles, launching <code>Pod</code> or <code>batch/v1 Job</code> workloads)</td></tr><tr><td><code>multiplay</code></td><td>Unity Multiplay implementation (fleet allocations via the Unity Services API)</td></tr><tr><td><code>admin</code></td><td>Superuser REST API and dashboard panel for listing/launching/stopping jobs across every deployed provider Element</td></tr><tr><td><code>debug</code></td><td>Local runner — boots a local MongoDB replica set, then the Elements runtime with whichever provider Elements you've configured</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:paragraph -->
<p>You typically deploy <code>admin</code> alongside whichever provider module(s) match your infrastructure. None of the modules depend on each other at runtime beyond sharing the <code>api</code> module's types.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-core-concepts">Core Concepts</h2>
<!-- /wp:heading -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-jobprofile">JobProfile</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>A <code>JobProfile</code> is a provider-specific job template, identified by a string <code>id</code>. What backs a profile differs per provider: an EdgeGap app + version, an ECS task definition family, a Kubernetes <code>PodTemplate</code>, or a Multiplay fleet + build configuration. Call <code>OrchestrationService.getAvailableProfiles()</code> to discover the profiles a given provider currently exposes, or <code>findAvailableProfile(id)</code> to look up one by id.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-jobrequest-and-jobexecution">JobRequest and JobExecution</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Calling <code>OrchestrationService.execute(request: JobRequest)</code> launches a job. A <code>JobRequest</code> carries:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><code>profile</code> — the <code>JobProfile</code> to launch</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>command</code> / <code>args</code> — optional command and argument overrides (provider support varies — see <a href="configuring-namazu-conductor-providers">Configuring Namazu Conductor Providers</a>)</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>environment</code> — optional environment variable overrides</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>placement</code> — a list of <code>JobPlacement</code> hints</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>scope</code> — a list of <code>JobScope</code> overrides</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p><code>execute()</code> returns a <code>JobExecution</code>: an <code>id</code>, a <code>status</code> (see below), a list of <code>endpoints</code> the job is reachable on (each a host/port/protocol triple), and an opaque provider-specific <code>details</code> payload. Use <code>getFutureForStatus()</code> / <code>getStageForStatus()</code> to wait for a job to reach a particular <code>JobStatus</code>, <code>listExecutions()</code> to see everything currently tracked, and <code>stop()</code> to terminate a job.</p>
<!-- /wp:paragraph -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"Note"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="3b0649"><div class="gb-notice-title" style="color:#fff"><p>Note</p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p>Hold on to the exact <code>JobExecution</code> returned by <code>execute()</code> (or one derived from it via <code>getFutureForStatus</code>/<code>getStageForStatus</code>) if you plan to call <code>streamStdio()</code> later. On EdgeGap and ECS, the credential needed to open a stdio session travels on that object's <code>details</code> field and isn't recoverable from <code>listExecutions()</code>.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-jobstatus">JobStatus</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Every job reports one of four statuses: <code>PENDING</code> (accepted, not yet running), <code>RUNNING</code>, <code>COMPLETED</code> (exited or was stopped normally), or <code>FAILED</code>.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-jobplacement">JobPlacement</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p><code>JobPlacement</code> is a hint about where a job should run. A <code>JobRequest</code> can carry any number of them; each provider honors the ones it understands and silently ignores the rest, so the same request can be sent to multiple providers without branching your code per platform.</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><code>RegionPlacement(id)</code> — a provider-native region/zone identifier</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>IpPlacement(ip)</code> — a target IP address</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>LatitudeLongitudePlacement(latitude, longitude)</code> — a geographic coordinate</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>See <a href="configuring-namazu-conductor-providers">Configuring Namazu Conductor Providers</a> for which placement types each provider actually acts on.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-jobscope">JobScope</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p><code>JobScope</code> overrides where, within a provider, a job is launched — for providers that have such a concept.</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><code>NamespaceScope(namespace)</code> — Kubernetes; overrides the default namespace a workload is created in</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>ClusterScope(cluster)</code> — ECS; overrides the default cluster a task is launched into</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>Providers without an equivalent concept — EdgeGap, whose scoping is fully determined by the profile's app/version, and Multiplay — silently ignore any <code>JobScope</code> entries.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-related-pages">Related Pages</h2>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><a href="configuring-namazu-conductor-providers">Configuring Namazu Conductor Providers</a> — attributes, placement/scope support, and provider-specific behavior for EdgeGap, ECS, Kubernetes, and Multiplay</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="streaming-job-stdio-in-namazu-conductor">Streaming Job Stdio in Namazu Conductor</a> — how <code>streamStdio()</code> works natively on Kubernetes and via the <code>namazu-stdio-bridge</code> sidecar on EdgeGap/ECS</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="namazu-conductor-admin-api">Namazu Conductor Admin API</a> — the cross-provider superuser REST API and dashboard panel</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->
