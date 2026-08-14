<h1>Packaging an Element with Maven</h1>

<!-- wp:paragraph -->
<p>Namazu Elements loads a Custom Element from a directory (or an archive with the same internal layout) containing some combination of the following:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><code>api</code> - a directory containing jars exported to other Elements (interfaces and DTOs your Element publishes for cross-Element use)</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>lib</code> - a directory containing the jars of the Element's own (non-<code>provided</code>) dependencies</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>classpath</code> - a directory containing the compiled classes and resources of the Element itself</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>static</code> / <code>ui</code> - optional static content and dashboard UI plugin content trees</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>dev.getelements.element.attributes.properties</code> - an optional file added at deployment time which contains configuration overrides</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"📝<mark style="background-color:rgba(0, 0, 0, 0)" class="has-inline-color has-black-color">Notes on Structure</mark>"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="840558"><div class="gb-notice-title" style="color:#fff"><p>📝<mark style="background-color:rgba(0, 0, 0, 0)" class="has-inline-color has-black-color">Notes on Structure</mark></p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p>All of the above are optional provided there exists one, and only one, <code>package-info</code> annotated with the <code>@ElementDefinition</code> annotation (either in jar form or in classpath form).</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:paragraph -->
<p>The <a href="https://github.com/NamazuStudios/element-example">Example Element project</a> builds this structure into a single archive with the <code>.elm</code> extension, using the Maven Antrun Plugin, Maven Dependency Plugin, and Build Helper Maven Plugin — no assembly descriptor required. This is the packaging approach used starting with the current example project; see <a href="element-example-complete-walkthrough">Building the Example Element: A Complete Walkthrough</a> for the full setup-to-running guide.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-staging-properties">1. Staging Properties</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p><code>element/pom.xml</code> defines two properties that control where the archive gets built:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>&lt;properties&gt;
    &lt;elm.builtin.spis&gt;DEFAULT&lt;/elm.builtin.spis&gt;
    &lt;elm.staging.dir&gt;${project.build.directory}/${project.groupId}.${project.artifactId}-${project.version}&lt;/elm.staging.dir&gt;
    &lt;elm.element.dir&gt;${elm.staging.dir}/${project.groupId}.${project.artifactId}&lt;/elm.element.dir&gt;
&lt;/properties&gt;</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p><code>elm.staging.dir</code> resolves to <code>target/com.example.element.element-1.0-SNAPSHOT</code>; <code>elm.element.dir</code> nests one level deeper, e.g. <code>target/com.example.element.element-1.0-SNAPSHOT/com.example.element.element</code>. Everything below copies into <code>elm.element.dir</code>, and the final archive is built from <code>elm.staging.dir</code>.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-copy-dependencies">2. Use <code>maven-dependency-plugin</code> to Copy API and Library Jars</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Two executions of the <a href="https://maven.apache.org/plugins/maven-dependency-plugin/" target="_blank" rel="noreferrer noopener">Maven Dependency Plugin</a>, both bound to <code>prepare-package</code>:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>&lt;plugin&gt;
    &lt;groupId&gt;org.apache.maven.plugins&lt;/groupId&gt;
    &lt;artifactId&gt;maven-dependency-plugin&lt;/artifactId&gt;
    &lt;version&gt;3.6.0&lt;/version&gt;
    &lt;executions&gt;
        &lt;!-- Copies this project's own API-classified jar(s) into api/ --&gt;
        &lt;execution&gt;
            &lt;id&gt;elm-copy-api-deps&lt;/id&gt;
            &lt;phase&gt;prepare-package&lt;/phase&gt;
            &lt;goals&gt;&lt;goal&gt;copy-dependencies&lt;/goal&gt;&lt;/goals&gt;
            &lt;configuration&gt;
                &lt;outputDirectory&gt;${elm.element.dir}/api&lt;/outputDirectory&gt;
                &lt;includeGroupIds&gt;${project.groupId}&lt;/includeGroupIds&gt;
                &lt;includeClassifiers&gt;${api.classifier}&lt;/includeClassifiers&gt;
                &lt;prependGroupId&gt;true&lt;/prependGroupId&gt;
            &lt;/configuration&gt;
        &lt;/execution&gt;

        &lt;!-- Copies all non-provided dependencies into lib/ --&gt;
        &lt;execution&gt;
            &lt;id&gt;elm-copy-lib-deps&lt;/id&gt;
            &lt;phase&gt;prepare-package&lt;/phase&gt;
            &lt;goals&gt;&lt;goal&gt;copy-dependencies&lt;/goal&gt;&lt;/goals&gt;
            &lt;configuration&gt;
                &lt;outputDirectory&gt;${elm.element.dir}/lib&lt;/outputDirectory&gt;
                &lt;excludeScope&gt;provided&lt;/excludeScope&gt;
                &lt;prependGroupId&gt;true&lt;/prependGroupId&gt;
            &lt;/configuration&gt;
        &lt;/execution&gt;
    &lt;/executions&gt;
&lt;/plugin&gt;</code></pre>
<!-- /wp:code -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"📝<mark style="background-color:rgba(0, 0, 0, 0)" class="has-inline-color has-black-color">Notes on Scope</mark>"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="0f985d"><div class="gb-notice-title" style="color:#fff"><p>📝<mark style="background-color:rgba(0, 0, 0, 0)" class="has-inline-color has-black-color">Notes on Scope</mark></p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p>Artifacts provided by Namazu Elements must be scoped as <code>provided</code> to avoid duplicate entries on the classpath. All SDK modules should be provided scope — the <code>sdk-bom</code> already does this for you. If you experience strange classpath errors, always double-check the provided dependencies.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-stage-classpath-and-content">3. Use <code>maven-antrun-plugin</code> to Stage the Classpath, Static Content, and Manifest</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The <a href="https://maven.apache.org/plugins/maven-antrun-plugin/" target="_blank" rel="noreferrer noopener">Maven Antrun Plugin</a> runs three <code>prepare-package</code> executions and one <code>package</code> execution:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>&lt;plugin&gt;
    &lt;groupId&gt;org.apache.maven.plugins&lt;/groupId&gt;
    &lt;artifactId&gt;maven-antrun-plugin&lt;/artifactId&gt;
    &lt;version&gt;3.1.0&lt;/version&gt;
    &lt;executions&gt;

        &lt;!-- Compiled classes + src/main/resources -&gt; classpath/ --&gt;
        &lt;execution&gt;
            &lt;id&gt;elm-stage-classpath&lt;/id&gt;
            &lt;phase&gt;prepare-package&lt;/phase&gt;
            &lt;goals&gt;&lt;goal&gt;run&lt;/goal&gt;&lt;/goals&gt;
            &lt;configuration&gt;
                &lt;target&gt;
                    &lt;copy todir="${elm.element.dir}/classpath" failonerror="false"&gt;
                        &lt;fileset dir="${project.build.outputDirectory}" erroronmissingdir="false" includes="**/*"/&gt;
                    &lt;/copy&gt;
                    &lt;copy todir="${elm.element.dir}/classpath" failonerror="false"&gt;
                        &lt;fileset dir="${basedir}/src/main/resources" erroronmissingdir="false" includes="**/*"/&gt;
                    &lt;/copy&gt;
                &lt;/target&gt;
            &lt;/configuration&gt;
        &lt;/execution&gt;

        &lt;!-- src/main/static -&gt; static/, src/main/ui -&gt; ui/ --&gt;
        &lt;execution&gt;
            &lt;id&gt;elm-stage-static-content&lt;/id&gt;
            &lt;phase&gt;prepare-package&lt;/phase&gt;
            &lt;goals&gt;&lt;goal&gt;run&lt;/goal&gt;&lt;/goals&gt;
            &lt;configuration&gt;
                &lt;target&gt;
                    &lt;copy todir="${elm.element.dir}/static" failonerror="false"&gt;
                        &lt;fileset dir="${basedir}/src/main/static" erroronmissingdir="false" includes="**/*"/&gt;
                    &lt;/copy&gt;
                    &lt;copy todir="${elm.element.dir}/ui" failonerror="false"&gt;
                        &lt;fileset dir="${basedir}/src/main/ui" erroronmissingdir="false" includes="**/*"/&gt;
                    &lt;/copy&gt;
                &lt;/target&gt;
            &lt;/configuration&gt;
        &lt;/execution&gt;

        &lt;!-- Writes dev.getelements.element.manifest.properties at the element root --&gt;
        &lt;execution&gt;
            &lt;id&gt;elm-write-manifest&lt;/id&gt;
            &lt;phase&gt;prepare-package&lt;/phase&gt;
            &lt;goals&gt;&lt;goal&gt;run&lt;/goal&gt;&lt;/goals&gt;
            &lt;configuration&gt;
                &lt;target&gt;
                    &lt;mkdir dir="${elm.element.dir}"/&gt;
                    &lt;exec executable="git" outputproperty="git.revision"
                          failifexecutionfails="false" errorproperty="git.error"&gt;
                        &lt;arg value="rev-parse"/&gt;
                        &lt;arg value="HEAD"/&gt;
                    &lt;/exec&gt;
                    &lt;property name="git.revision" value="unknown"/&gt;
                    &lt;echo file="${elm.element.dir}/dev.getelements.element.manifest.properties" append="false"&gt;
Element-Version=${project.version}
Element-Build-Time=${maven.build.timestamp}
Element-Revision=${git.revision}
Element-Builtin-Spis=${elm.builtin.spis}
                    &lt;/echo&gt;
                &lt;/target&gt;
            &lt;/configuration&gt;
        &lt;/execution&gt;

        &lt;!-- Zips the fully staged directory into the .elm archive --&gt;
        &lt;execution&gt;
            &lt;id&gt;elm-create-archive&lt;/id&gt;
            &lt;phase&gt;package&lt;/phase&gt;
            &lt;goals&gt;&lt;goal&gt;run&lt;/goal&gt;&lt;/goals&gt;
            &lt;configuration&gt;
                &lt;target&gt;
                    &lt;mkdir dir="${elm.element.dir}/api"/&gt;
                    &lt;mkdir dir="${elm.element.dir}/lib"/&gt;
                    &lt;mkdir dir="${elm.element.dir}/classpath"/&gt;
                    &lt;zip destfile="${elm.staging.dir}.elm" basedir="${elm.staging.dir}"/&gt;
                &lt;/target&gt;
            &lt;/configuration&gt;
        &lt;/execution&gt;

    &lt;/executions&gt;
&lt;/plugin&gt;</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>The final execution's <code>mkdir</code> calls guarantee <code>api/</code>, <code>lib/</code>, and <code>classpath/</code> always exist in the archive, even for an Element with no third-party dependencies or no exported API. The <code>&lt;zip&gt;</code> task is the entire packaging mechanism — no assembly descriptor, no shade plugin.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-attach-the-elm-artifact">4. Attach the <code>.elm</code> Artifact with <code>build-helper-maven-plugin</code></h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>By itself, the zip created above is just a file sitting in <code>target/</code> — it isn't installed or deployed as part of the Maven build. The <a href="https://www.mojohaus.org/build-helper-maven-plugin/" target="_blank" rel="noreferrer noopener">Build Helper Maven Plugin</a>'s <code>attach-artifact</code> goal fixes that:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>&lt;plugin&gt;
    &lt;groupId&gt;org.codehaus.mojo&lt;/groupId&gt;
    &lt;artifactId&gt;build-helper-maven-plugin&lt;/artifactId&gt;
    &lt;version&gt;3.6.0&lt;/version&gt;
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
<p>After this, <code>mvn install</code> publishes the archive into your local repository as <code>com.example.element:element:1.0-SNAPSHOT:elm</code>, and <code>mvn deploy</code> publishes it to a remote repository the same way. This is the exact coordinate a deployment configuration — or the <code>debug</code> module's <code>ElementsLocalBuilder</code>, via <code>.elmArtifact("com.example.element:element:elm:1.0-SNAPSHOT")</code> — references to pull in the built Element.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-optional-enable-crossfire-via-profile">5. (Optional) Enable Namazu Crossfire via Profile</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The existing Namazu Crossfire Guide covers how to enable Namazu Crossfire in the build. We include this section if you wish to use it alongside the packaging pipeline above.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="h-add-namazu-crossfire-dependencies">5.1 Add Namazu Crossfire Dependencies</h4>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code>&lt;dependency&gt;
    &lt;groupId&gt;dev.getelements.elements.crossfire&lt;/groupId&gt;
    &lt;artifactId&gt;server&lt;/artifactId&gt;
    &lt;version&gt;${crossfire.version}&lt;/version&gt;
    &lt;scope&gt;provided&lt;/scope&gt;
&lt;/dependency&gt;
&lt;dependency&gt;
    &lt;groupId&gt;dev.getelements.elements.crossfire&lt;/groupId&gt;
    &lt;artifactId&gt;server&lt;/artifactId&gt;
    &lt;version&gt;${crossfire.version}&lt;/version&gt;
    &lt;classifier&gt;element&lt;/classifier&gt;
    &lt;scope&gt;provided&lt;/scope&gt;
&lt;/dependency&gt;</code></pre>
<!-- /wp:code -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"📝<mark style="background-color:rgba(0, 0, 0, 0)" class="has-inline-color has-black-color">Notes on Scope</mark>"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="0f985d2"><div class="gb-notice-title" style="color:#fff"><p>📝<mark style="background-color:rgba(0, 0, 0, 0)" class="has-inline-color has-black-color">Notes on Scope</mark></p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p>Namazu Crossfire is deployed as its own, separate Element artifact (the shaded jar qualified with the <code>element</code> classifier) — it is never bundled inside your Element's own <code>.elm</code> archive. It's added here with <code>provided</code> scope purely so your code can compile and run locally against it.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="h-optional-add-to-local-element-lib">5.2 (Optional) Add to Local Classpath for IDE Debugging</h4>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>This ensures that local development runs (via the <code>debug</code> module) have Namazu Crossfire on the classpath. You do not need to perform this step if you don't need to debug Namazu Crossfire locally in your IDE.</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>&lt;plugin&gt;
    &lt;groupId&gt;org.apache.maven.plugins&lt;/groupId&gt;
    &lt;artifactId&gt;maven-dependency-plugin&lt;/artifactId&gt;
    &lt;version&gt;3.6.0&lt;/version&gt;
    &lt;executions&gt;
        &lt;execution&gt;
            &lt;id&gt;copy-crossfire-libs&lt;/id&gt;
            &lt;phase&gt;generate-resources&lt;/phase&gt;
            &lt;goals&gt;&lt;goal&gt;copy-dependencies&lt;/goal&gt;&lt;/goals&gt;
            &lt;configuration&gt;
                &lt;includeScope&gt;provided&lt;/includeScope&gt;
                &lt;prependGroupId&gt;true&lt;/prependGroupId&gt;
                &lt;includeGroupIds&gt;dev.getelements.elements.crossfire&lt;/includeGroupIds&gt;
                &lt;includeArtifactIds&gt;server&lt;/includeArtifactIds&gt;
                &lt;excludeClassifiers&gt;element&lt;/excludeClassifiers&gt;
                &lt;outputDirectory&gt;${project.build.directory}/element-libs&lt;/outputDirectory&gt;
            &lt;/configuration&gt;
        &lt;/execution&gt;
    &lt;/executions&gt;
&lt;/plugin&gt;</code></pre>
<!-- /wp:code -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"📝<mark style="background-color:rgba(0, 0, 0, 0)" class="has-inline-color has-black-color">Notes On Dependencies</mark>"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="81a373"><div class="gb-notice-title" style="color:#fff"><p>📝<mark style="background-color:rgba(0, 0, 0, 0)" class="has-inline-color has-black-color">Notes On Dependencies</mark></p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p>This forces Crossfire to share the classpath with your Element locally, which is likely not the desired behavior in production. This should not have any consequences unless you share dependencies with Namazu Crossfire itself. We are aware this is a limitation and will improve isolation in future builds.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="h-optional-fetch-crossfire-element-from-maven-central">5.3 (Optional) Fetch the Namazu Crossfire Element from Maven Central</h4>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Namazu Crossfire is available on Maven Central as a shaded jar, qualified with the classifier <code>element</code>, indicating it is an entirely self-contained instance of Crossfire ready to deploy on its own. This copies it to a directory for easy deployment alongside your Element:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>&lt;plugin&gt;
    &lt;groupId&gt;org.apache.maven.plugins&lt;/groupId&gt;
    &lt;artifactId&gt;maven-dependency-plugin&lt;/artifactId&gt;
    &lt;version&gt;3.6.0&lt;/version&gt;
    &lt;executions&gt;
        &lt;execution&gt;
            &lt;id&gt;copy-crossfire-element&lt;/id&gt;
            &lt;phase&gt;generate-resources&lt;/phase&gt;
            &lt;goals&gt;&lt;goal&gt;copy-dependencies&lt;/goal&gt;&lt;/goals&gt;
            &lt;configuration&gt;
                &lt;includeScope&gt;provided&lt;/includeScope&gt;
                &lt;prependGroupId&gt;true&lt;/prependGroupId&gt;
                &lt;includeGroupIds&gt;dev.getelements.elements.crossfire&lt;/includeGroupIds&gt;
                &lt;includeArtifactIds&gt;server&lt;/includeArtifactIds&gt;
                &lt;includeClassifiers&gt;element&lt;/includeClassifiers&gt;
                &lt;outputDirectory&gt;${project.build.directory}/dev.getelements.elements.crossfire.server/lib&lt;/outputDirectory&gt;
            &lt;/configuration&gt;
        &lt;/execution&gt;
    &lt;/executions&gt;
&lt;/plugin&gt;</code></pre>
<!-- /wp:code -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-verify-deployment">6. Verify the Build</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Run the full build from the project root:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>mvn clean install</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>You should find the archive at <code>element/target/com.example.element.element-1.0-SNAPSHOT.elm</code>. Inspect it with any zip tool — <code>.elm</code> is a plain zip file:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>unzip -l element/target/com.example.element.element-1.0-SNAPSHOT.elm</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>You should see output shaped like this (paths are relative to the archive root, which is the <code>com.example.element.element/</code> directory):</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>Archive:  com.example.element.element-1.0-SNAPSHOT.elm
  Length      Date    Time    Name
---------  ---------- -----   ----
        0  2026-01-01 00:00   api/
        0  2026-01-01 00:00   lib/
        0  2026-01-01 00:00   classpath/
        0  2026-01-01 00:00   static/
        0  2026-01-01 00:00   ui/
      xxx  2026-01-01 00:00   dev.getelements.element.manifest.properties
    xxxxx  2026-01-01 00:00   api/com.example.element.api-1.0-SNAPSHOT-com.example.element.api.jar
   xxxxxx  2026-01-01 00:00   lib/com.google.inject.guice-7.0.0.jar
   xxxxxx  2026-01-01 00:00   lib/io.swagger.core.v3.swagger-jaxrs2-jakarta-2.2.22.jar
   xxxxxx  2026-01-01 00:00   lib/io.swagger.core.v3.swagger-annotations-2.2.22.jar
        0  2026-01-01 00:00   classpath/com/
        0  2026-01-01 00:00   classpath/com/mystudio/
        0  2026-01-01 00:00   classpath/com/mystudio/mygame/
        0  2026-01-01 00:00   classpath/com/mystudio/mygame/rest/
        0  2026-01-01 00:00   classpath/com/mystudio/mygame/service/
        0  2026-01-01 00:00   classpath/com/mystudio/mygame/model/
        0  2026-01-01 00:00   classpath/com/mystudio/mygame/guice/
     xxxx  2026-01-01 00:00   classpath/com/mystudio/mygame/package-info.class
     xxxx  2026-01-01 00:00   classpath/com/mystudio/mygame/HelloWorldApplication.class
     xxxx  2026-01-01 00:00   classpath/com/mystudio/mygame/OpenAPISecurityConfig.class
     xxxx  2026-01-01 00:00   classpath/com/mystudio/mygame/rest/HelloWorld.class
     xxxx  2026-01-01 00:00   classpath/com/mystudio/mygame/rest/HelloWithAuthentication.class
     xxxx  2026-01-01 00:00   classpath/com/mystudio/mygame/rest/ExampleContent.class
     xxxx  2026-01-01 00:00   classpath/com/mystudio/mygame/service/GreetingService.class
     xxxx  2026-01-01 00:00   classpath/com/mystudio/mygame/service/GreetingServiceImpl.class
     xxxx  2026-01-01 00:00   classpath/com/mystudio/mygame/model/ExamplePostRequest.class
     xxxx  2026-01-01 00:00   classpath/com/mystudio/mygame/model/ExamplePostResponse.class
     xxxx  2026-01-01 00:00   classpath/com/mystudio/mygame/model/ExamplePutRequest.class
     xxxx  2026-01-01 00:00   classpath/com/mystudio/mygame/model/ExamplePutResponse.class
     xxxx  2026-01-01 00:00   classpath/com/mystudio/mygame/guice/MyGameModule.class
     xxxx  2026-01-01 00:00   static/index.html
     xxxx  2026-01-01 00:00   ui/index.html
     xxxx  2026-01-01 00:00   ui/superuser/plugin.json
     xxxx  2026-01-01 00:00   ui/superuser/plugin.bundle.js
     xxxx  2026-01-01 00:00   ui/user/plugin.json
     xxxx  2026-01-01 00:00   ui/user/plugin.bundle.js</code></pre>
<!-- /wp:code -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"📝<mark style="background-color:rgba(0, 0, 0, 0)" class="has-inline-color has-black-color">What to Look For</mark>"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="0941e3"><div class="gb-notice-title" style="color:#fff"><p>📝<mark style="background-color:rgba(0, 0, 0, 0)" class="has-inline-color has-black-color">What to Look For</mark></p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Only your own classified API jar appears in <code>api/</code> — Elements SDK artifacts do not, since they're all scoped <code>provided</code> and excluded from <code>lib/</code>.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Third-party dependencies (Guice, Swagger) are included automatically along with their own transitive dependencies.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Test classes are excluded — only production code appears under <code>classpath/</code>.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Jars in <code>lib/</code> and <code>api/</code> are fully-qualified with group and artifact id, thanks to <code>prependGroupId</code>.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>The dashboard UI plugin bundles only appear under <code>ui/</code> if you've already run <code>npm run build</code> in <code>ui/</code> (or run the build with <code>-Pbuild-ui</code>) before packaging — Maven does not build the frontend for you by default.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->
