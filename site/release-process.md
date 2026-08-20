<h1>Release Process (3.9+)</h1>

<!-- wp:paragraph -->
<p>Starting with Elements 3.9, every build artifact, from a snapshot straight off <code>main</code> to a formal release, comes from one of three channels. This page explains what each channel is, how stable it is, and where to get its jars, Maven coordinates, and Docker images. It's written for game and plugin developers choosing what to depend on; if you're a maintainer cutting a release, see <code>ACTIONS.md</code> in the <a href="https://github.com/NamazuStudios/elements">engine repository</a> for the GitHub Actions workflow reference.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"anchor":"h-the-three-channels"} -->
<h2 id="h-the-three-channels" class="wp-block-heading">The Three Channels</h2>
<!-- /wp:heading -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th>Channel</th><th>Version looks like</th><th>Stability</th><th>Jars / SDK</th><th>Docker images</th></tr></thead><tbody><tr><td>Snapshot (<code>main</code>)</td><td><code>3.10.0-SNAPSHOT</code></td><td>Unstable, untested by hand — every push at least passes the full integration test suite</td><td>Not available for normal use</td><td>Available</td></tr><tr><td>Release Candidate (<code>release/X.Y</code>)</td><td><code>3.9.0-rc-3</code></td><td>Not fully tested, potentially unstable</td><td>Maven Central + local SDK</td><td>Available</td></tr><tr><td>Formal Release (<code>release/X.Y</code>, tag)</td><td><code>3.9.0</code></td><td>Tested, stable</td><td>Maven Central + local SDK</td><td>Available</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:heading {"level":3,"anchor":"h-snapshots"} -->
<h3 id="h-snapshots" class="wp-block-heading">Snapshots</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Every push to <code>main</code> triggers a full build, test, and publish. Snapshot jars aren't published anywhere useful for pulling into a game project: Maven Central rejects <code>-SNAPSHOT</code> uploads outright, and the copy that does land in the GitHub Packages snapshot repository requires GitHub authentication to resolve, so treat snapshot jars as unavailable. Docker images are the one artifact that <em>is</em> readily available from this channel; see <a href="#h-docker-images">Docker Images</a> below.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Always consider a snapshot build unstable and untested, even though it's guaranteed to have passed the full integration test suite (a snapshot build that fails those tests never finishes publishing).</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3,"anchor":"h-release-candidates"} -->
<h3 id="h-release-candidates" class="wp-block-heading">Release Candidates</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>When a release line is ready to stabilize, it gets its own <code>release/X.Y</code> branch (e.g. <code>release/3.9</code>), cut from <code>main</code>. Every version on that branch is tagged <code>X.Y.Z-rc-N</code> until the line is formally released, e.g. <code>3.9.0-rc-1</code>, then <code>3.9.0-rc-2</code>, and so on as fixes land.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Release candidates are not fully tested and should be considered potentially unstable, but unlike snapshots they're real, resolvable artifacts: published to Maven Central under the <code>dev.getelements.elements</code> group, so you can pull an RC into your Element or SDK project exactly like a formal release, just by pinning the <code>-rc-N</code> version. Docker images are published as well.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3,"anchor":"h-formal-releases"} -->
<h3 id="h-formal-releases" class="wp-block-heading">Formal Releases</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Once a release candidate has been tested and is considered stable, the <code>-rc-N</code> designation is dropped (e.g. <code>3.9.0-rc-3</code> &rarr; <code>3.9.0</code>) and a formal release is tagged and published: Maven Central, Docker images, and a GitHub Release with generated notes. This is the version you should target for production.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>The release branch doesn't stop there — it immediately moves on to the next patch's first release candidate (e.g. <code>3.9.1-rc-1</code>) so bug-fix RCs for that line can keep iterating without a separate step.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"anchor":"h-maven-coordinates"} -->
<h2 id="h-maven-coordinates" class="wp-block-heading">Maven Coordinates</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Release and release-candidate artifacts share the <code>dev.getelements.elements</code> group ID on Maven Central. Point your SDK or Element project's dependency at the exact version you want:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>&lt;dependency&gt;
    &lt;groupId&gt;dev.getelements.elements&lt;/groupId&gt;
    &lt;artifactId&gt;sdk&lt;/artifactId&gt;
    &lt;version&gt;3.9.0-rc-3&lt;/version&gt; &lt;!-- or 3.9.0 once released --&gt;
&lt;/dependency&gt;</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>See <a href="packaging-an-element-with-maven">Packaging an Element with Maven</a> for how these coordinates fit into an Element project.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"anchor":"h-docker-images"} -->
<h2 id="h-docker-images" class="wp-block-heading">Docker Images</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Every channel, snapshot, RC, and formal release, publishes the same two images to both Docker Hub and GitHub Container Registry:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><code>elementalcomputing/elements-base</code> / <code>ghcr.io/namazustudios/elements-base</code></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>elementalcomputing/elements-jetty-ws</code> / <code>ghcr.io/namazustudios/elements-jetty-ws</code></li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>Each image is tagged with the exact Maven version it was built from (e.g. <code>3.9.0-rc-3</code>, <code>3.10.0-SNAPSHOT</code>), plus the full and short git commit SHA it was built from. Because a snapshot build reuses the same <code>-SNAPSHOT</code> version across many commits on <code>main</code>, that version tag is a moving target, pull it to always get the latest snapshot, or pin a commit SHA tag to get a specific snapshot build:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>docker pull elementalcomputing/elements-jetty-ws:3.9.0-rc-3
docker pull ghcr.io/namazustudios/elements-jetty-ws:3.10.0-SNAPSHOT</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>See <a href="common-issues-with-docker">Common Issues with Docker</a> if you run into trouble running these locally.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"anchor":"h-choosing-a-channel"} -->
<h2 id="h-choosing-a-channel" class="wp-block-heading">Choosing a Channel</h2>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>Production games:</strong> use a formal release.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Testing an upcoming release, or you need a fix that's already landed but not yet released:</strong> use the latest release candidate for that line.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Trying out unreleased engine work, or contributing to Elements itself:</strong> the snapshot Docker images are the only practical way to run <code>main</code> locally, since there's no resolvable snapshot jar.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->
