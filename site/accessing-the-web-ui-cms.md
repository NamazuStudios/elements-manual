<h1>Accessing the Web UI (CMS)</h1>

<!-- wp:paragraph -->
<p>Namazu Elements provides a simple interface to access its features and manage your in-game content via a locally run content management system, or CMS.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-deploy-in-docker">Deploy in Docker</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>To access the Namazu Elements CMS, you must have Namazu Elements running in Docker.</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><a href="https://namazustudios.com/docs/getting-started/elements-in-five-minutes-or-less/">Elements in Five Minutes or Less</a></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="https://github.com/NamazuStudios/docker-compose?tab=readme-ov-file">Running Elements in Docker</a></li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-login">Login</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Now that you have Elements running locally in Docker, navigate to <a href="http://localhost:8080/admin/login">http://localhost:8080/admin/login</a></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":22302,"width":"394px","height":"auto","sizeSlug":"large","linkDestination":"none","align":"center"} -->
<figure class="wp-block-image aligncenter size-large is-resized"><img src="https://namazustudios.com/wp-content/uploads/2025/08/Screenshot-2026-01-23-at-3.36.46-PM-759x1024.png" alt="" class="wp-image-22302" style="width:394px;height:auto"/><figcaption class="wp-element-caption">Login Prompt</figcaption></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>If there are no users in the database, Elements will create one for you:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>{
    "name": "root",
    "email": "root@example.com",
    "level": "SUPERUSER"
}</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>and password: <code>example</code></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>See <a href="../core-features/users-and-profiles">Users and Profiles</a> for more information on user levels.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>In this case, the User ID can be either the name or email of the user:</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>⚠️ <strong>For security purposes, it is highly recommended to create a new SUPERUSER to serve as your administrative account, and delete the default account.</strong></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Successfully logging in will take you to the Home screen, which should show you the version of Namazu Elements that you're running, as well as allow you to access all of the CMS tools.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":22303,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://namazustudios.com/wp-content/uploads/2025/08/Screenshot-2026-01-23-at-3.37.38-PM-1024x588.png" alt="" class="wp-image-22303"/></figure>
<!-- /wp:image -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-"></h2>
<!-- /wp:heading -->
