<h1>Element Static Content and Dashboard UI Plugins</h1>

<!-- wp:paragraph -->
<p>Beyond the <code>api/</code>, <code>lib/</code>, <code>spi/</code>, and <code>classpath/</code> trees covered in <a href="element-anatomy-a-technical-deep-dive">Element Anatomy: A Technical Deep Dive</a> and <a href="packaging-an-element-with-maven">Packaging an Element with Maven</a>, an Element can ship two additional content trees inside its <code>.elm</code> archive (or exploded directory): <code>static/</code> and <code>ui/</code>. Both are served over HTTP automatically when the Element is deployed, but they serve different purposes:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><code>static/</code> - arbitrary static assets bundled with the Element (a small SPA, JSON configuration, icons, downloadable files).</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>ui/</code> - dashboard UI plugin bundles, which extend the Elements admin console with custom pages.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>This page documents how both trees get loaded and served at runtime. For how they're populated during a Maven build, see <a href="packaging-an-element-with-maven">Packaging an Element with Maven</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"anchor":"h-loading-static-content"} -->
<h2 id="h-loading-static-content" class="wp-block-heading">Loading Static Content</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>When an Element is deployed, the loader reads its <code>static/</code> and <code>ui/</code> directories (if present) into an in-memory file listing before anything is served. This works identically whether the Element is deployed as an exploded directory on the element path or as a packaged <code>.elm</code> archive; the archive is simply opened as a zip filesystem and walked the same way as a real directory. An Element with neither directory simply serves nothing under either mount, which is why both trees are optional.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"anchor":"h-url-routing"} -->
<h2 id="h-url-routing" class="wp-block-heading">URL Routing</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Each content tree is mounted at its own URL prefix, computed independently at deploy time:</p>
<!-- /wp:paragraph -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th>Content tree</th><th>Default URL pattern</th><th>Override attribute</th></tr></thead><tbody><tr><td><code>static/</code></td><td><code>/app/static/{prefix}/...</code></td><td><code>dev.getelements.element.static.uri</code></td></tr><tr><td><code>ui/</code></td><td><code>/app/ui/{prefix}/...</code></td><td><code>dev.getelements.element.ui.uri</code></td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:paragraph -->
<p><code>{prefix}</code> comes from the Element's <code>dev.getelements.elements.app.serve.prefix</code> attribute, and falls back to the Element's own <code>@ElementDefinition</code> name if that attribute isn't set. Either mount point can be overridden per-Element with the corresponding attribute above, in which case the override value is used verbatim as the context path instead of the computed <code>/app/{static|ui}/{prefix}</code> path.</p>
<!-- /wp:paragraph -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"Note"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="3b0649"><div class="gb-notice-title" style="color:#fff"><p>Note</p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p>Before mounting a content tree, Elements checks the computed context path against its registry of reserved system paths (the REST API root, WebSocket root, and similar). If your override collides with a reserved path outright, deployment of that content tree is rejected; if it merely overlaps a catch-all route, the reserved system routes still take priority so they keep working.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:paragraph -->
<p>Reserved system paths are only part of the picture. Two ordinary deployments can also claim the same serve path. In that case the tree is not refused: it is remounted under a deployment-scoped path derived from the deployment's ID, so both deployments can run at once. A UI tree that conflicts at its default path is served at <code>/app/ui/{deployment-id}/{prefix}</code>, and an override that collides with another mounted tree is served at <code>/app/ui/{deployment-id}/{suffix}</code>. The same applies to the <code>static/</code> tree. The deployment log reports a warning naming the conflicting path and the deployment that owns it. Only a path that still falls inside a reserved namespace or another content tree after scoping is refused. Each deployment mounts its own content tree independently, so two deployments may expose the same <code>ui/</code> content at different scoped paths without interfering.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"anchor":"h-serving-behavior"} -->
<h2 id="h-serving-behavior" class="wp-block-heading">Serving Behavior</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Once mounted, each tree is served by a small built-in static file servlet with the behavior you'd expect from a CDN-style file server:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>An index file is served for directory-style requests. The default is <code>index.html</code>, and it can be overridden per content tree with an attribute of the form <code>dev.getelements.{static|ui}.index</code> (use <code>static</code> or <code>ui</code> depending on which tree you're configuring).</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Custom error pages can be configured per HTTP status code with an attribute of the form <code>dev.getelements.{static|ui}.error.&lt;code&gt;</code>, e.g. <code>dev.getelements.static.error.404</code>.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Conditional requests (<code>ETag</code> / <code>If-None-Match</code>, <code>Last-Modified</code>) and byte-range requests (<code>Range</code> / <code>If-Range</code>) are both supported, so large downloads and media files behave correctly with browsers and download managers.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>MIME types and any per-file headers are resolved once, at Element load time, not per request.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"anchor":"h-the-dashboard-ui-plugin-convention"} -->
<h2 id="h-the-dashboard-ui-plugin-convention" class="wp-block-heading">The Dashboard UI Plugin Convention</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The <code>ui/</code> tree is just static content, but the Elements admin dashboard imposes a specific convention on top of it so it can discover and load your plugin automatically. There is no special-cased server code for this; the dashboard finds your plugin purely by fetching well-known files over HTTP from the URLs above.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Lay out <code>ui/</code> with one subdirectory per dashboard surface you want to extend:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>ui/
  superuser/
    plugin.json
    plugin.bundle.js
  user/
    plugin.json
    plugin.bundle.js</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>Each subdirectory is served at <code>/app/ui/{prefix}/{segment}/</code>, where <code>{segment}</code> is <code>superuser</code> or <code>user</code>. The dashboard fetches <code>plugin.json</code> from each segment it finds and, if present, loads the referenced bundle. A minimal <code>plugin.json</code>:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>{
  "schema": "1",
  "entries": &#91;
    {
      "label": "Example Element",
      "icon": "Package",
      "bundlePath": "plugin.bundle.js",
      "route": "example-element"
    }
  ]
}</code></pre>
<!-- /wp:code -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th>Field</th><th>Description</th></tr></thead><tbody><tr><td><code>schema</code></td><td>Manifest schema version. Currently <code>"1"</code>.</td></tr><tr><td><code>entries[].label</code></td><td>Text shown for this plugin in the dashboard sidebar.</td></tr><tr><td><code>entries[].icon</code></td><td>Name of a Lucide icon to display next to the label.</td></tr><tr><td><code>entries[].bundlePath</code></td><td>Path to the plugin's JavaScript bundle, relative to <code>plugin.json</code>.</td></tr><tr><td><code>entries[].route</code></td><td>Route segment used to reach the plugin at <code>/plugin/{route}</code> in the dashboard.</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:paragraph -->
<p><code>plugin.bundle.js</code> must be a self-contained, immediately-invoked script (no module loader or bundler runtime of its own) that registers a component against the route named in <code>plugin.json</code>:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>(function () {
  var React = window.React;

  function ExamplePluginPage() {
    return React.createElement('div', null, 'Hello from my Element');
  }

  window.__elementsPlugins.register('example-element', ExamplePluginPage);
})();</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>The bundle reuses the dashboard's own <code>window.React</code> instance rather than bundling a copy of React itself, which keeps plugin bundles small and avoids duplicate-React errors. To discover plugins, the dashboard lists the Elements currently deployed, treats every exposed URI that is not a known REST, WebSocket, or standard static mount as a candidate UI base path (covering both the default <code>/app/ui/{prefix}/</code> path and any <code>dev.getelements.element.ui.uri</code> override), fetches <code>plugin.json</code> for each known segment, and injects a <code>&lt;script&gt;</code> tag for the referenced bundle.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>See <a href="element-example-complete-walkthrough">Building the Example Element: A Complete Walkthrough</a> for a full working example, including the TypeScript/React source the example project builds into <code>plugin.bundle.js</code>.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->
