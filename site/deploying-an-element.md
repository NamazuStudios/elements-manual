<h1>Deploying an Element</h1>

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-creating-an-application">Creating an Application</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>When you are ready to deploy your Element, first you must make sure that you have an Application created. If you have not done so, create a new Application either in the CMS or manually via the <a href="https://namazustudios.com/rest/api/#/operations/createApplication">Application API</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":22153,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://namazustudios.com/wp-content/uploads/2025/08/Screenshot-2025-08-27-at-1.28.16-PM-1024x581.png" alt="" class="wp-image-22153"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>Once the Application is created, you will have a new git repository for the Application to upload your code to. If you created the Application via the Application API, the property will be <code>scriptRepoUrl</code>. If you created it via the CMS, then go to Edit for the newly created Application to view the URL there.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":22154,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://namazustudios.com/wp-content/uploads/2025/08/Screenshot-2025-08-27-at-1.33.38-PM-1024x320.png" alt="" class="wp-image-22154"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>Once you have the repository URL, you can use the git CLI to <code>git clone</code> or use an application such as <a href="https://www.sourcetreeapp.com/">SourceTree</a> to pull the empty repository. This is now your deployment directory.</p>
<!-- /wp:paragraph -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"Note"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="3b0649"><div class="gb-notice-title" style="color:#fff"><p>Note</p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p>If you get a 403 when trying to pull, then you'll need to specify the credentials of a User with SUPERUSER privileges. This can either be done via a <a href="https://git-scm.com/doc/credential-helpers">credential helper</a> or by adding the credentials to the URL directly, for example: <br><code>git clone http://username:password@localhost:8080/code/MyApplication</code></p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-configuring-the-deployment-directory">Configuring the Deployment Directory</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Within the deployment directory, two things are needed: </p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>a properties file, named <code>dev.getelements.element.attributes.properties</code></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>and a <code>lib</code> folder</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>(see <a href="https://github.com/NamazuStudios/element-example">element-example-deployment</a> folder for an example of these in the example project repo)</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>The properties file contains a list of properties that can overwrite Elements <a href="../docs/custom-code/properties/">default properties</a>. This can be useful for adjusting settings based on the environment that you're deploying to.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>The <code>element-libs</code> folder will contain your packaged Element code, as well as the jars of any dependencies that were built alongside it.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-packaging-your-element-code">Packaging Your Element Code</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Then, you need to package up your project into a jar. You can do this easily with Maven using the CLI command: <code>mvn clean package</code></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>This will output two important things into the <code>target</code> folder in your project: the jar file for your code, and a dependencies folder called <code>elements-libs</code>, with other required jar files.</p>
<!-- /wp:paragraph -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"Note"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="3b0649"><div class="gb-notice-title" style="color:#fff"><p>Note</p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p>The jar file for your code will be named based on what is specified in the <code>pom.xml</code> file at the root of your project. For example: <br><code>&lt;groupId>org.example&lt;/groupId><br>&lt;artifactId>ElementSample&lt;/artifactId><br>&lt;version>1.0-SNAPSHOT&lt;/version></code><br>will generate <code>ElementSample-1.0-SNAPSHOT.jar</code></p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:image {"id":22156,"sizeSlug":"full","linkDestination":"none"} -->
<figure class="wp-block-image size-full"><img src="https://namazustudios.com/wp-content/uploads/2025/08/Screenshot-2025-08-27-at-2.25.56-PM.png" alt="" class="wp-image-22156"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>Next, copy your jar file, as well as all of the jars inside of <code>element-libs</code> (not the directory itself, but all of the contents) into your Deployment Directory <code>lib</code> folder. After copying these, your Deployment Directory should look like this:</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":22157,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://namazustudios.com/wp-content/uploads/2025/08/Screenshot-2025-08-27-at-2.37.30-PM-1024x808.png" alt="" class="wp-image-22157"/></figure>
<!-- /wp:image -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"Note"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="3b0649"><div class="gb-notice-title" style="color:#fff"><p>Note</p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p>Unless you've updated Namazu Elements or your dependencies, you can just replace the one jar for your Element code for subsequent deployments.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-deployment">Deployment</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Once the jars and properties are in place, make sure that you've added and committed your changes in git. Once committed, you're good to push!</p>
<!-- /wp:paragraph -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"Warning","noticeBackgroundColor":"#ffdd57"} -->
<div style="color:#32373c;background-color:#ffdd57" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="0eaadb"><div class="gb-notice-title" style="color:#fff"><p>Warning</p></div><div class="gb-notice-text" style="border-color:#ffdd57"><!-- wp:paragraph -->
<p>If you're using branches in your Deployment Directory, it will be necessary to specify that you want to push to <code>main</code>, as branching Element code is not supported internally, and changes pushed to anything other than the <code>main</code> branch will not be run. </p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>This can be done like so:<br><code>git push origin my-branch:main</code></p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:paragraph -->
<p>After pushing, you may need to restart the container to apply the changes. Via the CLI, you can use <code>docker ps</code> to get the list of containers and then <code>docker restart &lt;container-name></code>. In this case we would want to restart the ws (web services) container like so: </p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":22159,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://namazustudios.com/wp-content/uploads/2025/08/Screenshot-2025-08-27-at-2.43.36-PM-1-1024x86.png" alt="" class="wp-image-22159"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>You can also use Docker Desktop to restart the container:</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":22160,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://namazustudios.com/wp-content/uploads/2025/08/Screenshot-2025-08-27-at-2.48.21-PM-1024x158.png" alt="" class="wp-image-22160"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>Once the container has restarted, you should be able to confirm that your custom code was recognized in the logs, along with the relative root path for its API calls. For example:<br><code>21:43:22.220 [main] INFO o.e.j.server.handler.ContextHandler - Started oeje10s.ServletContextHandler@7fecc26f{/app/rest/example-element,/app/rest/example-element,b=null,a=AVAILABLE,h=oeje10s.ServletHandler@402adc9c{STARTED}}</code></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->
