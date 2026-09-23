<h1>Namazu Cloud Job and Service Templates</h1>

<!-- wp:paragraph -->
<p>Job and service templates let you run extra workloads inside an instance's isolated namespace, alongside the instance itself. A job template describes work that runs and then finishes; a service template describes something that keeps running and can be exposed on a network port. Both are defined and managed per instance from the instance page.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-jobs-and-services">Jobs and Services</h2>
<!-- /wp:heading -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th></th><th>Job template</th><th>Service template</th></tr></thead><tbody><tr><td>Runs</td><td>To completion, then stops</td><td>Continuously</td></tr><tr><td>Typical use</td><td>One-off tasks: migrations, seed scripts, batch processing, report generation</td><td>Long-running workloads: background workers, matchmaking helpers, custom services your game needs</td></tr><tr><td>Key settings</td><td>Image, command and arguments, environment, <a href="namazu-cloud-secrets">secret-backed</a> environment, retry on failure</td><td>Image, command and arguments, environment, secret-backed environment, ports, visibility, instance count, CPU autoscale target</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-defining-a-template">Defining a Template</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>A template names a container image and, optionally, overrides its command and arguments. Both template kinds accept environment variables. The environment can draw values from two places:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Literal values entered in the template, and</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Injected values from a <a href="namazu-cloud-secrets">secret</a>, referenced by secret name and key. The value never appears in the template itself; it is supplied to the workload when it launches.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>A job template can be marked to retry on failure, so a temporary error does not fail the job permanently.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-exposing-a-service">Exposing a Service</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>A service template declares one or more ports, each with a protocol (<code>TCP</code> or <code>UDP</code>), and a visibility:</p>
<!-- /wp:paragraph -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th>Visibility</th><th>Meaning</th></tr></thead><tbody><tr><td><code>PRIVATE</code></td><td>Only reachable inside the instance's network; the default.</td></tr><tr><td><code>SHARED</code></td><td>Reachable on a shared hostname across deployments.</td></tr><tr><td><code>DEDICATED</code></td><td>Given its own dedicated hostname for this instance.</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:paragraph -->
<p>A service also has a desired instance count. When capacity targets are configured, the service scales automatically: give it minimum and maximum counts and a CPU target, and the platform adjusts the running count to keep its CPU usage under the target.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-managing-templates">Managing Templates</h2>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Anyone in the organization's membership can read the templates defined for an instance.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Creating, updating (replacing the definition), and deleting a template requires an organization owner or administrator.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Template operations are recorded on the instance's <a href="namazu-cloud-instances">audit log</a>, so it is always possible to see who changed a workload and when.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"Note"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="3b0649"><div class="gb-notice-title" style="color:#fff"><p>Note</p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p>Job and service template management is part of the Namazu Cloud platform and may not be enabled in every environment yet. If the template controls do not appear on an instance, the feature has not been unlocked for that deployment.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-related-pages">Related Pages</h2>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><a href="namazu-cloud-overview">Namazu Cloud Overview</a></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="namazu-cloud-secrets">Namazu Cloud Secrets</a>. Supplying values to your templates without exposing them in the definition</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="namazu-cloud-instances">Namazu Cloud Instances</a>. Where templates live and are audited</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->