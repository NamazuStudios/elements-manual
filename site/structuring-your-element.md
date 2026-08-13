<h1>Structuring your Element</h1>

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-setting-up-your-element-project">Setting Up Your Element Project</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>See <a href="https://github.com/NamazuStudios/element-example">https://github.com/NamazuStudios/element-example</a> for a complete example. It is recommended to use this as a starting point as well.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":22164,"width":"314px","height":"auto","sizeSlug":"full","linkDestination":"none"} -->
<figure class="wp-block-image size-full is-resized"><img src="https://namazustudios.com/wp-content/uploads/2025/08/Screenshot-2025-08-27-at-4.32.10-PM.png" alt="" class="wp-image-22164" style="width:314px;height:auto"/></figure>
<!-- /wp:image -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-setting-up-the-application">Setting Up the Application</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The Application is required to tell Jakarta which endpoints to load and expose. In the example project, that looks like this:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>package com.mystudio.mygame;

import com.mystudio.mygame.rest.HelloWorld;
import com.mystudio.mygame.rest.HelloWithAuthentication;
import dev.getelements.elements.sdk.annotation.ElementServiceExport;
import dev.getelements.elements.sdk.annotation.ElementServiceImplementation;
import jakarta.ws.rs.core.Application;

import java.util.Set;

// Swagger OpenAPI JAX-RS resource
import io.swagger.v3.jaxrs2.integration.resources.OpenApiResource;

@ElementServiceImplementation
@ElementServiceExport(Application.class)
public class HelloWorldApplication extends Application {

    /**
     * Here we register all the classes that we want to be included in the Element.
     */
    @Override
    public Set&lt;Class&lt;?>> getClasses() {
        return Set.of(
                //Endpoints
                HelloWorld.class,
                HelloWithAuthentication.class,

                //Required if you want codegen to work for this
                OpenApiResource.class
        );
    }
}</code></pre>
<!-- /wp:code -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-defining-an-element-with-package-info-java">Defining an Element with <code>package-info.java</code></h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>At the root of your Element project (e.g. <code>src/main/java/com/mystudio/mygame</code>), you need a <code>package-info.java</code> file.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>This file tells Elements Core:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>What package is an Element</strong> (<code>@ElementDefinition</code>).</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Which Guice module to use</strong> for dependency injection (<code>@GuiceElementModule</code>).</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Which other packages to allow injection from</strong> (<code>@ElementDependency</code>).</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-example-package-info-java">Example: <code>package-info.java</code></h3>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code>// Required annotation for an Element. Will recursively search folders from this point to include classes in the Element if recursive is true. Otherwise, you must include additional package-info.java files in child packages.
@ElementDefinition(recursive = true)

// Enables DI via Guice
@GuiceElementModule(ExampleModule.class) 

// Allows injecting DAO layer from Elements Core
@ElementDependency("dev.getelements.elements.sdk.dao") 

// Allows injecting Service layer from Elements Core
@ElementDependency("dev.getelements.elements.sdk.service") 

package com.mystudio.mygame;

import com.mystudio.mygame.guice.ExampleModule;
import dev.getelements.elements.sdk.annotation.ElementDefinition;
import dev.getelements.elements.sdk.annotation.ElementDependency;
import dev.getelements.elements.sdk.spi.guice.annotations.GuiceElementModule;</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>This is required for your Element to be recognized and properly wired up by the Elements runtime. Without this file, your Guice bindings and service injections won’t work.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"level":1} -->
<h1 class="wp-block-heading" id="h-example-greeting-service-with-user-context">Example: Greeting Service with User Context</h1>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Let’s extend the greeting example so it uses the <strong>current logged-in user</strong>.<br>The <code>UserService</code> from the SDK can fetch information about the user making the request.</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>The user is identified automatically based on the session token provided in the header: <code>Elements-SessionSecret: &lt;session-token&gt;</code></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Elements Core resolves the session, and <code>UserService.getCurrentUser()</code> will give you the current user.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-service-interface">Service (Interface)</h2>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code>package com.mystudio.mygame.service;

import dev.getelements.elements.sdk.annotation.ElementServiceExport;

@ElementServiceExport
public interface GreetingService {

    /**
     * Attempts to fetch the current user for the session header and return an appropriate greeting
     * @return The greeting based on if a logged-in user is found
     */
    String getGreeting();
}</code></pre>
<!-- /wp:code -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"Note"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="3b0649"><div class="gb-notice-title" style="color:#fff"><p>Note</p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p>It is important to add the <code>@ElementServiceExport</code> annotation to make this service discoverable within the Element.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-service-uses-userservice">Service (uses <code>UserService</code>)</h2>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code>package com.mystudio.mygame.service;

import dev.getelements.elements.sdk.model.user.User;
import dev.getelements.elements.sdk.service.user.UserService;
import jakarta.inject.Inject;

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
       
        final User currentUser = userService.getCurrentUser();
        final boolean isLoggedIn = !User.Level.UNPRIVILEGED.equals(currentUser.getLevel());
        final String name = isLoggedIn ? currentUser.getName() : "Guest"; 
        
        return "Hello, " + name + "!";
    }
}</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>In this code:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><code>UserService</code> is injected by Guice.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>getCurrentUser()</code> returns the <code>User</code> associated with the request’s <code>Elements-SessionSecret</code> header.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>If no user is logged in, we fall back to <code>"Guest"</code>.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"Note"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="3b0649"><div class="gb-notice-title" style="color:#fff"><p>Note</p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p>Because we set the dev.getelements.elements.auth.enabled attribute to "true" in the HelloWorldApplication, the UserService will be automatically injected with the current user. This will apply an authentication filter to every request and every service that is used in this application.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-module-binds-the-service">Module (binds the service)</h2>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code>package com.mystudio.mygame.guice;

import com.google.inject.AbstractModule;
import com.mystudio.mygame.service.GreetingService;
import com.mystudio.mygame.service.GreetingServiceImpl;


public class ExampleModule extends AbstractModule {

  @Override
  protected void configure() {

    bind(GreetingService.class).to(GreetingServiceImpl.class);

    // Needed in conjunction with the @ElementServiceExport annotation
    // to make this discoverable by the service locator
    expose(GreetingService.class);
  }

}</code></pre>
<!-- /wp:code -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-endpoint-fetches-the-service">Endpoint (fetches the service)</h2>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code>package com.mystudio.mygame.endpoint;

import com.mystudio.mygame.service.GreetingService;
import dev.getelements.elements.sdk.spi.ElementSupplier;
import jakarta.ws.rs.GET;
import jakarta.ws.rs.Path;
import jakarta.ws.rs.Produces;
import jakarta.ws.rs.core.MediaType;

@Tag(name = "MyGame")
@Path("/greet")
public class GreetingEndpoint {

    private final Element element = ElementSupplier
            .getElementLocal(GreetingEndpoint.class)
            .get();

    private final GreetingService greetingService = element
            .getServiceLocator()
            .getInstance(GreetingService.class);

  @GET
  @Produces(MediaType.TEXT_PLAIN)
  @Operation(
    summary = "Gets a greeting", 
    description = "Checks if the session token in the header corresponds
    to at least a USER level user and returns a greeting with their name
    if so, or Guest if not."
  )
  public String greet() {
    return greetingService.getGreeting();
  }
}
</code></pre>
<!-- /wp:code -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-example-request">Example Request</h2>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code>GET /greet
Elements-SessionSecret: eyJhbGciOiJIUzI1...</code></pre>
<!-- /wp:code -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-response-if-user-logged-in">Response (if user logged in)</h3>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code>"Hello, Alice!"</code></pre>
<!-- /wp:code -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-response-if-not-logged-in">Response (if not logged in)</h3>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code>"Hello, Guest!"</code></pre>
<!-- /wp:code -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"level":1} -->
<h1 class="wp-block-heading" id="h-summary">Summary</h1>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><code>package-info.java</code> defines your Element and its dependencies.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Services can inject SDK-provided classes like <code>UserService</code>.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>The <strong>current user</strong> is resolved automatically from the <code>Elements-SessionSecret</code> header.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Endpoints should fetch services using <code>ElementSupplier.getElementLocal(...).get().getServiceLocator().getInstance(...)</code>.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->
