<h1>Introduction to Guice and Jakarta in Elements</h1>

<!-- wp:paragraph -->
<p>When you write custom code in Elements (an <strong>Element</strong>), you’ll be using two important Java libraries: <strong>Guice</strong> and <strong>Jakarta</strong>. These libraries help structure your code, manage dependencies, and expose endpoints for your game or application.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Don’t worry if you’ve never used them before — you don’t need to be a backend expert. This overview will get you familiar with the basic concepts and show you the minimal syntax you’ll need.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-guice-dependency-injection">Guice (Dependency Injection)</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p><strong>What it is:</strong><br><a href="https://github.com/google/guice">Google Guice</a> is a <em>dependency injection</em> (DI) framework. That’s a fancy way of saying it helps you manage how different pieces of your code fit together without having to manually wire everything yourself.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Instead of writing code like this:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>WeaponService service = new WeaponService(new WeaponDao());
</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>Guice lets you declare <em>what</em> you need, and it will provide it for you:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>@Inject
WeaponService service;
</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>Guice figures out how to build <code>WeaponService</code> and its dependencies (<code>WeaponDao</code>) automatically.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-why-it-matters-in-elements">Why it matters in Elements</h3>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Makes your code cleaner and less repetitive.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>You don’t have to worry about the order of object creation.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Encourages modular, testable code.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-how-it-looks-in-practice">How it looks in practice</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>You declare bindings (rules for how to build classes) in a <strong>Module</strong>:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>public class WeaponModule extends AbstractModule {

  @Override
  protected void configure() {

    bind(WeaponDao.class).to(MongoWeaponDao.class);

    bind(WeaponService.class);
  }
}
</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>This means: “Whenever someone asks for <code>WeaponDao</code>, give them a <code>MongoWeaponDao</code>.”</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>And in your service:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>public class WeaponService {

  private final WeaponDao dao;

  @Inject
  public WeaponService(WeaponDao dao) {
    this.dao = dao;
  }
}
</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>You don’t create the <code>WeaponDao</code> yourself — Guice does it for you.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-jakarta-rest-endpoints">Jakarta (REST Endpoints)</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p><strong>What it is:</strong><br><a>Jakarta EE</a> is a collection of APIs for building Java applications. In Elements, the most important part is <strong>Jakarta REST (JAX-RS)</strong>. It lets you define web endpoints (APIs) with simple annotations.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Instead of writing low-level HTTP handling, you just declare methods with annotations like <code>@GET</code> or <code>@POST</code>.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-example-endpoint">Example Endpoint</h3>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code>@Path("/weapons")
public class WeaponEndpoint {
  
  private final WeaponService service =
      ElementSupplier
          .getElementLocal(WeaponEndpoint.class)
          .get()
          .getServiceLocator()
          .getInstance(WeaponService.class);

  @GET
  @Path("/{id}")
  @Produces(MediaType.APPLICATION_JSON)
  public Weapon getWeapon(@PathParam("id") String id) {
    return service.getWeapon(id);
  }
}
</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>What this does:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><code>@Path("/weapons")</code> → Defines the base URL (<code>/weapons</code>).</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>@GET</code> → This method runs when a GET request is made.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>@Path("/{id}")</code> → The <code>{id}</code> part of the URL becomes a method parameter.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>@Produces(MediaType.APPLICATION_JSON)</code> → Response will be JSON.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>So if you hit:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>GET app/rest/weapons/123</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>You’ll get back the weapon with ID <code>123</code> as JSON.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading">How this works:</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>In Elements, <strong>Guice services don’t live in the same context as Jakarta endpoints</strong>.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>That means this won’t work inside an endpoint:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>@Inject
ExampleInterface example;  // ❌ Won’t be injected automatically</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>Instead, you must use the <code>ElementSupplier</code> to fetch your Guice-managed services.</p>
<!-- /wp:paragraph -->

<!-- wp:list {"ordered":true} -->
<ol class="wp-block-list"><!-- wp:list-item -->
<li><code>ElementSupplier.getElementLocal(MyEndpoint.class)</code> → gets the current Element context for this endpoint.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>.getServiceLocator()</code> → allows you to search and instantiate service classes that have been exposed or registered to this Element.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>.getInstance(ExampleInterface.class)</code> → fetches (and injects into) your service implementation.</li>
<!-- /wp:list-item --></ol>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>The first time you request it, Guice will call <code>@Inject</code> on the implementation (<code>ExampleInterfaceImplementation</code> in this case). After that, the same instance is reused as per your binding scope.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading">Recommended Practice</h2>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>Bind services in a Guice module</strong> (<code>bind(A.class).to(B.class)</code>).</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Fetch them in endpoints via <code>ElementSupplier</code></strong>.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Keep endpoints <em>thin</em> — let services hold most of your business logic.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Think of endpoints as <strong>API facades</strong>, and services/DAOs as the real workhorses.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-how-they-work-together-in-elements">How They Work Together in Elements</h2>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>Guice</strong> wires up your services and DAOs (your application logic).</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Jakarta</strong> exposes those services to the outside world through REST endpoints.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>So you might have this flow:</p>
<!-- /wp:paragraph -->

<!-- wp:list {"ordered":true} -->
<ol class="wp-block-list"><!-- wp:list-item -->
<li>Player calls <code>GET app/rest/weapons/123</code>.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Jakarta routes that request to <code>WeaponEndpoint</code>.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Guice injects a <code>WeaponService</code> into the endpoint.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>WeaponService</code> uses a <code>WeaponDao</code> to fetch data.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Jakarta returns the result as JSON.</li>
<!-- /wp:list-item --></ol>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>You just focus on your code — Guice and Jakarta take care of the plumbing.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-quick-recap">Quick Recap</h2>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>Guice</strong>: Handles <em>dependencies</em>. You declare what you need, and Guice gives it to you.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Jakarta</strong>: Handles <em>endpoints</em>. You annotate classes and methods to turn them into APIs.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>Together, they let you build modular, structured, and clean backend code inside Elements without worrying about low-level details.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->
