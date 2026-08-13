<h1>Namazu Elements in Five Minutes or Less</h1>

<!-- wp:paragraph -->
<p id="h-spin-up-a-local-elements-instance-and-explore-a-full-game-backend-runtime-in-minutes">Spin up a local Elements instance and explore a full game backend runtime in minutes.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-what-is-namazu-elements">What Is Namazu Elements?</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p><strong>Namazu</strong> <strong>Elements is a server-side runtime for connected games.</strong></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>It can:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>run authoritative game logic</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>manage players, inventory, rewards, missions, and leaderboards</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>host real-time or turn-based games</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>act as your game server via Custom Elements</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>You can run Elements locally to prototype, test integrations, or build full gameplay logic without deploying anything.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading">What You’ll Get Locally</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>When you run Elements locally, you get:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>A full Elements backend (API + database)</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Swagger API documentation</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Admin / CMS tools</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Support for Custom Elements (server-side Java code)</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>This environment behaves the same way as a production deployment, just running on your machine.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-prerequisites">Prerequisites <a href="#prerequisites" id="prerequisites"></a></h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Before you start, make sure you have:</p>
<!-- /wp:paragraph -->

<!-- wp:list {"ordered":true} -->
<ol class="wp-block-list"><!-- wp:list-item -->
<li><a href="https://www.docker.com/products/docker-desktop/">Docker Desktop</a> or <a href="https://docs.docker.com/compose/install/">Docker Compose</a></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="https://git-scm.com/">Git</a></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>...that’s it!</li>
<!-- /wp:list-item --></ol>
<!-- /wp:list -->

<!-- wp:heading -->
<h2 class="wp-block-heading">Run Elements Locally</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>From a terminal run these three commands:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>git clone https://github.com/NamazuStudios/docker-compose.git

cd docker-compose

docker compose up</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>After a short startup, Elements will be running locally, and your Elements runtime is live and ready to use.</p>
<!-- /wp:paragraph -->

<!-- wp:quote -->
<blockquote class="wp-block-quote"><!-- wp:paragraph -->
<p>More information about the Elements Docker Compose project is available here: <a href="https://github.com/Elemental-Computing/docker-compose/">https://github.com/Elemental-Computing/docker-compose</a></p>
<!-- /wp:paragraph --></blockquote>
<!-- /wp:quote -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading">Verify It’s Running</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Once started, you can immediately:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Open the admin UI (also known as the Content Management System, or CMS): <br><a href="http://localhost:8080/admin/login">http://localhost:8080/admin/login</a></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Explore the API with Swagger: <br><a href="http://localhost:8080/doc/swagger/">http://localhost:8080/doc/swagger/</a></li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>If those load, Elements is up and ready.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading">Your First Interaction</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Here’s a simple way to confirm everything is working.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Create a user via the API:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>curl -X POST http://localhost:8080/api/rest/user \
  -H "Content-Type: application/json" \
  -d '{"name":"player1","email":"player1@example.com", "password":"example"}'</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>You should receive a JSON response with a user ID and other user info.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>You’ve just interacted with your local Elements backend.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading">Where Does Game Logic Go?</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Elements is designed to run authoritative code, which is logic that players cannot modify.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>That logic can live:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>inside Custom Elements (running within Elements itself), or</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>in external services that call Elements APIs</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>If this is your first time working with server-side game logic, we recommend starting with the Fundamentals section.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Start here:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><a href="../docs/fundamentals/why-you-need-a-server-and-what-authoritative-means/">Why You Need Authority</a></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="../docs/fundamentals/elements-as-a-game-runtime/">Elements as a Game Runtime</a></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="../docs/fundamentals/lifecycles-and-flows/">Lifecycles and Flows</a></li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>These pages explain how Elements fits into a game architecture - without assuming backend experience.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-running-locally-in-the-ide">Running Locally in the IDE</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>If you need custom logic or an API that isn't covered by the Elements Core features, try making your own custom Element! Elements makes it easy to develop and debug by letting you run the entirety of Elements with your custom code directly in your IDE. This allows you to view logs and set breakpoints to fully test your application before even making your first deployment. </p>
<!-- /wp:paragraph -->

<!-- wp:quote -->
<blockquote class="wp-block-quote"><!-- wp:paragraph -->
<p>Since Elements uses Java, we highly recommend using <a href="https://www.jetbrains.com/idea/download/">IntelliJ</a> as your IDE.</p>
<!-- /wp:paragraph --></blockquote>
<!-- /wp:quote -->

<!-- wp:paragraph -->
<p>Head over to the <a href="https://namazustudios.com/docs/custom-code/element-structure/">Custom Code</a> section to get an overview, or check out the platform specific setup guides here:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><a href="https://namazustudios.com/docs/custom-code/setup-for-windows/">Windows</a></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="https://namazustudios.com/docs/custom-code/mac-os-setup/">Mac</a></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="https://namazustudios.com/docs/custom-code/ubuntu-linux-setup/">Linux</a></li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>Once you're done, check out our example project <a href="https://github.com/Elemental-Computing/element-example">here</a> to see how to build a custom Element, debug your work locally, and make a deployment.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-accessing-api-documentation">Accessing API Documentation <a id="accessing-documentation" href="#accessing-documentation"></a></h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Each instance of Elements provides a copy of the documentation specific to the release. It also provides Swagger json files which you can access. The documentation will vary based on the version. For example: <a href="https://javadoc.getelements.dev/3.6.24/index.html">https://javadoc.getelements.dev/3.6.24/index.html</a> </p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>These pages are also accessible via the CMS under the Core Elements section, like so:</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":22315,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://namazustudios.com/wp-content/uploads/2025/08/Screenshot-2026-01-26-at-10.20.40-AM-1024x290.png" alt="" class="wp-image-22315"/></figure>
<!-- /wp:image -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-what-s-next">What's Next?</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Now that you’re running Elements locally, start by:</p>
<!-- /wp:paragraph -->

<!-- wp:list {"ordered":true} -->
<ol class="wp-block-list"><!-- wp:list-item -->
<li>Exploring the fundamentals:<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Learn how <a href="../docs/fundamentals/why-you-need-a-server-and-what-authoritative-means/">authority</a>, <a href="../docs/fundamentals/lifecycles-and-flows/">lifecycles</a>, and <a href="../docs/fundamentals/elements-as-a-game-runtime/">Custom Elements</a> work together.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list --></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Or dive right in!<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><a href="../docs/getting-started/accessing-the-web-ui-cms/">Exploring the Content Management System (CMS)</a></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="../docs/namazu-elements-core/user-authentication-sign-in/creating-a-user/">Creating your first user</a></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="../docs/restful-apis/swagger-and-swagger-ui/">Exploring the API with Swagger</a></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="../docs/general-concepts/overview">Understanding core concepts</a></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="../docs/custom-code/element-structure">Building a Custom Element</a></li>
<!-- /wp:list-item --></ul>
<!-- /wp:list --></li>
<!-- /wp:list-item --></ol>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->
