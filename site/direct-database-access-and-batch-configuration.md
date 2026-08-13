<h1>Direct Database Access and Batch Configuration</h1>

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"level":6} -->
<h6 class="wp-block-heading" id="h-directly-edit-your-mongo-database-or-batch-configure-digital-goods-missions-or-other-items-quickly-and-easily">Directly edit your Mongo database or batch configure digital goods, missions, or other items quickly and easily.</h6>
<!-- /wp:heading -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-direct-database-access">Direct Database Access <a href="#direct-database-access" id="direct-database-access"></a></h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>You can use a tool like Robo 3T to browse and edit the Mongo database directly. Locally, the DB can be accessed at port 27017. Accessing the database for a remote instance on a service like AWS will require more advanced setup with ssh authentication.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-batch-configuration">Batch Configuration <a href="#batch-configuration" id="batch-configuration"></a></h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>If there are a lot of digital goods, missions, or other configurable items to be added to your instance, it may be desirable to batch upload or update many at once. Using bash scripts, it's possible to interact directly with the API to upload data in json format directly to the Elements database.</p>
<!-- /wp:paragraph -->
