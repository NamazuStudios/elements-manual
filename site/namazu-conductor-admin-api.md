<h1>Namazu Conductor Admin API</h1>

<!-- wp:paragraph -->
<p>The <code>admin</code> module of Namazu Conductor exposes a small REST API and a dashboard panel for listing, launching, and stopping jobs across <strong>every</strong> deployed provider Element at once — you don't need to know which specific provider (EdgeGap, ECS, Kubernetes, Multiplay) a given job set lives on to manage it from here.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Deploy the <code>admin</code> Element alongside whichever provider Element(s) you're running. It discovers them at request time via the Elements registry — no static configuration links it to a specific provider.</p>
<!-- /wp:paragraph -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"Note"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="3b0649"><div class="gb-notice-title" style="color:#fff"><p>Note</p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p>Every endpoint on this API requires an authenticated session at <code>SUPERUSER</code> level. Anything less returns <code>403 Forbidden</code>.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"anchor":"h-endpoints"} -->
<h2 id="h-endpoints" class="wp-block-heading">Endpoints</h2>
<!-- /wp:heading -->

<!-- wp:heading {"level":3,"anchor":"h-get-jobs"} -->
<h3 id="h-get-jobs" class="wp-block-heading">GET /jobs</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Returns a point-in-time snapshot of executions from every deployed <code>OrchestrationService</code> provider Element. Providers that fail to respond are still included, with a non-null <code>error</code> field instead of an execution list:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>{
  "status": "ok",
  "providers": &#91;
    {
      "element": "dev.getelements.conductor.kubernetes",
      "executions": &#91; { "id": "...", "status": "RUNNING", "endpoints": &#91; ... ] } ],
      "error": null
    },
    {
      "element": "dev.getelements.conductor.ecs",
      "executions": null,
      "error": "Timed out calling ECS"
    }
  ]
}</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p><code>status</code> summarizes the response: <code>ok</code> if every provider responded without error, <code>partial</code> if at least one did, <code>error</code> if none did. If no <code>OrchestrationService</code> providers are deployed at all, the call returns <code>503 Service Unavailable</code> instead of an empty list.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3,"anchor":"h-post-jobs"} -->
<h3 id="h-post-jobs" class="wp-block-heading">POST /jobs</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Dispatches a job to a specific provider element using one of its available profile ids. Request body:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>{
  "element": "dev.getelements.conductor.kubernetes",
  "profileId": "my-game-server-template",
  "args": &#91; "..." ],
  "command": &#91; "..." ],
  "environment": { "KEY": "value" },
  "placement": &#91; { "type": "REGION", "id": "us-east" } ]
}</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>On success, returns the <code>JobExecution</code> the provider handed back — typically <code>PENDING</code> at this point, since <code>execute()</code> returns immediately rather than waiting for the job to start. Returns <code>404</code> if the named element doesn't exist, doesn't expose <code>OrchestrationService</code>, or the profile id isn't one of its available profiles. Returns <code>500</code> if the provider accepted the request but the underlying execution call failed.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3,"anchor":"h-post-jobs-stop"} -->
<h3 id="h-post-jobs-stop" class="wp-block-heading">POST /jobs/stop</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Stops a running job. Request body:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>{
  "element": "dev.getelements.conductor.kubernetes",
  "id": "<job execution="" id="">"
}</job></code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>Returns <code>204 No Content</code> once the stop signal has been sent. Returns <code>404</code> if the named element doesn't exist or doesn't expose <code>OrchestrationService</code>, and <code>500</code> if the provider returns an error while stopping the job.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"anchor":"h-dashboard-panel"} -->
<h2 id="h-dashboard-panel" class="wp-block-heading">Dashboard Panel</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The <code>admin</code> Element also ships a "Conductor" entry in the superuser dashboard sidebar, giving you the same list/launch/stop functionality as the REST API above through a UI, without needing to script requests by hand.</p>
<!-- /wp:paragraph -->
