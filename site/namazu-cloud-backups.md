<h1>Namazu Cloud Backups</h1>

<!-- wp:paragraph -->
<p>Backups are point-in-time snapshots of an instance's storage. They give you a way back to a known-good state after an upgrade, a bad deployment, or accidental data loss. Namazu Cloud takes automatic backups on a schedule and lets you start a manual backup at any time; the Backups section of the portal is where you browse them.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-how-backups-are-made">How Backups Are Made</h2>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>Automatic</strong>: the platform takes a snapshot on the configured schedule for each instance's storage, so a healthy backup trail exists without you doing anything.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Manual</strong>: from an instance's page or the Backups section, trigger a backup now. Useful right before an upgrade or a risky change.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>Each backup records the instance it belongs to, the time it was taken, the type and cloud of the storage it snapshots, its region, and whether it was automatic or manual. A single manual request can produce one backup per storage volume on the instance.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-statuses">Statuses</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>A backup moves through these states:</p>
<!-- /wp:paragraph -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th>Status</th><th>Meaning</th></tr></thead><tbody><tr><td><code>PENDING</code></td><td>The snapshot is being taken and has not finished yet.</td></tr><tr><td><code>READY</code></td><td>The snapshot is complete and usable.</td></tr><tr><td><code>FAILED</code></td><td>The snapshot did not complete. No usable restore point exists for that attempt.</td></tr><tr><td><code>EXPIRED</code></td><td>The snapshot has been purged after its retention period.</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-browsing-backups">Browsing Backups</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The Backups section lists the backups for all of your organization's instances, newest first, and can be paginated. You can narrow the list to a single instance from that instance's page, or search by instance name. Each backup shows its instant and status at a glance.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-restoring">Restoring</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Restoration is handled by Namazu Cloud operations to the instance your backup belongs to. The restore action on a backup opens a support request noting the instance and backup, and Namazu Cloud's team performs the restore. This is deliberate: restoring an instance's storage is a careful operation on live infrastructure, so it goes through a human-run process rather than a one-click action.</p>
<!-- /wp:paragraph -->

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
<li><a href="namazu-cloud-instances">Namazu Cloud Instances</a>. Backups belong to an instance</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="namazu-cloud-billing">Namazu Cloud Billing</a>. Backup retention can be a metered unit on your SKU</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->