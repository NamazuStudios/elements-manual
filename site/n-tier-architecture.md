<h1>N-Tier Architecture</h1>

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"level":6} -->
<h6 class="wp-block-heading" id="h-elements-uses-the-n-tier-architecture-style-with-three-layers-including-the-presentation-layer-service-layer-and-dao-layer"><br>Elements uses the N-Tier Architecture style with three layers, including the Presentation Layer, Service Layer, and DAO Layer.</h6>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Internally, Elements uses an N-tier architecture style consisting of three layers.</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>The <a href="#presentation-layer"><strong>Presentation Layer</strong></a> is a thinly defined layer of abstraction over the underlying Service Layer. RESTful APIs provide a translation between the Service layer and the Presentation layer. JSON-RPC APIs (experimental) are essentially calling service layer methods directly. The endpoint code simply unpacks the requests and translates that to the Service layer. The Presentation layer does nothing to enforce security.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>The <a href="#service-layer"><strong>Service Layer</strong></a> (also sometimes called the <em>Logic Layer</em>) houses all the business logic of the system. This layer exists with multiple implementations of each service. All APIs labeled "service" (e.g. - <code>dev.getelements.elements.sdk.service.smartcontract.evm</code>) will always consider the context of the user making the request. In some cases, the Service layer implementation is configured to do nothing but throw exceptions.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>The <a href="#data-layer"><strong>DAO Layer</strong></a> (also sometimes called the <em>Data Layer</em>) is the layer that provides access to the database through layers of abstraction. All APIs labeled "dao" (e.g. - <code>namazu.elements.dao.user</code>) are part of the DAO layer.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-presentation-layer">Presentation Layer <a href="#presentation-layer" id="presentation-layer"></a></h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The presentation layer is the layer closet to the client code. In the context of Elements, this is the <a href="broken-reference">Resource</a> handling the cloud function invocation. Internally, Elements uses <a href="https://en.wikipedia.org/wiki/Jakarta_RESTful_Web_Services">JAX-RS</a> annotated methods at the presentation layer.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-service-layer">Service Layer <a href="#service-layer" id="service-layer"></a></h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The Service Layer houses the logic of the application. Each Service is a common interface through which client code makes calls. Depending on <a href="../../core-features/users-and-profiles#user-properties">User Acccess Level</a>, Elements will use a different implementation of the Service. Most services honor user access level. However, as the developer of the application, you should not worry about these details. When using the Scripting engine, invoking service layer code will typically enforce the appropriate permissions.</p>
<!-- /wp:paragraph -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"Warning","noticeBackgroundColor":"#ffdd57"} -->
<div style="color:#32373c;background-color:#ffdd57" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="0eaadb"><div class="gb-notice-title" style="color:#fff"><p>Warning</p></div><div class="gb-notice-text" style="border-color:#ffdd57"><!-- wp:paragraph -->
<p>If a Service is not available for a particular access level, the Service will throw the appropriate exception indicating that the permission check failed.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="h-scoped-services">Scoped Services <a href="#scoped-services" id="scoped-services"></a></h4>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>A service that specifically honors User Access Levels is said to be <em>scoped</em>, which means it may have a reference to the currently logged-in user when processing the request. Useful information may be inferred from this scoping.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="h-unscoped-services">Unscoped Services <a href="#unscoped-services" id="unscoped-services"></a></h4>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>A service that does not honor User Access Levels is said to be <em>unscoped</em>, meaning it will not have any reference to the currently logged-in user. Typically, unscoped services provide super-user access to the system. Alternatively, they may provide information that is available to all users without restriction.</p>
<!-- /wp:paragraph -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"Warning","noticeBackgroundColor":"#ffdd57"} -->
<div style="color:#32373c;background-color:#ffdd57" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="0eaadb"><div class="gb-notice-title" style="color:#fff"><p>Warning</p></div><div class="gb-notice-text" style="border-color:#ffdd57"><!-- wp:paragraph -->
<p>When writing cloud functions, you should handle the service layer with care because it is unscoped and super-user access is provided.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-dao-layer">DAO Layer <a href="#data-layer" id="data-layer"></a></h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The data layer has no user scope, and therefore provides unrestricted access to the database. The Data Layer abstracts the database details. The Data Layer performs operations such as querying, inserting, updating, and deleting.</p>
<!-- /wp:paragraph -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"Warning","noticeBackgroundColor":"#ffdd57"} -->
<div style="color:#32373c;background-color:#ffdd57" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="0eaadb"><div class="gb-notice-title" style="color:#fff"><p>Warning</p></div><div class="gb-notice-text" style="border-color:#ffdd57"><!-- wp:paragraph -->
<p>As with unscoped services, Data Layer code provides raw access to the database without regard for any scoping rules. Therefore, special care must be taken to make use of this.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:paragraph -->
<p>These three layers describe a single Elements instance. For how one instance calls a service hosted on another instance, see <a href="cluster-rpc-fabric">Cluster RPC (Fabric)</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->
