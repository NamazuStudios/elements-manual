<h1>Building the Example Element: A Complete Walkthrough</h1>

<!-- wp:paragraph -->
<p id="h-a-complete-tour-of-the-example-element-project-from-a-blank-checkout-to-a-running-backend-with-a-custom-rest-api-and-a-dashboard-plugin">A complete tour of the <a href="https://github.com/NamazuStudios/element-example">Example Element project</a>, from a blank checkout to a running backend with a custom REST API and a dashboard plugin.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Prefer video? Watch the quickstart walkthrough here: <a href="https://www.youtube.com/watch?v=TqkvpwvRJEc">https://www.youtube.com/watch?v=TqkvpwvRJEc</a></p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-what-youll-build">What You'll Build</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The Example Element is a reference Custom Element for Namazu Elements 3.8. Once running locally, it gives you:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>A REST API mounted at <code>/element/example/rest/api</code> with an open probe endpoint, an authenticated endpoint that greets the logged-in user, and a POST/PUT demo resource.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>A Guice-wired service layer that injects the SDK's own <code>UserService</code>.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>A dashboard UI plugin (React, built as a standalone bundle) that appears in the Elements admin dashboard sidebar.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>A packaged <code>.elm</code> archive you can deploy to any Elements instance.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>This guide covers setup end-to-end, then breaks down every Maven module and every source file so you understand exactly what each piece does and why it's there.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-prerequisites">Prerequisites</h2>
<!-- /wp:heading -->

<!-- wp:list {"ordered":true} -->
<ol class="wp-block-list"><!-- wp:list-item -->
<li><a href="https://www.oracle.com/java/technologies/downloads/#java21">Java 21 (JDK)</a></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="https://maven.apache.org/download.cgi">Apache Maven</a></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="https://www.docker.com/products/docker-desktop/">Docker</a> with Docker Compose (used to run a local MongoDB replica set)</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="https://git-scm.com/">Git</a></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Node.js (the build can also fetch its own pinned Node v22.14.0 via a Maven profile — see the Maven deep dive below — but a system Node install is the fastest path for local iteration)</li>
<!-- /wp:list-item --></ol>
<!-- /wp:list -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"Note"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="ee0a01"><div class="gb-notice-title" style="color:#fff"><p>Note</p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p>Since Elements is a Java 21 project, we recommend <a href="https://www.jetbrains.com/idea/download/">IntelliJ</a> as your IDE. See the platform-specific setup guides (<a href="setup-for-windows">Windows</a>, <a href="mac-os-setup">Mac</a>, <a href="ubuntu-linux-setup">Linux</a>) if you haven't set up a local Elements development environment before.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-step-by-step-setup-to-running">Step by Step: Setup to Running</h2>
<!-- /wp:heading -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-1-clone-the-repository">1. Clone the Repository</h3>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code>git clone https://github.com/NamazuStudios/element-example.git
cd element-example</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>The project is a four-module Maven build:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>element-example/
├── api/          # Exported interfaces (other Elements depend on this)
├── ui/           # TypeScript/React UI plugin source (Vite; not deployed directly)
├── element/      # Implementation module — builds the .elm archive
├── debug/        # Local development runner (not deployed)
└── services-dev/ # Docker Compose services (MongoDB) for local dev</code></pre>
<!-- /wp:code -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-2-build-everything">2. Build Everything</h3>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code>mvn install</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>This compiles <code>api</code> and <code>element</code> in dependency order, packages <code>element</code> into a <code>.elm</code> archive, and installs everything (including the classified API jar and the <code>.elm</code> artifact) into your local Maven repository. The <code>ui</code> module's real npm build is skipped unless you pass <code>-Pbuild-ui</code> — see the Maven deep dive below for why.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-3-start-mongodb">3. Start MongoDB</h3>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code>docker compose -f services-dev/docker-compose.yml up -d</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>This starts a single-node MongoDB 6.0.9 instance configured as replica set <code>local-test</code> on port 27017, plus a one-shot <code>rs-init</code> sidecar container that waits for Mongo to accept connections and then runs <code>rs.initiate(...)</code> to actually form the replica set. Elements' core SDK requires a replica set (it relies on transactions/change streams), so a plain standalone <code>mongod</code> will not work.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-4-run-the-element-locally">4. Run the Element Locally</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Run <code>debug/src/main/java/run.java</code> from your IDE, or from the command line:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>mvn -pl debug exec:java -Dexec.mainClass=run</code></pre>
<!-- /wp:code -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"Note"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="ee0a02"><div class="gb-notice-title" style="color:#fff"><p>Note</p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p>The working directory must be the project root (<code>element-example/</code>), because <code>run.java</code> shells out to <code>npm</code> in <code>ui/</code> and <code>docker compose</code> in <code>services-dev/</code> using relative paths. In IntelliJ: Run → Edit Configurations → set the working directory to the project root.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:paragraph -->
<p>When it runs, <code>run.java</code> does the following, in order:</p>
<!-- /wp:paragraph -->

<!-- wp:list {"ordered":true} -->
<ol class="wp-block-list"><!-- wp:list-item -->
<li>If <code>ui/node_modules</code> doesn't exist yet, runs <code>npm install</code> in <code>ui/</code> (first run only).</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Runs <code>npm run build</code> in <code>ui/</code>, which builds both the superuser and user dashboard plugin bundles and writes them directly into <code>element/src/main/ui/superuser/</code> and <code>element/src/main/ui/user/</code>.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Runs <code>docker compose up -d</code> in <code>services-dev/</code> (the same command as step 3 — safe to run again).</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Builds an <code>ElementsLocalBuilder</code> from the SDK's local runtime, configured to build and deploy <code>com.example.element:element:elm:1.0-SNAPSHOT</code> from source.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Calls <code>local.start()</code> — this triggers the Maven build of the <code>element</code> module (picking up the freshly built UI bundles), then boots the full local Elements runtime with your Element deployed.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Calls <code>local.run()</code>, which blocks and serves requests until you stop the process.</li>
<!-- /wp:list-item --></ol>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>After a short startup you'll see log output indicating the Elements runtime is listening, by default on port 8080.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-5-verify-its-working">5. Verify It's Working</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Hit the open probe endpoint — no authentication required:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>curl http://localhost:8080/element/example/rest/api/helloworld
# =&gt; Hello world!</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>Hit the authenticated endpoint without a session — you'll be greeted as a guest:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>curl http://localhost:8080/element/example/rest/api/hellowithauthentication
# =&gt; Hello, Guest!</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>Create a user, log in to get a session secret, then call it again with the <code>Elements-SessionSecret</code> header — see <a href="creating-a-user">Creating a User</a> and <a href="user-authentication-in-elements">User Authentication in Elements</a> if you need a refresher on those two calls. With a valid session you should get <code>Hello, &lt;your name&gt;!</code> instead of <code>Hello, Guest!</code>.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Explore the generated OpenAPI spec (this Element's routes appear alongside the rest of the platform's, tagged <code>Example</code>):</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>http://localhost:8080/api/rest/openapi.json</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>Finally, log in to the dashboard at <code>http://localhost:8080/admin/login</code> as a superuser and look for "Example Element" in the sidebar — that's the React plugin bundle shipped from <code>ui/src/superuser/ExamplePlugin.tsx</code>, served from this Element's UI content tree.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"level":1} -->
<h1 class="wp-block-heading" id="h-maven-structure-deep-dive">Maven Structure Deep Dive</h1>
<!-- /wp:heading -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-root-pom-xml">Root <code>pom.xml</code></h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The root is a pure aggregator (<code>&lt;packaging&gt;pom&lt;/packaging&gt;</code>) with no parent of its own. It declares the four modules, pins shared properties, and centralizes dependency versions and scopes:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>&lt;modules&gt;
    &lt;module&gt;api&lt;/module&gt;
    &lt;module&gt;ui&lt;/module&gt;
    &lt;module&gt;element&lt;/module&gt;
    &lt;module&gt;debug&lt;/module&gt;
&lt;/modules&gt;

&lt;properties&gt;
    &lt;maven.compiler.source&gt;21&lt;/maven.compiler.source&gt;
    &lt;maven.compiler.target&gt;21&lt;/maven.compiler.target&gt;
    &lt;elements.version&gt;3.8.14&lt;/elements.version&gt;
    &lt;api.classifier&gt;${project.groupId}.api&lt;/api.classifier&gt;
    &lt;!-- swagger.version, guice.version, rs.api, jakarta.websocket.version,
         crossfire.version, servlet.api, logback.version also declared here --&gt;
&lt;/properties&gt;</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>The important piece is <code>dependencyManagement</code>, which imports the Elements SDK BOM:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>&lt;dependencyManagement&gt;
    &lt;dependencies&gt;
        &lt;dependency&gt;
            &lt;groupId&gt;dev.getelements.elements&lt;/groupId&gt;
            &lt;artifactId&gt;sdk-bom&lt;/artifactId&gt;
            &lt;version&gt;${elements.version}&lt;/version&gt;
            &lt;type&gt;pom&lt;/type&gt;
            &lt;scope&gt;import&lt;/scope&gt;
        &lt;/dependency&gt;

        &lt;dependency&gt;
            &lt;groupId&gt;com.example.element&lt;/groupId&gt;
            &lt;artifactId&gt;api&lt;/artifactId&gt;
            &lt;version&gt;${project.version}&lt;/version&gt;
            &lt;scope&gt;provided&lt;/scope&gt;
        &lt;/dependency&gt;

        &lt;dependency&gt;
            &lt;groupId&gt;com.example.element&lt;/groupId&gt;
            &lt;artifactId&gt;api&lt;/artifactId&gt;
            &lt;version&gt;${project.version}&lt;/version&gt;
            &lt;classifier&gt;${api.classifier}&lt;/classifier&gt;
            &lt;scope&gt;provided&lt;/scope&gt;
        &lt;/dependency&gt;
    &lt;/dependencies&gt;
&lt;/dependencyManagement&gt;</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>Importing <code>sdk-bom</code> means every child module gets the correct version <strong>and</strong> the correct scope for every SDK artifact automatically — child <code>pom.xml</code> files never declare a <code>&lt;version&gt;</code> or <code>&lt;scope&gt;</code> for an SDK dependency. The <code>api</code> artifact is declared twice: once as a plain dependency (used at compile time by <code>element</code>) and once with the <code>${api.classifier}</code> classifier (a second, classified jar of the same code — see below).</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-api-pom-xml-the-classified-jar">The <code>api</code> Module: a Classified Jar</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p><code>api/pom.xml</code> depends on nothing but the core SDK (scope <code>provided</code>) — by design. The API module should stay as lean as possible, since every API jar in a deployment shares a common classpath with every other Element's API jar; bloating it with third-party libraries invites classpath conflicts across unrelated Elements.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>The interesting part is its build configuration:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>&lt;plugin&gt;
    &lt;groupId&gt;org.apache.maven.plugins&lt;/groupId&gt;
    &lt;artifactId&gt;maven-jar-plugin&lt;/artifactId&gt;
    &lt;executions&gt;
        &lt;execution&gt;
            &lt;id&gt;classified-jar&lt;/id&gt;
            &lt;phase&gt;package&lt;/phase&gt;
            &lt;goals&gt;&lt;goal&gt;jar&lt;/goal&gt;&lt;/goals&gt;
            &lt;configuration&gt;
                &lt;classifier&gt;${api.classifier}&lt;/classifier&gt;
            &lt;/configuration&gt;
        &lt;/execution&gt;
    &lt;/executions&gt;
&lt;/plugin&gt;</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>This produces a <strong>second</strong> jar — <code>api-1.0-SNAPSHOT-com.example.element.api.jar</code> — in addition to the normal jar. The plain jar is what <code>element</code> compiles against; the classified jar is what gets copied into the <code>api/</code> directory inside the final <code>.elm</code> archive, which is how other deployed Elements can depend on and resolve this Element's exported interfaces at runtime.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-element-pom-xml-dependencies">The <code>element</code> Module: Dependencies</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p><code>element/pom.xml</code> declares no versions or scopes — every dependency's scope comes from the imported BOM:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>&lt;dependencies&gt;
    &lt;dependency&gt; &lt;!-- own API, classified jar --&gt;
        &lt;groupId&gt;com.example.element&lt;/groupId&gt;
        &lt;artifactId&gt;api&lt;/artifactId&gt;
        &lt;classifier&gt;${api.classifier}&lt;/classifier&gt;
    &lt;/dependency&gt;
    &lt;dependency&gt;&lt;groupId&gt;dev.getelements.elements&lt;/groupId&gt;&lt;artifactId&gt;sdk&lt;/artifactId&gt;&lt;/dependency&gt;
    &lt;dependency&gt;&lt;groupId&gt;dev.getelements.elements&lt;/groupId&gt;&lt;artifactId&gt;sdk-model&lt;/artifactId&gt;&lt;/dependency&gt;
    &lt;dependency&gt;&lt;groupId&gt;dev.getelements.elements&lt;/groupId&gt;&lt;artifactId&gt;sdk-service&lt;/artifactId&gt;&lt;/dependency&gt;
    &lt;dependency&gt;&lt;groupId&gt;dev.getelements.elements&lt;/groupId&gt;&lt;artifactId&gt;sdk-spi-guice&lt;/artifactId&gt;&lt;/dependency&gt;
    &lt;dependency&gt;&lt;groupId&gt;dev.getelements.elements&lt;/groupId&gt;&lt;artifactId&gt;sdk-jakarta-rs&lt;/artifactId&gt;&lt;/dependency&gt;
    &lt;dependency&gt;&lt;groupId&gt;com.google.inject&lt;/groupId&gt;&lt;artifactId&gt;guice&lt;/artifactId&gt;&lt;/dependency&gt;
    &lt;dependency&gt;&lt;groupId&gt;jakarta.ws.rs&lt;/groupId&gt;&lt;artifactId&gt;jakarta.ws.rs-api&lt;/artifactId&gt;&lt;/dependency&gt;
    &lt;dependency&gt;&lt;groupId&gt;jakarta.websocket&lt;/groupId&gt;&lt;artifactId&gt;jakarta.websocket-api&lt;/artifactId&gt;&lt;/dependency&gt;
    &lt;dependency&gt;&lt;groupId&gt;io.swagger.core.v3&lt;/groupId&gt;&lt;artifactId&gt;swagger-annotations&lt;/artifactId&gt;&lt;/dependency&gt;
    &lt;dependency&gt;&lt;groupId&gt;io.swagger.core.v3&lt;/groupId&gt;&lt;artifactId&gt;swagger-jaxrs2-jakarta&lt;/artifactId&gt;&lt;/dependency&gt;
&lt;/dependencies&gt;</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>The BOM scopes <code>sdk</code>, <code>sdk-model</code>, <code>sdk-service</code>, <code>sdk-spi-guice</code>, and <code>sdk-jakarta-rs</code> as <code>provided</code> — the Elements runtime already has these on its classpath, so they're compiled against but never bundled. <code>guice</code>, the Jakarta RS/WebSocket APIs, and the Swagger artifacts are also provided by the runtime. This Element only uses the Guice SPI (<code>sdk-spi-guice</code>) — there's no non-Guice <code>sdk-spi</code> variant in use here.</p>
<!-- /wp:paragraph -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"Note"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="ee0a03"><div class="gb-notice-title" style="color:#fff"><p>Note</p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p>Only artifacts that are <strong>not</strong> <code>provided</code> get bundled into the <code>.elm</code>'s <code>lib/</code> directory. Always double-check the BOM's scoping if you see duplicate-class errors at runtime — it usually means something that should be <code>provided</code> got bundled twice.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-the-elm-packaging-pipeline">The <code>.elm</code> Packaging Pipeline</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Unlike older Elements projects that used the Maven Assembly Plugin to zip a <code>libs/</code> + <code>classpath/</code> directory pair, this project builds its <code>.elm</code> archive with the Maven Antrun Plugin and Maven Dependency Plugin, no assembly descriptor required. Two properties define the staging layout:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>&lt;elm.staging.dir&gt;${project.build.directory}/${project.groupId}.${project.artifactId}-${project.version}&lt;/elm.staging.dir&gt;
&lt;elm.element.dir&gt;${elm.staging.dir}/${project.groupId}.${project.artifactId}&lt;/elm.element.dir&gt;</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>i.e. <code>target/com.example.element.element-1.0-SNAPSHOT/com.example.element.element/</code>. Five executions build up that directory and then zip it, in this order:</p>
<!-- /wp:paragraph -->

<!-- wp:list {"ordered":true} -->
<ol class="wp-block-list"><!-- wp:list-item -->
<li><code>elm-copy-api-deps</code> (<code>maven-dependency-plugin</code>, phase <code>prepare-package</code>) — copies this project's own <code>${api.classifier}</code>-classified jar (and any transitive ones sharing the same groupId) into <code>&lt;elm.element.dir&gt;/api</code>.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>elm-copy-lib-deps</code> (<code>maven-dependency-plugin</code>, phase <code>prepare-package</code>) — copies every non-<code>provided</code>-scope dependency into <code>&lt;elm.element.dir&gt;/lib</code>, prepending the group id to each filename.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>elm-stage-classpath</code> (<code>maven-antrun-plugin</code>, phase <code>prepare-package</code>) — copies compiled classes and <code>src/main/resources</code> into <code>&lt;elm.element.dir&gt;/classpath</code>.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>elm-stage-static-content</code> (<code>maven-antrun-plugin</code>, phase <code>prepare-package</code>) — copies <code>src/main/static</code> into <code>&lt;elm.element.dir&gt;/static</code> and <code>src/main/ui</code> into <code>&lt;elm.element.dir&gt;/ui</code>.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>elm-write-manifest</code> (<code>maven-antrun-plugin</code>, phase <code>prepare-package</code>) — resolves <code>git rev-parse HEAD</code> (falling back to <code>unknown</code>) and writes <code>dev.getelements.element.manifest.properties</code> at the element root with <code>Element-Version</code>, <code>Element-Build-Time</code>, <code>Element-Revision</code>, and <code>Element-Builtin-Spis</code>.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>elm-create-archive</code> (<code>maven-antrun-plugin</code>, phase <code>package</code>) — ensures <code>api/</code>, <code>lib/</code>, and <code>classpath/</code> exist even if empty, then zips the whole staging directory to <code>&lt;elm.staging.dir&gt;.elm</code>: <code>&lt;zip destfile="${elm.staging.dir}.elm" basedir="${elm.staging.dir}"/&gt;</code>.</li>
<!-- /wp:list-item --></ol>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>Finally, <code>build-helper-maven-plugin</code>'s <code>attach-artifact</code> goal (phase <code>package</code>) attaches that <code>.elm</code> file to the Maven project as artifact type <code>elm</code>:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>&lt;plugin&gt;
    &lt;groupId&gt;org.codehaus.mojo&lt;/groupId&gt;
    &lt;artifactId&gt;build-helper-maven-plugin&lt;/artifactId&gt;
    &lt;executions&gt;
        &lt;execution&gt;
            &lt;id&gt;attach-elm&lt;/id&gt;
            &lt;phase&gt;package&lt;/phase&gt;
            &lt;goals&gt;&lt;goal&gt;attach-artifact&lt;/goal&gt;&lt;/goals&gt;
            &lt;configuration&gt;
                &lt;artifacts&gt;
                    &lt;artifact&gt;
                        &lt;file&gt;${elm.staging.dir}.elm&lt;/file&gt;
                        &lt;type&gt;elm&lt;/type&gt;
                    &lt;/artifact&gt;
                &lt;/artifacts&gt;
            &lt;/configuration&gt;
        &lt;/execution&gt;
    &lt;/executions&gt;
&lt;/plugin&gt;</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>This is what makes <code>mvn install</code> publish <code>com.example.element:element:1.0-SNAPSHOT:elm</code> into your local repository alongside the normal jar — exactly the coordinate <code>run.java</code> references with <code>.elmArtifact("com.example.element:element:elm:1.0-SNAPSHOT")</code>, and the coordinate you'd reference from a deployment configuration.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>The resulting <code>.elm</code> archive layout:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>com.example.element.element/
  api/            &lt;- classified API jar(s)
  lib/            &lt;- bundled runtime jars (non-provided scope)
  classpath/      &lt;- compiled classes + src/main/resources
  static/         &lt;- src/main/static
  ui/             &lt;- src/main/ui (superuser/user plugin bundles)
  dev.getelements.element.manifest.properties</code></pre>
<!-- /wp:code -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-debug-pom-xml">The <code>debug</code> Module</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p><code>debug/pom.xml</code> intentionally depends on nothing but the SDK's local runtime and logging:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>&lt;dependencies&gt;
    &lt;dependency&gt;&lt;groupId&gt;dev.getelements.elements&lt;/groupId&gt;&lt;artifactId&gt;sdk-local&lt;/artifactId&gt;&lt;scope&gt;compile&lt;/scope&gt;&lt;/dependency&gt;
    &lt;dependency&gt;&lt;groupId&gt;dev.getelements.elements&lt;/groupId&gt;&lt;artifactId&gt;sdk-local-maven&lt;/artifactId&gt;&lt;scope&gt;compile&lt;/scope&gt;&lt;/dependency&gt;
    &lt;dependency&gt;&lt;groupId&gt;dev.getelements.elements&lt;/groupId&gt;&lt;artifactId&gt;sdk-logback&lt;/artifactId&gt;&lt;scope&gt;compile&lt;/scope&gt;&lt;/dependency&gt;
    &lt;dependency&gt;&lt;groupId&gt;ch.qos.logback&lt;/groupId&gt;&lt;artifactId&gt;logback-classic&lt;/artifactId&gt;&lt;scope&gt;compile&lt;/scope&gt;&lt;/dependency&gt;
&lt;/dependencies&gt;</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>The module's own comment sums up its purpose: <code>sdk-local</code> is a thin wrapper around a fully configured instance of Namazu Elements, and this configuration should almost never need changes. It's never deployed — it exists purely so you can boot the whole platform, with your Element loaded from source, inside your IDE.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-ui-pom-xml">The <code>ui</code> Module</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p><code>ui/pom.xml</code> is packaged <code>pom</code> and does no work by default. Its <code>build-ui</code> profile uses <code>frontend-maven-plugin</code> to install a pinned Node version and run the npm build, for CI/release builds that shouldn't depend on the machine's own Node install:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>&lt;profile&gt;
    &lt;id&gt;build-ui&lt;/id&gt;
    &lt;build&gt;
        &lt;plugins&gt;
            &lt;plugin&gt;
                &lt;groupId&gt;com.github.eirslett&lt;/groupId&gt;
                &lt;artifactId&gt;frontend-maven-plugin&lt;/artifactId&gt;
                &lt;version&gt;1.15.0&lt;/version&gt;
                &lt;configuration&gt;
                    &lt;nodeVersion&gt;v22.14.0&lt;/nodeVersion&gt;
                &lt;/configuration&gt;
                &lt;executions&gt;
                    &lt;execution&gt;&lt;id&gt;install-node-and-npm&lt;/id&gt;&lt;goals&gt;&lt;goal&gt;install-node-and-npm&lt;/goal&gt;&lt;/goals&gt;&lt;/execution&gt;
                    &lt;execution&gt;&lt;id&gt;npm-install&lt;/id&gt;&lt;goals&gt;&lt;goal&gt;npm&lt;/goal&gt;&lt;/goals&gt;&lt;/execution&gt;
                    &lt;execution&gt;
                        &lt;id&gt;npm-build&lt;/id&gt;
                        &lt;goals&gt;&lt;goal&gt;npm&lt;/goal&gt;&lt;/goals&gt;
                        &lt;configuration&gt;&lt;arguments&gt;run build&lt;/arguments&gt;&lt;/configuration&gt;
                    &lt;/execution&gt;
                &lt;/executions&gt;
            &lt;/plugin&gt;
        &lt;/plugins&gt;
    &lt;/build&gt;
&lt;/profile&gt;</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>Activate it with <code>mvn install -Pbuild-ui</code>. Day to day, developers run <code>npm</code> directly in <code>ui/</code> (or let <code>run.java</code> do it) using their own Node install — this profile exists for build environments that shouldn't assume Node is already present.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"level":1} -->
<h1 class="wp-block-heading" id="h-java-source-deep-dive">Java Source Deep Dive</h1>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>All source lives under <code>com.mystudio.mygame</code>, split across the <code>api</code> and <code>element</code> modules. Here's every file, in the order you'd read them to understand how the pieces connect.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-package-info-java">1. <code>package-info.java</code> — Declaring the Element</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p><code>element/src/main/java/com/mystudio/mygame/package-info.java</code>:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>// Required annotation for an Element. Will recursively search folders
// from this point to include classes in the Element if recursive is true.
// Otherwise, you must include additional package-info.java files in child packages.
@ElementDefinition(recursive = true)
// Enables DI via Guice
@GuiceElementModule(MyGameModule.class)
// Allows injecting DAO layer from Elements Core
@ElementDependency("dev.getelements.elements.sdk.dao")
// Allows injecting Service layer from Elements Core
@ElementDependency("dev.getelements.elements.sdk.service")
package com.mystudio.mygame;

import com.mystudio.mygame.guice.MyGameModule;
import dev.getelements.elements.sdk.annotation.ElementDefinition;
import dev.getelements.elements.sdk.annotation.ElementDependency;
import dev.getelements.elements.sdk.spi.guice.annotations.GuiceElementModule;</code></pre>
<!-- /wp:code -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><code>@ElementDefinition(recursive = true)</code> is what makes the SDK's classloading/discovery mechanism recognize <code>com.mystudio.mygame</code> — and every sub-package, because <code>recursive = true</code> — as one Element. Without it, nothing here would be treated as an Element at all. If <code>recursive</code> were <code>false</code>, each sub-package (<code>rest</code>, <code>service</code>, <code>model</code>, <code>guice</code>) would need its own <code>package-info.java</code>.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>@GuiceElementModule(MyGameModule.class)</code> tells the SDK which Guice module to install when it bootstraps this Element's private injector.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>The two <code>@ElementDependency</code> annotations declare dependencies on other Elements — the core SDK's DAO layer and service layer. This is what lets <code>GreetingServiceImpl</code> (below) <code>@Inject</code> the SDK's own <code>UserService</code> even though this Element never binds it itself.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-mygamemodule-java">2. <code>guice/MyGameModule.java</code> — the Guice Module</h2>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code>package com.mystudio.mygame.guice;

import com.google.inject.PrivateModule;
import com.mystudio.mygame.service.GreetingService;
import com.mystudio.mygame.service.GreetingServiceImpl;

public class MyGameModule extends PrivateModule {

    @Override
    protected void configure() {

        bind(GreetingService.class).to(GreetingServiceImpl.class);

        expose(GreetingService.class);
    }
}</code></pre>
<!-- /wp:code -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>It extends <code>PrivateModule</code>, not plain <code>AbstractModule</code>. Elements gives each Element its own private Guice environment so internal bindings can't leak into, or collide with, other Elements' bindings.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>bind(GreetingService.class).to(GreetingServiceImpl.class)</code> is a standard interface-to-implementation binding.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>expose(GreetingService.class)</code> is required precisely <strong>because</strong> this is a <code>PrivateModule</code> — nothing inside one is visible outside it by default. Without this call, the service-locator lookup in <code>HelloWithAuthentication</code> (below) would fail.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-greetingservice-java">3. <code>service/GreetingService.java</code> — the API Interface</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Lives in the <strong>api</strong> module (<code>api/src/main/java/com/mystudio/mygame/service/GreetingService.java</code>), not <code>element</code>, so other Elements can depend on the interface without pulling in the implementation or Guice wiring:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>package com.mystudio.mygame.service;

public interface GreetingService {

    /**
     * Attempts to fetch the current user for the session header and return an appropriate greeting
     * @return The greeting based on if a logged-in user is found
     */
    String getGreeting();

}</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>Note that the interface itself carries no <code>@ElementServiceExport</code> annotation — in this project that annotation is applied to the implementation class instead.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-greetingserviceimpl-java">4. <code>service/GreetingServiceImpl.java</code> — the Implementation</h2>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code>package com.mystudio.mygame.service;

import dev.getelements.elements.sdk.annotation.ElementServiceExport;
import dev.getelements.elements.sdk.model.user.User;
import dev.getelements.elements.sdk.service.user.UserService;
import jakarta.inject.Inject;


@ElementServiceExport(GreetingService.class)
public class GreetingServiceImpl implements GreetingService {

    private UserService userService;

    public UserService getUserService() {
        return userService;
    }

    @Inject
    public void setUserService(final UserService userService) {
        this.userService = userService;
    }

    @Override
    public String getGreeting() {
        // Because we set the dev.getelements.elements.auth.enabled attribute to "true" in the HelloWorldApplication,
        // the UserService will be automatically injected with the current user. This will apply an authentication
        // filter to every request and every service that is used in this application.
        final User currentUser = userService.getCurrentUser();
        final boolean isLoggedIn = !User.Level.UNPRIVILEGED.equals(currentUser.getLevel());
        final String name = isLoggedIn ? currentUser.getName() : "Guest";

        return "Hello, " + name + "!";
    }
}</code></pre>
<!-- /wp:code -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><code>@ElementServiceExport(GreetingService.class)</code> tells the SDK to expose this concrete class under the <code>GreetingService</code> service-locator key — the annotation-driven counterpart to the explicit <code>bind()</code>/<code>expose()</code> calls in <code>MyGameModule</code>. Both point at the same binding.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>setUserService</code> is Guice <strong>setter injection</strong> (<code>@Inject</code> on a setter rather than a constructor) — a common pattern for SDK-provided dependencies.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>UserService</code> comes from <code>dev.getelements.elements.sdk.service.user</code> — resolvable here only because <code>package-info.java</code> declared <code>@ElementDependency("dev.getelements.elements.sdk.service")</code>.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>The auth check is: fetch <code>currentUser</code>, then compare its level against <code>User.Level.UNPRIVILEGED</code> to tell a real logged-in user from an anonymous/guest request. <strong>This is the only auth-level check in the project</strong>, and it distinguishes "logged in vs. not" — it does not check for <code>SUPERUSER</code>.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-helloworldapplication-java">5. <code>HelloWorldApplication.java</code> — Registering Endpoints</h2>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code>package com.mystudio.mygame;

import com.mystudio.mygame.rest.ExampleContent;
import com.mystudio.mygame.rest.HelloWithAuthentication;
import com.mystudio.mygame.rest.HelloWorld;
import dev.getelements.elements.sdk.annotation.ElementDefaultAttribute;
import dev.getelements.elements.sdk.annotation.ElementServiceExport;
import dev.getelements.elements.sdk.annotation.ElementServiceImplementation;
import io.swagger.v3.jaxrs2.integration.resources.OpenApiResource;
import jakarta.ws.rs.core.Application;

import java.util.Set;

@ElementServiceImplementation
@ElementServiceExport(Application.class)
public class HelloWorldApplication extends Application {

    @ElementDefaultAttribute("true")
    public static final String AUTH_ENABLED = "dev.getelements.elements.auth.enabled";

    @ElementDefaultAttribute("/element/example/rest/api")
    public static final String RS_ROOT = "dev.getelements.elements.element.rs.root";

    @ElementDefaultAttribute("/element/example/ws")
    public static final String WS_ROOT = "dev.getelements.elements.element.ws.root";

    @ElementDefaultAttribute("/app/static/test/path")
    public static final String STATIC_CONTENT_URI = "dev.getelements.element.static.uri";

    @ElementDefaultAttribute("/app/ui/test/path")
    public static final String UI_CONTENT_URI = "dev.getelements.element.ui.uri";

    public static final String OPENAPI_TAG = "Example";

    /**
     * Here we register all the classes that we want to be included in the Element.
     */
    @Override
    public Set&lt;Class&lt;?&gt;&gt; getClasses() {
        return Set.of(
                //Endpoints
                HelloWorld.class,
                HelloWithAuthentication.class,
                ExampleContent.class,

                // Exposes the default security rules for the API. Assumes you are using the builtin Elements auth
                // system by setting `dev.getelements.elements.auth.enabled` to true in the annotation above.
                OpenAPISecurityConfig.class

        );
    }

}</code></pre>
<!-- /wp:code -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><code>@ElementServiceImplementation</code> marks this as a concrete implementation the SDK should instantiate and manage, rather than a plain POJO.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>@ElementServiceExport(Application.class)</code> is the key wiring annotation — it exposes this class under the JAX-RS <code>Application</code> service-locator key, which is how the Elements HTTP runtime discovers which <code>Application</code> subclass to mount for this Element, and at what root path.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Each <code>@ElementDefaultAttribute</code> field declares a default value for a named configuration attribute — the string constant is the attribute key, the annotation value is its default. <code>AUTH_ENABLED</code> defaulting to <code>"true"</code> is what turns on the built-in Elements auth filter for every request and service in this Element, which is exactly what makes <code>UserService.getCurrentUser()</code> populated in <code>GreetingServiceImpl</code>. <code>RS_ROOT</code> and <code>WS_ROOT</code> set the REST and WebSocket mount points; <code>STATIC_CONTENT_URI</code> and <code>UI_CONTENT_URI</code> are example placeholder paths, not wired to real content.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>getClasses()</code> is the actual JAX-RS registration point. Note that <code>OpenApiResource</code> is imported but never added to the returned set in this project's current form — only <code>HelloWorld</code>, <code>HelloWithAuthentication</code>, <code>ExampleContent</code>, and <code>OpenAPISecurityConfig</code> are registered.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-openapisecurityconfig-java">6. <code>OpenAPISecurityConfig.java</code> — Documenting the Auth Scheme</h2>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code>package com.mystudio.mygame;

import dev.getelements.elements.sdk.model.Headers;
import io.swagger.v3.oas.annotations.ExternalDocumentation;
import io.swagger.v3.oas.annotations.OpenAPIDefinition;
import io.swagger.v3.oas.annotations.info.Contact;
import io.swagger.v3.oas.annotations.info.Info;
import io.swagger.v3.oas.annotations.security.SecurityRequirement;
import io.swagger.v3.oas.annotations.security.SecurityScheme;
import io.swagger.v3.oas.annotations.security.SecuritySchemes;

import static dev.getelements.elements.sdk.jakarta.rs.AuthSchemes.SESSION_SECRET;
import static io.swagger.v3.oas.annotations.enums.SecuritySchemeIn.HEADER;
import static io.swagger.v3.oas.annotations.enums.SecuritySchemeType.APIKEY;

@OpenAPIDefinition(
        info = @Info(
                title = "Example Element",
                description = "An example element.",
                contact = @Contact(
                        url = "https://namazustudios.com",
                        email = "info@namazustudios.com",
                        name = "Namazu Studios"
                )
        ),
        externalDocs = @ExternalDocumentation(
                url = "https://namazustudios.com/docs",
                description = "Please see the Namazu Elements Manual for more information."
        ),
        security = {
                @SecurityRequirement(name = SESSION_SECRET)
        }
)
@SecuritySchemes({
        @SecurityScheme(
                type = APIKEY,
                in = HEADER,
                name = SESSION_SECRET,
                paramName = SESSION_SECRET,
                description = "Session secret required for authenticated endpoints")
})
public class OpenAPISecurityConfig {}</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>This class has no fields or methods — it exists solely to carry class-level OpenAPI annotations, and is registered in <code>getClasses()</code> purely so Swagger's scanner picks them up when generating the API spec. It defines the <code>Elements-SessionSecret</code> header as an API-key-style security scheme and sets it as the document-wide default. It plays <strong>no role at request-processing time</strong> — it's documentation metadata only, not enforcement.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-helloworld-java">7. <code>rest/HelloWorld.java</code> — an Open Probe Endpoint</h2>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code>package com.mystudio.mygame.rest;

import io.swagger.v3.oas.annotations.Operation;
import io.swagger.v3.oas.annotations.tags.Tag;
import jakarta.ws.rs.Consumes;
import jakarta.ws.rs.GET;
import jakarta.ws.rs.Path;
import jakarta.ws.rs.Produces;
import jakarta.ws.rs.core.MediaType;

import static com.mystudio.mygame.HelloWorldApplication.OPENAPI_TAG;

@Tag(name = OPENAPI_TAG)
@Path("/helloworld")
public class HelloWorld {

    @GET
    @Produces(MediaType.TEXT_PLAIN)
    @Operation(summary = "Hello world probe", description = "Returns a simple greeting")
    public String sayHello() {

        return "Hello world!";

    }

}</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>The simplest possible JAX-RS resource — a plain <code>GET</code> returning static text, useful as a health-probe. It declares no <code>@SecurityRequirement</code>, so it's reachable even though this Element enables auth globally: authorization here works by <em>services</em> resolving the current user (or not) rather than a filter rejecting unauthenticated requests outright.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-hellowithauthentication-java">8. <code>rest/HelloWithAuthentication.java</code> — the Service Locator Pattern</h2>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code>package com.mystudio.mygame.rest;

import com.mystudio.mygame.service.GreetingService;
import dev.getelements.elements.sdk.Element;
import dev.getelements.elements.sdk.ElementSupplier;
import io.swagger.v3.oas.annotations.Operation;
import io.swagger.v3.oas.annotations.security.SecurityRequirement;
import io.swagger.v3.oas.annotations.tags.Tag;
import jakarta.ws.rs.Consumes;
import jakarta.ws.rs.GET;
import jakarta.ws.rs.Path;
import jakarta.ws.rs.Produces;
import jakarta.ws.rs.core.MediaType;

import static com.mystudio.mygame.HelloWorldApplication.OPENAPI_TAG;
import static dev.getelements.elements.sdk.jakarta.rs.AuthSchemes.SESSION_SECRET;


@Tag(name = OPENAPI_TAG)
@Path("/hellowithauthentication")
public class HelloWithAuthentication {

    private final Element element = ElementSupplier
            .getElementLocal(HelloWithAuthentication.class)
            .get();

    private final GreetingService greetingService = element
            .getServiceLocator()
            .getInstance(GreetingService.class);


    @GET
    @Produces(MediaType.TEXT_PLAIN)
    @Consumes(MediaType.TEXT_PLAIN)
    @Operation(
            summary = "Greeting with login check",
            description = "Checks if the session token in the header corresponds to at least a USER level user.",
            security = { @SecurityRequirement(name = SESSION_SECRET) }
    )
    public String sayHelloWithAuth() {
        return greetingService.getGreeting();
    }

}</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>This is the class to study for the service-locator pattern:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><code>ElementSupplier.getElementLocal(HelloWithAuthentication.class).get()</code> resolves the <code>Element</code> instance associated with the calling class's classloader. This is necessary because JAX-RS resources are instantiated by the Jakarta RS container, not by Guice — so they can't use <code>@Inject</code> directly. This static lookup is how a plain, container-instantiated resource reaches back into its own Element's private Guice injector.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>element.getServiceLocator().getInstance(GreetingService.class)</code> then pulls the singleton out of that injector. This only works because <code>MyGameModule</code> called <code>expose(GreetingService.class)</code> — remove that call and this lookup throws.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>@SecurityRequirement(name = SESSION_SECRET)</code> on the <code>@Operation</code> is, again, OpenAPI documentation metadata — it tells Swagger this endpoint expects the header, but the actual "logged in or guest" branching happens inside <code>GreetingServiceImpl</code>, not in a JAX-RS filter defined here.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-examplecontent-java">9. <code>rest/ExampleContent.java</code> — POST/PUT and Path Params</h2>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code>package com.mystudio.mygame.rest;

import com.mystudio.mygame.model.ExamplePostRequest;
import com.mystudio.mygame.model.ExamplePostResponse;
import com.mystudio.mygame.model.ExamplePutRequest;
import com.mystudio.mygame.model.ExamplePutResponse;
import io.swagger.v3.oas.annotations.Operation;
import io.swagger.v3.oas.annotations.tags.Tag;
import jakarta.ws.rs.*;
import jakarta.ws.rs.core.MediaType;

import java.util.Map;

import static com.mystudio.mygame.HelloWorldApplication.OPENAPI_TAG;

@Tag(name = OPENAPI_TAG)
@Path("/examplecontent")
public class ExampleContent {

    @POST
    @Consumes(MediaType.APPLICATION_JSON)
    @Produces(MediaType.APPLICATION_JSON)
    @Operation(summary = "Example POST request", description = "Example produces/consumes for POST")
    public ExamplePostResponse examplePost(ExamplePostRequest examplePostRequest) {

        //Normally we'd create a new object in the database with a POST request, but for demonstration
        //purposes, we'll just return an example response object
        final var response = new ExamplePostResponse();

        response.setName(examplePostRequest.getName());

        return response;
    }

    @PUT
    @Consumes(MediaType.APPLICATION_JSON)
    @Produces(MediaType.APPLICATION_JSON)
    @Operation(summary = "Example PUT request", description = "Example produces/consumes for PUT")
    public ExamplePutResponse examplePost(ExamplePutRequest examplePutRequest) {

        //Normally we'd overwrite an existing object in the database with a PUT request, but for demonstration
        //purposes, we'll just return an example response object
        final var response = new ExamplePutResponse();

        response.setName(examplePutRequest.getName());

        return response;
    }

    @PUT
    @Path("{name}")
    @Consumes(MediaType.APPLICATION_JSON)
    @Produces(MediaType.APPLICATION_JSON)
    @Operation(summary = "Example PUT request with a path param", description = "Example produces/consumes for PUT with a path param")
    public ExamplePutResponse examplePutWithPathParam(@PathParam("name") String name, ExamplePutRequest examplePutRequest) {

        //Normally we'd overwrite an existing object in the database with a "name" property that matches the "name" path
        // param with this PUT request, but for demonstration purposes, we'll just return an example response object
        final var response = new ExamplePutResponse();

        response.setName(examplePutRequest.getName());
        response.setMetadata(Map.of("name", name));

        return response;
    }
}</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>This resource never touches a database — its own comments say so explicitly. It exists to demonstrate JSON request/response bodies, a validated request DTO, and a <code>@PathParam</code>. One of the four request/response DTOs, <code>ExamplePutResponse</code> (in <code>model/</code>), shows the shape all of them share:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>package com.mystudio.mygame.model;

import dev.getelements.elements.sdk.model.Constants;
import io.swagger.v3.oas.annotations.media.Schema;
import jakarta.validation.constraints.NotNull;
import jakarta.validation.constraints.Pattern;

import java.util.Map;

@Schema
public class ExamplePutResponse {

    @NotNull
    @Pattern(regexp = Constants.Regexp.NO_WHITE_SPACE)
    @Schema(description = "A unique name for the object that we're creating. No spaces allowed.")
    private String name;

    @Schema(description = "The type of request being made. For example/debugging purposes.")
    private String requestType = "ExamplePutResponse";

    @Schema(description = "Any additional information to return.")
    private Map&lt;String, Object&gt; metadata;

    // getters/setters omitted for brevity
}</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p><code>@Schema</code> annotations document the field for OpenAPI generation, and <code>@NotNull</code>/<code>@Pattern</code> (using the SDK's own <code>Constants.Regexp.NO_WHITE_SPACE</code>) are standard Jakarta Bean Validation constraints, enforced automatically by the JAX-RS runtime before the resource method body even runs.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-whats-not-in-this-repo">What's Not Demonstrated in This Repo</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>To avoid sending you looking for code that isn't there, note explicitly what this example does <strong>not</strong> include, even though these are all valid Elements SDK capabilities described elsewhere in the manual:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>No WebSocket endpoint (no <code>@ServerEndpoint</code> class), despite <code>HelloWorldApplication.WS_ROOT</code> declaring a base path for one. See <a href="websockets">WebSockets</a> for the general pattern.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>No Morphia <code>@Entity</code>/DAO classes — nothing in this project persists to MongoDB directly.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>No <code>User.Level.SUPERUSER</code> check — the only level check is <code>UNPRIVILEGED</code> vs. logged-in.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>No use of <code>ElementRegistry</code> for cross-Element service discovery — <code>GreetingService</code> is only ever resolved locally.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"level":1} -->
<h1 class="wp-block-heading" id="h-the-debug-runner">The Debug Runner: <code>run.java</code></h1>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code>import dev.getelements.elements.sdk.local.ElementsLocalBuilder;

import java.io.File;
import java.io.IOException;

/**
 * Runs your local Element in the SDK.
 *
 * Working directory must be the project root (element-example/).
 * IntelliJ: Run → Edit Configurations → Working directory → set to this project root.
 */
public class run {
    public static void main(final String[] args ) throws IOException, InterruptedException {

        // Install npm dependencies on first run, then build both segment bundles.
        // The bundles are written directly to element/src/main/ui/{superuser,user}/
        // so that the Maven build triggered by local.start() picks them up.
        final var uiDir = new File("ui");

        if (!new File(uiDir, "node_modules").exists()) {
            new ProcessBuilder("npm", "install")
                    .directory(uiDir)
                    .inheritIO()
                    .start()
                    .waitFor();
        }

        new ProcessBuilder("npm", "run", "build")
                .directory(uiDir)
                .inheritIO()
                .start()
                .waitFor();

        new ProcessBuilder("docker", "compose", "up", "-d")
                .directory(new File("services-dev"))
                .inheritIO()
                .start()
                .waitFor();

        final var local = ElementsLocalBuilder.getDefault()
                .withSourceRoot()
                .withDeployment(builder -&gt; builder
                        .useDefaultRepositories(true)
                        .elementPackage()
                        .elmArtifact("com.example.element:element:elm:1.0-SNAPSHOT")
                        .endElementPackage()
                        .build()
                )
                .build();

        local.start();
        local.run();

    }

}</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>Note it's a bare, package-less class — the Javadoc comment doubles as its usage instructions. The four steps (npm install/build, docker compose, then <code>ElementsLocalBuilder</code>) were walked through in the setup section above; the key API points here are <code>.withSourceRoot()</code> (build against the local source tree, not a published artifact) and <code>.elmArtifact("com.example.element:element:elm:1.0-SNAPSHOT")</code> (the exact Maven coordinate, including the <code>elm</code> packaging type, produced by the <code>element</code> module's <code>attach-artifact</code> step described above).</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"level":1} -->
<h1 class="wp-block-heading" id="h-dashboard-ui-plugin-deep-dive">Dashboard UI Plugin Deep Dive</h1>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Elements can inject custom pages into the Elements admin dashboard by shipping a React component bundle alongside the Java code. The dashboard discovers these at runtime via a <code>plugin.json</code> manifest — no dashboard changes required. The <code>ui/</code> module is a Vite/TypeScript project that builds these bundles.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-ui-source-layout">Source Layout</h2>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code>ui/
├── package.json
├── vite.base.config.ts       # shared dev-server / library-build config factory
├── vite.superuser.config.ts  # createConfig('superuser')
├── vite.user.config.ts       # createConfig('user')
├── tsconfig.json
├── tailwind.config.ts
├── postcss.config.ts
└── src/
    ├── dev.css                # Tailwind + light/dark tokens, dev shell only
    ├── superuser/
    │   ├── ExamplePlugin.tsx  # the component shown in the dashboard
    │   ├── plugin-entry.ts    # registers the component with window.__elementsPlugins
    │   ├── dev-entry.tsx      # mounts the component for standalone dev (not shipped)
    │   └── index.html         # dev server entry point (not shipped)
    └── user/                  # same four files, simpler component</code></pre>
<!-- /wp:code -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-ui-package-json">Build Scripts</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p><code>ui/package.json</code>:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>{
  "scripts": {
    "dev:superuser": "vite --config vite.superuser.config.ts",
    "dev:user": "vite --config vite.user.config.ts",
    "build": "vite build --config vite.superuser.config.ts &amp;&amp; vite build --config vite.user.config.ts"
  }
}</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>React and its dev tooling are listed only under <code>devDependencies</code> — there's no runtime <code>dependencies</code> block. That's deliberate: the built bundle never ships its own copy of React.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-vite-base-config">The Dual-Mode Vite Config</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p><code>vite.base.config.ts</code> exports a <code>createConfig(segment)</code> factory used by both <code>vite.superuser.config.ts</code> and <code>vite.user.config.ts</code>. It branches on Vite's <code>command</code>:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>export function createConfig(segment: string) {
  return defineConfig(({ command }) =&gt; {
    if (command === 'serve') {
      // Standalone dev server with HMR: npm run dev:superuser / dev:user
      // API calls are proxied to a running Elements instance (override with ELEMENTS_URL).
      const elementsUrl = process.env.ELEMENTS_URL ?? 'http://localhost:8080'
      return {
        plugins: [react({ jsxRuntime: 'classic' })],
        root: `src/${segment}`,
        server: { proxy: { '/api': elementsUrl, '/app': elementsUrl } },
      }
    }

    // Library/IIFE build: npm run build
    return {
      esbuild: { jsx: 'transform', jsxFactory: 'React.createElement', jsxFragment: 'React.Fragment' },
      build: {
        lib: {
          entry: `src/${segment}/plugin-entry.ts`,
          name: 'ElementPlugin',
          formats: ['iife'],
          fileName: () =&gt; 'plugin.bundle.js',
        },
        outDir: `../element/src/main/ui/${segment}`,
        emptyOutDir: false,
        minify: false,
        rollupOptions: {
          external: ['react'],
          output: { globals: { react: 'window.React' } },
        },
      },
    }
  })
}</code></pre>
<!-- /wp:code -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>In <code>serve</code> mode, Vite runs a normal dev server with hot-module-reload against the segment's own root, proxying <code>/api</code> and <code>/app</code> to a running Elements instance so relative fetches work regardless of the dev server's port.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>In build mode, <code>outDir</code> resolves to <code>element/src/main/ui/{segment}</code> — this is the mechanism that gets the bundle into the right place for the antrun <code>elm-stage-static-content</code> step to pick up later. <code>emptyOutDir: false</code> so the build never deletes the <code>plugin.json</code> file sitting next to it.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>external: ['react']</code> plus <code>globals: { react: 'window.React' }</code> rewrites every <code>import React from 'react'</code> into <code>var React = window.React</code> in the compiled IIFE — the bundle never embeds its own React, it shares the host dashboard's instance.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>minify: false</code> is intentional — the shipped bundles are left readable.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-exampleplugin-tsx">The Plugin Component</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p><code>ui/src/superuser/ExamplePlugin.tsx</code> fetches an unauthenticated platform endpoint and renders the result:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>import React from 'react'

interface VersionInfo {
  version: string
  revision: string
  timestamp: string
}

export function ExamplePlugin() {
  const [info, setInfo] = React.useState&lt;VersionInfo | null&gt;(null)
  const [loading, setLoading] = React.useState(false)
  const [error, setError] = React.useState&lt;string | null&gt;(null)

  async function fetchVersion() {
    setLoading(true)
    setError(null)
    try {
      const res = await fetch('/api/rest/version')
      if (!res.ok) throw new Error(`${res.status} ${res.statusText}`)
      setInfo(await res.json())
    } catch (e) {
      setError(e instanceof Error ? e.message : String(e))
    } finally {
      setLoading(false)
    }
  }

  return (
    &lt;div className="p-6 max-w-2xl"&gt;
      &lt;h1 className="text-2xl font-bold mb-2"&gt;Example Element&lt;/h1&gt;
      &lt;p className="text-muted-foreground mb-6"&gt;
        This page is served from the Example Element&amp;rsquo;s superuser UI content directory.
      &lt;/p&gt;
      &lt;button onClick={fetchVersion} disabled={loading}&gt;
        {loading ? 'Loading…' : 'Get Platform Version'}
      &lt;/button&gt;
      {error &amp;&amp; &lt;div&gt;{error}&lt;/div&gt;}
      {info &amp;&amp; (
        &lt;div&gt;
          &lt;div&gt;Version: {info.version}&lt;/div&gt;
          &lt;div&gt;Revision: {info.revision}&lt;/div&gt;
          &lt;div&gt;Built: {info.timestamp}&lt;/div&gt;
        &lt;/div&gt;
      )}
    &lt;/div&gt;
  )
}</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>The <code>user</code> segment's version is simpler — a static informational panel with no fetch call at all. Both are wired to the dashboard's plugin registry the same way, in <code>plugin-entry.ts</code>:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>import { ExamplePlugin } from './ExamplePlugin'

declare const window: Window &amp; {
  __elementsPlugins?: {
    register(route: string, component: unknown): void
  }
}

window.__elementsPlugins?.register('example-element', ExamplePlugin)</code></pre>
<!-- /wp:code -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"Note"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="ee0a04"><div class="gb-notice-title" style="color:#fff"><p>Note</p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p>If your Element's REST endpoint requires authentication, the platform's convention is to send <code>window.__elementsApiClient.getSessionToken()</code> as an <code>Elements-SessionSecret</code> header on the fetch call (cookies alone aren't reliable in every dashboard context). This example's own fetch call only hits the unauthenticated <code>/api/rest/version</code> platform endpoint, so it doesn't exercise that pattern — if you want to call <code>/element/example/rest/api/hellowithauthentication</code> from a plugin, you'll need to add that header yourself, following the same shape shown for server-side auth elsewhere in this guide.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-plugin-json">The <code>plugin.json</code> Manifest</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>After <code>npm run build</code>, the compiled bundle lands next to a manifest at <code>element/src/main/ui/superuser/plugin.json</code> (and the equivalent under <code>user/</code>):</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>{
  "schema": "1",
  "entries": [
    {
      "label": "Example Element",
      "icon": "Package",
      "bundlePath": "plugin.bundle.js",
      "route": "example-element"
    }
  ]
}</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p><code>label</code> is the sidebar text, <code>icon</code> is any <a href="https://lucide.dev/icons/">Lucide</a> icon name, <code>bundlePath</code> is relative to the manifest, and <code>route</code> is the unique key used both in the dashboard URL (<code>/plugin/{route}</code>) and in the <code>.register(route, ...)</code> call above — the two must match. Both the manifest and the built <code>plugin.bundle.js</code> get staged into the <code>.elm</code>'s <code>ui/</code> tree by the <code>elm-stage-static-content</code> antrun execution described earlier, and served at <code>/app/ui/{element-prefix}/{segment}/</code>.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"level":1} -->
<h1 class="wp-block-heading" id="h-static-and-ui-content-serving">Static & UI Content Serving</h1>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Two source directories are copied verbatim into the <code>.elm</code> archive by the Maven build, with no extra configuration required:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><code>element/src/main/static/</code> — served at <code>/app/static/{prefix}/</code></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>element/src/main/ui/</code> — served at <code>/app/ui/{prefix}/</code> (this is where the dashboard plugin bundles live)</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>Serving behavior for both trees — index file, custom routing rules, response headers, error pages — is controlled by the <code>StaticRuleEngine</code> reading attributes from the Element's configuration:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><code>dev.getelements.static.index</code> / <code>dev.getelements.ui.index</code> — file served at the tree's context root (default <code>index.html</code>)</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>dev.getelements.static.rule.&lt;name&gt;.regex</code> / <code>dev.getelements.ui.rule.&lt;name&gt;.regex</code> — a regex rule matching file paths</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>dev.getelements.static.rule.&lt;name&gt;.header.&lt;Header&gt;.value</code> / the equivalent <code>ui</code> key — a response header template for matched files (supports <code>$filename</code>, <code>$path</code>, <code>$[0]</code>, <code>$[N]</code>)</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>dev.getelements.static.error.&lt;code&gt;</code> / the equivalent <code>ui</code> key — file served for a given HTTP error code</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>dev.getelements.element.static.uri</code> / <code>dev.getelements.element.ui.uri</code> — override the full serve URI, default <code>/app/static/{prefix}</code> / <code>/app/ui/{prefix}</code></li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>These attributes are normally embedded in the deployed <code>.elm</code> at <code>dev.getelements.element.attributes.properties</code> (element root, same level as <code>api/</code>, <code>lib/</code>, <code>classpath/</code>). This example project does not currently ship that file — if you add one, place it at <code>element/src/main/elm/dev.getelements.element.attributes.properties</code> and add an antrun copy step to <code>element/pom.xml</code> alongside the existing <code>elm-stage-*</code> executions to stage it into <code>${elm.element.dir}</code>.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-next-steps">Next Steps</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Watch the quickstart video again if any step above didn't click: <a href="https://www.youtube.com/watch?v=TqkvpwvRJEc">https://www.youtube.com/watch?v=TqkvpwvRJEc</a></p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><a href="element-structure">Custom Code Overview</a> — the broader picture of how Elements load and isolate custom code</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="introduction-to-guice-and-jakarta-in-elements">Introduction to Guice and Jakarta in Elements</a> — background on the two frameworks used throughout this example</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="structuring-your-element">Structuring Your Element</a> — a shorter, pattern-focused companion to this guide</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="packaging-an-element-with-maven">Packaging an Element with Maven</a> — more detail on the <code>.elm</code> packaging mechanism</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->
