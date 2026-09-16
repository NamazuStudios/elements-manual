<h1>Application CDN Git Deployment</h1>

<!-- wp:paragraph -->
<p>Namazu Elements includes a git-push deployment pipeline for publishing an entire static site (a single-page app, a marketing page, downloadable assets, and so on) under an Application, independent of any single Element. You push a git repository, tell Elements which revision to publish, and it serves that revision's file tree over HTTP.</p>
<!-- /wp:paragraph -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"Note"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="app-cdn-1"><div class="gb-notice-title" style="color:#fff"><p>Note</p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p>This is a different system from <a href="element-static-content-and-dashboard-ui-plugins">Element Static Content and Dashboard UI Plugins</a> (which serves files bundled inside a specific Element's <code>.elm</code> archive) and from the <a href="large-object-api">Large Object API</a> (which stores individual files as first-class database records). All three happen to live under a <code>/cdn</code>-ish or <code>/app</code>-ish URL depending on the system, but they are separate storage mechanisms serving different use cases.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-how-it-works">How It Works</h2>
<!-- /wp:heading -->

<!-- wp:list {"ordered":true} -->
<ol class="wp-block-list"><!-- wp:list-item -->
<li>You push a git repository containing your static site to an Application-scoped git endpoint over HTTP.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>You call the Deployment REST API with the git commit SHA (the "revision") you want to publish.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Elements checks out that revision's file tree into local storage and atomically points a version symlink at it.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Requests to the serving endpoint for that Application and version are answered from the checked-out tree.</li>
<!-- /wp:list-item --></ol>
<!-- /wp:list -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-pushing-content">Pushing Content</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Every Application has its own git remote at <code>/cdn/git/{applicationName}.git</code>, backed by a standard git-over-HTTP smart transport and protected with HTTP Basic authentication. Point a normal git remote at it and push:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>git remote add elements-cdn https://{username}:{password}@{host}/cdn/git/{applicationName}.git
git push elements-cdn main</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>Pushing content does not publish it by itself; it only makes that commit available for Elements to check out. Publishing happens in the next step.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-publishing-a-deployment">Publishing a Deployment</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>A <strong>Deployment</strong> ties a version label to a git revision for a given Application. Deployments are managed through the Deployment REST resource:</p>
<!-- /wp:paragraph -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th>Method</th><th>Path</th><th>Description</th></tr></thead><tbody><tr><td><code>POST</code></td><td><code>/deployment/{applicationId}</code></td><td>Creates a new Deployment for the Application from a given <code>revision</code> (git commit SHA).</td></tr><tr><td><code>PUT</code></td><td><code>/deployment/{applicationId}/{version}</code></td><td>Updates an existing version to point at a new <code>revision</code>.</td></tr><tr><td><code>GET</code></td><td><code>/deployment/{applicationId}</code></td><td>Gets the Application's current Deployment.</td></tr><tr><td><code>DELETE</code></td><td><code>/deployment/{applicationId}/{version}</code></td><td>Removes a Deployment and its checked-out content.</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:paragraph -->
<p>A Deployment record has an <code>id</code>, a <code>version</code>, the <code>revision</code> it was published from, and the owning <code>application</code>. Creating or updating a Deployment (and deleting one) requires Superuser access. Unauthenticated callers may only read the current Deployment; they cannot create, update, or delete one.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-how-content-is-stored-and-served">How Content Is Stored and Served</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>When a Deployment is created or updated, Elements clones the repository content at the given revision into a uniquely named directory, then atomically creates (or repoints) a symlink named after the version at the serving location:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>{storage.directory}/{applicationName}/{clone.endpoint}/{uuid}/...           &lt;- checked-out revision content
{storage.directory}/{applicationName}/{serve.endpoint}/{version} -&gt; ...{uuid}   &lt;- symlink used for serving</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>Deleting a Deployment removes both the symlink and the underlying checked-out directory tree. Published content is served publicly at:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>/cdn/static/app/{applicationName}/{serve.endpoint}/{version}/{path...}</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>The serving endpoint resolves the Application from the URL, refuses to serve any path that would resolve outside that Application's own content directory, and supports conditional requests (<code>ETag</code> / <code>If-None-Match</code>) with a configurable public cache lifetime.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-configuration">Configuration</h2>
<!-- /wp:heading -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th>Attribute</th><th>Default</th><th>Purpose</th></tr></thead><tbody><tr><td><code>dev.getelements.elements.cdnserve.storage.directory</code></td><td><code>content</code></td><td>Filesystem directory used to store checked-out and served content per Application.</td></tr><tr><td><code>dev.getelements.elements.cdnserve.endpoint.clone</code></td><td><code>clone</code></td><td>Subdirectory name used for checked-out revision content.</td></tr><tr><td><code>dev.getelements.elements.cdnserve.endpoint.serve</code></td><td><code>serve</code></td><td>Subdirectory name used for the version symlinks that are actually served.</td></tr><tr><td><code>dev.getelements.elements.git.cdn.storage.directory</code></td><td><code>cdn-repos/git</code></td><td>Filesystem directory used to store the bare git repositories pushed to <code>/cdn/git/*</code>.</td></tr><tr><td><code>dev.getelements.elements.cdn.public.max.age</code></td><td><code>300</code></td><td>Cache lifetime, in seconds, sent as <code>Cache-Control: public, max-age=...</code> for served content. Shared with the <a href="large-object-api">Large Object API</a>'s serving endpoint.</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->
