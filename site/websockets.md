<h1>Websockets</h1>

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-how-to-build-websocket-apis-in-elements">How to build Websocket APIs in Elements</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Elements 3.0 offers <a href="https://datatracker.ietf.org/doc/html/rfc6455">Standards Compliant Websockets</a> built in using the <a href="https://jakarta.ee/specifications/websocket/2.1/">Jakarta Websocket API 2.1</a>. Websockets are useful in creating high performance bi-directional communication between client and server code. Generally speaking, Websockets are considerably faster than HTTP requests for authoritative code and work well with practically all clients including Web, Unity3d, Unreal, .NET and many other connected services.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-steps-to-defining-a-websocket-element">Steps to Defining a Websocket Element</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>To use the Jakarta RS in your own Element, you must perform the following steps:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><a href="../element-structure#defining-an-element">Define the Element</a> by annotating the <code>package-info</code> type in your code.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Add all compiled classes and jars into the <a href="../element-structure#packaging-an-element">Element package structure</a>.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Annotate each Websocket endpoint with the <a href="https://jakarta.ee/specifications/websocket/2.1/apidocs/server/jakarta/websocket/server/serverendpoint"><code>ServerEndpoint</code></a> annotation.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-complete-example">Complete Example</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The following example walks through the necessary files to define a simple Websocket echo server.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="h-step1-define-the-element">Step1: Define the Element</h4>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>{% code title="package-info.java" %}</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>@ElementDefinition
package dev.getelements.elements.sdk.test.element.ws;

import dev.getelements.elements.sdk.annotation.ElementDefinition;</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>{% endcode %}</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="h-step-2-define-the-element">Step 2: Define the Element</h4>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>{% code title="EchoEndpoint.java" %}</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>package dev.getelements.elements.sdk.test.element.ws;

import jakarta.websocket.*;
import jakarta.websocket.server.ServerEndpoint;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

@ServerEndpoint("/echo")
public class EchoEndpoint {

    private static final Logger logger = LoggerFactory.getLogger(EchoEndpoint.class);

    @OnOpen
    public void onOpen(final Session session) {
        logger.info("Opened {}", session.getId());
    }

    @OnMessage
    public String onMessage(final Session session, final String message) {
        logger.info("Received {}. Echoing.", message);
        return message;
    }

    @OnClose
    public void onClose(final Session session, final CloseReason closeReason) {
        logger.info("Closed {} - {}", session.getId(), closeReason);
    }

}</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>{% endcode %}</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="h-step-3-ensure-all-dependencies-are-included">Step 3: Ensure All Dependencies are Included</h4>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>{% code title="pom.xml" %}</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>&lt;?xml version="1.0"?&gt;
&lt;project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd"&gt;

    &lt;modelVersion&gt;4.0.0&lt;/modelVersion&gt;

    &lt;parent&gt;
        &lt;groupId&gt;dev.getelements.elements&lt;/groupId&gt;
        &lt;artifactId&gt;eci-elements&lt;/artifactId&gt;
        &lt;version&gt;2.2.0-SNAPSHOT&lt;/version&gt;
    &lt;/parent&gt;

    &lt;artifactId&gt;sdk-test-element-ws&lt;/artifactId&gt;
    &lt;version&gt;2.2.0-SNAPSHOT&lt;/version&gt;

    &lt;dependencies&gt;
        &lt;dependency&gt;
            &lt;groupId&gt;dev.getelements.elements&lt;/groupId&gt;
            &lt;artifactId&gt;sdk&lt;/artifactId&gt;
            &lt;scope&gt;provided&lt;/scope&gt;
        &lt;/dependency&gt;
        &lt;dependency&gt;
            &lt;groupId&gt;jakarta.websocket&lt;/groupId&gt;
            &lt;artifactId&gt;jakarta.websocket-api&lt;/artifactId&gt;
            &lt;scope&gt;provided&lt;/scope&gt;
        &lt;/dependency&gt;
        &lt;dependency&gt;
            &lt;groupId&gt;jakarta.websocket&lt;/groupId&gt;
            &lt;artifactId&gt;jakarta.websocket-client-api&lt;/artifactId&gt;
            &lt;scope&gt;provided&lt;/scope&gt;
        &lt;/dependency&gt;
    &lt;/dependencies&gt;

&lt;/project&gt;</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>{% endcode %}</p>
<!-- /wp:paragraph -->
