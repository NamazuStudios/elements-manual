<h1>Health Checks and Mission Critical Deployments</h1>

<!-- wp:paragraph -->
<p>Elements exposes a health check endpoint that infrastructure probes, load balancers, and orchestrators can use to decide whether an instance is fit to receive traffic.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="the-health-endpoint">The health endpoint</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The endpoint is a plain <code>GET</code> against <code>/api/rest/health</code>. It requires no authentication, so any load balancer or uptime monitor can reach it directly.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>What the response contains depends on who is asking. Callers with <code>SUPERUSER</code> access receive the full health status document, which lists each individual check, any problems detected, and the health of every database. Every other caller, including anonymous callers, receives only a bare indication of health:</p>
<!-- /wp:paragraph -->

<!-- wp:table -->
<figure class="wp-block-table"><table><tbody><tr><th>Caller</th><th>Healthy</th><th>Unhealthy</th></tr><tr><td><code>SUPERUSER</code></td><td><code>200</code> with the full health status as JSON</td><td><code>500</code> with a health error document describing the failures</td></tr><tr><td>Anyone else</td><td><code>200</code> with the plain text body <code>OK</code></td><td><code>503</code> with the plain text body <code>Unhealthy</code></td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:paragraph -->
<p>This split exists so that a probe can decide whether to take an instance out of rotation, without exposing the internals of the instance to untrusted callers. A bare <code>200 OK</code> means the instance can serve requests. A <code>503</code> means it has internal problems and should be taken offline. Only superusers can see why.</p>
<!-- /wp:paragraph -->

<!-- wp:quote -->
<blockquote class="wp-block-quote"><p>If you are wiring this into a load balancer or Kubernetes probe, use the non-superuser form and treat <code>503</code> as the failure signal. The <code>500</code> returned to superusers is an error condition, not a probe result, and is not what a probe will see.</p></blockquote>
<!-- /wp:quote -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="what-affects-health">What affects health</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Out of the box the health check covers three things:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>Databases</strong>: the health of each database the instance is connected to.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Mission critical deployments</strong>: whether any deployment flagged as mission critical failed to load.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Service discovery</strong>: the instance hosts known to the discovery service.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>The instance is considered healthy only when every check passes. Because the checks are extensible, a plugin can contribute its own, and a failing check contributed by an Element will make the whole instance report itself unhealthy.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="mission-critical-deployments">Mission critical deployments</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>By default, a deployment which fails to load is logged but does not affect the health check, so the instance keeps serving traffic with a silently reduced feature set. If a deployment is required for the instance to function correctly, flag it as mission critical in the admin console using the <strong>Mission critical deployment</strong> checkbox in the deployment form. Flagged deployments are also marked with a <strong>mission critical</strong> badge in the deployment list. The same flag is available when writing an Element by hand, via <code>deployment.missionCritical(true)</code>. See <a href="element-anatomy-a-technical-deep-dive">Element Anatomy</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>A mission critical deployment makes the instance report itself unhealthy in either of these cases:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>The runtime fails to load the deployment at all.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Loading completes with errors, so at least one Element in the deployment did not load.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>Deployments that load cleanly, or that load with warnings only, are treated as healthy. Deployments which are not mission critical never affect the health check regardless of their state.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>This is most useful in a multi-instance deployment. Marking a core deployment as mission critical means an instance which cannot load it is taken out of the load balancer rather than left in rotation advertising features it cannot actually provide.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="using-it-from-the-rest-api">Using it from the REST API</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The flag is part of the standard deployment model, so it can be set and read like any other deployment field:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>// Create a deployment flagged as mission critical
curl -X POST http://localhost:8080/api/rest/elements/deployment \
  -H "Elements-SessionSecret: $SESSION_SECRET" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "core-payments",
    "missionCritical": true,
    "state": "ENABLED"
  }'</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>The <code>missionCritical</code> field defaults to <code>false</code> and can be changed at any time with an update request. The health check evaluates the flag against the deployment's current runtime state, so marking an already healthy deployment as mission critical does not make the instance unhealthy. The instance only reports itself unhealthy once a mission critical deployment is actually in a failed or unstable state.</p>
<!-- /wp:paragraph -->
