<h1>Structuring your Element</h1>

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-setting-up-your-element-project">Setting Up Your Element Project</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>See <a href="https://github.com/NamazuStudios/element-example">https://github.com/NamazuStudios/element-example</a> for a complete example. It is recommended to use this as a starting point as well. For a full step-by-step walkthrough of that project — setup, running locally, and a breakdown of every file — see <a href="element-example-complete-walkthrough">Building the Example Element: A Complete Walkthrough</a>.</p>
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

import com.mystudio.mygame.rest.ExampleContent;
import com.mystudio.mygame.rest.HelloWithAuthentication;
import com.mystudio.mygame.rest.HelloWorld;
import dev.getelements.elements.sdk.annotation.ElementServiceExport;
import dev.getelements.elements.sdk.annotation.ElementServiceImplementation;
import jakarta.ws.rs.core.Application;

import java.util.Set;

@ElementServiceImplementation
@ElementServiceExport(Application.class)
public class HelloWorldApplication extends Application {

    /**
     * Here we register all the classes that we want to be included in the Element.
     */
    @Override
    public Set&lt;Class&lt;?&gt;&gt; getClasses() {
        return Set.of(
                //Endpoints
                HelloWorld.class,
                HelloWithAuthentication.class,
                ExampleContent.class,

                // Exposes the default security rules for the API. Assumes you are using the builtin
                // Elements auth system by setting `dev.getelements.elements.auth.enabled` to true.
                OpenAPISecurityConfig.class
        );
    }
}</code></pre>
<!-- /wp:code -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"Note"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="7a2c19"><div class="gb-notice-title" style="color:#fff"><p>Note</p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p>You'll also need <code>@ElementDefaultAttribute</code> fields on this class to configure your REST/WebSocket root paths and turn on the auth filter — see the <a href="element-example-complete-walkthrough">complete walkthrough</a> for the full annotated version used by the example project.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

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
<p>This interface lives in the <code>api</code> module so other Elements can depend on it without pulling in your implementation. <code>@ElementServiceExport</code> is important for making the service discoverable within the Element — you can put it on the interface or on the implementation class; the example project puts it on the implementation (shown below).</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-service-uses-userservice">Service (uses <code>UserService</code>)</h2>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code>package com.mystudio.mygame.service;

import dev.getelements.elements.sdk.annotation.ElementServiceExport;
import dev.getelements.elements.sdk.model.user.User;
import dev.getelements.elements.sdk.service.user.UserService;
import jakarta.inject.Inject;

@ElementServiceExport(GreetingService.class)
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

import com.google.inject.PrivateModule;
import com.mystudio.mygame.service.GreetingService;
import com.mystudio.mygame.service.GreetingServiceImpl;


public class MyGameModule extends PrivateModule {

  @Override
  protected void configure() {

    bind(GreetingService.class).to(GreetingServiceImpl.class);

    // Required because PrivateModule hides all bindings by default. Without this,
    // the service locator lookup below would fail even with @ElementServiceExport present.
    expose(GreetingService.class);
  }

}</code></pre>
<!-- /wp:code -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"Note"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="4c7e21"><div class="gb-notice-title" style="color:#fff"><p>Note</p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p>We recommend <code>PrivateModule</code> over plain <code>AbstractModule</code> so your Element's internal bindings can't leak into, or collide with, other Elements' bindings. With <code>PrivateModule</code>, <code>expose()</code> is not optional — it's the only way anything you bind becomes visible outside the module.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-endpoint-fetches-the-service">Endpoint (fetches the service)</h2>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code>package com.mystudio.mygame.rest;

import com.mystudio.mygame.service.GreetingService;
import dev.getelements.elements.sdk.Element;
import dev.getelements.elements.sdk.ElementSupplier;
import io.swagger.v3.oas.annotations.Operation;
import io.swagger.v3.oas.annotations.tags.Tag;
import jakarta.ws.rs.GET;
import jakarta.ws.rs.Path;
import jakarta.ws.rs.Produces;
import jakarta.ws.rs.core.MediaType;

@Tag(name = "MyGame")
@Path("/hellowithauthentication")
public class HelloWithAuthentication {

    private final Element element = ElementSupplier
            .getElementLocal(HelloWithAuthentication.class)
            .get();

    private final GreetingService greetingService = element
            .getServiceLocator()
            .getInstance(GreetingService.class);

  @GET
  @Produces(MediaType.TEXT_PLAIN)
  @Operation(
    summary = "Gets a greeting",
    description = "Checks if the session token in the header corresponds to at least a USER level user and returns a greeting with their name if so, or Guest if not."
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
<pre class="wp-block-code"><code>GET /element/example/rest/api/hellowithauthentication
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
