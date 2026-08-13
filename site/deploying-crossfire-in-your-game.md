<h1>Deploying Namazu Crossfire in your game</h1>

<!-- wp:paragraph -->
<p><strong>Namazu Crossfire</strong> is currently distributed as separate <strong>Element</strong>, which means you must install it separately. We publish stable releases to <a href="https://central.sonatype.com/artifact/dev.getelements.elements.crossfire/crossfire/1.0.2">Maven Central</a> and therefore you can simply include Crossfire as a dependency in your game's <a href="https://namazustudios.com/docs-category/custom-code/">Custom Code</a>. <strong>Namazu Crossfire</strong> implements a single <a href="https://namazustudios.com/docs/custom-code/websockets/">WebSocket</a> endpoint which will handle the real time matchmaking process.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-prerequisites">Prerequisites</h2>
<!-- /wp:heading -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"📝 \u003cmark style=\u0022background-color:rgba(0, 0, 0, 0)\u0022 class=\u0022has-inline-color has-black-color\u0022\u003eNote\u003c/mark\u003e"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="cd6687"><div class="gb-notice-title" style="color:#fff"><p>📝 <mark style="background-color:rgba(0, 0, 0, 0)" class="has-inline-color has-black-color">Note</mark></p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p>Starting with Namazu Crossfire 1.0.4 and later, the element artifact has been renamed to <code>server</code> with the <em><a href="https://www.baeldung.com/maven-artifact-classifiers" target="_blank" rel="noreferrer noopener">Classifier</a></em> <code>element</code></p>
<!-- /wp:paragraph -->

<!-- wp:preformatted -->
<pre class="wp-block-preformatted">&lt;dependency><br>    &lt;groupId>dev.getelements.elements.crossfire&lt;/groupId><br>    &lt;artifactId>server&lt;/artifactId><br>    &lt;version>${crossfire.version}&lt;/version><br>    &lt;classifier>element&lt;/classifier><br>    &lt;scope>provided&lt;/scope><br>&lt;/dependency></pre>
<!-- /wp:preformatted --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>This guide assumes you have already completed the steps required to establish an <strong>application configuration for Matchmaking</strong>. If you haven't already done so, please work through the process of <a href="https://namazustudios.com/docs/namazu-elements-core/features/configuration/matchmaking/"><strong>Creating a Matchmaking Application Configuration</strong></a></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>You have sufficient access to publish <strong><a href="https://namazustudios.com/docs-category/custom-code/">Custom Code</a></strong> to your instance of Namazu Elements.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>You have reviewed or are familiar with making a <strong>Custom Element</strong>. Reviewing the Example Code is a great place to start.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>You have completed the steps necessary to setup your <strong>local environment</strong> and have <a href="https://git-scm.com/" target="_blank" rel="noreferrer noopener">git</a> installed.<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><a href="https://namazustudios.com/docs/getting-started/setup-for-windows/">🖥️ - Windows</a></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="https://namazustudios.com/docs/getting-started/mac-os-setup/">🍎 - Mac</a></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="https://namazustudios.com/docs/getting-started/%f0%9f%90%a7ubuntu-linux-setup/">🐧 - Linux</a></li>
<!-- /wp:list-item --></ul>
<!-- /wp:list --></li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":1} -->
<h1 class="wp-block-heading" id="h-local-development-in-ide">Local Development in IDE</h1>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>It is possible to run Namazu Crossfire directly in your local IDE along side your game's custom Element. This enables you to rapidly iterate on development cycles without needing to push to the cloud. Additionally, you can step through and debug any additional endpoint code.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-automatic-method">Automatic Method</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The latest <a href="https://github.com/NamazuStudios/element-example"><strong>Element Example project</strong></a> incorporates Namazu Crossfire right out of the box as of the 3.4 release. However, it must be enabled by activating the profile to include it as a dependency.</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>    &lt;profiles&gt;
        &lt;profile&gt;
            &lt;id&gt;namazu-crossfire&lt;/id&gt;
            &lt;activation&gt;
<mark style="background-color:#f78da7" class="has-inline-color">-               &lt;activeByDefault&gt;false&lt;/activeByDefault&gt;</mark>
<mark style="background-color:#7bdcb5" class="has-inline-color">+               &lt;activeByDefault&gt;true&lt;/activeByDefault&gt;</mark>
            &lt;/activation&gt;
            &lt;dependencies&gt;
                &lt;dependency&gt;
                    &lt;groupId&gt;dev.getelements.elements.crossfire&lt;/groupId&gt;
                    &lt;artifactId&gt;element&lt;/artifactId&gt;
                    &lt;version&gt;${crossfire.version}&lt;/version&gt;
                &lt;/dependency&gt;
            &lt;/dependencies&gt;
        &lt;/profile&gt;
    &lt;/profiles&gt;</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>By changing &lt;activeByDefault&gt; to true and rebuilding the project, you have enabled Namazu Crossfire in your local environment.</p>
<!-- /wp:paragraph -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"📝 \u003cmark style=\u0022background-color:rgba(0, 0, 0, 0)\u0022 class=\u0022has-inline-color has-black-color\u0022\u003eNote\u003c/mark\u003e"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="cd6687"><div class="gb-notice-title" style="color:#fff"><p>📝 <mark style="background-color:rgba(0, 0, 0, 0)" class="has-inline-color has-black-color">Note</mark></p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p>It is sometimes necessary to clean the build before running as re-running local does not always immediately clean up the classpath. Cleaning the build guarantees a complete rebuild of the element path.</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>mvn clean</code></pre>
<!-- /wp:code --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-manual-method">Manual Method</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>This guide is here to incorporate Crossfire into an existing game.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-1-changes-to-lt-dependencies-gt-in-pom-xml">1. Changes to <code>&lt;dependencies&gt;</code> in <code>pom.xml</code></h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Crossfire has a single dependency that needs to be added to your Element.</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>        &lt;dependency&gt;
            &lt;groupId&gt;dev.getelements.elements.crossfire&lt;/groupId&gt;
            &lt;artifactId&gt;element&lt;/artifactId&gt;
            &lt;version&gt;${crossfire.version}&lt;/version&gt;
        &lt;/dependency&gt;</code></pre>
<!-- /wp:code -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-2-update-lt-properties-gt-in-pom-xml">2. Update <code>&lt;properties&gt;</code> in <code>pom.xml</code></h3>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code>    &lt;properties>
        &lt;! -- Existing Properties -->
        &lt;crossfire.version>1.0.4&lt;/crossfire.version>
    &lt;/properties></code></pre>
<!-- /wp:code -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"📝 \u003cmark style=\u0022background-color:rgba(0, 0, 0, 0)\u0022 class=\u0022has-inline-color has-black-color\u0022\u003eNote\u003c/mark\u003e"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="cd6687"><div class="gb-notice-title" style="color:#fff"><p>📝 <mark style="background-color:rgba(0, 0, 0, 0)" class="has-inline-color has-black-color">Note</mark></p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p>Do not replace the entire properties section. only add <code>&lt;crossfire.version>1.0.4&lt;/crossfire.version></code> to the existing properties</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:code -->
<pre class="wp-block-code"><code>&lt;crossfire.version&gt;1.0.2&lt;/crossfire.version&gt;</code></pre>
<!-- /wp:code -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-3-make-the-sdk-aware-of-namazu-crossfire">3. Make the SDK Aware of Namazu Crossfire</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>In order to make the SDK aware of the Crossfire Element, you must update the loader to include it in the application.</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>        // Create the local instance of the Elements server
        final var local = ElementsLocalBuilder.getDefault()
                .withElementNamed(
                        "example",
                        "com.mystudio.mygame",
                        PropertiesAttributes.wrap(elementProperties))
<mark style="background-color:#7bdcb5" class="has-inline-color has-black-color">+               .withElementNamed(
<mark style="background-color:#7bdcb5" class="has-inline-color has-black-color">+</mark>                       "example",
<mark style="background-color:#7bdcb5" class="has-inline-color has-black-color">+</mark>                       "dev.getelements.elements.crossfire",
<mark style="background-color:#7bdcb5" class="has-inline-color has-black-color">+</mark>                       PropertiesAttributes.wrap(crossfireProperties))</mark>
                .build();</code></pre>
<!-- /wp:code -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"📝 \u003cmark style=\u0022background-color:rgba(0, 0, 0, 0)\u0022 class=\u0022has-inline-color has-black-color\u0022\u003eNote\u003c/mark\u003e"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="cd6687"><div class="gb-notice-title" style="color:#fff"><p>📝 <mark style="background-color:rgba(0, 0, 0, 0)" class="has-inline-color has-black-color">Note</mark></p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p>Be sure to replace the name "example" with the application you are using in your particular case.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-verify-successful-deployment">Verify Successful Deployment</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>After enabling in your local IDE, perform the following steps to ensure that Namazu Crossfire was successfully added to the project.</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Launch the Namazu Elements in your IDE.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Visit your local instance's browser at <a href="http://localhost:8080/admin/" target="_blank" rel="noreferrer noopener">http://localhost:8080/admin/</a></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Login using the default credentials or credentials you know to work for a SUPERUSER<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Default Username: root</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Default Password: example</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list --></li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:image {"id":22230,"sizeSlug":"full","linkDestination":"none"} -->
<figure class="wp-block-image size-full"><img src="https://namazustudios.com/wp-content/uploads/2025/10/image-5.png" alt="" class="wp-image-22230"/></figure>
<!-- /wp:image -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Check the left-hand side of the Namazu Elements Admin Panel</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Near the bottom you should see your game's Element along side Namazu Crossfire</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:image {"id":22231,"sizeSlug":"full","linkDestination":"none"} -->
<figure class="wp-block-image size-full"><img src="https://namazustudios.com/wp-content/uploads/2025/10/image-6.png" alt="" class="wp-image-22231"/></figure>
<!-- /wp:image -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Ensure that "CLEAN" appears under the application, in this case the application is named "example"</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Click on "crossfire" to see the deployment status.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:image {"id":22232,"sizeSlug":"full","linkDestination":"none"} -->
<figure class="wp-block-image size-full"><img src="https://namazustudios.com/wp-content/uploads/2025/10/image-7.png" alt="" class="wp-image-22232"/></figure>
<!-- /wp:image -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"📝 \u003cmark style=\u0022background-color:rgba(0, 0, 0, 0)\u0022 class=\u0022has-inline-color has-black-color\u0022\u003eNote\u003c/mark\u003e"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="cd6687"><div class="gb-notice-title" style="color:#fff"><p>📝 <mark style="background-color:rgba(0, 0, 0, 0)" class="has-inline-color has-black-color">Note</mark></p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p>If you use advanced configuration parameters, such as overriding Namazu Crossfire's default URL these settings may appear slightly differently. The most important things to observe are the deployment status (CLEAN) and the URI from where Namazu Crossfire is hosted.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->
