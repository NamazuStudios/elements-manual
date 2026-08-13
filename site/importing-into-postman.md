<h1>Importing into Postman</h1>

<!-- wp:paragraph -->
<p>Postman is a great tool for testing APIs and REST calls. Since the Elements Core (and, if you've set it up, your Custom Elements as well) can generate an OAS3 formatted spec of the code. This allows you to cleanly import it directly into Postman.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>First click on the Import button near the top left corner of the window.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":22418,"width":"667px","height":"auto","sizeSlug":"full","linkDestination":"none"} -->
<figure class="wp-block-image size-full is-resized"><img src="https://namazustudios.com/wp-content/uploads/2026/02/Screenshot-2026-02-04-at-3.58.06-PM.png" alt="" class="wp-image-22418" style="width:667px;height:auto"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>Next, put in the URL of the spec that you want to generate. If you're running locally and want to generate the Elements Core API, you can use <a href="http://localhost:8080/api/rest/openapi.json">http://localhost:8080/api/rest/openapi.json</a>. </p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>For a custom Element, you can use something like </p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><code>{urlRoot}/app/rest/{applicationName}/openapi.json</code> </p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>For example, if you've installed the <a href="https://github.com/NamazuStudios/element-example">Example Element</a> locally, you can use <a href="http://localhost:8080/app/rest/example-element/openapi.json">http://localhost:8080/app/rest/example-element/openapi.json</a></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p> </p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":22419,"width":"633px","height":"auto","aspectRatio":"1.4776537848386564","sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large is-resized"><img src="https://namazustudios.com/wp-content/uploads/2026/02/Screenshot-2026-02-04-at-3.59.27-PM-1024x693.png" alt="" class="wp-image-22419" style="aspect-ratio:1.4776537848386564;width:633px;height:auto"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>Import as a Postman collection</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":22420,"width":"682px","height":"auto","aspectRatio":"2.900928124325491","sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large is-resized"><img src="https://namazustudios.com/wp-content/uploads/2026/02/Screenshot-2026-02-04-at-3.59.53-PM-1024x353.png" alt="" class="wp-image-22420" style="aspect-ratio:2.900928124325491;width:682px;height:auto"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>You should now see the imported collection under your collections tab</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":22421,"width":"368px","height":"auto","sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large is-resized"><img src="https://namazustudios.com/wp-content/uploads/2026/02/Screenshot-2026-02-04-at-4.00.30-PM-526x1024.png" alt="" class="wp-image-22421" style="width:368px;height:auto"/></figure>
<!-- /wp:image -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-variables-in-postman">Variables in Postman</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>A useful thing that Postman offers is the ability to assign variables to multiple requests. The baseUrl should be automatically added for you.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":22422,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://namazustudios.com/wp-content/uploads/2026/02/Screenshot-2026-02-04-at-4.07.04-PM-1024x692.png" alt="" class="wp-image-22422"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>Elements also requires that many requests be authenticated. The quickest way to do this is to go to the session folder in your collection and select <code>POST Creates a Session</code></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":22423,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://namazustudios.com/wp-content/uploads/2026/02/Screenshot-2026-02-04-at-4.08.33-PM-1024x589.png" alt="" class="wp-image-22423"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>Navigate to the Body tab and replace the contents with your admin credentials. For example, if you haven't changed them from the default yet, you can use:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>{ 
    "userId": "root",
    "password": "example", 
}</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>If you get an error, there's a good chance the Postman tried assigning some form of default auth to the request headers. To make sure that you don't have this, try selecting the collection itself (parent folder under Collections), go to the Authorization tab, and select No Auth. This will disable it as a default header for all requests. This is important because not all requests use auth (like signing up/in) and can cause errors internally.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":22424,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://namazustudios.com/wp-content/uploads/2026/02/Screenshot-2026-02-04-at-4.24.24-PM-1024x412.png" alt="" class="wp-image-22424"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>If the request is successful, you should see something like this in the response body:</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":22425,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://namazustudios.com/wp-content/uploads/2026/02/Screenshot-2026-02-04-at-4.28.30-PM-1024x478.png" alt="" class="wp-image-22425"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>Now, you can copy the sessionSecret into the header for authenticated requests. To make life a little easier, you can also define this as a variable. Select the Namazu Elements collection again and this time go to the Variables tab.   </p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":22427,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://namazustudios.com/wp-content/uploads/2026/02/Screenshot-2026-02-04-at-4.30.32-PM-1024x220.png" alt="" class="wp-image-22427"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>Let's give it a try! Go to user -> <code>GET Search Users</code>. Try modifying the query params as you see below:</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":22426,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://namazustudios.com/wp-content/uploads/2026/02/Screenshot-2026-02-04-at-4.29.38-PM-1024x525.png" alt="" class="wp-image-22426"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>Next, go to Headers and add the Elements-SessionSecret to the Key field, and the sessionSecret value to the value field. If you defined a variable, you can use this for the value, for example {{variableName}}. </p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":22429,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://namazustudios.com/wp-content/uploads/2026/02/Screenshot-2026-02-04-at-4.37.44-PM-1024x502.png" alt="" class="wp-image-22429"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>Since this a common header for authenticated requests in Elements, you can add it as a preset so that you can quickly assign it in the future.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":22428,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://namazustudios.com/wp-content/uploads/2026/02/Screenshot-2026-02-04-at-4.35.39-PM-1024x130.png" alt="" class="wp-image-22428"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>Click Send in the top right and you should see a list of Users in the response body!</p>
<!-- /wp:paragraph -->
