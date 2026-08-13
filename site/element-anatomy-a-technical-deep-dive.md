<h1>Element Anatomy: A Technical Deep Dive</h1>

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"⚠️ Pre-Release ⚠️"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="61bdce"><div class="gb-notice-title" style="color:#fff"><p>⚠️ Pre-Release ⚠️</p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p>This document covers topcis that are in development for the 3.7 Release of Namazu Elements. Prior versions may not support everything mentioned here.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="introduction">Introduction</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Namazu Elements provides a sophisticated plugin architecture for extending Java backend servers with custom business logic.&nbsp;At its core,&nbsp;an Element is a self-contained,&nbsp;isolated module with carefully controlled dependencies and classloading boundaries.&nbsp;This document explains the anatomy of Elements,&nbsp;their packaging formats,&nbsp;and how they integrate with the Java ecosystem.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><strong>Target Audience:</strong>&nbsp;Technical users with basic understanding of Java concepts.&nbsp;Prior knowledge of Java classloaders,&nbsp;Maven,&nbsp;and dependency management is helpful but not required.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="what-is-an-element">What is an Element?</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>An&nbsp;<strong>Element</strong>&nbsp;is a deployable unit of functionality within the Namazu Elements framework.&nbsp;Think of it as a plugin or module that can be:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Dynamically<strong> </strong>loaded at runtime without restarting the server</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Isolated from other Elements to prevent version conflicts</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Configured independently with custom attributes</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Distributed as either directories or packaged <code>.elm</code> files</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Sourced from Maven Central, GitHub Packages, or any Maven repository</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>Each Element contains business logic&nbsp;(your custom code)&nbsp;along with its dependencies,&nbsp;organized in a specific structure that enables safe,&nbsp;isolated loading.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="directory-structure">Directory Structure</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>An Element on disk follows a flat,&nbsp;organized directory structure:</p>
<!-- /wp:paragraph -->

<!-- wp:preformatted -->
<pre class="wp-block-preformatted"><code>deployment/<br>├── api/                    # Shared API JARs (optional)<br>│   ├── shared-api-1.0.jar<br>│   └── common-types.jar<br>│<br>├── my-element/            # Element directory (name is arbitrary)<br>│   ├── api/               # Element-specific API JARs (optional)<br>│   │   └── my-element-api-1.0.jar<br>│   │<br>│   ├── spi/               # Service Provider Interface JARs (optional)<br>│   │   └── my-spi-impl-1.0.jar<br>│   │<br>│   ├── lib/               # Implementation JARs (required)<br>│   │   ├── my-element-impl-1.0.jar<br>│   │   ├── guava-31.1-jre.jar<br>│   │   └── jackson-core-2.14.0.jar<br>│   │<br>│   ├── classpath/         # Raw class directories (optional)<br>│   │   └── (compiled classes)<br>│   │<br>│   └── dev.getelements.element.attributes.properties  # Configuration (optional)<br>│<br>└── another-element/       # Another Element (completely isolated)<br>    └── ...<br></code></pre>
<!-- /wp:preformatted -->

<!-- wp:paragraph -->
<p><strong>Key Principles:</strong></p>
<!-- /wp:paragraph -->

<!-- wp:list {"ordered":true} -->
<ol class="wp-block-list"><!-- wp:list-item -->
<li>Flat Structure: Each element is a top-level subdirectory. No nesting of elements within elements.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Name Agnostic: Element directory names are arbitrary; metadata comes from annotations in the code, not directory names.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Self-Contained: Each element directory contains everything needed to run that element.</li>
<!-- /wp:list-item --></ol>
<!-- /wp:list -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="the-four-core-components">The Four Core Components</h2>
<!-- /wp:heading -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="api-shared-interfaces">API: Shared Interfaces</h3>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Location<strong>:</strong> <code>api/</code> directory </li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Purpose<strong>:</strong> Contains interface definitions and data types that multiple Elements share </li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Visibility<strong>:</strong> Shared across ALL Elements in the deployment </li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Classloader Level<strong>:</strong> Highest in the hierarchy (most visible)</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="what-goes-here">What Goes Here?</h4>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Interface definitions that multiple elements need to communicate</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Data transfer objects (DTOs) that cross element boundaries</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Exception types that elements throw/catch across boundaries</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Annotations used by multiple elements</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="why-it-matters">Why It Matters</h4>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>API JARs create a shared vocabulary between Elements. When <code>ElementA</code> calls a service from <code>ElementB</code>, they both need to agree on the interface definition. The API classloader ensures both see the exact same class definitions, preventing <code>ClassCastException</code> errors.</p>
<!-- /wp:paragraph -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"Best Practice"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="39d990"><div class="gb-notice-title" style="color:#fff"><p>Best Practice</p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p>Keep API JARs extremely lean. Only include interfaces, data classes, and minimal dependencies. Heavy implementation details belong in <code>lib/</code>.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="example-api-interface">Example API Interface</h4>
<!-- /wp:heading -->

<!-- wp:preformatted -->
<pre class="wp-block-preformatted"><code>package com.example.payment.api;<br><br>public interface PaymentProcessor {<br>    PaymentResult processPayment(PaymentRequest request);<br>}<br></code></pre>
<!-- /wp:preformatted -->

<!-- wp:paragraph -->
<p>Maven Coordinates<strong> -</strong> Available from any Maven repository:</p>
<!-- /wp:paragraph -->

<!-- wp:preformatted -->
<pre class="wp-block-preformatted"><code>&lt;dependency&gt;<br>    &lt;groupId&gt;com.example&lt;/groupId&gt;<br>    &lt;artifactId&gt;payment-api&lt;/artifactId&gt;<br>    &lt;version&gt;1.0.0&lt;/version&gt;<br>&lt;/dependency&gt;<br></code></pre>
<!-- /wp:preformatted -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="spi-service-provider-interface">SPI: Service Provider Interface</h3>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Location<strong>:</strong> <code>spi/</code> directory </li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Purpose<strong>:</strong> Implements <a href="https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/ServiceLoader.html">Java Service Provider Interface</a> pattern for dependency injection at deployment time </li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Visibility<strong>:</strong> Element-specific, not shared with other elements </li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Classloader Level<strong>:</strong> Between API and Implementation</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="what-goes-here-1">What Goes Here?</h4>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Service provider implementations discovered via <a href="https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/ServiceLoader.html"><code>ServiceLoader</code></a></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Plugin frameworks like SLF4J bindings, JDBC drivers, etc.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Runtime-selected implementations that you want to swap without recompiling</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="why-it-matters-1">Why It Matters</h4>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The SPI layer enables deployment-time flexibility. You can ship an Element with an interface in API and choose the implementation at deployment by adding the appropriate SPI JARs. This is crucial for:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Multi-database support: JDBC drivers selected at deployment</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Logging frameworks: SLF4J binding chosen at deployment (Logback vs Log4j)</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Cloud provider SDKs: AWS vs Azure vs GCP implementations</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="example-logging-configuration">Example: Logging Configuration</h4>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Your Element might use SLF4J&nbsp;(API):</p>
<!-- /wp:paragraph -->

<!-- wp:preformatted -->
<pre class="wp-block-preformatted"><code>import org.slf4j.Logger;<br>import org.slf4j.LoggerFactory;<br><br>public class MyElement {<br>    private static final Logger logger = LoggerFactory.getLogger(MyElement.class);<br>}<br></code></pre>
<!-- /wp:preformatted -->

<!-- wp:paragraph -->
<p>At&nbsp;<strong>deployment time</strong>,&nbsp;you choose the binding in&nbsp;<code>spi/</code>:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><code>spi/logback-classic-1.4.14.jar</code>&nbsp;→ Uses Logback</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>spi/log4j-slf4j2-impl-2.20.0.jar</code>&nbsp;→ Uses Log4j2</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p><strong>Maven Coordinates:</strong>&nbsp;Runtime dependency selection:</p>
<!-- /wp:paragraph -->

<!-- wp:preformatted -->
<pre class="wp-block-preformatted"><code>&lt;!-- In your build --&gt;<br>&lt;dependency&gt;<br>    &lt;groupId&gt;org.slf4j&lt;/groupId&gt;<br>    &lt;artifactId&gt;slf4j-api&lt;/artifactId&gt;<br>    &lt;scope&gt;compile&lt;/scope&gt;<br>&lt;/dependency&gt;<br><br>&lt;!-- At deployment time (added to spi/) --&gt;<br>&lt;dependency&gt;<br>    &lt;groupId&gt;ch.qos.logback&lt;/groupId&gt;<br>    &lt;artifactId&gt;logback-classic&lt;/artifactId&gt;<br>    &lt;version&gt;1.4.14&lt;/version&gt;<br>    &lt;scope&gt;runtime&lt;/scope&gt;<br>&lt;/dependency&gt;<br></code></pre>
<!-- /wp:preformatted -->

<!-- wp:paragraph -->
<p><strong>Learn More:</strong></p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><a href="https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/ServiceLoader.html">Java ServiceLoader Tutorial</a></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="https://www.baeldung.com/java-spi">SPI Pattern Overview</a></li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="lib-implementation-libraries">LIB: Implementation Libraries</h3>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Location<strong>:</strong> <code>lib/</code> directory </li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Purpose<strong>:</strong> Contains your Element's implementation code and all dependencies </li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Visibility<strong>:</strong> Element-specific, completely isolated from other elements</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Classloader Level<strong>:</strong> Lowest in the hierarchy (most isolated)</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="what-goes-here-2">What Goes Here?</h4>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Your Element's implementation code (the JAR you build)</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Third-party dependencies: Guava, Apache Commons, Jackson, etc.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Transitive dependencies: Everything your dependencies need</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Framework libraries: Spring, Hibernate, etc. (if not provided by the platform)</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="why-it-matters-2">Why It Matters</h4>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The <code>lib/</code> directory provides dependency isolation. Each Element can use different versions of the same library without conflict:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><code>ElementA</code>&nbsp;can use Guava 30.0</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>ElementB</code>&nbsp;can use Guava 31.1</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>No version conflicts!</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>This is the primary benefit of the Element architecture compared to traditional WAR/EAR deployments where all code shares a single classpath.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="example-multiple-library-versions">Example: Multiple Library Versions</h4>
<!-- /wp:heading -->

<!-- wp:preformatted -->
<pre class="wp-block-preformatted"><code>deployment/<br>├── shopping-cart/<br>│   └── lib/<br>│       ├── shopping-cart-1.0.jar<br>│       ├── guava-30.0-jre.jar        # Uses older Guava<br>│       └── jackson-core-2.13.0.jar<br>│<br>└── inventory/<br>    └── lib/<br>        ├── inventory-1.0.jar<br>        ├── guava-31.1-jre.jar        # Uses newer Guava - NO CONFLICT!<br>        └── jackson-core-2.14.0.jar<br></code></pre>
<!-- /wp:preformatted -->

<!-- wp:paragraph -->
<p>Maven Integration<strong>:</strong> All dependencies automatically resolved:</p>
<!-- /wp:paragraph -->

<!-- wp:preformatted -->
<pre class="wp-block-preformatted"><code>&lt;dependencies&gt;<br>    &lt;dependency&gt;<br>        &lt;groupId&gt;com.google.guava&lt;/groupId&gt;<br>        &lt;artifactId&gt;guava&lt;/artifactId&gt;<br>        &lt;version&gt;31.1-jre&lt;/version&gt;<br>    &lt;/dependency&gt;<br>    &lt;dependency&gt;<br>        &lt;groupId&gt;com.fasterxml.jackson.core&lt;/groupId&gt;<br>        &lt;artifactId&gt;jackson-core&lt;/artifactId&gt;<br>        &lt;version&gt;2.14.0&lt;/version&gt;<br>    &lt;/dependency&gt;<br>&lt;/dependencies&gt;<br></code></pre>
<!-- /wp:preformatted -->

<!-- wp:paragraph -->
<p>The build process automatically fetches from:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><a href="https://search.maven.org/">Maven Central</a></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-apache-maven-registry">GitHub Packages</a></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Custom enterprise repositories</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Any Maven-compatible repository</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="classpath-raw-class-files">Classpath: Raw Class Files</h3>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Location<strong>:</strong> <code>classpath/</code> directory </li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Purpose<strong>:</strong> Direct access to compiled <code>.class</code> files (rare, advanced use case) </li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Visibility<strong>:</strong> Element-specific </li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Classloader Level<strong>:</strong> Same as <code>lib/</code></li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="what-goes-here-3">What Goes Here?</h4>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Unpackaged compiled classes (not in a JAR)</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Generated code from annotation processors</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Hot-reload development scenarios</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="why-it-exists">Why It Exists</h4>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The&nbsp;<code>classpath/</code>&nbsp;directory is primarily for:</p>
<!-- /wp:paragraph -->

<!-- wp:list {"ordered":true} -->
<ol class="wp-block-list"><!-- wp:list-item -->
<li>Development workflows: Compile directly to a directory for fast iteration</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Code generation: Annotation processors that generate classes at runtime</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Advanced scenarios: When you explicitly need directory-based classpath entries</li>
<!-- /wp:list-item --></ol>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p><strong>Note:</strong>&nbsp;For production deployments,&nbsp;prefer packaging everything as JARs in&nbsp;<code>lib/</code>.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="elm-files-portable-packaging">ELM Files: Portable Packaging</h2>
<!-- /wp:heading -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="what-is-an-elm-file">What is an ELM File?</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>An&nbsp;<strong>ELM file</strong>&nbsp;(<code>.elm</code>&nbsp;extension)&nbsp;is simply a&nbsp;<strong>ZIP archive</strong>&nbsp;containing an Element's directory structure.&nbsp;That's it.&nbsp;There's no special format,&nbsp;no proprietary tooling&nbsp;-&nbsp;it's just a renamed ZIP file.</p>
<!-- /wp:paragraph -->

<!-- wp:preformatted -->
<pre class="wp-block-preformatted"><code># An ELM file is literally just a ZIP file<br>$ unzip -l my-element.elm<br><br>Archive:  my-element.elm<br>  Length      Date    Time    Name<br>---------  ---------- -----   ----<br>        0  2024-01-15 10:30   my-element/<br>        0  2024-01-15 10:30   my-element/api/<br>   154832  2024-01-15 10:30   my-element/api/my-api-1.0.jar<br>        0  2024-01-15 10:30   my-element/spi/<br>   421644  2024-01-15 10:30   my-element/spi/logback-1.4.14.jar<br>        0  2024-01-15 10:30   my-element/lib/<br>  1204832  2024-01-15 10:30   my-element/lib/my-element-impl-1.0.jar<br>  2538194  2024-01-15 10:30   my-element/lib/guava-31.1-jre.jar<br></code></pre>
<!-- /wp:preformatted -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="why-elm-files">Why ELM Files?</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>ELM files provide portability and convenience:</p>
<!-- /wp:paragraph -->

<!-- wp:list {"ordered":true} -->
<ol class="wp-block-list"><!-- wp:list-item -->
<li>Single File Distribution: Ship your Element as one file instead of a directory tree</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Maven Integration: Publish to Maven Central or any Maven repository</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Versioning: Use Maven's versioning system (<code>1.0.0</code>, <code>2.0.0-SNAPSHOT</code>, etc.)</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Dependency Management: Let Maven resolve ELM files just like JARs</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>No Extraction Needed: Java's <a href="https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/nio/file/FileSystems.html"><code>FileSystems</code> API</a> reads ZIP files directly</li>
<!-- /wp:list-item --></ol>
<!-- /wp:list -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="elm-files-as-zip-filesystems">ELM Files as ZIP Filesystems</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>One of the most elegant aspects of ELM files is that they don't need to be extracted. Java can treat ZIP files as virtual filesystems:</p>
<!-- /wp:paragraph -->

<!-- wp:preformatted -->
<pre class="wp-block-preformatted"><code>// Open an ELM file as a filesystem<br>Path elmFile = Path.of("/deployments/my-element.elm");<br>FileSystem fs = FileSystems.newFileSystem(elmFile);<br><br>// Navigate inside like a directory<br>Path apiDir = fs.getPath("/my-element/api");<br>Path libDir = fs.getPath("/my-element/lib");<br><br>// Read JARs directly from the ELM<br>// No extraction to disk needed!<br></code></pre>
<!-- /wp:preformatted -->

<!-- wp:paragraph -->
<p>This means:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Zero I/O overhead: No unpacking to temporary directories</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Memory efficient: JARs loaded directly from the ZIP</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Atomic updates: Replace the ELM file for instant deployment updates</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p><strong>Learn More:</strong></p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><a href="https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/nio/file/FileSystem.html">Java NIO FileSystem Documentation</a></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="https://docs.oracle.com/en/java/javase/21/docs/api/jdk.zipfs/module-summary.html">ZIP File System Provider</a></li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="creating-an-elm-file">Creating an ELM File</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Using Maven&nbsp;(recommended):</p>
<!-- /wp:paragraph -->

<!-- wp:preformatted -->
<pre class="wp-block-preformatted"><code>&lt;build&gt;<br>    &lt;plugins&gt;<br>        &lt;plugin&gt;<br>            &lt;groupId&gt;org.apache.maven.plugins&lt;/groupId&gt;<br>            &lt;artifactId&gt;maven-assembly-plugin&lt;/artifactId&gt;<br>            &lt;configuration&gt;<br>                &lt;descriptors&gt;<br>                    &lt;descriptor&gt;src/assembly/elm.xml&lt;/descriptor&gt;<br>                &lt;/descriptors&gt;<br>                &lt;appendAssemblyId&gt;false&lt;/appendAssemblyId&gt;<br>                &lt;finalName&gt;${project.artifactId}-${project.version}&lt;/finalName&gt;<br>            &lt;/configuration&gt;<br>            &lt;executions&gt;<br>                &lt;execution&gt;<br>                    &lt;phase&gt;package&lt;/phase&gt;<br>                    &lt;goals&gt;<br>                        &lt;goal&gt;single&lt;/goal&gt;<br>                    &lt;/goals&gt;<br>                &lt;/execution&gt;<br>            &lt;/executions&gt;<br>        &lt;/plugin&gt;<br>    &lt;/plugins&gt;<br>&lt;/build&gt;<br></code></pre>
<!-- /wp:preformatted -->

<!-- wp:paragraph -->
<p>Using command line:</p>
<!-- /wp:paragraph -->

<!-- wp:preformatted -->
<pre class="wp-block-preformatted"><code># Package a directory as an ELM file<br>cd deployment/<br>zip -r my-element.elm my-element/<br><br># Or use jar command (same format)<br>jar -cf my-element.elm -C deployment/ my-element/<br></code></pre>
<!-- /wp:preformatted -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="publishing-to-maven">Publishing to Maven</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>ELM files are first-class Maven artifacts:</p>
<!-- /wp:paragraph -->

<!-- wp:preformatted -->
<pre class="wp-block-preformatted"><code>&lt;dependency&gt;<br>    &lt;groupId&gt;com.example&lt;/groupId&gt;<br>    &lt;artifactId&gt;payment-element&lt;/artifactId&gt;<br>    &lt;version&gt;1.0.0&lt;/version&gt;<br>    &lt;type&gt;elm&lt;/type&gt;  &lt;!-- ELM files use custom type --&gt;<br>&lt;/dependency&gt;<br></code></pre>
<!-- /wp:preformatted -->

<!-- wp:paragraph -->
<p>Deploy to Maven repository:</p>
<!-- /wp:paragraph -->

<!-- wp:preformatted -->
<pre class="wp-block-preformatted"><code>mvn deploy:deploy-file \<br>    -Dfile=my-element.elm \<br>    -DgroupId=com.example \<br>    -DartifactId=my-element \<br>    -Dversion=1.0.0 \<br>    -Dpackaging=elm \<br>    -DrepositoryId=my-repo \<br>    -Durl=https://maven.example.com/repository<br></code></pre>
<!-- /wp:preformatted -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="maven-integration">Maven Integration</h2>
<!-- /wp:heading -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="universal-artifact-resolution">Universal Artifact Resolution</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Every component of an Element can be sourced from Maven repositories:</p>
<!-- /wp:paragraph -->

<!-- wp:preformatted -->
<pre class="wp-block-preformatted"><code>// Deploying an Element with Maven coordinates<br>ElementPathDefinition definition = new ElementPathDefinition(<br>    List.of("com.google.guava:guava:31.1-jre"),              // API artifacts<br>    List.of("ch.qos.logback:logback-classic:1.4.14"),       // SPI artifacts<br>    List.of("com.example:my-element-impl:1.0.0"),           // Implementation<br>    "my-element-path",                                       // Deployment path<br>    Map.of("environment", "production")                      // Attributes<br>);<br></code></pre>
<!-- /wp:preformatted -->

<!-- wp:paragraph -->
<p>At deployment time,&nbsp;the framework:</p>
<!-- /wp:paragraph -->

<!-- wp:list {"ordered":true} -->
<ol class="wp-block-list"><!-- wp:list-item -->
<li><strong>Resolves Maven coordinates</strong>&nbsp;to JAR files</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Downloads transitively</strong>&nbsp;all dependencies</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Organizes JARs</strong>&nbsp;into api/, spi/, lib/ directories</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Loads the Element</strong>&nbsp;with proper isolation</li>
<!-- /wp:list-item --></ol>
<!-- /wp:list -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="multiple-repository-support">Multiple Repository Support</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Configure repositories for artifact resolution:</p>
<!-- /wp:paragraph -->

<!-- wp:preformatted -->
<pre class="wp-block-preformatted"><code>// Use Maven Central (default)<br>deployment.useDefaultRepositories(true);<br><br>// Add custom repositories<br>deployment.addRepository(new ArtifactRepository(<br>    "github-packages",<br>    "https://maven.pkg.github.com/myorg/myrepo"<br>));<br><br>deployment.addRepository(new ArtifactRepository(<br>    "corporate-nexus",<br>    "https://nexus.company.com/repository/maven-releases"<br>));<br></code></pre>
<!-- /wp:preformatted -->

<!-- wp:paragraph -->
<p>Supported repository types:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong><a href="https://search.maven.org/">Maven Central</a></strong>: Default public repository</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong><a href="https://docs.github.com/en/packages">GitHub Packages</a></strong>: Host alongside code</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong><a href="https://www.sonatype.com/products/nexus-repository">Sonatype Nexus</a></strong>: Enterprise repository manager</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong><a href="https://jfrog.com/artifactory/">JFrog Artifactory</a></strong>: Universal artifact repository</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Custom Maven repositories</strong>: Any HTTP/HTTPS Maven repo</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="dependency-resolution">Dependency Resolution</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The framework uses Maven's dependency resolution algorithm:</p>
<!-- /wp:paragraph -->

<!-- wp:preformatted -->
<pre class="wp-block-preformatted"><code>com.example:my-element:1.0.0<br>├── com.google.guava:guava:31.1-jre<br>│   ├── com.google.guava:failureaccess:1.0.1<br>│   └── com.google.guava:listenablefuture:9999.0-empty-to-avoid-conflict-with-guava<br>├── com.fasterxml.jackson.core:jackson-core:2.14.0<br>└── org.slf4j:slf4j-api:2.0.0<br></code></pre>
<!-- /wp:preformatted -->

<!-- wp:paragraph -->
<p>All transitive dependencies are automatically:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Downloaded from configured repositories</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Cached locally (typically <code>~/.m2/repository</code>)</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Copied to the appropriate lib/ directory</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Isolated per Element</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p><strong>Learn More:</strong></p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><a href="https://maven.apache.org/guides/introduction/introduction-to-dependency-mechanism.html">Maven Dependency Mechanism</a></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="https://search.maven.org/">Maven Central Repository</a></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-apache-maven-registry">Working with GitHub Packages</a></li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="classloader-hierarchy">Classloader Hierarchy</h2>
<!-- /wp:heading -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="the-three-tier-architecture">The Three-Tier Architecture</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Elements use a sophisticated&nbsp;<a href="https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/lang/ClassLoader.html">classloader</a>&nbsp;hierarchy to achieve isolation while enabling controlled sharing:</p>
<!-- /wp:paragraph -->

<!-- wp:preformatted -->
<pre class="wp-block-preformatted"><code>┌─────────────────────────────────────┐<br>│   Bootstrap ClassLoader             │  (JDK classes)<br>│   (java.*, javax.*, etc.)           │<br>└─────────────┬───────────────────────┘<br>              │<br>              │ delegates to<br>              ↓<br>┌─────────────────────────────────────┐<br>│   Platform ClassLoader              │  (Platform modules)<br>│   (org.slf4j, etc.)                 │<br>└─────────────┬───────────────────────┘<br>              │<br>              │ delegates to<br>              ↓<br>┌─────────────────────────────────────┐<br>│   System ClassLoader                │  (Application classpath)<br>│   (Elements Framework SDK)          │<br>└─────────────┬───────────────────────┘<br>              │<br>              │ delegates to<br>              ↓<br>┌─────────────────────────────────────┐<br>│   PermittedTypes ClassLoader        │  (Framework internals)<br>│   (Selective type borrowing)        │<br>└─────────────┬───────────────────────┘<br>              │<br>              │ delegates to<br>              ↓<br>┌─────────────────────────────────────┐<br>│   API ClassLoader (SHARED)          │  ← All Elements see these classes<br>│   api/*.jar from ALL elements       │<br>└─────────────┬───────────────────────┘<br>              │<br>              │ delegates to<br>              ↓<br>┌─────────────────────────────────────┐<br>│   SPI ClassLoader (per-element)     │  ← Element-specific<br>│   spi/*.jar for this element        │<br>└─────────────┬───────────────────────┘<br>              │<br>              │ delegates to<br>              ↓<br>┌─────────────────────────────────────┐<br>│   Implementation ClassLoader        │  ← Element-specific<br>│   lib/*.jar + classpath/            │<br>│   (fully isolated)                  │<br>└─────────────────────────────────────┘<br></code></pre>
<!-- /wp:preformatted -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="why-this-hierarchy">Why This Hierarchy?</h3>
<!-- /wp:heading -->

<!-- wp:list {"ordered":true} -->
<ol class="wp-block-list"><!-- wp:list-item -->
<li><strong>API Layer (Shared)</strong>: Ensures all Elements use the exact same interface definitions<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Prevents&nbsp;<code>ClassCastException</code>&nbsp;when Elements communicate</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Single source of truth for shared contracts</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list --></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>SPI Layer (Per-Element)</strong>: Allows different implementations of the same service<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Each Element can choose its own JDBC driver</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Different logging implementations per Element</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list --></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Implementation Layer (Isolated)</strong>: Complete dependency independence<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Each Element can use different library versions</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>No version conflicts between Elements</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Memory isolated (unload Element = free memory)</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list --></li>
<!-- /wp:list-item --></ol>
<!-- /wp:list -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="class-resolution-example">Class Resolution Example</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>When code in&nbsp;<code>ElementA</code>&nbsp;calls a method:</p>
<!-- /wp:paragraph -->

<!-- wp:preformatted -->
<pre class="wp-block-preformatted"><code>// ElementA code<br>PaymentProcessor processor = getProcessor();  // Returns interface from API<br>PaymentResult result = processor.processPayment(request);<br></code></pre>
<!-- /wp:preformatted -->

<!-- wp:paragraph -->
<p>Class loading flow:</p>
<!-- /wp:paragraph -->

<!-- wp:list {"ordered":true} -->
<ol class="wp-block-list"><!-- wp:list-item -->
<li><code>PaymentProcessor</code>&nbsp;interface → Found in&nbsp;<strong>API ClassLoader</strong>&nbsp;(shared)</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>PaymentRequest</code>&nbsp;DTO → Found in&nbsp;<strong>API ClassLoader</strong>&nbsp;(shared)</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>PaymentResult</code>&nbsp;DTO → Found in&nbsp;<strong>API ClassLoader</strong>&nbsp;(shared)</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Actual implementation class → Found in&nbsp;<strong>ElementA's Implementation ClassLoader</strong>&nbsp;(isolated)</li>
<!-- /wp:list-item --></ol>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p><strong>Learn More:</strong></p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><a href="https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/lang/ClassLoader.html">Java ClassLoader Documentation</a></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="https://www.baeldung.com/java-classloaders">Understanding Java ClassLoaders</a></li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="configuration-and-attributes">Configuration and Attributes</h2>
<!-- /wp:heading -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="element-attributes">Element Attributes</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Elements can be configured using a Java Properties file:</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><strong>File:</strong>&nbsp;<code>dev.getelements.element.attributes.properties</code></p>
<!-- /wp:paragraph -->

<!-- wp:preformatted -->
<pre class="wp-block-preformatted"><code># Element configuration<br>environment=production<br>database.url=jdbc:postgresql://localhost:5432/mydb<br>cache.enabled=true<br>max.connections=50<br></code></pre>
<!-- /wp:preformatted -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="reading-attributes-in-code">Reading Attributes in Code</h3>
<!-- /wp:heading -->

<!-- wp:preformatted -->
<pre class="wp-block-preformatted"><code>@ElementDefinition(<br>    name = "com.example.my-element",<br>    version = "1.0.0"<br>)<br>public class MyElement {<br><br>    public void onCreate(Attributes attributes) {<br>        String env = attributes.getAttribute("environment");<br>        boolean cacheEnabled = Boolean.parseBoolean(<br>            attributes.getAttribute("cache.enabled", "false")<br>        );<br><br>        // Configure element based on attributes<br>    }<br>}<br></code></pre>
<!-- /wp:preformatted -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="deployment-level-attributes">Deployment-Level Attributes</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Attributes can also be specified at deployment time,&nbsp;overriding file-based configuration:</p>
<!-- /wp:paragraph -->

<!-- wp:preformatted -->
<pre class="wp-block-preformatted"><code>// Deployment configuration with path-specific attributes<br>Map&lt;String, Map&lt;String, Object&gt;&gt; pathAttributes = Map.of(<br>    "/my-element", Map.of(<br>        "environment", "staging",<br>        "debug.enabled", true<br>    )<br>);<br><br>ElementDeployment deployment = new ElementDeployment(<br>    deploymentId,<br>    application,<br>    null,  // ELM file<br>    pathAttributes,  // Override attributes<br>    elementDefinitions,<br>    packageDefinitions,<br>    true,  // Use default repositories<br>    customRepositories,<br>    ElementDeploymentState.ENABLED,<br>    version<br>);<br></code></pre>
<!-- /wp:preformatted -->

<!-- wp:paragraph -->
<p><strong>Attribute Precedence</strong>&nbsp;(highest to lowest):</p>
<!-- /wp:paragraph -->

<!-- wp:list {"ordered":true} -->
<ol class="wp-block-list"><!-- wp:list-item -->
<li>Deployment-level path attributes (runtime)</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Element definition attributes (deployment config)</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Properties file attributes (packaged with Element)</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Default values (in code)</li>
<!-- /wp:list-item --></ol>
<!-- /wp:list -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="loading-process">Loading Process</h2>
<!-- /wp:heading -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="from-disk-to-running-element">From Disk to Running Element</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The complete Element loading process:</p>
<!-- /wp:paragraph -->

<!-- wp:preformatted -->
<pre class="wp-block-preformatted"><code>1. DISCOVERY:<br>   Scan deployment directory or ELM file<br>   Identify element directories<br><br>2. VALIDATION:<br>   Verify directory structure<br>   Check for api/, spi/, lib/, or classpath/<br>   Load attributes from properties file<br><br>3. CLASSLOADER CONSTRUCTION:<br>   Build API ClassLoader (shared across all elements)<br>   Build SPI ClassLoader (per-element, optional)<br>   Build Implementation ClassLoader (per-element)<br><br>4. SERVICE LOADING:<br>   Use Java ServiceLoader to find ElementLoader SPI<br>   Scan element classpath for @ElementDefinition<br><br>5. ELEMENT REGISTRATION:<br>   Register Element with ElementRegistry<br>   Invoke onCreate() lifecycle method<br>   Element is now active<br><br>6. LIFECYCLE MANAGEMENT:<br>   Element runs until unloaded<br>   onClose() called during unload<br>   ClassLoaders garbage collected<br>   Resources freed</code></pre>
<!-- /wp:preformatted -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="practical-examples">Practical Examples</h2>
<!-- /wp:heading -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="example-1-payment-processing-element">Example 1: Payment Processing Element</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p><strong>Structure:</strong></p>
<!-- /wp:paragraph -->

<!-- wp:preformatted -->
<pre class="wp-block-preformatted"><code>payment-element/<br>├── api/<br>│   └── payment-api-1.0.jar          # PaymentProcessor interface<br>├── spi/<br>│   └── stripe-adapter-1.0.jar       # Stripe implementation<br>├── lib/<br>│   ├── payment-impl-1.0.jar         # Your implementation<br>│   ├── stripe-java-22.0.0.jar       # Stripe SDK<br>│   └── jackson-databind-2.14.0.jar  # JSON processing<br>└── dev.getelements.element.attributes.properties<br></code></pre>
<!-- /wp:preformatted -->

<!-- wp:paragraph -->
<p><strong>API Interface</strong>&nbsp;(<code>payment-api-1.0.jar</code>):</p>
<!-- /wp:paragraph -->

<!-- wp:preformatted -->
<pre class="wp-block-preformatted"><code>package com.example.payment.api;<br><br>public interface PaymentProcessor {<br>    PaymentResult processPayment(PaymentRequest request);<br>    RefundResult refundPayment(String transactionId);<br>}<br></code></pre>
<!-- /wp:preformatted -->

<!-- wp:paragraph -->
<p><strong>SPI Adapter</strong>&nbsp;(<code>stripe-adapter-1.0.jar</code>):</p>
<!-- /wp:paragraph -->

<!-- wp:preformatted -->
<pre class="wp-block-preformatted"><code>package com.example.payment.stripe;<br><br>@ServiceProvider(PaymentProcessor.class)<br>public class StripePaymentProcessor implements PaymentProcessor {<br><br>    @Override<br>    public PaymentResult processPayment(PaymentRequest request) {<br>        // Stripe-specific implementation<br>        StripeClient client = new StripeClient(apiKey);<br>        return client.charge(request.getAmount(), request.getCurrency());<br>    }<br>}<br></code></pre>
<!-- /wp:preformatted -->

<!-- wp:paragraph -->
<p><strong>Element Implementation</strong>&nbsp;(<code>payment-impl-1.0.jar</code>):</p>
<!-- /wp:paragraph -->

<!-- wp:preformatted -->
<pre class="wp-block-preformatted"><code>package com.example.payment;<br><br>@ElementDefinition(<br>    name = "com.example.payment-element",<br>    version = "1.0.0"<br>)<br>public class PaymentElement {<br><br>    private PaymentProcessor processor;<br><br>    public void onCreate(Attributes attributes) {<br>        // ServiceLoader finds StripePaymentProcessor from spi/<br>        ServiceLoader&lt;PaymentProcessor&gt; loader =<br>            ServiceLoader.load(PaymentProcessor.class);<br><br>        processor = loader.findFirst()<br>            .orElseThrow(() -&gt; new IllegalStateException("No payment processor found"));<br>    }<br><br>    @RestEndpoint("/api/payment")<br>    public PaymentResult handlePayment(PaymentRequest request) {<br>        return processor.processPayment(request);<br>    }<br>}<br></code></pre>
<!-- /wp:preformatted -->

<!-- wp:paragraph -->
<p><strong>Maven Deployment:</strong></p>
<!-- /wp:paragraph -->

<!-- wp:preformatted -->
<pre class="wp-block-preformatted"><code>&lt;dependency&gt;<br>    &lt;groupId&gt;com.example&lt;/groupId&gt;<br>    &lt;artifactId&gt;payment-element&lt;/artifactId&gt;<br>    &lt;version&gt;1.0.0&lt;/version&gt;<br>    &lt;type&gt;elm&lt;/type&gt;<br>&lt;/dependency&gt;<br></code></pre>
<!-- /wp:preformatted -->

<!-- wp:paragraph -->
<p><strong>Key Takeaways:</strong></p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>API published separately for other elements to depend on</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>SPI layer enables swapping Stripe for PayPal without recompiling</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Implementation isolated with all Stripe SDK dependencies</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Everything resolved from Maven Central</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="example-2-multi-version-deployment">Example 2: Multi-Version Deployment</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Running two elements with conflicting dependencies:</p>
<!-- /wp:paragraph -->

<!-- wp:preformatted -->
<pre class="wp-block-preformatted"><code>deployment/<br>├── legacy-reports/<br>│   └── lib/<br>│       ├── reports-v1-1.0.jar<br>│       └── guava-20.0.jar           # Old version<br>│<br>└── modern-analytics/<br>    └── lib/<br>        ├── analytics-2.0.jar<br>        └── guava-31.1-jre.jar       # New version<br></code></pre>
<!-- /wp:preformatted -->

<!-- wp:paragraph -->
<p><strong>Both run simultaneously without conflicts</strong>&nbsp;due to classloader isolation.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="summary">Summary</h2>
<!-- /wp:heading -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="key-concepts">Key Concepts</h3>
<!-- /wp:heading -->

<!-- wp:list {"ordered":true} -->
<ol class="wp-block-list"><!-- wp:list-item -->
<li><strong>Elements are isolated modules</strong>&nbsp;with controlled dependency boundaries</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Four component types</strong>&nbsp;(API, SPI, LIB, Classpath) serve different purposes</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>ELM files are ZIP archives</strong>&nbsp;- portable, Maven-compatible packaging</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Everything from Maven</strong>&nbsp;- APIs, SPIs, implementations all from Maven repos</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Three-tier classloader hierarchy</strong>&nbsp;enables isolation + sharing</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>No extraction needed</strong>&nbsp;- ELM files read directly as ZIP filesystems</li>
<!-- /wp:list-item --></ol>
<!-- /wp:list -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="benefits">Benefits</h3>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>Dependency isolation</strong>: Different versions of libraries per Element</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Hot reload</strong>: Update Elements without server restart</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Maven integration</strong>: Leverage existing Maven infrastructure</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Portable packaging</strong>: Single ELM file distribution</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Memory efficiency</strong>: Unload Element = free memory</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Interface sharing</strong>: Elements communicate via shared APIs</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Flexible deployment</strong>: Directories, ELM files, or Maven coordinates</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="additional-resources">Additional Resources</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p><strong>Java Fundamentals:</strong></p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><a href="https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/lang/ClassLoader.html">Java ClassLoader Documentation</a></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/ServiceLoader.html">Java ServiceLoader Tutorial</a></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/nio/file/FileSystem.html">Java NIO FileSystem</a></li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p><strong>Maven Resources:</strong></p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><a href="https://search.maven.org/">Maven Central Repository</a></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="https://maven.apache.org/guides/introduction/introduction-to-dependency-mechanism.html">Maven Dependency Mechanism</a></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="https://maven.apache.org/repository-management.html">Maven Repository Managers</a></li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p><strong>Advanced Topics:</strong></p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><a href="https://www.baeldung.com/java-classloaders">Understanding Java ClassLoaders</a></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="https://www.baeldung.com/java-spi">Java SPI Pattern</a></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-apache-maven-registry">Working with GitHub Packages</a></li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->
