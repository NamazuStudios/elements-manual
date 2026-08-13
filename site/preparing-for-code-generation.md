<h1>Preparing for code generation</h1>

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-openapi-amp-client-code-generation-in-elements">OpenAPI &amp; Client Code Generation in Elements</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Elements supports <strong>automatic client code generation</strong> using <strong>OpenAPI</strong>. This allows you to expose your Element’s API in a standardized format (OAS3 / Swagger), and then generate client libraries for different languages (Java, C#, TypeScript, etc.) without writing them by hand.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-what-is-openapi">What is OpenAPI?</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>OpenAPI is a standard specification for describing REST APIs.<br>When your Element exposes endpoints, OpenAPI automatically generates a <strong>JSON schema</strong> (OAS3 file) that describes:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Available endpoints</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Request/response formats</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Authentication requirements</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Data models</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>This JSON file is then used by client code generators (e.g. Swagger Codegen, OpenAPI Generator) to produce strongly typed client libraries.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-organizing-apis-with-tag">Organizing APIs with <code>@Tag</code></h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Each <strong>endpoint class</strong> in Elements should be annotated with a <strong><code>@Tag</code></strong>.<br>This tells the generator which <strong>API file</strong> the methods should be grouped into.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>For example:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>import jakarta.ws.rs.GET;
import jakarta.ws.rs.Path;
import io.swagger.v3.oas.annotations.tags.Tag;

@Path("/example")
@Tag(name = "Example")  // &lt;-- Groups this endpoint into ExampleApi
public class ExampleEndpoint {

    @GET
    public String hello() {
        return "Hello from Example!";
    }
}
</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>When client code is generated, this endpoint will appear inside an <code>ExampleApi</code> file in the generated client.</p>
<!-- /wp:paragraph -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"Warning","noticeBackgroundColor":"#ffdd57"} -->
<div style="color:#32373c;background-color:#ffdd57" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="0eaadb"><div class="gb-notice-title" style="color:#fff"><p>Warning</p></div><div class="gb-notice-text" style="border-color:#ffdd57"><!-- wp:paragraph -->
<p>Not tagging an endpoint class will put the generated code into a DefaultApi class. If you have multiple DefaultApi classes in the client project, they could overwrite each other. </p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-registering-openapi-in-your-application">Registering OpenAPI in Your Application</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>For OpenAPI code generation to work, you must <strong>register the Swagger OpenAPI JAX-RS resource</strong> (<code>OpenApiResource</code>) in your Element’s <code>Application</code> class.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Example:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>// Swagger OpenAPI JAX-RS resource
import io.swagger.v3.jaxrs2.integration.resources.OpenApiResource;

import jakarta.ws.rs.core.Application;
import dev.getelements.elements.sdk.annotation.ElementServiceImplementation;
import dev.getelements.elements.sdk.annotation.ElementServiceExport;

@ElementServiceImplementation
@ElementServiceExport(Application.class)
public class HelloWorldApplication extends Application {

    /**
     * Here we register all the classes that we want
     * to be included in the Element.
     */
    @Override
    public Set&lt;Class&lt;?&gt;&gt; getClasses() {
        return Set.of(
            // Endpoints
            HelloWorld.class,
            HelloWithAuthentication.class,

            // Required for OpenAPI JSON generation
            OpenApiResource.class
        );
    }
}
</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>Without <code>OpenApiResource</code>, your endpoints will run, but no OpenAPI spec will be generated — which means no client code generation.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-summary">Summary</h2>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>OpenAPI</strong> describes your API in a standardized JSON format.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Use <strong><code>@Tag(name = "...")</code></strong> to group endpoints into client-side API files.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Always register <strong><code>OpenApiResource</code></strong> in your <code>Application</code> class to enable codegen.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>With these pieces in place, you can run OpenAPI tools to generate client libraries for your users, ensuring type-safe access to your API without manual boilerplate.</p>
<!-- /wp:paragraph -->
