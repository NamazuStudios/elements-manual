<h1>RESTful APIs</h1>

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-how-to-build-restful-apis-in-elements">How to build RESTful APIs in Elements</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Elements 3.0 Provides a complete implementation of <a href="https://jakarta.ee/specifications/restful-ws/4.0/">Jakarta RESTful Web Services 4.0.0</a>. The full usage of Jakarta RS is beyond the scope of this document. What you need to know most:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Jakarta RS Allows you to generate RESTful endpoints for your game's code which can be called from in-engine code using any standard HTTP client library.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Specific endpoints are developed using a set of <a href="https://jakarta.ee/specifications/restful-ws/4.0/apidocs/jakarta.ws.rs/jakarta/ws/rs/package-summary">annotations</a> in Java code and handled automatically by the application container.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Existing RESTul APIs can be imported directly into an Element with almost no modification.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-steps-to-defining-a-jakarta-rs-element">Steps to Defining a Jakarta RS Element</h2>
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
<li>Expose the <a href="https://jakarta.ee/specifications/restful-ws/4.0/apidocs/jakarta.ws.rs/jakarta/ws/rs/core/application"><code>Application</code></a> type as a service.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-complete-example-code">Complete Example Code</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The following example walks through the necessary files to define a simple endpoint for CRUD (Create, Read, Update, Delete) operations for a message based service.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="h-step-1-define-the-element">Step 1: Define the Element</h4>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>{% code title="package-info.java" %}</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>@ElementDefinition
package dev.getelements.elements.sdk.test.element.rs;

import dev.getelements.elements.sdk.annotation.ElementDefinition;</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>{% endcode %}</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="h-step-2-define-the-application">Step 2: Define the Application</h4>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>{% code title="TestApplication.java" %}</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>package dev.getelements.elements.sdk.test.element.rs;

import com.fasterxml.jackson.jakarta.rs.json.JacksonJsonProvider;
import dev.getelements.elements.sdk.annotation.ElementDefaultAttribute;
import dev.getelements.elements.sdk.annotation.ElementServiceExport;
import dev.getelements.elements.sdk.annotation.ElementServiceImplementation;
import jakarta.ws.rs.ApplicationPath;
import jakarta.ws.rs.core.Application;

import java.util.Set;

@ApplicationPath("/")
@ElementServiceImplementation
@ElementServiceExport(Application.class)
public class TestApplication extends Application {

    @ElementDefaultAttribute("myapp")
    public static final String APP_SERVE_PREFIX = "dev.getelements.elements.app.serve.prefix";

    @Override
    public Set&lt;Class&lt;?&gt;&gt; getClasses() {
        return Set.of(
                MessageEndpoint.class,
                JacksonJsonProvider.class
        );
    }

}</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>{% endcode %}</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="h-step-3-define-the-endpoint-code">Step 3: Define the Endpoint Code</h4>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>{% code title="MessageEndpoint.java" %}</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>package dev.getelements.elements.sdk.test.element.rs;

import jakarta.ws.rs.*;
import jakarta.ws.rs.core.Response;

import java.util.Map;
import java.util.concurrent.ConcurrentSkipListMap;
import java.util.concurrent.atomic.AtomicInteger;

@Path("/message")
public class MessageEndpoint {

    private static final AtomicInteger counter = new AtomicInteger();

    private static final Map&lt;Integer, Message&gt; messages = new ConcurrentSkipListMap&lt;&gt;();

    @POST
    public Response createMessage(
            final CreateMessageRequest createMessageRequest) {

        if (createMessageRequest.getMessage() == null) {
            return Response.status(Response.Status.BAD_REQUEST).build();
        }

        final int id = counter.incrementAndGet();
        final long now = System.currentTimeMillis();

        final var message = new Message();
        message.setId(id);
        message.setMessage(createMessageRequest.getMessage());
        message.setCreated(now);
        message.setUpdated(now);

        if (messages.putIfAbsent(id, message) != null) {
            return Response.status(Response.Status.INTERNAL_SERVER_ERROR).build();
        }

        return Response
                .status(Response.Status.CREATED)
                .entity(message).build();

    }

    @PUT
    @Path("{messageId}")
    public Response updateMessage(
            @PathParam("messageId")
            final String messageId,
            final UpdateMessageRequest updateMessageRequest) {

        final int id;

        try {
            id = Integer.parseInt(messageId);
        } catch (NumberFormatException ex) {
            return Response.status(Response.Status.NOT_FOUND).build();
        }

        final var result = messages.computeIfPresent(id, (_id, existing) -&gt; {
            final var updated = new Message();
            updated.setId(_id);
            updated.setCreated(existing.getCreated());
            updated.setUpdated(System.currentTimeMillis());
            updated.setMessage(updateMessageRequest.getMessage());
            return updated;
        });

        return result == null
                ? Response.status(Response.Status.NOT_FOUND).build()
                : Response.status(Response.Status.OK).entity(result).build();

    }

    @GET
    public Response getMessages() {
        return Response
                .status(Response.Status.OK)
                .entity(messages.values())
                .build();
    }

    @GET
    @Path("{messageId}")
    public Response getMessage(
            @PathParam("messageId")
            final String messageId) {

        final int id;

        try {
            id = Integer.parseInt(messageId);
        } catch (NumberFormatException ex) {
            return Response.status(Response.Status.NOT_FOUND).build();
        }

        final var message = messages.get(id);

        return message == null
                ? Response.status(Response.Status.NOT_FOUND).build()
                : Response.status(Response.Status.OK).entity(message).build();

    }

    @DELETE
    @Path("{messageId}")
    public Response deleteMessage(
            @PathParam("messageId")
            final String messageId) {

        final int id;

        try {
            id = Integer.parseInt(messageId);
        } catch (NumberFormatException ex) {
            return Response.status(Response.Status.NOT_FOUND).build();
        }

        final var removed = messages.remove(id);

        return removed == null
                ? Response.status(Response.Status.NOT_FOUND).build()
                : Response.status(Response.Status.NO_CONTENT).entity(removed).build();

    }

}</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>{% endcode %}</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="h-step-4-ensure-all-dependencies-are-included">Step 4: Ensure all Dependencies are Included</h4>
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

    &lt;artifactId&gt;sdk-test-element-rs&lt;/artifactId&gt;
    &lt;version&gt;2.2.0-SNAPSHOT&lt;/version&gt;

    &lt;dependencies&gt;
        &lt;dependency&gt;
            &lt;groupId&gt;dev.getelements.elements&lt;/groupId&gt;
            &lt;artifactId&gt;sdk&lt;/artifactId&gt;
            &lt;scope&gt;provided&lt;/scope&gt;
        &lt;/dependency&gt;
        &lt;dependency&gt;
            &lt;groupId&gt;jakarta.ws.rs&lt;/groupId&gt;
            &lt;artifactId&gt;jakarta.ws.rs-api&lt;/artifactId&gt;
            &lt;scope&gt;provided&lt;/scope&gt;
        &lt;/dependency&gt;
        &lt;dependency&gt;
            &lt;groupId&gt;com.fasterxml.jackson.jakarta.rs&lt;/groupId&gt;
            &lt;artifactId&gt;jackson-jakarta-rs-json-provider&lt;/artifactId&gt;
            &lt;version&gt;2.18.3&lt;/version&gt;
        &lt;/dependency&gt;
    &lt;/dependencies&gt;

    &lt;build&gt;
        &lt;plugins&gt;
            &lt;plugin&gt;
                &lt;groupId&gt;org.apache.maven.plugins&lt;/groupId&gt;
                &lt;artifactId&gt;maven-dependency-plugin&lt;/artifactId&gt;
                &lt;version&gt;3.6.0&lt;/version&gt; &lt;!-- Use the latest version --&gt;
                &lt;executions&gt;
                    &lt;execution&gt;
                        &lt;id&gt;copy-dependencies&lt;/id&gt;
                        &lt;phase&gt;package&lt;/phase&gt;
                        &lt;goals&gt;
                            &lt;goal&gt;copy-dependencies&lt;/goal&gt;
                        &lt;/goals&gt;
                        &lt;configuration&gt;
                            &lt;outputDirectory&gt;${project.build.directory}/libs&lt;/outputDirectory&gt;
                            &lt;includeScope&gt;runtime&lt;/includeScope&gt;
                        &lt;/configuration&gt;
                    &lt;/execution&gt;
                &lt;/executions&gt;
            &lt;/plugin&gt;
        &lt;/plugins&gt;
    &lt;/build&gt;

&lt;/project&gt;</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>{% endcode %}</p>
<!-- /wp:paragraph -->
