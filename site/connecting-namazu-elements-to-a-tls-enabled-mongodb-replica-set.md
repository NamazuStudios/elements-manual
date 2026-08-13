<h1>Connecting Namazu Elements to a TLS-Enabled MongoDB Replica Set</h1>

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-overview">Overview</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Namazu Elements connects to MongoDB over TLS using mutual certificate authentication. The application side converts raw PEM certificates into PKCS12 (.p12) format for use by the Java client, while MongoDB itself consumes PEM files natively via OpenSSL. Ten environment variables drive the configuration, covering the connection URI, TLS certificate paths and passphrases, and the cryptographic algorithm selections.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-prerequisites">Prerequisites</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The following tools must be available on the host running the Elements initialization script:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><a href="https://docs.oracle.com/en/java/javase/21/docs/specs/man/keytool.html">keytool</a> (bundled with the JDK)</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="https://wiki.openssl.org/index.php/Command_Line_Utilities">openssl</a></li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>You will also need the following PEM files available at boot time, typically mounted as file secrets:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>ca.pem - the certificate authority certificate</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>certificate.pem - the client private key concatenated with the client certificate (private key first)</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-mongodb-node-configuration">MongoDB Node Configuration</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>MongoDB natively understands PEM files via its built-in OpenSSL support. No special Java-side handling is needed on the database node. The relevant mongod.conf settings are:</p>
<!-- /wp:paragraph -->

<!-- wp:betterdocs/code-snippet {"blockId":"betterdocs-code-snippet-528abb8e","blockMeta":{"desktop":" .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-528abb8e { border-width: 0px !important; border-radius: 0px !important; } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-528abb8e .betterdocs-code-snippet-header.betterdocs-file-preview-header { border-bottom-width: 1px !important; border-bottom-style: solid !important; } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-528abb8e .betterdocs-code-snippet-header .betterdocs-file-name .file-name-text { font-size: 14px; } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-528abb8e .betterdocs-code-snippet-header .betterdocs-code-snippet-copy-button { } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-528abb8e .betterdocs-code-snippet-content .betterdocs-code-snippet-line-numbers { border-right-width: 1px !important; border-right-style: solid !important; } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-528abb8e .betterdocs-code-snippet-content .betterdocs-code-snippet-line-numbers .line-number { } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-528abb8e .betterdocs-code-snippet-code { } ","tab":" ","mobile":" "},"codeContent":"net:\n  tls:\n    mode: requireTLS\n    CAFile: /etc/mongod.conf.d/ca.pem\n    certificateKeyFile: /etc/mongod.conf.d/certificate.pem\n  ipv6: true\n  bindIpAll: true\nreplication:\n  replSetName: \u003cyour-replica-set-name\u003e","language":"yaml","fileName":"mongod.yaml"} /-->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"Replica Set is Mandatory","noticeBackgroundColor":"#ffdd57","noticeTitleColor":"#000000"} -->
<div style="color:#32373c;background-color:#ffdd57" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="8eb132"><div class="gb-notice-title" style="color:#000000"><p>Replica Set is Mandatory</p></div><div class="gb-notice-text" style="border-color:#ffdd57"><!-- wp:paragraph -->
<p>Namazu Elements uses MongoDB transactions which require a replica set, even if the set only has a single member. Without that some APIs will not work and we provide no guarantees which will and will not work with transactions disabled.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:paragraph -->
<p>The certificateKeyFile must be a single PEM file containing the private key followed by the certificate, concatenated together:</p>
<!-- /wp:paragraph -->

<!-- wp:betterdocs/code-snippet {"blockId":"betterdocs-code-snippet-028a0e73","blockMeta":{"desktop":" .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-028a0e73 { border-width: 0px !important; border-radius: 0px !important; } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-028a0e73 .betterdocs-code-snippet-header.betterdocs-file-preview-header { border-bottom-width: 1px !important; border-bottom-style: solid !important; } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-028a0e73 .betterdocs-code-snippet-header .betterdocs-file-name .file-name-text { font-size: 14px; } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-028a0e73 .betterdocs-code-snippet-header .betterdocs-code-snippet-copy-button { } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-028a0e73 .betterdocs-code-snippet-content .betterdocs-code-snippet-line-numbers { border-right-width: 1px !important; border-right-style: solid !important; } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-028a0e73 .betterdocs-code-snippet-content .betterdocs-code-snippet-line-numbers .line-number { } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-028a0e73 .betterdocs-code-snippet-code { } ","tab":" ","mobile":" "},"codeContent":"\u002d\u002d\u002d\u002d-BEGIN CERTIFICATE\u002d\u002d\u002d\u002d-\n\u003cpublic key material\u003e\n\u002d\u002d\u002d\u002d-END CERTIFICATE\u002d\u002d\u002d\u002d-\n\u002d\u002d\u002d\u002d-BEGIN PRIVATE KEY\u002d\u002d\u002d\u002d-\n\u003cprivate key material\u003e\n\u002d\u002d\u002d\u002d-END PRIVATE KEY\u002d\u002d\u002d\u002d-\n","fileName":"server.pem"} /-->

<!-- wp:paragraph -->
<p>On a Kubernetes-based deployment, these can be mounted as file secrets in the pod. On an AMI-based deployment, they should be placed at the equivalent paths on disk before mongod starts.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-application-node-configuration">Application Node Configuration</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The Elements application node cannot consume PEM files directly from Java. The initialization script converts them to PKCS12 format using keytool and openssl before the application starts.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-certificate-conversion-script-snippet">Certificate Conversion Script Snippet</h3>
<!-- /wp:heading -->

<!-- wp:betterdocs/code-snippet {"blockId":"betterdocs-code-snippet-220b8c5a","blockMeta":{"desktop":" .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-220b8c5a { border-width: 0px !important; border-radius: 0px !important; } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-220b8c5a .betterdocs-code-snippet-header.betterdocs-file-preview-header { border-bottom-width: 1px !important; border-bottom-style: solid !important; } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-220b8c5a .betterdocs-code-snippet-header .betterdocs-file-name .file-name-text { font-size: 14px; } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-220b8c5a .betterdocs-code-snippet-header .betterdocs-code-snippet-copy-button { } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-220b8c5a .betterdocs-code-snippet-content .betterdocs-code-snippet-line-numbers { border-right-width: 1px !important; border-right-style: solid !important; } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-220b8c5a .betterdocs-code-snippet-content .betterdocs-code-snippet-line-numbers .line-number { } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-220b8c5a .betterdocs-code-snippet-code { } ","tab":" ","mobile":" "},"codeContent":"# Convert CA certificate to PKCS12 keystore\necho \u0022yes\u0022 | keytool \u005c\n  -importcert \u005c\n  -alias \u0022MongoDB Certificate Authority\u0022 \u005c\n  -file \u0022${ELEMENTS_CONF}/mongo/ca.pem\u0022 \u005c\n  -keystore \u0022${dev_getelements_elements_mongo_tls_ca}\u0022 \u005c\n  -storepass \u0022${dev_getelements_elements_mongo_tls_ca_passphrase}\u0022\n\n# Convert client certificate + key to PKCS12\nopenssl pkcs12 \u005c\n  -export \u005c\n  -name \u0022MongoDB Client Certificate\u0022 \u005c\n  -in \u0022${ELEMENTS_CONF}/mongo/certificate.pem\u0022 \u005c\n  -out \u0022${dev_getelements_elements_mongo_tls_client_certificate}\u0022 \u005c\n  -passout \u0022pass:${dev_getelements_elements_mongo_tls_client_certificate_passphrase}\u0022","language":"bash","fileName":"mongo_init.sh"} /-->

<!-- wp:paragraph -->
<p>The output <code>.p12</code> files are then chowned to the <code>elements</code> user before the application process starts. The input PEM files at <code>${ELEMENTS_CONF}/mongo/</code> should be the same CA and concatenated client certificate/key described in the MongoDB node section above.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><strong>Default output paths</strong> (overridable via environment variables).</p>
<!-- /wp:paragraph -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><tbody><tr><td class="has-text-align-center" data-align="center"><strong>Variable</strong></td><td class="has-text-align-center" data-align="center"><strong>Default</strong></td></tr><tr><td class="has-text-align-center" data-align="center"><code>dev_getelements_elements_mongo_tls_ca</code></td><td class="has-text-align-center" data-align="center">${ELEMENTS_CONF}/mongo_ca.p12</td></tr><tr><td class="has-text-align-center" data-align="center"><code>dev_getelements_elements_mongo_tls_client_certificate</code></td><td class="has-text-align-center" data-align="center"><code>${ELEMENTS_CONF}/mongo_certificate.p12</code></td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"Note","noticeTitleColor":"#000000"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="3b0649"><div class="gb-notice-title" style="color:#000000"><p>Note</p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p>These variables are recommended as part of an initialization script as they overlap the variables understood by the Namazu Elements application code. The intent is that these are written to the container "just in time" as part of the server startup, but practically they need only be run at some time before application boot.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-environment-variables">Environment Variables</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The following variables configure the Elements MongoDB connection. All are read by MongoConfigurationService at startup.</p>
<!-- /wp:paragraph -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><tbody><tr><td><strong>Variable</strong></td><td><strong>Default</strong></td><td><strong>Sensitive</strong></td><td><strong>Description</strong></td></tr><tr><td><code>dev_getelements_elements_mongo_uri</code></td><td><code>mongodb://localhost</code></td><td>No</td><td>MongoDB connection URI. Set <code>tls=true</code> in the query string to enable TLS.</td></tr><tr><td><code>dev_getelements_elements_mongo_database_name</code></td><td><code>elements</code></td><td>No</td><td>Name of the MongoDB database used for Elements data storage.</td></tr><tr><td><code>dev_getelements_elements_mongo_tls_format</code></td><td>PKCS12</td><td>No</td><td>KeyStore format for the CA and client certificate files.</td></tr><tr><td><code>dev_getelements_elements_mongo_tls_ca</code></td><td><em>(blank)</em></td><td>No</td><td>Absolute path to the CA KeyStore file (<code>.p12</code>). May be blank if TLS is disabled.</td></tr><tr><td><code>dev_getelements_elements_mongo_tls_ca_passphrase</code></td><td><em>(blank)</em></td><td>Yes</td><td>Passphrase for the CA KeyStore. If blank, <code>null</code> is passed to <code><a href="https://docs.oracle.com/en/java/javase/21/docs//api/java.base/java/security/KeyStore.html#load(java.io.InputStream,char%5B%5D)">KeyStore.load()</a></code>.</td></tr><tr><td><code>dev_getelements_elements_mongo_tls_client_certificate</code></td><td><em>(blank)</em></td><td>Yes</td><td>Absolute path to the client certificate KeyStore file (<code>.p12</code>).</td></tr><tr><td><code>dev_getelements_elements_mongo_tls_client_certificate_passphrase</code></td><td><em>(blank)</em></td><td>Yes</td><td>Passphrase for the client certificate KeyStore. Used both to load the KeyStore and to initialize the <code>KeyManagerFactory</code>. An empty string and a null are not equivalent here.</td></tr><tr><td><code>dev_getelements_elements_mongo_tls_protocol</code></td><td>TLS</td><td>No</td><td>TLS/SSL protocol string passed to the SSL context.</td></tr><tr><td><code>dev_getelements_elements_mongo_tls_trust_algorithm</code></td><td><em>System Default</em></td><td>No</td><td><code>TrustManagerFactory</code> algorithm. Defaults to <code><a href="https://docs.oracle.com/en/java/javase/21/docs//api/java.base/javax/net/ssl/TrustManagerFactory.html#getDefaultAlgorithm()">TrustManagerFactory.getDefaultAlgorithm()</a></code>.</td></tr><tr><td><code>dev_getelements_elements_mongo_tls_key_algorithm</code></td><td><em>System Default</em></td><td>No</td><td><code>KeyManagerFactory</code> algorithm. Defaults to <code><a href="https://docs.oracle.com/en/java/javase/21/docs/api/java.base/javax/net/ssl/KeyManagerFactory.html#getDefaultAlgorithm()">KeyManagerFactory.getDefaultAlgorithm()</a></code>.</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-connection-uri-format">Connection URI Format</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>When used in Kubernetes or with SRV records, this is the example format of the connection URI. The actual DNS record may vary depending on your specific setup (for example Route53).</p>
<!-- /wp:paragraph -->

<!-- wp:betterdocs/code-snippet {"blockId":"betterdocs-code-snippet-68e67ca9","blockMeta":{"desktop":" .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-68e67ca9 { border-width: 0px !important; border-radius: 0px !important; } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-68e67ca9 .betterdocs-code-snippet-header.betterdocs-file-preview-header { border-bottom-width: 1px !important; border-bottom-style: solid !important; } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-68e67ca9 .betterdocs-code-snippet-header .betterdocs-file-name .file-name-text { font-size: 14px; } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-68e67ca9 .betterdocs-code-snippet-header .betterdocs-code-snippet-copy-button { } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-68e67ca9 .betterdocs-code-snippet-content .betterdocs-code-snippet-line-numbers { border-right-width: 1px !important; border-right-style: solid !important; } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-68e67ca9 .betterdocs-code-snippet-content .betterdocs-code-snippet-line-numbers .line-number { } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-68e67ca9 .betterdocs-code-snippet-code { } ","tab":" ","mobile":" "},"codeContent":"mongodb+srv://\u003chost\u003e.\u003cnamespace\u003e.svc.cluster.local/?replicaSet=\u003creplica-set-name\u003e\u0026tls=true","language":"java","showHeader":false} /-->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"Usage Notes","noticeTitleColor":"#000000"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="3b7b54"><div class="gb-notice-title" style="color:#000000"><p>Usage Notes</p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p>The <code>tls=true</code> parameter in the URI is what activates TLS in Elements. The Java code checks <code>ConnectionString.getSslEnabled()</code> and skips all certificate loading if it is not set. The host should resolve to all replica set members. When not using SRV for replica set resolution, use a standard <code>mongodb://</code> URI with the appropriate hostnames.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Additionally, MongoDB has some idiosyncratic expectations with the format of the connection string and hostname. Specifically, the host is expected to be <code>_mongodb._tcp</code>. Pay careful attention to the hostname format and refer to the <a href="https://mongodb.github.io/mongo-java-driver/4.2/apidocs/mongodb-driver-core/com/mongodb/ConnectionString.html">MongoDB Driver Documentation</a> if you get stuck.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-source-references">Source References</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>For deeper implementation detail:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>Certificate loading logic:</strong> <code><a href="https://github.com/NamazuStudios/elements/blob/dd26cebd465f60dc278140ec10e7fa6cb0a915fd/sdk-mongo/src/main/java/dev/getelements/elements/sdk/mongo/StandardMongoConfigurationService.java#L19">sdk-mongo/src/main/java/dev/getelements/elements/sdk/mongo/StandardMongoConfigurationService.java</a></code></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Integration tests:</strong> <code><a href="https://github.com/NamazuStudios/elements/tree/dd26cebd465f60dc278140ec10e7fa6cb0a915fd/sdk-mongo-test/src/test/java/dev/getelements/elements/sdk/mongo/test">sdk-mongo-test/src/test/java/dev/getelements/elements/sdk/mongo/test/</a></code>. These tests stand up a live replica set, configure it with test certificates, and exercise the full connection path. They serve as a working reference implementation.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-other-references">Other References</h2>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><a href="https://neilmadden.blog/2017/11/17/java-keystores-the-gory-details/">Java KeyStores - the gory details</a>. Shoutout to <a href="https://neilmadden.blog/about/">Neil Maden</a> for a great article on understanding the technical nuances of the Java KeyStore.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="https://www.reddit.com/r/sysadmin/comments/1cwvhdj/whats_the_most_annoying_thing_you_deal_with_and/">Reddit</a> if you just need to vent. We get it.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->
