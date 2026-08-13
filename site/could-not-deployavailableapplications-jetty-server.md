<h1>Could not deployAvailableApplications Jetty server Failed to bind to /0.0.0.0:8080 Address already in use</h1>

<!-- wp:paragraph -->
<p>If you are trying to run Elements, or your custom Element project, directly in your IDE, and you see an exception like this:</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":22347,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://namazustudios.com/wp-content/uploads/2026/01/Screenshot-2026-01-30-at-10.31.52-AM-1024x412.png" alt="" class="wp-image-22347"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>the important parts are:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>Exception in thread "main" <strong>dev.getelements.elements.sdk.model.exception.InternalException: Could not deployAvailableApplications Jetty server.</strong>
...
<strong>Caused by: java.io.IOException: Failed to bind to /0.0.0.0:8080</strong>
...
<strong>Caused by: java.net.BindException: Address already in use</strong>
...</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>Then the likely culprit is that you are already running another instance of Elements locally. If you look at your running containers in Docker Desktop (or using <code>docker ps</code> in the terminal), and you see that a ws container is running (ws-1 in this case), then it will already be using the address that the IDE will attempt to bind to. Shutting down this container will allow you to run in the IDE again. You can use the stop button in Docker Desktop, or via command line using <code>docker stop &lt;container name></code>, e.g. <code>docker stop docker-compose-main-ws-1</code> (use <code>docker ps -a</code> to see all containers and their names - use the name of the container to stop it). </p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":22345,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://namazustudios.com/wp-content/uploads/2026/01/Screenshot-2026-01-30-at-10.25.23-AM-1024x247.png" alt="" class="wp-image-22345"/><figcaption class="wp-element-caption">Docker Desktop containers</figcaption></figure>
<!-- /wp:image -->

<!-- wp:image {"id":22346,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://namazustudios.com/wp-content/uploads/2026/01/Screenshot-2026-01-30-at-10.28.53-AM-1024x23.png" alt="" class="wp-image-22346"/><figcaption class="wp-element-caption">docker ps -a result</figcaption></figure>
<!-- /wp:image -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"Reminder!"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="68bd1c"><div class="gb-notice-title" style="color:#fff"><p>Reminder!</p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p>Bear in mind that you'll still want the mongo container running, even when running in the IDE.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->
