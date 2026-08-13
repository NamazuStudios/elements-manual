<h1>Exception in monitor thread while connecting to server localhost:27017</h1>

<!-- wp:paragraph -->
<p>When running in the IDE, if you get an error during startup that looks like this:</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":22349,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://namazustudios.com/wp-content/uploads/2026/01/Screenshot-2026-01-30-at-11.31.36-AM-1024x247.png" alt="" class="wp-image-22349"/></figure>
<!-- /wp:image -->

<!-- wp:code -->
<pre class="wp-block-code"><code>&#91;main] INFO  d.g.e.d.m.p.MongoClientProvider - Using Connection String mongodb://localhost
&#91;cluster-ClusterId{value='697d0682597b1a49061a2baa', description='null'}-localhost:27017] INFO  org.mongodb.driver.cluster - Exception in monitor thread while connecting to server localhost:27017
com.mongodb.MongoSocketOpenException: Exception opening socket
...
Caused by: java.net.ConnectException: Connection refused</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>then you likely don't have a mongodb container running. Mongo is the database that Elements uses, and Elements attempts to make a connection to a local instance running at the default mongo port (27017). </p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>If you've already run mongo before using the <code>docker compose up</code> command that you previously used, or via Docker Desktop. </p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":22350,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://namazustudios.com/wp-content/uploads/2026/01/Screenshot-2026-01-30-at-11.43.30-AM-1024x419.png" alt="" class="wp-image-22350"/></figure>
<!-- /wp:image -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"Warning","noticeBackgroundColor":"#ffdd57"} -->
<div style="color:#32373c;background-color:#ffdd57" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="0eaadb"><div class="gb-notice-title" style="color:#fff"><p>Warning</p></div><div class="gb-notice-text" style="border-color:#ffdd57"><!-- wp:paragraph -->
<p>If you have multiple instances of mongo, make sure that you run the same one each time for data consistency.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>There are a few ways to get mongo running if you haven't already. Since you're running in the IDE, it's probably safe to assume that you have either the <a href="https://github.com/NamazuStudios/elements">Elements source project</a> or the <a href="https://github.com/NamazuStudios/element-example">example element project</a> (or some other custom element project). Included in these is a services-dev folder (<a href="https://github.com/NamazuStudios/elements/tree/main/docker-config/services-dev">source</a> / <a href="https://github.com/NamazuStudios/element-example/tree/main/services-dev">example</a>) with a docker compose file that will fetch and run mongo for you.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->
