<h1>Applications</h1>

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"level":6} -->
<h6 class="wp-block-heading" id="h-elements-allows-the-creation-of-applications-in-a-multi-tenant-format-a-suite-of-applications-that-communicate-with-the-database-across-a-single-shared-instance">Elements allows the creation of Applications in a multi-tenant format - a suite of applications that communicate with the database across a single shared instance.</h6>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Applications define each app that you run on your Elements instance. Elements supports the concept of multi-tenancy, which is the ability to host multiple separate and independent applications from a single shared database of users. For example, <a href="users-and-profiles">Users</a>, <a href="digital-goods">Items</a>, and <a href="progress-and-missions">Missions</a> are shared across all applications on the instance.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>This enables your enterprise to publish a suite of applications under one instance of Elements. Such use cases for multiple applications may include a series of episodic games, separation of production and testing environments, cross promotion and sharing of digital goods, or the ability to publish multiple similar related applications sharing the back end.</p>
<!-- /wp:paragraph -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"Note"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="3b0649"><div class="gb-notice-title" style="color:#fff"><p>Note</p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p>For your customers who opt to use user name and password login, multi-tenancy greatly reduce password fatigue and allows you to offer single-sign on to all of your apps.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:paragraph -->
<p>Each Application is the nexus for all subordinate configurations. It is even possible to support multiple independent iOS/Google Play Application IDs under one Application within elements. This can be useful if your company implements a strategy of using alternative application IDs for testing or pre-release content.</p>
<!-- /wp:paragraph -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"Warning","noticeBackgroundColor":"#ffdd57"} -->
<div style="color:#32373c;background-color:#ffdd57" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="0eaadb"><div class="gb-notice-title" style="color:#fff"><p>Warning</p></div><div class="gb-notice-text" style="border-color:#ffdd57"><!-- wp:paragraph -->
<p>For Applications, the following restrictions apply:</p>
<!-- /wp:paragraph -->

<!-- wp:list {"ordered":true} -->
<ol class="wp-block-list"><!-- wp:list-item -->
<li>There is one and only one repository of script code for executing cloud functions. </li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>There is one and only one endpoint for CDN support.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>A single Application may not host the same Google Play, iOS, or other unique application identifier.</li>
<!-- /wp:list-item --></ol>
<!-- /wp:list --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-application-configurations">Application Configurations <a href="#application-configurations" id="application-configurations"></a></h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Application configurations add support for features such as Facebook SSO, Firebase push notifications, and Android and iOS In-App Products. Application configurations are created and configured from inside the Application editor in the console.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Each application has its own set of application configurations, which includes the data connecting it to various services such as push notifications, Facebook, and more.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Product bundles are connected to individual applications via corresponding application configurations. These are used to manage in-app purchases for platforms like iOS and Android.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-application-structure">Application Structure <a href="#application-structure" id="application-structure"></a></h3>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>_id:</strong> This is the unique id automatically assigned when the application is created.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>name:</strong> This is the application's unique name.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>description:</strong> This string is the application's description.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-managing-applications-using-the-console">Managing Applications Using the Console <a href="#managing-applications-using-the-console" id="managing-applications-using-the-console"></a></h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Applications are managed in the admin console by selecting Applications from the nav bar or the hamburger menu.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="h-add-or-delete-an-application">Add or Delete an Application <a href="#add-or-delete-an-application" id="add-or-delete-an-application"></a></h4>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Use the "Create Application" button to add a new application.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Applications can be deleted by using the "Delete" button (trash icon) next to them.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":22321,"sizeSlug":"full","linkDestination":"none"} -->
<figure class="wp-block-image size-full"><img src="https://namazustudios.com/wp-content/uploads/2025/08/Screenshot-2026-01-26-at-11.57.22-AM-scaled.png" alt="" class="wp-image-22321"/></figure>
<!-- /wp:image -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="h-edit-an-application">Edit an Application <a href="#edit-an-application" id="edit-an-application"></a></h4>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Applications can be edited by tapping the "Edit" button for each application.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>In the edit panel, you'll also find the URL for the git script repo for the application, as well as links to the API documentation for the application.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Application configurations are also accessed here, but we will cover those in a separate section below.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":22323,"sizeSlug":"full","linkDestination":"none"} -->
<figure class="wp-block-image size-full"><img src="https://namazustudios.com/wp-content/uploads/2025/08/Screenshot-2026-01-26-at-11.59.50-AM.png" alt="" class="wp-image-22323"/></figure>
<!-- /wp:image -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="h-json-structure-of-applications">JSON Structure of Applications <a href="#json-structure-of-applications" id="json-structure-of-applications"></a></h4>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code>&#91;{
    "_id" : ObjectId("5cdb1088e96c3c4f2bfe1da7"),
    "active" : true,
    "name" : "NAME",
    "description" : "This is a description"
}]</code></pre>
<!-- /wp:code -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-adding-an-application-configuration">Adding an Application Configuration <a href="#adding-an-application-configuration" id="adding-an-application-configuration"></a></h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>In the Applications section of the admin console, after having added an application, you are able to add additional metadata to that application.</p>
<!-- /wp:paragraph -->

<!-- wp:list {"ordered":true} -->
<ol class="wp-block-list"><!-- wp:list-item -->
<li>Edit the chosen Application</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>In the Application Editor, tap the "Add…" button to open the drop down menu</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Select the desired application configuration type</li>
<!-- /wp:list-item --></ol>
<!-- /wp:list -->
