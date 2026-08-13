<h1>Direct MongoDB Access (3.5+)</h1>

<!-- wp:paragraph -->
<p>This document describes how to connect to the MongoDB instance provided by <strong>Namazu Elements</strong>. It explains the configuration flow, how SSL is handled, and how an individual Element can establish a connection using the MongoDB driver of its choice. </p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>This is availble in Namazu Elements 3.5 and above.</p>
<!-- /wp:paragraph -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"\u003cstrong\u003e\u003cmark style=\u0022background-color:rgba(0, 0, 0, 0)\u0022 class=\u0022has-inline-color has-black-color\u0022\u003e📖 Using MongoDB Drivers\u003c/mark\u003e\u003c/strong\u003e"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="1982fe"><div class="gb-notice-title" style="color:#fff"><p><strong><mark style="background-color:rgba(0, 0, 0, 0)" class="has-inline-color has-black-color">📖 Using MongoDB Drivers</mark></strong></p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p>This guide only documents how to set up the connection to MongoDB. Making use of the drivers is beyond the scope of this document. To understand more about the drivers and their usages, please refer to the following from MongoDB's official Documentation:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><a href="https://www.mongodb.com/docs/drivers/java/sync/current/get-started/" target="_blank" rel="noreferrer noopener">MongoDB Synchronous Driver</a></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="https://www.mongodb.com/docs/languages/java/reactive-streams-driver/current/" target="_blank" rel="noreferrer noopener">MongoDB Reactive Streams Driver</a></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="https://www.mongodb.com/docs/languages/java/mongodb-hibernate/current/" target="_blank" rel="noreferrer noopener">Hibernate ORM Extension for MongoDB</a></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="https://www.mongodb.com/resources/languages/morphia" target="_blank" rel="noreferrer noopener">Morhpia</a></li>
<!-- /wp:list-item --></ul>
<!-- /wp:list --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-overview">Overview</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Namazu Elements exposes a service named <code>MongoConfigurationService</code> that other Elements can use to retrieve the connection details for MongoDB. This service reads configuration from the application's configuration layer and returns an instance of <code>MongoConfiguration</code>.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Each Element then uses its preferred MongoDB driver and the retrieved configuration to establish a secure connection to the shared MongoDB instance.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-mongoconfigurationservice">MongoConfigurationService</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p><code>MongoConfigurationService</code> is the main entry point for obtaining your MongoDB configuration. Its responsibilities include:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Reading application configuration settings that define how MongoDB should be accessed.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Producing an initialized instance of <code>MongoConfiguration</code>.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>Elements should request this service through the usual dependency injection or service lookup mechanisms used within Namazu Elements. Built in to Namazu Elements is a type</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-mongoconfiguration">MongoConfiguration</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The <code>MongoConfiguration</code> object returned by the service contains the information needed to connect to MongoDB.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-connection-string">Connection String</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p><code>MongoConfiguration</code> always includes a connection string. This connection string can include additional driver settings such as replica set information, authentication data, and flags for TLS.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Example:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>mongodb+srv://example.mongodb.net/?retryWrites=true&amp;w=majority</code></pre>
<!-- /wp:code -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-ssl-context">SSL Context</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>If SSL is configured for your application, <code>MongoConfiguration</code> will also include a <code><a href="https://docs.oracle.com/en/java/javase/21/docs/api/java.base/javax/net/ssl/SSLContext.html" target="_blank" rel="noreferrer noopener">javax.net.ssl.SslContext</a></code>. This SSL context is prepared with the required trust manager and key manager settings.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>This means the Element does not need to manually configure certificates, keystores, or truststores. All SSL requirements for talking to the MongoDB server are handled for you.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>If SSL is not configured, the SSL context will be absent.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-connecting-using-the-mongodb-java-driver">Connecting Using the MongoDB Java Driver</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Elements can use any MongoDB driver they choose. This section demonstrates how to connect using the <strong>MongoDB Java Driver</strong>.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Below is a reference implementation illustrating how to:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Extract the connection string.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Build the SSL configuration.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Apply both to the client settings.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Create a <code>MongoClient</code> and verify the connection.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-example-code">Example Code</h3>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code>public MongoClient connect() {

    final ElementRegistry registry = /* fetch registry */;


    final MongoConfigurationService mongoConfigurationService = registry
        .find("dev.getelements.elements.sdk.mongo")
        .findFirst()
        .get()
        .getServiceLocator()
        .getInstance(MongoConfigurationService.class);


    final MongoConfiguration conf = 
        mongoConfigurationService.getMongoConfiguration();


    final ConnectionString connectionString = new ConnectionString(conf.connectionString());

    final SslSettings sslSettings = conf
        .findSslConfiguration()
        .map(MongoSslConfiguration::newSslContext)
        .map(sslContext -> SslSettings.builder()
            .enabled(true)
            .context(sslContext)
        )
        .orElseGet(() -> SslSettings.builder().applyConnectionString(connectionString))
        .build();


    final MongoClientSettings clientSettings = MongoClientSettings
        .builder()
        .applyConnectionString(connectionString)
        .applyToSslSettings(builder -> builder.applySettings(sslSettings))
        .build();


    return MongoClients.create(clientSettings);

}</code></pre>
<!-- /wp:code -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-how-the-example-works">How the Example Works</h3>
<!-- /wp:heading -->

<!-- wp:list {"ordered":true} -->
<ol class="wp-block-list"><!-- wp:list-item -->
<li><strong>Connection String</strong>: Retrieved directly from the configuration.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>SSL Configuration</strong>: If present, an SSL context is created and injected into <code>SslSettings</code>.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Fallback Behavior</strong>: If SSL is not configured, the SSL settings fall back to whatever is specified in the connection string.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Client Settings</strong>: Both the connection string and SSL settings are applied to <code>MongoClientSettings</code>.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Client Lifecycle</strong>: A <code>MongoClient</code> is created, used, and automatically closed via the try-with-resources block.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Verification</strong>: The cluster description is logged to confirm that the connection was successful.</li>
<!-- /wp:list-item --></ol>
<!-- /wp:list -->

<!-- wp:heading -->
<h2 class="wp-block-heading">Using Guice Injection</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Elements that use Guice for dependency injection can simplify access to the <code>MongoConfigurationService</code>. Namazu Elements exposes its services using standard annotations.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading">package-info.java</h3>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code>@ElementPublic
@ElementDependency("dev.getelements.elements.sdk.mongo")
package dev.getelements.elements.sdk.mongo;


import dev.getelements.elements.sdk.annotation.ElementDependency;
import dev.getelements.elements.sdk.annotation.ElementPublic;</code></pre>
<!-- /wp:code -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading">Injecting the Service in Your Game Code</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>You can inject the <code>MongoConfigurationService</code> directly into your Element or game service.</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>@Inject
public void setMongoConfigurationService(MongoConfigurationService service) {
    this.service = service;
}</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>Once injected, you can build your Mongo client as shown earlier by constructing the connection settings from the injected service.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-summary">Summary</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Connecting to MongoDB through Namazu Elements involves three main steps:</p>
<!-- /wp:paragraph -->

<!-- wp:list {"ordered":true} -->
<ol class="wp-block-list"><!-- wp:list-item -->
<li>Retrieve a <code>MongoConfiguration</code> instance using <code>MongoConfigurationService</code>.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Extract the connection string and optional SSL context.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Use your chosen MongoDB driver to configure and initialize a client.</li>
<!-- /wp:list-item --></ol>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>Namazu Elements handles most of the complexity for you, especially around SSL, so Elements can focus on application logic while maintaining secure and consistent database access.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->
