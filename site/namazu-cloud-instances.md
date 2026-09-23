<h1>Namazu Cloud Instances</h1>

<!-- wp:paragraph -->
<p>An instance is a dedicated, hosted run of Elements provisioned for an organization. Create it from a product version and a SKU, attach a subdomain your organization has reserved, and Namazu Cloud deploys the infrastructure behind it. The instance page shows its status, its nodes, an audit log of every action, and the controls to start, stop, upgrade, or destroy it.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"anchor":"h-creating-an-instance"} -->
<h2 id="h-creating-an-instance" class="wp-block-heading">Creating an Instance</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Creating an instance requires a few things to be true first:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Your organization has <a href="namazu-cloud-billing">billing set up</a> with a payment method on file.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Your organization holds a <a href="namazu-cloud-subdomains">reserved subdomain</a> that is not attached to another instance.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>There is a <a href="namazu-cloud-products-and-skus">product version and a SKU</a> available, with availability zones that suit your needs.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>With those in place, fill in the create form:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>Name</strong>: your label for the instance.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Product version</strong>: which version of which product to deploy. Version availability is shown from the catalog.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>SKU</strong>: how the instance is billed. A SKU lists the availability zones it supports; pick from those.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Subdomain</strong>: a reserved subdomain from your organization's pool, shown as <code>label.cloud.namazustudios.com</code>. This is where the live instance is reachable.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Availability zones</strong>: the zones the instance's infrastructure is placed in.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Default user</strong>: the user name, email, and password seeded into the deployed instance for first login. The password is write-only and is never returned by the server.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>Then, while provisioning runs, watch the instance move through its creation statuses (see below). Deployment is asynchronous: the instance is updated by the runner as it proceeds, and the portal shows a progress spinner while work is in flight.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"anchor":"h-statuses"} -->
<h2 id="h-statuses" class="wp-block-heading">Statuses</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Every instance reports a single status, and the portal never guesses: it shows the last confirmed state. Statuses fall into a few groups:</p>
<!-- /wp:paragraph -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th>Group</th><th>Statuses</th><th>Meaning</th></tr></thead><tbody><tr><td>Dormant</td><td><code>READY</code></td><td>Created in Namazu Cloud but never deployed. Also the position an instance returns to conceptually when stopped.</td></tr><tr><td>Pending</td><td><code>CREATE_PENDING</code>, <code>START_PENDING</code>, <code>STOP_PENDING</code>, <code>PENDING_UPGRADE</code>, <code>PENDING_CHANGE_SKU</code>, <code>PENDING_DESTROY</code></td><td>An action has been requested and is next in line to run.</td></tr><tr><td>In progress</td><td><code>CREATING</code>, <code>STARTING</code>, <code>STOPPING</code>, <code>UPGRADING</code>, <code>CHANGING_SKU</code>, <code>DESTROYING</code></td><td>The runner is actively applying the operation. The portal shows a spinner and disables dispatch buttons while these are active.</td></tr><tr><td>Running</td><td><code>STARTED</code>, <code>HEALTHY</code>, <code>DEGRADED</code></td><td>The instance is up. <code>HEALTHY</code> means its health check passes; <code>DEGRADED</code> means it is up but reporting warnings or errors.</td></tr><tr><td>Stopped</td><td><code>STOPPED</code></td><td>The instance is stopped and its infrastructure is switched off.</td></tr><tr><td>Error</td><td><code>ERROR_CREATING</code>, <code>ERROR_STARTING</code>, <code>ERROR_STOPPING</code>, <code>ERROR_UPGRADING</code>, <code>ERROR_CHANGING_SKU</code>, <code>ERROR_DESTROYING</code></td><td>The most recent operation failed. You can retry the same operation or destroy the instance; no support ticket is needed to recover.</td></tr><tr><td>Terminal</td><td><code>DESTROYED</code>, <code>FAILED</code></td><td>Final states an instance never leaves. <code>DESTROYED</code> means the instance's resources are gone. <code>FAILED</code> means the instance needs administrator intervention.</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"How to read this"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="803648"><div class="gb-notice-title" style="color:#fff"><p>How to read this</p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p>The <code>paused</code> state is not a separate value. A stopped instance is simply <code>STOPPED</code>, and starting it again moves it through <code>START_PENDING</code>, <code>STARTING</code>, and back to <code>STARTED</code> or <code>HEALTHY</code>. Each operation on an instance moves it forward along one of these paths, and an <code>ERROR_*</code> status means simply that the last operation did not complete: retry it, or destroy the instance.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"anchor":"h-lifecycle-actions"} -->
<h2 id="h-lifecycle-actions" class="wp-block-heading">Lifecycle Actions</h2>
<!-- /wp:heading -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th>Action</th><th>When it applies</th><th>What happens</th></tr></thead><tbody><tr><td>Start</td><td><code>READY</code>, <code>STOPPED</code>, and <code>ERROR_*</code> states</td><td>Brings the instance up. It transitions through <code>START_PENDING</code> then <code>STARTING</code>, lands at <code>STARTED</code>, and finally reaches <code>HEALTHY</code> once its health check passes.</td></tr><tr><td>Stop</td><td><code>STARTED</code>, <code>HEALTHY</code>, <code>DEGRADED</code></td><td>Switches the instance off via <code>STOP_PENDING</code> and <code>STOPPING</code> into <code>STOPPED</code>. Stop for the long term: stopped instances still keep their subdomain and configuration.</td></tr><tr><td>Upgrade</td><td>Any running or stopped instance with a newer version available</td><td>Moves the instance to a newer version of the same product through <code>PENDING_UPGRADE</code> and <code>UPGRADING</code>, landing at <code>STARTED</code>. The product cannot be swapped, and the SKU is kept so billing stays continuous.</td></tr><tr><td>Change SKU</td><td>An instance with a compatible non-archived SKU available</td><td>Switches how the instance is billed without changing the product, via <code>PENDING_CHANGE_SKU</code> and <code>CHANGING_SKU</code>. The new SKU must match the instance's availability zones.</td></tr><tr><td>Destroy</td><td>Almost any non-terminal state</td><td>Tears the instance down through <code>PENDING_DESTROY</code> and <code>DESTROYING</code> into <code>DESTROYED</code>. This is the end of the instance's resources. Destroying an instance does not release its subdomain, which goes back to your organization's pool as <code>RESERVED</code>.</td></tr><tr><td>Delete</td><td>Any time</td><td>Archives the instance so it is hidden from listings. This is a soft delete available alongside destroy.</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"Note"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="3b0649"><div class="gb-notice-title" style="color:#fff"><p>Note</p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p>Three fields are fixed at creation and cannot be changed later: the product version's product, the subdomain, and the availability zones. An update can only rename the instance and adjust display-level configuration. To change the deployed version, use Upgrade rather than trying to edit the instance.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"anchor":"h-provisioning"} -->
<h2 id="h-provisioning" class="wp-block-heading">Provisioning</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>While any lifecycle action is in flight, the instance carries a <em>provisioning</em> flag. The portal reads that flag to show a spinner with elapsed time and to disable every dispatch button, because only one operation may run against an instance at a time. When the operation finishes, the flag clears. If an operation cannot even be dispatched, the attempt is recorded on the audit log with a dispatch failure and the instance is left in its previous state, so you can simply retry.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"anchor":"h-audit-log"} -->
<h2 id="h-audit-log" class="wp-block-heading">Audit Log</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Every instance keeps a paginated audit log of the actions taken against it: creation, start, stop, upgrade, SKU change, destroy, template and secret operations, billing events, and dispatch failures or status resets. You can filter it by action and by time window, which is handy when diagnosing when an instance last changed and why.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"anchor":"h-nodes"} -->
<h2 id="h-nodes" class="wp-block-heading">Nodes</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>An instance is backed by cloud resources. The instance page lists those nodes, each tagged <code>ACTIVE</code>, <code>DESTROYED</code>, or <code>TIMED_OUT</code>. Nodes that fail to report their <a href="namazu-cloud-backups">backup</a> or billing state in time are marked <code>TIMED_OUT</code> so you can tell at a glance whether the instance's infrastructure is healthy end to end.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"anchor":"h-related-pages"} -->
<h2 id="h-related-pages" class="wp-block-heading">Related Pages</h2>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><a href="namazu-cloud-overview">Namazu Cloud Overview</a></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="namazu-cloud-products-and-skus">Namazu Cloud Products and SKUs</a>. Picking what to deploy and how it is billed</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="namazu-cloud-subdomains">Namazu Cloud Subdomains</a>. Reserving the hostname an instance attaches at creation</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="namazu-cloud-billing">Namazu Cloud Billing</a>. The gate that applies before an instance can be created</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="namazu-cloud-job-and-service-templates">Namazu Cloud Job and Service Templates</a> and <a href="namazu-cloud-secrets">Namazu Cloud Secrets</a>. Workloads that run in an instance's namespace</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->
