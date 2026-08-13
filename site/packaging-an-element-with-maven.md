<h1>Packaging an Element with Maven</h1>

<!-- wp:paragraph -->
<p>Starting with Elements 3.4, we have updated the example project to include an advanced packaging scheme which automatically gathers dependencies and builds a zip file containing your Element and it's dependencies. This greatly simplifies the process required to package an Element. The fundamentals of packaging an Element described in <a href="https://namazustudios.com/docs/custom-code/deploying-an-element/">Deploying an Element</a> have not changed. The structure of an Element remains the same with the following directory structure:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><code>libs</code> - a directory containing the jars of the Element's dependencies</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>classpath</code> - a directory containing the classpath of the Element itself</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>attributes.properties</code> - a file added at deployment time which contains configuration</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"📝\u003cmark style=\u0022background-color:rgba(0, 0, 0, 0)\u0022 class=\u0022has-inline-color has-black-color\u0022\u003eNotes on Structure\u003c/mark\u003e"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="840558"><div class="gb-notice-title" style="color:#fff"><p>📝<mark style="background-color:rgba(0, 0, 0, 0)" class="has-inline-color has-black-color">Notes on Structure</mark></p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p>All of the above are optional provided there exists one, and only one, <code>package-info</code> annotated with the <code>@ElemmentDefinition</code> annotation (either in jar form or in classpath form)</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-manual-migration-in-maven">Manual Migration in Maven</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>It may be possible to merge the Example Element to your current project and apply these changes. Before embarking on a rework of your code, see if a merge from our public repository completes without any conflicts.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-1-add-properties-to-the-build">1. Add Properties to The Build</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Not strictly necessary, but every subsequent step assumes you complete this step. This makes it easy to edit the deployment configuration later.</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>    &lt;properties>
        &lt;!-- Existing Properties -->
<mark style="background-color:#7bdcb5" class="has-inline-color">        &lt;element.target.dir>${project.build.directory}/element&lt;/element.target.dir>
        &lt;element.distribution.dir>${element.target.dir}/${groupId}.${artifactId}&lt;/element.distribution.dir>
        &lt;element.distribution.zip>${element.target.dir}/${project.artifactId}-${project.version}.zip&lt;/element.distribution.zip></mark>
    &lt;/properties></code></pre>
<!-- /wp:code -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-2-use-maven-antrun-plugin-to-ensure-the-element-s-code-is-included">2. Use <code>maven-antrun-plugin</code> to Ensure the Element's Code is Included</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The <a href="https://maven.apache.org/plugins/maven-antrun-plugin/" target="_blank" rel="noreferrer noopener">Maven Antrun Plugin</a> executes Ant tasks which can easily copy the build product into the Element bundle.</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>            &lt;plugin&gt;
                &lt;groupId&gt;org.apache.maven.plugins&lt;/groupId&gt;
                &lt;artifactId&gt;maven-antrun-plugin&lt;/artifactId&gt;
                &lt;version&gt;3.1.0&lt;/version&gt;
                &lt;executions&gt;
                    &lt;execution&gt;
                        &lt;id&gt;copy-element-build&lt;/id&gt;
                        &lt;phase&gt;process-classes&lt;/phase&gt;
                        &lt;goals&gt;
                            &lt;goal&gt;run&lt;/goal&gt;
                        &lt;/goals&gt;
                        &lt;configuration&gt;
                            &lt;target&gt;
                                &lt;copy todir="${element.distribution.dir}/classpath"&gt;
                                    &lt;fileset dir="${project.build.directory}/classes" /&gt;
                                &lt;/copy&gt;
                            &lt;/target&gt;
                        &lt;/configuration&gt;
                    &lt;/execution&gt;
                &lt;/executions&gt;
            &lt;/plugin&gt;</code></pre>
<!-- /wp:code -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-3-use-the-maven-resources-plugin-to-copy-over-resoruces">3. Use the <code>maven-resources-plugin</code> to Copy over Resoruces</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The <a href="https://maven.apache.org/plugins/maven-resources-plugin/" target="_blank" rel="noreferrer noopener">Maven Resources Plugin</a> ensures that everything in <code>src/main/resoruces</code> gets bundled in the Element. </p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>            &lt;plugin&gt;
                &lt;artifactId&gt;maven-resources-plugin&lt;/artifactId&gt;
                &lt;version&gt;3.3.1&lt;/version&gt;
                &lt;executions&gt;
                    &lt;execution&gt;
                        &lt;id&gt;copy-element-resources&lt;/id&gt;
                        &lt;phase&gt;process-resources&lt;/phase&gt;
                        &lt;goals&gt;
                            &lt;goal&gt;copy-resources&lt;/goal&gt;
                        &lt;/goals&gt;
                        &lt;configuration&gt;
                            &lt;outputDirectory&gt;${element.distribution.dir}/classpath&lt;/outputDirectory&gt;
                            &lt;resources&gt;
                                &lt;resource&gt;
                                    &lt;directory&gt;src/main/resources&lt;/directory&gt;
                                &lt;/resource&gt;
                            &lt;/resources&gt;
                        &lt;/configuration&gt;
                    &lt;/execution&gt;
                &lt;/executions&gt;
            &lt;/plugin&gt;</code></pre>
<!-- /wp:code -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-4-use-the-maven-dependency-plugin-to-copy-dependencies-over">4. Use the <code>maven-dependency-plugin</code> To Copy Dependencies Over</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The <a href="https://maven.apache.org/plugins/maven-dependency-plugin/" target="_blank" rel="noreferrer noopener">Maven Dependency Plugin</a> copies all dependencies over to the final output. </p>
<!-- /wp:paragraph -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"📝\u003cmark style=\u0022background-color:rgba(0, 0, 0, 0)\u0022 class=\u0022has-inline-color has-black-color\u0022\u003eNotes on Multi-Module Projects\u003c/mark\u003e"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="542608"><div class="gb-notice-title" style="color:#fff"><p>📝<mark style="background-color:rgba(0, 0, 0, 0)" class="has-inline-color has-black-color">Notes on Multi-Module Projects</mark></p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p>Because the <code>maven-dependency-plugin</code> uses the local repository to find jars, it may be necessary to invoke a manual <code>mvn build install</code> during local development as the jars will only be copied from the local installed repository.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:code -->
<pre class="wp-block-code"><code>            &lt;plugin&gt;
                &lt;groupId&gt;org.apache.maven.plugins&lt;/groupId&gt;
                &lt;artifactId&gt;maven-dependency-plugin&lt;/artifactId&gt;
                &lt;version&gt;3.6.0&lt;/version&gt;
                &lt;executions&gt;
                    &lt;execution&gt;
                        &lt;id&gt;copy-element-dependencies&lt;/id&gt;
                        &lt;phase&gt;process-classes&lt;/phase&gt;
                        &lt;goals&gt;
                            &lt;goal&gt;copy-dependencies&lt;/goal&gt;
                        &lt;/goals&gt;
                        &lt;configuration&gt;
                            &lt;prependGroupId&gt;true&lt;/prependGroupId&gt;
                            &lt;excludeScope&gt;provided&lt;/excludeScope&gt;
                            &lt;excludeGroupIds&gt;ch.qos.logback,dev.getelements.elements.crossfire&lt;/excludeGroupIds&gt;
                            &lt;excludeArtifactIds&gt;sdk-local,sdk-local-maven,sdk-logback&lt;/excludeArtifactIds&gt;
                            &lt;outputDirectory&gt;${element.distribution.dir}/lib&lt;/outputDirectory&gt;
                        &lt;/configuration&gt;
                    &lt;/execution&gt;
                &lt;/executions&gt;
            &lt;/plugin&gt;</code></pre>
<!-- /wp:code -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"📝\u003cmark style=\u0022background-color:rgba(0, 0, 0, 0)\u0022 class=\u0022has-inline-color has-black-color\u0022\u003eNotes on Scope\u003c/mark\u003e"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="0f985d"><div class="gb-notice-title" style="color:#fff"><p>📝<mark style="background-color:rgba(0, 0, 0, 0)" class="has-inline-color has-black-color">Notes on Scope</mark></p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p>Artifacts provided by Namazu Elements must be scoped as <code>provided</code> to avoid duplicate entries on the Classpath. All SDK modules should be provided scope. If you experience strange classpath errors, always double-check the provided dependencies.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-5-optional-add-a-zip-bundle">5. (Optional) Add a Zip Bundle</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>To make distributing your Element easier, you can add the following to generate zip of your element bundled with all of its dependencies. The <a href="https://maven.apache.org/plugins/maven-assembly-plugin/" target="_blank" rel="noreferrer noopener">Maven Assembly Plugin</a> can do this.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="h-5-1-generate-assembly-xml">5.1 Generate <code>assembly.xml</code></h4>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Create a file called <code>src/assembly/zip.xml</code> within the project and populate it with the assembly definition.</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>&lt;assembly xmlns="http://maven.apache.org/plugins/maven-assembly-plugin/assembly/1.1.3"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://maven.apache.org/plugins/maven-assembly-plugin/assembly/1.1.3
                              https://maven.apache.org/xsd/assembly-1.1.3.xsd"&gt;

    &lt;id&gt;target-zip&lt;/id&gt;

    &lt;formats&gt;
        &lt;format&gt;zip&lt;/format&gt;
    &lt;/formats&gt;

    &lt;includeBaseDirectory&gt;false&lt;/includeBaseDirectory&gt;

    &lt;fileSets&gt;
        &lt;fileSet&gt;
            &lt;directory&gt;${element.distribution.dir}&lt;/directory&gt;
            &lt;outputDirectory&gt;/&lt;/outputDirectory&gt;
        &lt;/fileSet&gt;
    &lt;/fileSets&gt;

&lt;/assembly&gt;</code></pre>
<!-- /wp:code -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="h-5-1-add-to-pom-xml">5.1 Add to <code>pom.xml</code></h4>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Add the plugin referencing the assembly to the <code>pom.xml</code></p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>            &lt;plugin&gt;
                &lt;groupId&gt;org.apache.maven.plugins&lt;/groupId&gt;
                &lt;artifactId&gt;maven-assembly-plugin&lt;/artifactId&gt;
                &lt;version&gt;3.6.0&lt;/version&gt;
                &lt;executions&gt;
                    &lt;execution&gt;
                        &lt;id&gt;zip-target-dir&lt;/id&gt;
                        &lt;phase&gt;package&lt;/phase&gt;
                        &lt;goals&gt;
                            &lt;goal&gt;single&lt;/goal&gt;
                        &lt;/goals&gt;
                        &lt;configuration&gt;
                            &lt;attach&gt;false&lt;/attach&gt;
                            &lt;appendAssemblyId&gt;false&lt;/appendAssemblyId&gt;
                            &lt;finalName&gt;${project.artifactId}-${project.version}&lt;/finalName&gt;
                            &lt;descriptors&gt;
                                &lt;descriptor&gt;src/assembly/zip.xml&lt;/descriptor&gt;
                            &lt;/descriptors&gt;
                        &lt;/configuration&gt;
                    &lt;/execution&gt;
                &lt;/executions&gt;
            &lt;/plugin&gt;</code></pre>
<!-- /wp:code -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-6-optional-enable-crossfire-via-profile">6. (Optional) Enable Crossfire via Profile</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The existing Namazu Crossfire Guide covers how to enable Namazu Crossfire in the build. We include this section if you wish to use it with the build.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="h-6-1-add-namazu-crossfire-dependencies">6.1 Add Namazu Crossfire Dependencies</h4>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code>                &lt;dependency&gt;
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

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"📝\u003cmark style=\u0022background-color:rgba(0, 0, 0, 0)\u0022 class=\u0022has-inline-color has-black-color\u0022\u003eNotes on Scope\u003c/mark\u003e"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="0f985d"><div class="gb-notice-title" style="color:#fff"><p>📝<mark style="background-color:rgba(0, 0, 0, 0)" class="has-inline-color has-black-color">Notes on Scope</mark></p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p>As Namazu Crossfire is added separately, we want it to be listed as provided.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="h-6-1-optional-add-to-local-element-lib">6.1 (Optional) Add to local <code>element-lib</code></h4>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>This ensures that local development builds contain Namazu Crossfire. You do not need to perform this step if you do not wish to debug Namazu Crossfire locally in your IDE.</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>                        &lt;executions&gt;
                            &lt;execution&gt;
                                &lt;id&gt;copy-crossfire-libs&lt;/id&gt;
                                &lt;phase&gt;generate-resources&lt;/phase&gt;
                                &lt;goals&gt;
                                    &lt;goal&gt;copy-dependencies&lt;/goal&gt;
                                &lt;/goals&gt;
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

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"📝\u003cmark style=\u0022background-color:rgba(0, 0, 0, 0)\u0022 class=\u0022has-inline-color has-black-color\u0022\u003eNotes On Dependencies\u003c/mark\u003e"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="81a373"><div class="gb-notice-title" style="color:#fff"><p>📝<mark style="background-color:rgba(0, 0, 0, 0)" class="has-inline-color has-black-color">Notes On Dependencies</mark></p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p>This forces Crossfire to share the classpath with your Element, which is likely not the desired behavior. This should not have any consequences unless you share dependencies with Namazu Crossfire itself. We are aware this is a limitation will fix this with better isolation in future builds.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>If you do not need to use Namazu Crossfire in your local IDE, then you can omit this step.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="h-6-2-optional-instruct-maven-to-fetch-namazu-crossfire-element-from-maven-central">6.2 (Optional) Instruct Maven to Fetch Namazu Crossfire Element from Maven Central</h4>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Namazu Crossfire is available on Maven Central as a shaded jar. The shaded jar is qualified with the classifier <code>element</code> indicating that it is an entirely self-contained instance of Crossfire. This code copies it to a directory for easy deployment.</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>                    &lt;plugin&gt;
                        &lt;groupId&gt;org.apache.maven.plugins&lt;/groupId&gt;
                        &lt;artifactId&gt;maven-dependency-plugin&lt;/artifactId&gt;
                        &lt;version&gt;3.6.0&lt;/version&gt;
                        &lt;executions&gt;
                            &lt;execution&gt;
                                &lt;id&gt;copy-crossfire-element&lt;/id&gt;
                                &lt;phase&gt;generate-resources&lt;/phase&gt;
                                &lt;goals&gt;
                                    &lt;goal&gt;copy-dependencies&lt;/goal&gt;
                                &lt;/goals&gt;
                                &lt;configuration&gt;
                                    &lt;includeScope&gt;provided&lt;/includeScope&gt;
                                    &lt;prependGroupId&gt;true&lt;/prependGroupId&gt;
                                    &lt;includeGroupIds&gt;dev.getelements.elements.crossfire&lt;/includeGroupIds&gt;
                                    &lt;includeArtifactIds&gt;server&lt;/includeArtifactIds&gt;
                                    &lt;includeClassifiers&gt;element&lt;/includeClassifiers&gt;
                                    &lt;outputDirectory&gt;${element.target.dir}/dev.getelements.elements.crossfire.server/lib&lt;/outputDirectory&gt;
                                &lt;/configuration&gt;
                            &lt;/execution&gt;
                        &lt;/executions&gt;
                    &lt;/plugin&gt;</code></pre>
<!-- /wp:code -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-7-verify-deployment">7. Verify Deployment</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Now that you have all of the sections added to your Maven <code>pom.xml</code> you can easily perform a build to ensure that Maven makes a neatly packaged release for you. The simple method to include this is to run Maven from the comand line:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>mvn clean package</code></pre>
<!-- /wp:code -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="h-7-1-verify-bundle-and-zip-was-correctly-made">7.1 Verify Bundle and Zip was Correctly Made</h4>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Within your IDE you shoudl see the following in the target directory. You can also verify this by simply inspecting the directory using your OS's file browser.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":22237,"sizeSlug":"full","linkDestination":"none"} -->
<figure class="wp-block-image size-full"><img src="https://namazustudios.com/wp-content/uploads/2025/11/image.png" alt="" class="wp-image-22237"/><figcaption class="wp-element-caption">The Element showing the Bundle and Contents</figcaption></figure>
<!-- /wp:image -->

<!-- wp:image {"id":22238,"sizeSlug":"full","linkDestination":"none"} -->
<figure class="wp-block-image size-full"><img src="https://namazustudios.com/wp-content/uploads/2025/11/image-1.png" alt="" class="wp-image-22238"/><figcaption class="wp-element-caption">A more Expanded view of Above showing Crossfire</figcaption></figure>
<!-- /wp:image -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="h-7-2-verify-zip-file-created-properly">7.2 Verify Zip File Created Properly</h4>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Use the zip tool to ensure that the element  has everything in the right place.</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>unzip -l target/ElementSample-1.0-SNAPSHOT.zip</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>This should show something similar to the following output:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>Archive:  target/ElementSample-1.0-SNAPSHOT.zip
  Length      Date    Time    Name
---------  ---------- -----   ----
        0  2025-11-04 14:26   lib/
        0  2025-11-04 14:26   classpath/
        0  2025-11-04 14:26   classpath/com/
        0  2025-11-04 14:26   classpath/com/mystudio/
        0  2025-11-04 14:26   classpath/com/mystudio/mygame/
        0  2025-11-04 14:26   classpath/com/mystudio/mygame/rest/
        0  2025-11-04 14:26   classpath/com/mystudio/mygame/service/
        0  2025-11-04 14:26   classpath/com/mystudio/mygame/model/
        0  2025-11-04 14:26   classpath/com/mystudio/mygame/guice/
    16138  2025-10-26 19:28   lib/dev.getelements.elements.sdk-spi-guice-3.4.12.jar
    10681  2021-10-16 11:56   lib/jakarta.inject.jakarta.inject-api-2.0.1.jar
  2974216  2021-09-27 12:19   lib/com.google.guava.guava-31.0.1-jre.jar
   794714  2023-12-24 08:32   lib/org.javassist.javassist-3.30.2-GA.jar
   801785  2023-05-12 10:41   lib/com.google.inject.guice-7.0.0.jar
     4617  2018-11-19 09:57   lib/com.google.guava.failureaccess-1.0.1.jar
   334352  2023-08-27 01:26   lib/org.yaml.snakeyaml-2.2.jar
    17364  2024-03-09 12:06   lib/com.fasterxml.jackson.jakarta.rs.jackson-jakarta-rs-json-provider-2.16.2.jar
    54824  2025-10-26 19:28   lib/dev.getelements.elements.sdk-spi-3.4.12.jar
    14835  2021-05-14 16:04   lib/com.google.errorprone.error_prone_annotations-2.7.1.jar
    95629  2024-05-15 02:42   lib/io.swagger.core.v3.swagger-jaxrs2-jakarta-2.2.22.jar
    16205  2025-10-26 19:28   lib/dev.getelements.elements.sdk-guice-3.4.12.jar
   574448  2024-08-20 00:01   lib/io.github.classgraph.classgraph-4.8.175.jar
    19936  2017-03-30 21:55   lib/com.google.code.findbugs.jsr305-3.0.2.jar
    41513  2021-07-20 04:56   lib/org.slf4j.slf4j-api-1.7.32.jar
     2199  2018-09-11 12:40   lib/com.google.guava.listenablefuture-9999.0-empty-to-avoid-conflict-with-guava.jar
     8781  2017-01-18 15:09   lib/com.google.j2objc.j2objc-annotations-1.3.jar
     4467  2005-08-01 02:23   lib/aopalliance.aopalliance-1.0.jar
   208835  2021-04-01 11:48   lib/org.checkerframework.checker-qual-3.12.0.jar
    32408  2024-03-09 11:09   lib/com.fasterxml.jackson.module.jackson-module-jakarta-xmlbind-annotations-2.16.2.jar
   136369  2024-05-15 02:42   lib/io.swagger.core.v3.swagger-models-jakarta-2.2.22.jar
   582784  2024-03-09 10:43   lib/com.fasterxml.jackson.core.jackson-core-2.16.2.jar
    32475  2024-03-09 12:05   lib/com.fasterxml.jackson.jakarta.rs.jackson-jakarta-rs-base-2.16.2.jar
     1414  2025-11-04 14:26   classpath/com/mystudio/mygame/OpenAPISecurityConfig.class
      791  2025-11-04 14:26   classpath/com/mystudio/mygame/rest/HelloWorld.class
     1766  2025-11-04 14:26   classpath/com/mystudio/mygame/rest/HelloWithAuthentication.class
     2523  2025-11-04 14:26   classpath/com/mystudio/mygame/rest/ExampleContent.class
      289  2025-11-04 14:26   classpath/com/mystudio/mygame/service/GreetingService.class
     2138  2025-11-04 14:26   classpath/com/mystudio/mygame/service/GreetingServiceImpl.class
     2110  2025-11-04 14:26   classpath/com/mystudio/mygame/HelloWorldApplication.class
     1263  2025-11-04 14:26   classpath/com/mystudio/mygame/model/ExamplePostRequest.class
     1825  2025-11-04 14:26   classpath/com/mystudio/mygame/model/ExamplePutResponse.class
     1829  2025-11-04 14:26   classpath/com/mystudio/mygame/model/ExamplePostResponse.class
     1259  2025-11-04 14:26   classpath/com/mystudio/mygame/model/ExamplePutRequest.class
      612  2025-11-04 14:26   classpath/com/mystudio/mygame/package-info.class
      846  2025-11-04 14:26   classpath/com/mystudio/mygame/guice/MyGameModule.class
---------                     -------
  6798240                     45 files</code></pre>
<!-- /wp:code -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"📝\u003cmark style=\u0022background-color:rgba(0, 0, 0, 0)\u0022 class=\u0022has-inline-color has-black-color\u0022\u003eWhat to Look For\u003c/mark\u003e"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="0941e3"><div class="gb-notice-title" style="color:#fff"><p>📝<mark style="background-color:rgba(0, 0, 0, 0)" class="has-inline-color has-black-color">What to Look For</mark></p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>The only Namazu Elements' SDK entries are SPI related.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Third party dependencies (eg guice) are included automatically along with their dependencies.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Test classes are excluded and only the production code appears in <code>classpath</code></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>The Jars are fully-qualified with group and artifact id</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->
