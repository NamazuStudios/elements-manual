<h1>Configuring Namazu Conductor Providers</h1>

<!-- wp:paragraph -->
<p>Each Namazu Conductor provider Element is configured independently via attributes, and each honors a different subset of the common <a href="namazu-conductor">core concepts</a> — profiles come from a different provider-native resource, and placement/scope hints are only applied where the provider has an equivalent concept. This page covers EdgeGap, AWS ECS, Kubernetes, and Unity Multiplay.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-placement-and-scope-support">Placement and Scope Support</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>You can send the same <code>JobRequest</code> — including any combination of <code>JobPlacement</code> and <code>JobScope</code> entries — to every provider you have deployed. Each one only acts on what it understands:</p>
<!-- /wp:paragraph -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th>Provider</th><th><code>RegionPlacement</code></th><th><code>IpPlacement</code></th><th><code>LatitudeLongitudePlacement</code></th><th><code>NamespaceScope</code></th><th><code>ClusterScope</code></th><th><code>command</code> / <code>args</code> override</th></tr></thead><tbody><tr><td>EdgeGap</td><td>Ignored</td><td>Honored (<code>ip_list</code>)</td><td>Honored (<code>geo_ip_list</code>)</td><td>Ignored</td><td>Ignored</td><td>Not supported by the EdgeGap deploy API</td></tr><tr><td>ECS</td><td>Ignored — placement is governed by the configured subnets/security groups (<code>awsvpc</code>) or the target container instance (EC2)</td><td>Ignored</td><td>Ignored</td><td>Ignored</td><td>Honored — overrides the cluster a task is launched into</td><td>Honored (container override on the profile's primary container)</td></tr><tr><td>Kubernetes</td><td>Honored — sets a <code>topology.kubernetes.io/zone</code> node selector</td><td>Ignored</td><td>Ignored</td><td>Honored — overrides where the workload (and any Service) is created</td><td>Ignored</td><td>Honored (container override)</td></tr><tr><td>Multiplay</td><td>Honored — the first entry's <code>id</code> is sent as the allocation's <code>regionId</code></td><td>Ignored</td><td>Ignored</td><td>Ignored</td><td>Ignored</td><td>Not supported by the Multiplay allocation API</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-edgegap">EdgeGap</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The EdgeGap provider talks to the EdgeGap REST API v1. A <code>JobProfile</code> corresponds to one active app version (<code>&lt;appName&gt;:&lt;versionName&gt;</code>); <code>getAvailableProfiles()</code> lists every active version across every app visible to your API key.</p>
<!-- /wp:paragraph -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th>Attribute</th><th>Default</th><th>Description</th></tr></thead><tbody><tr><td><code>dev.getelements.conductor.edgegap.api.key</code></td><td><em>none — required</em></td><td>EdgeGap API key, sent as the bare <code>Authorization</code> header value. Marked sensitive.</td></tr><tr><td><code>dev.getelements.conductor.edgegap.base.url</code></td><td><code>https://api.edgegap.com</code></td><td>EdgeGap API base URL. Override for a regional mirror or a test environment.</td></tr><tr><td><code>dev.getelements.conductor.edgegap.stdio.bridge.port</code></td><td><code>10080</code></td><td>Port the <code>namazu-stdio-bridge</code> sidecar listens on, if your app version's image includes one. See <a href="streaming-job-stdio-in-namazu-conductor">Streaming Job Stdio in Namazu Conductor</a>.</td></tr><tr><td><code>dev.getelements.conductor.edgegap.stdio.bridge.base.path</code></td><td><em>(empty)</em></td><td>Base path prefix for the bridge's WebSocket endpoints. Must match the bridge's own <code>NAMAZU_CONDUCTOR_STDIO_URI</code>.</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:paragraph -->
<p>Status polling runs against <code>GET /v1/status/{request_id}</code> every 5 seconds (fixed, not currently configurable). EdgeGap's own status strings are mapped onto <code>JobStatus</code> as: anything ending in <code>INITIALIZING</code> or <code>WAITING</code> → <code>PENDING</code>; <code>RUNNING</code> → <code>RUNNING</code>; <code>TERMINATED</code>/<code>TERMINATING</code> → <code>COMPLETED</code>; anything else → <code>FAILED</code>.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-aws-ecs">AWS ECS</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The ECS provider talks to AWS ECS via the AWS SDK v2. A <code>JobProfile</code> corresponds to one active task definition family; <code>getAvailableProfiles()</code> only surfaces families tagged <code>namazu.conductor:jobSet=&lt;value&gt;</code> matching your configured job set.</p>
<!-- /wp:paragraph -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th>Attribute</th><th>Default</th><th>Description</th></tr></thead><tbody><tr><td><code>dev.getelements.conductor.ecs.region</code></td><td><em>none — required</em></td><td>AWS region the cluster lives in (e.g. <code>us-east-1</code>).</td></tr><tr><td><code>dev.getelements.conductor.ecs.cluster</code></td><td><em>none — required</em></td><td>Default cluster name or ARN tasks launch into. Overridable per request via <code>ClusterScope</code>.</td></tr><tr><td><code>dev.getelements.conductor.ecs.subnets</code></td><td><em>none</em></td><td>Comma-separated subnet IDs. Required for task definitions using <code>awsvpc</code> network mode.</td></tr><tr><td><code>dev.getelements.conductor.ecs.security.groups</code></td><td><em>none</em></td><td>Comma-separated security group IDs. Required for <code>awsvpc</code> task definitions.</td></tr><tr><td><code>dev.getelements.conductor.ecs.job.set</code></td><td><code>default</code></td><td>Only task definition families tagged <code>namazu.conductor:jobSet=&lt;value&gt;</code> matching this are surfaced as profiles.</td></tr><tr><td><code>dev.getelements.conductor.ecs.stdio.bridge.port</code></td><td><code>10080</code></td><td>Port the <code>namazu-stdio-bridge</code> sidecar listens on, if your task definition's image includes one.</td></tr><tr><td><code>dev.getelements.conductor.ecs.stdio.bridge.base.path</code></td><td><em>(empty)</em></td><td>Base path prefix for the bridge's WebSocket endpoints.</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-fargate-vs-ec2">Fargate vs. EC2 launch type</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>There's no separate attribute to pick Fargate vs. EC2 — the launch type is read per task definition family from an ECS resource tag, <code>namazu.conductor:launchType</code>. If the tag is absent, the family defaults to Fargate. Similarly, whether a launched task gets a public IP is controlled by the <code>namazu.conductor:assignPublicIp</code> tag (default: disabled).</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Subnet/security group/public-IP configuration is only applied when the task definition's network mode is <code>awsvpc</code>. For EC2 tasks running in bridge network mode, that configuration is skipped entirely and Conductor instead resolves the job's reachable host/endpoints from the underlying container instance's EC2 IP.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-clusterscope-and-task-tracking">ClusterScope and task tracking</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>When a request's <code>ClusterScope</code> overrides the configured default cluster, ECS task ARNs (<code>arn:aws:ecs:&lt;region&gt;:&lt;account&gt;:task/&lt;cluster&gt;/&lt;task-id&gt;</code>) carry the cluster name, so <code>stop()</code> and status polling can recover it without Conductor persisting any extra state. The one exception is <code>listExecutions()</code>, which always queries the <em>configured default</em> cluster — tasks launched into a cluster-scoped override won't appear there.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-kubernetes">Kubernetes</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The Kubernetes provider maps <code>PodTemplate</code> resources to <code>JobProfile</code>s using Fabric8. Client configuration comes from Fabric8's auto-detection (in-cluster service account, then <code>~/.kube/config</code>) unless <code>KUBECONFIG_PATH</code> / <code>MASTER_URL</code> are set.</p>
<!-- /wp:paragraph -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th>Attribute</th><th>Default</th><th>Description</th></tr></thead><tbody><tr><td><code>dev.getelements.conductor.kubernetes.namespace</code></td><td><code>default</code></td><td>Namespace <code>PodTemplate</code>s are discovered in and workloads are created in by default. Overridable per request via <code>NamespaceScope</code>.</td></tr><tr><td><code>dev.getelements.conductor.kubernetes.job.set</code></td><td><code>default</code></td><td>Only <code>PodTemplate</code>s labeled <code>namazu.conductor/job-set=&lt;value&gt;</code> matching this are surfaced as profiles.</td></tr><tr><td><code>dev.getelements.conductor.kubernetes.kubeconfig.path</code></td><td><em>(empty)</em></td><td>Optional path to a kubeconfig file, overriding Fabric8 auto-detection.</td></tr><tr><td><code>dev.getelements.conductor.kubernetes.master.url</code></td><td><em>(empty)</em></td><td>Optional Kubernetes API server URL override.</td></tr><tr><td><code>dev.getelements.conductor.kubernetes.poll.interval.ms</code></td><td><code>5000</code></td><td>Polling interval, in milliseconds, while waiting for a target status — used unless watches are enabled below.</td></tr><tr><td><code>dev.getelements.conductor.kubernetes.watch.enabled</code></td><td><code>false</code></td><td>When <code>true</code>, status transitions are observed via a Kubernetes watch on the Pod/Job instead of polling. Falls back to polling if the watch closes with an error before reaching a terminal status.</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:paragraph -->
<p>Behavior is otherwise driven entirely by labels and annotations on the <code>PodTemplate</code> itself:</p>
<!-- /wp:paragraph -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th>Label / Annotation</th><th>Values</th><th>Effect</th></tr></thead><tbody><tr><td><code>namazu.conductor/job-set</code> (label)</td><td>string</td><td>Must match the configured job set for the template to be surfaced as a profile.</td></tr><tr><td><code>namazu.conductor/workload-kind</code></td><td><code>pod</code> (default) or <code>job</code></td><td><code>pod</code> launches a long-standing, bare <code>Pod</code>; <code>job</code> launches a one-off <code>batch/v1 Job</code>. Pod phases / Job status map onto <code>JobStatus</code> accordingly.</td></tr><tr><td><code>namazu.conductor/expose-ports</code></td><td>e.g. <code>"7777/udp,8080/tcp"</code></td><td>If present, a <code>Service</code> selecting the workload is created. If absent, no <code>Service</code> is created and endpoints fall back to the pod IP.</td></tr><tr><td><code>namazu.conductor/service-type</code></td><td><code>NodePort</code> (default), <code>LoadBalancer</code>, or <code>ClusterIP</code></td><td>Type of the created <code>Service</code>, when one is created.</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:paragraph -->
<p>The following annotations apply only when <code>namazu.conductor/workload-kind: job</code>. Each is an optional integer string; if absent, the field is omitted and the Kubernetes default applies; if invalid (non-numeric or negative), a warning is logged and the field is likewise omitted.</p>
<!-- /wp:paragraph -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th>Annotation</th><th>Job field</th><th>Kubernetes default</th></tr></thead><tbody><tr><td><code>namazu.conductor/ttl-seconds-after-finished</code></td><td><code>spec.ttlSecondsAfterFinished</code></td><td>none</td></tr><tr><td><code>namazu.conductor/backoff-limit</code></td><td><code>spec.backoffLimit</code></td><td>6</td></tr><tr><td><code>namazu.conductor/active-deadline-seconds</code></td><td><code>spec.activeDeadlineSeconds</code></td><td>none</td></tr><tr><td><code>namazu.conductor/completions</code></td><td><code>spec.completions</code></td><td>1</td></tr><tr><td><code>namazu.conductor/parallelism</code></td><td><code>spec.parallelism</code></td><td>1</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:paragraph -->
<p>Created <code>Service</code>s carry a <code>namazu.conductor/owned-by=&lt;workload-name&gt;</code> label, which is how <code>stop()</code> finds and deletes them without Conductor persisting any extra state of its own.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><code>streamStdio()</code> is native here — it attaches to the pod's first container the same way <code>kubectl attach</code> does. The pod must be in the <code>Running</code> phase; otherwise the call throws.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-unity-multiplay">Unity Multiplay</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The Multiplay provider authenticates against Unity's Services API using a service account (key ID + secret), exchanging it for an access token that's cached for 55 minutes (tokens themselves expire after an hour). A <code>JobProfile</code> corresponds to one fleet + build configuration pair; <code>getAvailableProfiles()</code> lists every pair referenced across your project's fleets and environments.</p>
<!-- /wp:paragraph -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th>Attribute</th><th>Default</th><th>Description</th></tr></thead><tbody><tr><td><code>dev.getelements.conductor.multiplay.key.id</code></td><td><em>(empty) — required</em></td><td>Unity service account key ID.</td></tr><tr><td><code>dev.getelements.conductor.multiplay.key.secret</code></td><td><em>(empty) — required</em></td><td>Unity service account key secret.</td></tr><tr><td><code>dev.getelements.conductor.multiplay.project.id</code></td><td><em>(empty) — required</em></td><td>Unity project ID (GUID) that owns the Multiplay resources.</td></tr><tr><td><code>dev.getelements.conductor.multiplay.environment.id</code></td><td><em>(empty) — required</em></td><td>Unity environment ID (GUID) within the project, e.g. production or staging.</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:paragraph -->
<p>Executing a job creates a Multiplay allocation. Job status is derived from the allocation's own status: <code>ALLOCATED</code> → <code>RUNNING</code>, <code>FAILED</code> → <code>FAILED</code>, <code>CANCELLED</code> → <code>COMPLETED</code>, anything else → <code>PENDING</code>. Multiplay's allocation API has no container-exec equivalent, so <code>streamStdio()</code> isn't supported on this provider.</p>
<!-- /wp:paragraph -->
