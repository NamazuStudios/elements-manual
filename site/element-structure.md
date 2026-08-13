<h1>Custom Code Overview</h1>

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-deploy-custom-code-to-your-instance-of-namazu-elements">Deploy custom code to your instance of Namazu Elements.</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Elements is highly extensible and allows you to develop custom code. Elements 3 is written in Java21 and therefore any VM compatible languages will work seamlessly in Elements. Building your custom server side logic in Java-Based Languages has several distinct advantages.</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Java has one of the most complete ecosystems of readily available Open Source libraries available. Chances are, whatever you're looking for is available in a package on <a href="https://central.sonatype.com">Maven Central</a>.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Java offers incredible server side performance through JIT Compliation, which essentially compiles your code to machine code as it is loaded.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>As of October 2023, it is estimated that the developer community size is approximately <a href="https://www.griddynamics.com/blog/number-software-developers-world">17.1 Million</a> developers, ensuring you will have no problems building your team long-term.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>An existing suite of powerful development tools including IDEs and profilers/benchmarking tools are readily available.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>Check out our example Element project <a href="https://github.com/Elemental-Computing/element-example">here</a>!</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-here-s-where-you-learn-to-develop-your-first-element">Here's where you learn to develop your first Element.</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Starting with Elements 3, we are now providing a way to develop custom extensions in Java Virtual Machine (or JVM) based languages. Refer to the <a href="https://en.wikipedia.org/wiki/List_of_JVM_languages">Wikipedia</a> page for a complete list and determine the best language which works for your project. The most popular choices are Java and Kotlin. Elements is natively written in Java which is 100% compatible with any other JVM based language.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>We call our fundamental plug-in an Element. Each Element runs in an isolated environment using a <a href="https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/lang/ClassLoader.html">ClassLoader</a> which isolates Element from the rest of the system. A common problem Java developers face is conflicting dependencies within a single application, colloquially known as "<a href="https://en.wikipedia.org/wiki/Java_class_loader#JAR_hell">Jar Hell.</a>"</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Our approach allows you to use develop in an environment without having to worry about conflicts between the Elements 3 code base and your Element's code. We provide a set of interfaces, but isolate their implementations from your code. When developing an Element, these are the key points you must know:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>You can incorporate almost any existing Java code base into Elements that are based on standard Java frameworks. Elements 3 Currently Supports:</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Jakarta RS 4.0.0</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Jakarta Websockets 2.1.0</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>You can include almost any third-party library into your code base without having to worry about it conflicting with the frameworks used to build Elements</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Note: You will need to copy all dependencies into your Element, except those as provided by Elements 3.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>An Element does not have strict isolation, nor does it run in a sandbox. This is by design, as trying to enforce strong encapsulation using a Security Manager (or similar) would introduce a lot of overhead for no benefits. Take special care when accessing the <a href="https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/lang/ClassLoader.html#getSystemClassLoader()">System ClassLoader</a>.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-defining-an-element">Defining an Element</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>An Element exists as a Java package and, optionally, include all sub-packages recursively. Elements are similar to <a href="https://www.oracle.com/corporate/features/understanding-java-9-modules.html">JPMS Modules</a> but without the one jar per module restrictions.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>The following example shows how to define an Element in your Java code. Note we do require that you define a <code>package-info</code> for the package. The following is the simplest Element possible.</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>// Defines the Package. Specifies as recursive.
@ElementDefinition(recursive = true)
package com.mystudio.mygame.api;

// Imports the Element SDK Annotation
import dev.getelements.elements.sdk.annotation.Element</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>This indicates that the package <code>com.mystudio.mygame.api</code>and all sub packages will be included in the Element for associated services.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-packaging-an-element">Packaging an Element</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Packaging an Element or collection of Elements uses a simple directory structure. Elements 3.0 will search for the following files and directories when loading the assets associated with an Element:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><code>dev.getelements.element.attributes.properties</code> - A standard <a href="https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/Properties.html">Java Properties</a> file which define the Element's application</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>libs</code> - A directory containing jar files. The loader scans the directory for jar files and adds them to the Element's classpath.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>classpath</code> - A directory containing assets to add directly to the Element's classpath.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>Additionally, the following rules apply when scanning a directory for Elements.</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Any file not otherwise specified would be ignored.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Any directory will be treated as a new Element, provided that that at least <code>libs</code> or <code>classpath</code> exist.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Note:</strong> Elements defined in directories will inherit the classpath of the parent which allows you to put common code in the parent directory.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-next-steps">Next steps:</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>See our intro to <a href="https://namazustudios.com/docs/custom-code/introduction-to-guice-and-jakarta-in-elements/">Jakarta and Guice</a> if you're not familiar with those tools, then check out <a href="https://namazustudios.com/docs/custom-code/structuring-your-element/">Structuring an Element</a> to get started!</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->
