<h1>Building the Kotlin Example Element: A Complete Walkthrough</h1>

<!-- wp:paragraph -->
<p>A complete tour of the <a href="https://github.com/NamazuStudios/element-example-kotlin">Kotlin Example Element project</a>, from a blank checkout to a running backend with a custom REST API and a dashboard plugin — written entirely in <strong>Kotlin</strong> (with one Java file, and there's a good reason for that). If you're looking for the Java version of this same walkthrough, see <a href="element-example-complete-walkthrough">Building the Example Element: A Complete Walkthrough</a>; the two projects share the same module layout and REST/Guice patterns; the differences called out here are specifically the Kotlin ones.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Prefer video? Watch the Kotlin quickstart walkthrough here:</p>
<!-- /wp:paragraph -->

<!-- wp:embed {"url":"https://www.youtube.com/watch?v=6kLWRMex-ug","type":"video","providerNameSlug":"youtube","responsive":true,"className":"wp-embed-aspect-16-9 wp-has-aspect-ratio"} -->
<figure class="wp-block-embed is-type-video is-provider-youtube wp-block-embed-youtube wp-embed-aspect-16-9 wp-has-aspect-ratio"><div class="wp-block-embed__wrapper">
https://www.youtube.com/watch?v=6kLWRMex-ug
</div></figure>
<!-- /wp:embed -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-what-youll-build">What You'll Build</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The Kotlin Example Element is a reference Custom Element for Namazu Elements 3.8, structured identically to the Java example but implemented in Kotlin. Once running locally, it gives you:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>A REST API with an open probe endpoint, an authenticated endpoint that greets the logged-in user, and a POST/PUT demo resource.</li>
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
<p>This guide covers setup end-to-end (every command below was run and verified against a live local instance), then breaks down every Maven module and every source file — Kotlin, and the one Java file — so you understand exactly what each piece does and why it's there.</p>
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
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="kt0001"><div class="gb-notice-title" style="color:#fff"><p>Note</p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p>Since Elements is a Java 21 project, we recommend <a href="https://www.jetbrains.com/idea/download/">IntelliJ</a> as your IDE, with the bundled Kotlin plugin enabled (it is by default in current IntelliJ releases). See the platform-specific setup guides (<a href="setup-for-windows">Windows</a>, <a href="mac-os-setup">Mac</a>, <a href="ubuntu-linux-setup">Linux</a>) if you haven't set up a local Elements development environment before.</p>
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
<pre class="wp-block-code"><code>git clone https://github.com/NamazuStudios/element-example-kotlin.git
cd element-example-kotlin</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>The project is a four-module Maven build, the same shape as the Java example:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>element-example-kotlin/
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
<p>This starts a single-node MongoDB 6.0.9 instance configured as replica set <code>local-test</code> on port 27017, plus a one-shot <code>rs-init</code> sidecar container that initiates the replica set. Elements' core SDK relies on multi-document transactions, which require a replica set — a plain standalone <code>mongod</code> will not work.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-4-run-the-element-locally">4. Run the Element Locally</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Run <code>debug/src/main/kotlin/run.kt</code> from your IDE, or from the command line:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>mvn -pl debug exec:java -Dexec.mainClass=Run -Dexec.classpathScope=compile</code></pre>
<!-- /wp:code -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"📝<mark style="background-color:rgba(0, 0, 0, 0)" class="has-inline-color has-black-color">Notes on the Run Command</mark>"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="kt0002"><div class="gb-notice-title" style="color:#fff"><p>📝<mark style="background-color:rgba(0, 0, 0, 0)" class="has-inline-color has-black-color">Notes on the Run Command</mark></p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p><code>debug/pom.xml</code> has no <code>exec-maven-plugin</code> binding of its own, so both flags on the command line matter. <code>-Dexec.mainClass=Run</code> is required because <code>run.kt</code> is a top-level file (no wrapping class) annotated <code>@file:JvmName("Run")</code>, which is what makes the compiled facade class <code>Run</code> rather than the default <code>RunKt</code>. <code>-Dexec.classpathScope=compile</code> is also required: the SDK's local-runtime deployment classes this entrypoint calls into (e.g. <code>dev.getelements.elements.sdk.deployment.*</code>) come from artifacts scoped <code>provided</code>, and <code>exec:java</code>'s default classpath scope is <code>runtime</code>, which excludes <code>provided</code> dependencies — running with the default scope fails with a <code>NoClassDefFoundError</code>. The working directory must also be the project root (<code>element-example-kotlin/</code>), because <code>run.kt</code> shells out to <code>npm</code> in <code>ui/</code> and <code>docker compose</code> in <code>services-dev/</code> using relative paths. In IntelliJ: Run → Edit Configurations → set the working directory to the project root.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:paragraph -->
<p>When it runs, <code>run.kt</code> does the following, in order:</p>
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
<pre class="wp-block-code"><code>curl http://localhost:8080/app/rest/example-element/helloworld
# =&gt; Hello world!</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>Hit the authenticated endpoint without a session — you'll be greeted as a guest:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>curl http://localhost:8080/app/rest/example-element/hellowithauthentication
# =&gt; Hello, Guest!</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>Create a user, log in to get a session secret, then call it again with the <code>Elements-SessionSecret</code> header — see <a href="creating-a-user">Creating a User</a> and <a href="user-authentication-in-elements">User Authentication in Elements</a> if you need a refresher on those two calls. With a valid session you should get <code>Hello, &lt;your name&gt;!</code> instead of <code>Hello, Guest!</code>.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Try the POST/PUT demo resource:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>curl -X POST http://localhost:8080/app/rest/example-element/examplecontent 
  -H "Content-Type: application/json" -d '{"name":"test-name"}'
# =&gt; {"name":"test-name","requestType":"ExamplePostResponse","metadata":null}

curl -X PUT http://localhost:8080/app/rest/example-element/examplecontent/pathname 
  -H "Content-Type: application/json" -d '{"name":"test-name"}'
# =&gt; {"name":"test-name","requestType":"ExamplePutResponse","metadata":{"name":"pathname"}}</code></pre>
<!-- /wp:code -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"Note"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="kt0003"><div class="gb-notice-title" style="color:#fff"><p>Note</p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p>Every endpoint in this project mounts under <code>/app/rest/example-element/</code> — the path is built from the platform's standard <code>/app/rest/{prefix}</code> convention using the <code>dev.getelements.elements.app.serve.prefix</code> attribute (default <code>example-element</code>, set in <code>HelloWorldApplication</code>), not from the <code>RS_ROOT</code>/<code>WS_ROOT</code> constants also declared there. Those two constants set <code>dev.getelements.elements.element.rs.root</code>/<code>...ws.root</code>, which is separate configuration and does not change the externally observed mount path — don't be misled by their default values (<code>/element/example/api</code>, <code>/element/example/ws</code>) into expecting the API there.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

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
<p>The root is a pure aggregator (<code>&lt;packaging&gt;pom&lt;/packaging&gt;</code>) with no parent of its own. It declares the four modules, pins shared properties — including a Kotlin-specific one — and centralizes dependency versions and scopes:</p>
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
    &lt;kotlin.version&gt;2.1.0&lt;/kotlin.version&gt;
    &lt;elements.version&gt;3.8.14&lt;/elements.version&gt;
    &lt;api.classifier&gt;${project.groupId}.api&lt;/api.classifier&gt;
    &lt;!-- swagger.version, guice.version, rs.api, jakarta.websocket.version,
         crossfire.version, servlet.api, logback.version also declared here --&gt;
&lt;/properties&gt;

&lt;build&gt;
    &lt;pluginManagement&gt;
        &lt;plugins&gt;
            &lt;plugin&gt;
                &lt;groupId&gt;org.jetbrains.kotlin&lt;/groupId&gt;
                &lt;artifactId&gt;kotlin-maven-plugin&lt;/artifactId&gt;
                &lt;version&gt;${kotlin.version}&lt;/version&gt;
                &lt;configuration&gt;
                    &lt;jvmTarget&gt;21&lt;/jvmTarget&gt;
                &lt;/configuration&gt;
            &lt;/plugin&gt;
        &lt;/plugins&gt;
    &lt;/pluginManagement&gt;
&lt;/build&gt;</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>The <code>kotlin-maven-plugin</code> version and <code>jvmTarget</code> are pinned once here in <code>pluginManagement</code> so every child module's declaration of the plugin (they all need one, since Kotlin isn't a default Maven language) inherits the same version and target without repeating it. The rest of <code>dependencyManagement</code> imports the Elements SDK BOM, exactly as in the Java example, plus one extra managed dependency:</p>
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

        &lt;dependency&gt;
            &lt;groupId&gt;org.jetbrains.kotlin&lt;/groupId&gt;
            &lt;artifactId&gt;kotlin-stdlib&lt;/artifactId&gt;
            &lt;version&gt;${kotlin.version}&lt;/version&gt;
        &lt;/dependency&gt;
    &lt;/dependencies&gt;
&lt;/dependencyManagement&gt;</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>The managed <code>kotlin-stdlib</code> dependency has no <code>&lt;scope&gt;</code> pinned here — each module scopes it independently (<code>provided</code> in <code>api</code> and <code>debug</code>'s own declarations are unscoped/compile, matching what each module actually needs), which is why you'll see the scope vary slightly module to module below.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-api-pom-xml-the-classified-jar">The <code>api</code> Module: a Classified Kotlin Jar</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p><code>api/pom.xml</code> depends on nothing but the core SDK and the Kotlin standard library (both scope <code>provided</code>) — by design. The API module should stay as lean as possible, since every API jar in a deployment shares a common classpath with every other Element's API jar; bloating it with third-party libraries invites classpath conflicts across unrelated Elements. In this project, <code>api</code>'s entire source is a single dependency-free Kotlin interface (see the source deep dive below) — it doesn't even need the SDK to compile.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>The build configuration points the source directory at Kotlin and runs the Kotlin compiler, then produces the same "classified jar" as the Java example:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>&lt;build&gt;
    &lt;sourceDirectory&gt;${project.basedir}/src/main/kotlin&lt;/sourceDirectory&gt;
    &lt;plugins&gt;
        &lt;plugin&gt;
            &lt;groupId&gt;org.jetbrains.kotlin&lt;/groupId&gt;
            &lt;artifactId&gt;kotlin-maven-plugin&lt;/artifactId&gt;
            &lt;executions&gt;
                &lt;execution&gt;
                    &lt;id&gt;compile&lt;/id&gt;
                    &lt;goals&gt;&lt;goal&gt;compile&lt;/goal&gt;&lt;/goals&gt;
                &lt;/execution&gt;
            &lt;/executions&gt;
        &lt;/plugin&gt;
        &lt;plugin&gt;
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
        &lt;/plugin&gt;
    &lt;/plugins&gt;
&lt;/build&gt;</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>This produces a <strong>second</strong> jar — <code>api-1.0-SNAPSHOT-com.example.element.api.jar</code> — in addition to the normal jar, exactly like the Java example. The plain jar is what <code>element</code> compiles against; the classified jar is what gets copied into the <code>api/</code> directory inside the final <code>.elm</code> archive.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-element-pom-xml-dependencies">The <code>element</code> Module: Dependencies</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p><code>element/pom.xml</code> declares the same dependency set as the Java example, plus <code>kotlin-stdlib</code> (unscoped — it's needed at both compile and runtime here, unlike in <code>api</code>):</p>
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
    &lt;dependency&gt;&lt;groupId&gt;org.jetbrains.kotlin&lt;/groupId&gt;&lt;artifactId&gt;kotlin-stdlib&lt;/artifactId&gt;&lt;/dependency&gt;
&lt;/dependencies&gt;</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>The BOM scopes <code>sdk</code>, <code>sdk-model</code>, <code>sdk-service</code>, <code>sdk-spi-guice</code>, and <code>sdk-jakarta-rs</code> as <code>provided</code> — the Elements runtime already has these on its classpath, so they're compiled against but never bundled. Because <code>kotlin-stdlib</code> is <strong>not</strong> <code>provided</code> here, it ends up in the <code>.elm</code>'s <code>lib/</code> directory — the Elements runtime itself has no reason to ship Kotlin's standard library, so this Element must bring its own.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-compiling-kotlin-and-java-together">Compiling Kotlin and Java Together</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>This is the one build detail with no equivalent in the Java example. Almost everything in <code>element</code> is Kotlin, but <code>package-info.java</code> (below) has to stay Java, because package-level annotations use <code>ElementType.PACKAGE</code>, a target Kotlin does not support. <code>element/pom.xml</code> handles this with two separate compiler executions, run in a specific order:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>&lt;plugin&gt;
    &lt;groupId&gt;org.jetbrains.kotlin&lt;/groupId&gt;
    &lt;artifactId&gt;kotlin-maven-plugin&lt;/artifactId&gt;
    &lt;executions&gt;
        &lt;execution&gt;
            &lt;id&gt;compile&lt;/id&gt;
            &lt;goals&gt;&lt;goal&gt;compile&lt;/goal&gt;&lt;/goals&gt;
            &lt;configuration&gt;
                &lt;sourceDirs&gt;
                    &lt;sourceDir&gt;${project.basedir}/src/main/kotlin&lt;/sourceDir&gt;
                    &lt;sourceDir&gt;${project.basedir}/src/main/java&lt;/sourceDir&gt;
                &lt;/sourceDirs&gt;
            &lt;/configuration&gt;
        &lt;/execution&gt;
    &lt;/executions&gt;
&lt;/plugin&gt;

&lt;!--
    Compiles Java sources (e.g. package-info.java) after Kotlin compilation.
    package-info.java is required for package-level annotations such as @ElementDefinition,
    which use ElementType.PACKAGE — a target that Kotlin does not natively support.
--&gt;
&lt;plugin&gt;
    &lt;groupId&gt;org.apache.maven.plugins&lt;/groupId&gt;
    &lt;artifactId&gt;maven-compiler-plugin&lt;/artifactId&gt;
    &lt;executions&gt;
        &lt;execution&gt;
            &lt;id&gt;default-compile&lt;/id&gt;
            &lt;phase&gt;none&lt;/phase&gt;
        &lt;/execution&gt;
        &lt;execution&gt;
            &lt;id&gt;java-compile&lt;/id&gt;
            &lt;phase&gt;compile&lt;/phase&gt;
            &lt;goals&gt;&lt;goal&gt;compile&lt;/goal&gt;&lt;/goals&gt;
            &lt;configuration&gt;
                &lt;sourceDirs&gt;
                    &lt;sourceDir&gt;${project.basedir}/src/main/java&lt;/sourceDir&gt;
                &lt;/sourceDirs&gt;
            &lt;/configuration&gt;
        &lt;/execution&gt;
    &lt;/executions&gt;
&lt;/plugin&gt;</code></pre>
<!-- /wp:code -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>The Kotlin plugin's <code>compile</code> execution is given <strong>both</strong> source directories. It compiles the actual Kotlin files and, since <code>package-info.java</code> references <code>MyGameModule</code> (a Kotlin class) in <code>@GuiceElementModule(MyGameModule.class)</code>, Kotlin needs to see that Java file exists during its own compilation pass to resolve the reference — but Kotlin's compiler does not generate a <code>.class</code> file for <code>package-info.java</code> itself.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>The Maven Compiler Plugin's built-in <code>default-compile</code> execution is disabled (<code>&lt;phase&gt;none&lt;/phase&gt;</code>) so it doesn't try to recompile everything with plain <code>javac</code>. A second, explicit execution named <code>java-compile</code> is bound to the same <code>compile</code> phase, restricted to just <code>src/main/java</code> — in practice, just <code>package-info.java</code>.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Because the Kotlin plugin is declared first in <code>&lt;plugins&gt;</code>, it runs before the compiler plugin within the same <code>compile</code> phase. By the time <code>java-compile</code> runs <code>javac</code> against <code>package-info.java</code>, <code>MyGameModule.class</code> already exists on the output classpath, so the reference resolves.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"Note"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="kt0004"><div class="gb-notice-title" style="color:#fff"><p>Note</p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p>If you add more Java files to your own Kotlin Element, this is the pattern to keep: Kotlin source under <code>src/main/kotlin</code>, any package-level-annotation-only Java under <code>src/main/java</code>, and both compiler executions pointed at the right directories in the right order. You almost never need more Java than a single <code>package-info.java</code>.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-the-elm-packaging-pipeline">The <code>.elm</code> Packaging Pipeline</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Like the Java example, this project builds its <code>.elm</code> archive with the Maven Antrun Plugin and Maven Dependency Plugin — no assembly descriptor. The staging properties are identical:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>&lt;elm.staging.dir&gt;${project.build.directory}/${project.groupId}.${project.artifactId}-${project.version}&lt;/elm.staging.dir&gt;
&lt;elm.element.dir&gt;${elm.staging.dir}/${project.groupId}.${project.artifactId}&lt;/elm.element.dir&gt;</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>i.e. <code>target/com.example.element.element-1.0-SNAPSHOT/com.example.element.element/</code>. The Maven Dependency Plugin runs <strong>three</strong> <code>copy-dependencies</code> executions here, not two — the extra one is Kotlin-specific:</p>
<!-- /wp:paragraph -->

<!-- wp:list {"ordered":true} -->
<ol class="wp-block-list"><!-- wp:list-item -->
<li><code>elm-copy-api-deps</code> — copies this project's own <code>${api.classifier}</code>-classified jar into <code>&lt;elm.element.dir&gt;/api</code>.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>elm-copy-kotlin-stdlib-api</code> — copies every <code>org.jetbrains.kotlin</code>-groupId dependency (i.e. <code>kotlin-stdlib</code>) into <code>&lt;elm.element.dir&gt;/api</code> as well, alongside the API jar.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>elm-copy-lib-deps</code> — copies every non-<code>provided</code>-scope dependency into <code>&lt;elm.element.dir&gt;/lib</code>, prepending the group id to each filename (this also picks up <code>kotlin-stdlib</code> a second time, for <code>element</code>'s own runtime classpath).</li>
<!-- /wp:list-item --></ol>
<!-- /wp:list -->

<!-- wp:code -->
<pre class="wp-block-code"><code>&lt;execution&gt;
    &lt;id&gt;elm-copy-kotlin-stdlib-api&lt;/id&gt;
    &lt;phase&gt;prepare-package&lt;/phase&gt;
    &lt;goals&gt;&lt;goal&gt;copy-dependencies&lt;/goal&gt;&lt;/goals&gt;
    &lt;configuration&gt;
        &lt;outputDirectory&gt;${elm.element.dir}/api&lt;/outputDirectory&gt;
        &lt;includeGroupIds&gt;org.jetbrains.kotlin&lt;/includeGroupIds&gt;
        &lt;prependGroupId&gt;true&lt;/prependGroupId&gt;
    &lt;/configuration&gt;
&lt;/execution&gt;</code></pre>
<!-- /wp:code -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"📝<mark style="background-color:rgba(0, 0, 0, 0)" class="has-inline-color has-black-color">Why kotlin-stdlib Goes in api/ Too</mark>"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="kt0005"><div class="gb-notice-title" style="color:#fff"><p>📝<mark style="background-color:rgba(0, 0, 0, 0)" class="has-inline-color has-black-color">Why kotlin-stdlib Goes in api/ Too</mark></p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p>The <code>api/</code> directory's classes load in a separate, shared classloader visible to every deployed Element (that's the whole point of exporting an API). If your Element exports Kotlin types — data classes, enums, sealed classes — through its classified API jar, and the platform or another Element reflects over those types at startup (Swagger scanning response bodies is the common case), that shared API classloader needs <code>kotlin-stdlib</code> on it too, or you get <code>NoClassDefFoundError: kotlin/jvm/internal/Intrinsics</code> at startup. This project's own <code>api</code> module (<code>GreetingService</code>) doesn't actually need this — it's a bare interface with no Kotlin-specific runtime dependency — but the execution is harmless to keep (it only adds about 1&nbsp;MB) and saves you from rediscovering this the hard way the first time you add a Kotlin data class to your own <code>api</code> module.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:paragraph -->
<p>The remaining antrun executions (stage classpath, stage static/UI content, write the manifest, zip the archive) and the <code>build-helper-maven-plugin</code> <code>attach-artifact</code> step are identical to the Java example — see <a href="packaging-an-element-with-maven">Packaging an Element with Maven</a> for the full breakdown of that shared mechanism. The resulting archive layout is the same, plus the extra <code>kotlin-stdlib</code> jar under <code>api/</code>:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>com.example.element.element/
  api/            &lt;- classified API jar(s) + kotlin-stdlib
  lib/            &lt;- bundled runtime jars (non-provided scope, incl. kotlin-stdlib)
  classpath/      &lt;- compiled classes + src/main/resources
  static/         &lt;- src/main/static
  ui/             &lt;- src/main/ui (superuser/user plugin bundles)
  dev.getelements.element.manifest.properties</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>This is exactly the coordinate <code>run.kt</code> references with <code>.elmArtifact("com.example.element:element:elm:1.0-SNAPSHOT")</code>, and the coordinate you'd reference from a deployment configuration or <code>mvn deploy</code>.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-debug-pom-xml">The <code>debug</code> Module</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p><code>debug/pom.xml</code> intentionally depends on nothing but the SDK's local runtime, Kotlin's standard library, and logging:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>&lt;build&gt;
    &lt;sourceDirectory&gt;${project.basedir}/src/main/kotlin&lt;/sourceDirectory&gt;
    &lt;plugins&gt;
        &lt;plugin&gt;
            &lt;groupId&gt;org.jetbrains.kotlin&lt;/groupId&gt;
            &lt;artifactId&gt;kotlin-maven-plugin&lt;/artifactId&gt;
            &lt;executions&gt;
                &lt;execution&gt;
                    &lt;id&gt;compile&lt;/id&gt;
                    &lt;goals&gt;&lt;goal&gt;compile&lt;/goal&gt;&lt;/goals&gt;
                &lt;/execution&gt;
            &lt;/executions&gt;
        &lt;/plugin&gt;
    &lt;/plugins&gt;
&lt;/build&gt;

&lt;dependencies&gt;
    &lt;dependency&gt;&lt;groupId&gt;org.jetbrains.kotlin&lt;/groupId&gt;&lt;artifactId&gt;kotlin-stdlib&lt;/artifactId&gt;&lt;scope&gt;compile&lt;/scope&gt;&lt;/dependency&gt;
    &lt;dependency&gt;&lt;groupId&gt;dev.getelements.elements&lt;/groupId&gt;&lt;artifactId&gt;sdk-local&lt;/artifactId&gt;&lt;scope&gt;compile&lt;/scope&gt;&lt;/dependency&gt;
    &lt;dependency&gt;&lt;groupId&gt;dev.getelements.elements&lt;/groupId&gt;&lt;artifactId&gt;sdk-local-maven&lt;/artifactId&gt;&lt;scope&gt;compile&lt;/scope&gt;&lt;/dependency&gt;
    &lt;dependency&gt;&lt;groupId&gt;dev.getelements.elements&lt;/groupId&gt;&lt;artifactId&gt;sdk-logback&lt;/artifactId&gt;&lt;scope&gt;compile&lt;/scope&gt;&lt;/dependency&gt;
    &lt;dependency&gt;&lt;groupId&gt;ch.qos.logback&lt;/groupId&gt;&lt;artifactId&gt;logback-classic&lt;/artifactId&gt;&lt;scope&gt;compile&lt;/scope&gt;&lt;/dependency&gt;
&lt;/dependencies&gt;</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>The module's own comments sum up its purpose: <code>sdk-local</code> is a thin wrapper around a fully configured instance of Namazu Elements, and this configuration should almost never need changes. It's never deployed — it exists purely so you can boot the whole platform, with your Element loaded from source, inside your IDE (or via <code>mvn exec:java</code>, per the run command above).</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-ui-pom-xml">The <code>ui</code> Module</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p><code>ui/pom.xml</code> is identical in shape to the Java example's — it's a TypeScript/Vite project, so the backend language of the Element it's paired with is irrelevant to it. It's packaged <code>pom</code> and does no work by default; its <code>build-ui</code> profile uses <code>frontend-maven-plugin</code> to install a pinned Node version (v22.14.0) and run the npm build, for CI/release environments that shouldn't depend on the machine's own Node install. Activate it with:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>mvn install -Pbuild-ui</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>Day to day, developers run <code>npm</code> directly in <code>ui/</code> (or let <code>run.kt</code> do it) using their own Node install — this profile exists for build environments that shouldn't assume Node is already present. See the <a href="#h-dashboard-ui-plugin-deep-dive">Dashboard UI Plugin Deep Dive</a> section below for the full build pipeline.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"level":1} -->
<h1 class="wp-block-heading" id="h-kotlin-source-deep-dive">Kotlin Source Deep Dive</h1>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>All source lives under <code>com.mystudio.mygame</code>, split across the <code>api</code> and <code>element</code> modules. Every file is Kotlin, with exactly one exception. Here's every file, in the order you'd read them to understand how the pieces connect.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-package-info-java">1. <code>package-info.java</code> — Declaring the Element (the One Java File)</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p><code>element/src/main/java/com/mystudio/mygame/package-info.java</code> — note the path: <code>src/main/java</code>, not <code>src/main/kotlin</code>:</p>
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

<!-- wp:paragraph -->
<p>This file is byte-for-byte the same idea as the Java example's — it just can't be Kotlin. Kotlin has no <code>package-info.kt</code> equivalent because package-level annotations require <code>ElementType.PACKAGE</code> as a target, which the Kotlin annotation model doesn't support. Every Kotlin Element project needs this one Java file (see the Maven deep dive above for how the build compiles it alongside the Kotlin sources).</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><code>@ElementDefinition(recursive = true)</code> is what makes the SDK's classloading/discovery mechanism recognize <code>com.mystudio.mygame</code> — and every sub-package, because <code>recursive = true</code> — as one Element.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>@GuiceElementModule(MyGameModule.class)</code> tells the SDK which Guice module to install when it bootstraps this Element's private injector — pointing at the Kotlin class below.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>The two <code>@ElementDependency</code> annotations declare dependencies on other Elements — the core SDK's DAO layer and service layer. This is what lets <code>GreetingServiceImpl</code> (below) <code>@Inject</code> the SDK's own <code>UserService</code> even though this Element never binds it itself.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-mygamemodule-kt">2. <code>guice/MyGameModule.kt</code> — the Guice Module</h2>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code>package com.mystudio.mygame.guice

import com.google.inject.PrivateModule
import com.mystudio.mygame.service.GreetingService
import com.mystudio.mygame.service.GreetingServiceImpl

class MyGameModule : PrivateModule() {

    override fun configure() {
        bind(GreetingService::class.java).to(GreetingServiceImpl::class.java)
        expose(GreetingService::class.java)
    }

}</code></pre>
<!-- /wp:code -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Same pattern as the Java example — <code>PrivateModule</code>, not plain <code>AbstractModule</code> — just Kotlin's <code>: PrivateModule()</code> constructor-call syntax and <code>::class.java</code> in place of Java's <code>.class</code>.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>expose(GreetingService::class.java)</code> is required precisely <strong>because</strong> this is a <code>PrivateModule</code> — nothing inside one is visible outside it by default. Without this call, the service-locator lookup in <code>HelloWithAuthentication</code> (below) would fail.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-greetingservice-kt">3. <code>service/GreetingService.kt</code> — the API Interface</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Lives in the <strong>api</strong> module (<code>api/src/main/kotlin/com/mystudio/mygame/service/GreetingService.kt</code>), and is the <em>entire</em> contents of that module:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>package com.mystudio.mygame.service

interface GreetingService {

    /**
     * Attempts to fetch the current user for the session header and return an appropriate greeting
     * @return The greeting based on if a logged-in user is found
     */
    fun getGreeting(): String

}</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>Zero imports, zero SDK dependency — this is what "keep the API module lean" looks like taken to its logical conclusion. Like the Java example, <code>@ElementServiceExport</code> is applied to the implementation, not this interface.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-greetingserviceimpl-kt">4. <code>service/GreetingServiceImpl.kt</code> — the Implementation</h2>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code>package com.mystudio.mygame.service

import dev.getelements.elements.sdk.annotation.ElementServiceExport
import dev.getelements.elements.sdk.model.user.User
import dev.getelements.elements.sdk.service.user.UserService
import jakarta.inject.Inject

@ElementServiceExport(GreetingService::class)
class GreetingServiceImpl : GreetingService {

    private lateinit var userService: UserService

    @Inject
    fun setUserService(userService: UserService) {
        this.userService = userService
    }

    override fun getGreeting(): String {
        // Because we set the dev.getelements.elements.auth.enabled attribute to "true" in the HelloWorldApplication,
        // the UserService will be automatically injected with the current user. This will apply an authentication
        // filter to every request and every service that is used in this application.
        val currentUser: User = userService.getCurrentUser()
        val isLoggedIn = currentUser.level != User.Level.UNPRIVILEGED
        val name = if (isLoggedIn) currentUser.name else "Guest"
        return "Hello, $name!"
    }

}</code></pre>
<!-- /wp:code -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><code>@ElementServiceExport(GreetingService::class)</code> exposes this concrete class under the <code>GreetingService</code> service-locator key — the annotation-driven counterpart to the explicit <code>bind()</code>/<code>expose()</code> calls in <code>MyGameModule</code>. Both point at the same binding.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>private lateinit var userService: UserService</code> paired with a separate <code>@Inject</code>-annotated setter is Kotlin's version of Guice setter injection. <code>lateinit</code> is required here because Kotlin non-nullable properties must otherwise be initialized in the constructor — this tells the compiler "trust me, this will be set before it's read," which holds because Guice calls <code>setUserService</code> during injector construction, before any request reaches <code>getGreeting()</code>.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>currentUser.level</code> and <code>currentUser.name</code> are Kotlin property syntax calling the underlying Java getters (<code>getLevel()</code>, <code>getName()</code>) on the SDK's <code>User</code> model class — Kotlin automatically exposes JavaBean-style getters/setters as properties.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>The auth check is identical to the Java example: compare <code>currentUser.level</code> against <code>User.Level.UNPRIVILEGED</code> to distinguish a real logged-in user from an anonymous/guest request. This is the only auth-level check in the project — it does not check for <code>SUPERUSER</code>.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-helloworldapplication-kt">5. <code>HelloWorldApplication.kt</code> — Registering Endpoints</h2>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code>package com.mystudio.mygame

import com.mystudio.mygame.rest.ExampleContent
import com.mystudio.mygame.rest.HelloWithAuthentication
import com.mystudio.mygame.rest.HelloWorld
import dev.getelements.elements.sdk.annotation.ElementDefaultAttribute
import dev.getelements.elements.sdk.annotation.ElementServiceExport
import dev.getelements.elements.sdk.annotation.ElementServiceImplementation
import jakarta.ws.rs.core.Application

@ElementServiceImplementation
@ElementServiceExport(Application::class)
class HelloWorldApplication : Application() {

    companion object {

        @JvmField
        @ElementDefaultAttribute("true")
        val AUTH_ENABLED: String = "dev.getelements.elements.auth.enabled"

        @JvmField
        @ElementDefaultAttribute("example-element")
        val APPLICATION_PREFIX: String = "dev.getelements.elements.app.serve.prefix"

        @JvmField
        @ElementDefaultAttribute("/element/example/api")
        val RS_ROOT: String = "dev.getelements.elements.element.rs.root"

        @JvmField
        @ElementDefaultAttribute("/element/example/ws")
        val WS_ROOT: String = "dev.getelements.elements.element.ws.root"

        @JvmField
        @ElementDefaultAttribute("/app/static/test/path")
        val STATIC_CONTENT_URI: String = "dev.getelements.element.static.uri"

        @JvmField
        @ElementDefaultAttribute("/app/ui/test/path")
        val UI_CONTENT_URI: String = "dev.getelements.element.ui.uri"

        const val OPENAPI_TAG: String = "Example"
    }

    /**
     * Here we register all the classes that we want to be included in the Element.
     */
    override fun getClasses(): Set&lt;Class&lt;*&gt;&gt; = setOf(
        // Endpoints
        HelloWorld::class.java,
        HelloWithAuthentication::class.java,
        ExampleContent::class.java,

        // Exposes the default security rules for the API. Assumes you are using the builtin Elements auth
        // system by setting `dev.getelements.elements.auth.enabled` to true in the annotation above.
        OpenAPISecurityConfig::class.java
    )

}</code></pre>
<!-- /wp:code -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>The attribute-key constants live in a Kotlin <code>companion object</code> rather than as <code>static final</code> fields directly on the class, since Kotlin classes have no native concept of static members.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Each one is also marked <code>@JvmField</code>. Without it, a Kotlin <code>val</code> in a companion object compiles to a <em>private</em> backing field plus a generated getter method — the SDK's annotation processor reads fields directly via reflection, not getters, so it would find nothing. <code>@JvmField</code> tells the compiler to expose the property as a plain public static field instead, exactly like the Java example's <code>public static final String AUTH_ENABLED = "...";</code>.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>OPENAPI_TAG</code> uses <code>const val</code> instead — since it has no <code>@ElementDefaultAttribute</code> annotation to preserve field access for, a Kotlin compile-time constant (which <em>does</em> compile to a plain static final field automatically) works without <code>@JvmField</code>.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>AUTH_ENABLED</code> defaulting to <code>"true"</code> is what turns on the built-in Elements auth filter for every request and service in this Element — exactly what makes <code>UserService.getCurrentUser()</code> populated in <code>GreetingServiceImpl</code>. As noted in the setup section above, <code>APPLICATION_PREFIX</code> (not <code>RS_ROOT</code>) is what actually determines the externally observed <code>/app/rest/{prefix}/...</code> mount path.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>getClasses()</code> is a Kotlin expression-body override returning a <code>Set&lt;Class&lt;*&gt;&gt;</code> — Kotlin's star-projection <code>*</code> stands in for Java's unbounded wildcard <code>Class&lt;?&gt;</code>.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-openapisecurityconfig-kt">6. <code>OpenAPISecurityConfig.kt</code> — Documenting the Auth Scheme</h2>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code>package com.mystudio.mygame

import dev.getelements.elements.sdk.jakarta.rs.AuthSchemes.SESSION_SECRET
import io.swagger.v3.oas.annotations.ExternalDocumentation
import io.swagger.v3.oas.annotations.OpenAPIDefinition
import io.swagger.v3.oas.annotations.info.Contact
import io.swagger.v3.oas.annotations.info.Info
import io.swagger.v3.oas.annotations.security.SecurityRequirement
import io.swagger.v3.oas.annotations.security.SecurityScheme
import io.swagger.v3.oas.annotations.security.SecuritySchemes
import io.swagger.v3.oas.annotations.enums.SecuritySchemeIn.HEADER
import io.swagger.v3.oas.annotations.enums.SecuritySchemeType.APIKEY

@OpenAPIDefinition(
    info = Info(
        title = "Example Element",
        description = "An example element.",
        contact = Contact(
            url = "https://namazustudios.com",
            email = "info@namazustudios.com",
            name = "Namazu Studios"
        )
    ),
    externalDocs = ExternalDocumentation(
        url = "https://namazustudios.com/docs",
        description = "Please see the Namazu Elements Manual for more information."
    ),
    security = [
        SecurityRequirement(name = SESSION_SECRET)
    ]
)
@SecuritySchemes(
    value = [SecurityScheme(
        type = APIKEY,
        `in` = HEADER,
        name = SESSION_SECRET,
        paramName = SESSION_SECRET,
        description = "Session secret required for authenticated endpoints"
    )]
)
class OpenAPISecurityConfig</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>Functionally identical to the Java example — a class with no fields or methods, registered in <code>getClasses()</code> purely so Swagger's scanner picks up its class-level annotations describing the <code>Elements-SessionSecret</code> API-key security scheme. Two Kotlin syntax notes:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Annotation array attributes use Kotlin's <code>[ ... ]</code> collection-literal syntax (<code>security = [SecurityRequirement(...)]</code>) instead of Java's <code>{ ... }</code> array-initializer syntax.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>in</code> is a reserved keyword in Kotlin (used for ranges and <code>for</code> loops), but it's also the actual parameter name on <code>@SecurityScheme</code>. Wrapping it in backticks — <code>`in` = HEADER</code> — lets Kotlin use it as an identifier anyway.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-helloworld-kt">7. <code>rest/HelloWorld.kt</code> — an Open Probe Endpoint</h2>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code>package com.mystudio.mygame.rest

import com.mystudio.mygame.HelloWorldApplication
import io.swagger.v3.oas.annotations.Operation
import io.swagger.v3.oas.annotations.tags.Tag
import jakarta.ws.rs.GET
import jakarta.ws.rs.Path
import jakarta.ws.rs.Produces
import jakarta.ws.rs.core.MediaType

@Tag(name = HelloWorldApplication.OPENAPI_TAG)
@Path("/helloworld")
class HelloWorld {

    @GET
    @Produces(MediaType.TEXT_PLAIN)
    @Operation(summary = "Hello world probe", description = "Returns a simple greeting")
    fun sayHello(): String = "Hello world!"

}</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>The simplest possible JAX-RS resource — a plain <code>GET</code> returning static text via a one-line expression body, useful as a health probe. It declares no <code>@SecurityRequirement</code>, so it's reachable even though this Element enables auth globally: authorization here works by <em>services</em> resolving the current user (or not) rather than a filter rejecting unauthenticated requests outright. Note it references <code>HelloWorldApplication.OPENAPI_TAG</code> directly as a companion-object member, Kotlin's equivalent of Java's <code>static</code> field access — no <code>import static</code> needed.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-hellowithauthentication-kt">8. <code>rest/HelloWithAuthentication.kt</code> — the Service Locator Pattern</h2>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code>package com.mystudio.mygame.rest

import com.mystudio.mygame.HelloWorldApplication
import com.mystudio.mygame.service.GreetingService
import dev.getelements.elements.sdk.Element
import dev.getelements.elements.sdk.ElementSupplier
import dev.getelements.elements.sdk.jakarta.rs.AuthSchemes.SESSION_SECRET
import io.swagger.v3.oas.annotations.Operation
import io.swagger.v3.oas.annotations.security.SecurityRequirement
import io.swagger.v3.oas.annotations.tags.Tag
import jakarta.ws.rs.Consumes
import jakarta.ws.rs.GET
import jakarta.ws.rs.Path
import jakarta.ws.rs.Produces
import jakarta.ws.rs.core.MediaType

@Tag(name = HelloWorldApplication.OPENAPI_TAG)
@Path("/hellowithauthentication")
class HelloWithAuthentication {

    private val element: Element = ElementSupplier
        .getElementLocal(HelloWithAuthentication::class.java)
        .get()

    private val greetingService: GreetingService = element
        .serviceLocator
        .getInstance(GreetingService::class.java)

    @GET
    @Produces(MediaType.TEXT_PLAIN)
    @Consumes(MediaType.TEXT_PLAIN)
    @Operation(
        summary = "Greeting with login check",
        description = "Checks if the session token in the header corresponds to at least a USER level user.",
        security = [SecurityRequirement(name = SESSION_SECRET)]
    )
    fun sayHelloWithAuth(): String = greetingService.getGreeting()

}</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>This is the class to study for the service-locator pattern:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><code>ElementSupplier.getElementLocal(HelloWithAuthentication::class.java).get()</code> resolves the <code>Element</code> instance associated with the calling class's classloader. This is necessary because JAX-RS resources are instantiated by the Jakarta RS container, not by Guice — so they can't use <code>@Inject</code> directly. This static lookup is how a plain, container-instantiated resource reaches back into its own Element's private Guice injector.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>element.serviceLocator.getInstance(GreetingService::class.java)</code> then pulls the singleton out of that injector — <code>element.serviceLocator</code> is Kotlin property syntax calling the Java interface's <code>getServiceLocator()</code> getter. This only works because <code>MyGameModule</code> called <code>expose(GreetingService::class.java)</code> — remove that call and this lookup throws.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Both lookups happen in property initializers (<code>private val element = ...</code>), run once when the resource is constructed, rather than in a Kotlin <code>init</code> block or lazily — same eager-initialization behavior as the Java example's <code>final</code> fields.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-examplecontent-kt">9. <code>rest/ExampleContent.kt</code> — POST/PUT and Path Params</h2>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code>package com.mystudio.mygame.rest

import com.mystudio.mygame.HelloWorldApplication
import com.mystudio.mygame.model.ExamplePostRequest
import com.mystudio.mygame.model.ExamplePostResponse
import com.mystudio.mygame.model.ExamplePutRequest
import com.mystudio.mygame.model.ExamplePutResponse
import io.swagger.v3.oas.annotations.Operation
import io.swagger.v3.oas.annotations.tags.Tag
import jakarta.ws.rs.*
import jakarta.ws.rs.core.MediaType

@Tag(name = HelloWorldApplication.OPENAPI_TAG)
@Path("/examplecontent")
class ExampleContent {

    @POST
    @Consumes(MediaType.APPLICATION_JSON)
    @Produces(MediaType.APPLICATION_JSON)
    @Operation(summary = "Example POST request", description = "Example produces/consumes for POST")
    fun examplePost(examplePostRequest: ExamplePostRequest): ExamplePostResponse {

        // Normally we'd create a new object in the database with a POST request, but for demonstration
        // purposes, we'll just return an example response object
        val response = ExamplePostResponse()
        response.name = examplePostRequest.name
        return response
    }

    @PUT
    @Consumes(MediaType.APPLICATION_JSON)
    @Produces(MediaType.APPLICATION_JSON)
    @Operation(summary = "Example PUT request", description = "Example produces/consumes for PUT")
    fun examplePost(examplePutRequest: ExamplePutRequest): ExamplePutResponse {

        // Normally we'd overwrite an existing object in the database with a PUT request, but for demonstration
        // purposes, we'll just return an example response object
        val response = ExamplePutResponse()
        response.name = examplePutRequest.name
        return response
    }

    @PUT
    @Path("{name}")
    @Consumes(MediaType.APPLICATION_JSON)
    @Produces(MediaType.APPLICATION_JSON)
    @Operation(summary = "Example PUT request with a path param", description = "Example produces/consumes for PUT with a path param")
    fun examplePutWithPathParam(@PathParam("name") name: String, examplePutRequest: ExamplePutRequest): ExamplePutResponse {

        // Normally we'd overwrite an existing object in the database with a "name" property that matches the "name" path
        // param with this PUT request, but for demonstration purposes, we'll just return an example response object
        val response = ExamplePutResponse()
        response.name = examplePutRequest.name
        response.metadata = mapOf("name" to name)
        return response
    }

}</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>This resource never touches a database — its own comments say so explicitly; it exists to demonstrate JSON request/response bodies, a validated request DTO, and a <code>@PathParam</code>. Note the two methods both named <code>examplePost</code> — this compiles because Kotlin (like Java) allows overloading by parameter type, and JAX-RS dispatches by HTTP method + path + media type at request time, not by the method name Kotlin sees at compile time.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>One of the four request/response DTOs, <code>ExamplePutResponse</code> (in <code>model/</code>), shows the shape all of them share:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>package com.mystudio.mygame.model

import dev.getelements.elements.sdk.model.Constants
import io.swagger.v3.oas.annotations.media.Schema
import jakarta.validation.constraints.NotNull
import jakarta.validation.constraints.Pattern

@Schema
class ExamplePutResponse {

    @NotNull
    @Pattern(regexp = Constants.Regexp.NO_WHITE_SPACE)
    @Schema(description = "A unique name for the object that we're creating. No spaces allowed.")
    var name: String? = null

    @Schema(description = "The type of request being made. For example/debugging purposes.")
    var requestType: String = "ExamplePutResponse"

    @Schema(description = "Any additional information to return.")
    var metadata: Map&lt;String, Any&gt;? = null

}</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p><code>@Schema</code> annotations document each field for OpenAPI generation, and <code>@NotNull</code>/<code>@Pattern</code> (using the SDK's own <code>Constants.Regexp.NO_WHITE_SPACE</code>) are standard Jakarta Bean Validation constraints, enforced automatically by the JAX-RS runtime before the resource method body runs. Kotlin's <code>var name: String? = null</code> compiles to a private field plus public getter/setter, same shape as the Java example's explicit getters/setters, just without writing them out — Jackson (the runtime's JSON library) serializes and deserializes these via the generated accessors with no special Kotlin configuration needed (see the note on Jackson below).</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-kotlin-specific-considerations">Kotlin-Specific Considerations</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Pulling together the Kotlin-only details scattered through the sections above, plus two more that don't have a single obvious home:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong><code>package-info.java</code> must stay Java</strong> — package-level annotations require <code>ElementType.PACKAGE</code>, which Kotlin doesn't support. See the Maven deep dive above for the dual-compiler-execution setup this requires.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong><code>@JvmField</code> on annotated companion-object properties</strong> — required wherever an SDK annotation processor reads a field via reflection (like <code>@ElementDefaultAttribute</code> in <code>HelloWorldApplication</code>), since a plain Kotlin <code>val</code> in a companion object is a private field behind a getter, not a public static field.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong><code>@file:JvmName("Run")</code> on top-level entry points</strong> — needed if you want a clean, predictable class name for a Kotlin file with a top-level <code>fun main()</code>, since Kotlin otherwise names the generated facade class after the file (<code>RunKt</code> for <code>run.kt</code>). See the debug runner section below.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong><code>kotlin-stdlib</code> must ship inside the <code>.elm</code></strong> — both in <code>lib/</code> (for <code>element</code>'s own runtime classpath) and in <code>api/</code> (in case any exported Kotlin type gets reflected over from another Element's classloader). The Elements runtime has no reason to provide Kotlin's standard library itself.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Do not add <code>KotlinModule</code> to Jackson</strong> — the Elements runtime already configures a shared <code>ObjectMapper</code>. Registering Jackson's <code>KotlinModule</code> yourself causes classloader conflicts with that shared instance. Plain Kotlin <code>data class</code>/<code>class</code> DTOs (like the model classes above) serialize correctly via their generated getters with no special configuration — as demonstrated by the live JSON responses in the setup section above.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

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
<li>No Morphia <code>@Entity</code>/DAO classes — nothing in this project persists to MongoDB directly. See <a href="direct-database-access-and-batch-configuration">Direct Database Access and Batch Configuration</a> and this repository's own <code>MORPHIA.md</code> for the <code>Transaction</code>/<code>Datastore</code>/<code>@ElementTypeRequest</code> patterns you'd use to add it.</li>
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
<h1 class="wp-block-heading" id="h-the-debug-runner">The Debug Runner: <code>run.kt</code></h1>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code>@file:JvmName("Run")

import dev.getelements.elements.sdk.local.ElementsLocalBuilder
import java.io.File

/**
 * Runs your local Element in the SDK.
 *
 * Working directory must be the project root (element-example-kotlin/).
 * IntelliJ: Run → Edit Configurations → Working directory → set to this project root.
 */
fun main() {

    // Install npm dependencies on first run, then build both segment bundles.
    // The bundles are written directly to element/src/main/ui/{superuser,user}/
    // so that the Maven build triggered by local.start() picks them up.
    val uiDir = File("ui")

    if (!File(uiDir, "node_modules").exists()) {
        ProcessBuilder("npm", "install")
            .directory(uiDir)
            .inheritIO()
            .start()
            .waitFor()
    }

    ProcessBuilder("npm", "run", "build")
        .directory(uiDir)
        .inheritIO()
        .start()
        .waitFor()

    ProcessBuilder("docker", "compose", "up", "-d")
        .directory(File("services-dev"))
        .inheritIO()
        .start()
        .waitFor()

    val local = ElementsLocalBuilder.getDefault()
        .withSourceRoot()
        .withDeployment { builder -&gt;
            builder
                .useDefaultRepositories(true)
                .elementPackage()
                    .elmArtifact("com.example.element:element:elm:1.0-SNAPSHOT")
                .endElementPackage()
                .build()
        }
        .build()

    local.start()
    local.run()

}</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>Where the Java example wraps its entrypoint in a bare, package-less <code>public class run</code> with a <code>static void main(String[] args)</code>, Kotlin doesn't require a wrapping class at all — <code>run.kt</code> is a top-level file with a single top-level <code>fun main()</code>. Two Kotlin-specific things to note:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><code>@file:JvmName("Run")</code> at the top of the file is a file-level annotation that renames the generated facade class from the Kotlin default (<code>RunKt</code>, derived from the filename) to <code>Run</code>. This is why the run command above passes <code>-Dexec.mainClass=Run</code> rather than <code>RunKt</code> — and it's a good habit generally, since <code>&lt;FileName&gt;Kt</code> is not a name you'd want to type or see in a stack trace.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>No <code>package</code> declaration — the file sits directly under <code>debug/src/main/kotlin/</code> with no sub-package, so both <code>Run</code> and <code>RunKt</code> would resolve at the default (unnamed) package if you ever needed to reference either directly.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>Functionally, the four steps (npm install/build, docker compose, then <code>ElementsLocalBuilder</code>) are identical to the Java example and were walked through in the setup section above; the key API points are <code>.withSourceRoot()</code> (build against the local source tree, not a published artifact) and <code>.elmArtifact("com.example.element:element:elm:1.0-SNAPSHOT")</code> (the exact Maven coordinate, including the <code>elm</code> packaging type, produced by the <code>element</code> module's <code>attach-artifact</code> step described above).</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"level":1} -->
<h1 class="wp-block-heading" id="h-dashboard-ui-plugin-deep-dive">Dashboard UI Plugin Deep Dive</h1>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Elements can inject custom pages into the Elements admin dashboard by shipping a React component bundle alongside the Element's code — this mechanism has nothing to do with the backend language, so everything in this section is identical between the Kotlin and Java example projects. The dashboard discovers these at runtime via a <code>plugin.json</code> manifest — no dashboard changes required. The <code>ui/</code> module is a Vite/TypeScript project that builds these bundles.</p>
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
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="kt0006"><div class="gb-notice-title" style="color:#fff"><p>Note</p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p>If your Element's REST endpoint requires authentication, the platform's convention is to send <code>window.__elementsApiClient.getSessionToken()</code> as an <code>Elements-SessionSecret</code> header on the fetch call (cookies alone aren't reliable in every dashboard context). This example's own fetch call only hits the unauthenticated <code>/api/rest/version</code> platform endpoint, so it doesn't exercise that pattern — if you want to call <code>/app/rest/example-element/hellowithauthentication</code> from a plugin, you'll need to add that header yourself.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-plugin-json">The <code>plugin.json</code> Manifest</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>After <code>npm run build</code>, the compiled bundle lands next to a manifest at <code>element/src/main/ui/superuser/plugin.json</code> (and the equivalent under <code>user/</code>) — verified live at <code>http://localhost:8080/app/ui/test/path/superuser/plugin.json</code> in the setup above:</p>
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
<p><code>label</code> is the sidebar text, <code>icon</code> is any <a href="https://lucide.dev/icons/">Lucide</a> icon name, <code>bundlePath</code> is relative to the manifest, and <code>route</code> is the unique key used both in the dashboard URL (<code>/plugin/{route}</code>) and in the <code>.register(route, ...)</code> call above — the two must match. Both the manifest and the built <code>plugin.bundle.js</code> get staged into the <code>.elm</code>'s <code>ui/</code> tree by the <code>elm-stage-static-content</code> antrun execution described earlier.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"level":1} -->
<h1 class="wp-block-heading" id="h-static-and-ui-content-serving">Static &amp; UI Content Serving</h1>
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

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"Note"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="kt0007"><div class="gb-notice-title" style="color:#fff"><p>Note</p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p>In this project, <code>{prefix}</code> for these two trees is not the same as the REST API's <code>example-element</code> prefix. <code>HelloWorldApplication</code> overrides the full serve URI directly via <code>dev.getelements.element.static.uri</code> (default <code>/app/static/test/path</code>) and <code>dev.getelements.element.ui.uri</code> (default <code>/app/ui/test/path</code>) — both clearly placeholder values, not wired to anything meaningful. Verified live: <code>curl http://localhost:8080/app/static/test/path/index.html</code> returns the contents of <code>element/src/main/static/index.html</code>, and <code>curl http://localhost:8080/app/ui/test/path/superuser/plugin.json</code> returns the manifest shown above. For your own Element, either drop these two attribute overrides (to get the standard <code>/app/static/{prefix}</code> / <code>/app/ui/{prefix}</code> behavior shown in the list above) or set them to something meaningful.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

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
<li><code>dev.getelements.element.static.uri</code> / <code>dev.getelements.element.ui.uri</code> — override the full serve URI, default <code>/app/static/{prefix}</code> / <code>/app/ui/{prefix}</code> (overridden in this project, per the note above)</li>
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
<p>Watch the Kotlin quickstart video again if any step above didn't click: <a href="https://www.youtube.com/watch?v=6kLWRMex-ug">https://www.youtube.com/watch?v=6kLWRMex-ug</a></p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><a href="element-example-complete-walkthrough">Building the Example Element: A Complete Walkthrough</a> — the Java version of this same project, useful for comparing the two side by side</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="element-structure">Custom Code Overview</a> — the broader picture of how Elements load and isolate custom code</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="introduction-to-guice-and-jakarta-in-elements">Introduction to Guice and Jakarta in Elements</a> — background on the two frameworks used throughout this example</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="structuring-your-element">Structuring Your Element</a> — a shorter, pattern-focused companion to this guide</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="packaging-an-element-with-maven">Packaging an Element with Maven</a> — more detail on the <code>.elm</code> packaging mechanism shared by both the Kotlin and Java examples</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="direct-database-access-and-batch-configuration">Direct Database Access and Batch Configuration</a> — for adding real MongoDB/Morphia persistence, not demonstrated in this project</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->
