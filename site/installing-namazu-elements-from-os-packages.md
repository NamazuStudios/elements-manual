<h1>Installing Namazu Elements from OS Packages</h1>

<!-- wp:paragraph -->
<p>Elements ships as native OS packages that install the same Jetty-based server the Docker images run, as a systemd service under a dedicated <code>elements</code> user. The <code>.deb</code> covers Debian and Ubuntu hosts; the <code>.rpm</code> (architecture independent) covers Amazon Linux 2023 and the RHEL 9 family. Both packages install identical files, so everything on this page applies to either.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Each GitHub release of Elements attaches <code>namazu-elements_&lt;version&gt;_all.deb</code> and <code>namazu-elements-&lt;version&gt;.noarch.rpm</code> as release assets. Signed apt and yum repositories at packages.namazustudios.com are on the roadmap; until they arrive, install the release asset directly as shown below.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>If you prefer to run Elements in containers, see <a href="elements-in-five-minutes-or-less">Elements in Five Minutes or Less</a> instead.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-what-the-package-installs">What the package installs</h2>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><code>/usr/share/namazu-elements/</code> the application: <code>lib/</code> (the server jars) and <code>classpath/</code> (the packaged logging configuration)</li>
<!-- /wp:list-item --><!-- wp:list-item -->
<li><code>/usr/bin/namazu-elements</code> the launcher, also used for database migrations</li>
<!-- /wp:list-item --><!-- wp:list-item -->
<li><code>/etc/elements/elements.properties</code> your configuration file, created from the packaged example on first install and never touched by upgrades</li>
<!-- /wp:list-item --><!-- wp:list-item -->
<li><code>/etc/elements/classpath/</code> classpath overrides (for example, a custom logging configuration)</li>
<!-- /wp:list-item --><!-- wp:list-item -->
<li><code>/etc/default/namazu-elements</code> JVM options (<code>JAVA_OPTS</code>)</li>
<!-- /wp:list-item --><!-- wp:list-item -->
<li><code>/var/lib/namazu-elements/</code> state: <code>cdn-repos</code>, <code>script-repos</code>, <code>script-storage</code>, <code>tmp</code></li>
<!-- /wp:list-item --><!-- wp:list-item -->
<li><code>/var/log/namazu-elements/</code> logs, rotated by logrotate</li>
<!-- /wp:list-item --><!-- wp:list-item -->
<li><code>namazu-elements.service</code> the systemd unit, with the alias <code>elements.service</code>, so both <code>systemctl status namazu-elements</code> and <code>systemctl status elements</code> work</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-requirements">Requirements</h2>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>A Java 21 headless runtime. Both packages declare the dependency, so the package manager pulls the right one (<code>default-jre-headless</code> on Debian and Ubuntu, Amazon Corretto 21 on Amazon Linux).</li>
<!-- /wp:list-item --><!-- wp:list-item -->
<li>systemd.</li>
<!-- /wp:list-item --><!-- wp:list-item -->
<li>A MongoDB deployment the server can reach. The package contains only the Elements server; point it at your MongoDB with the configuration below. For TLS-enabled replica sets see <a href="connecting-namazu-elements-to-a-tls-enabled-mongodb-replica-set">Connecting Namazu Elements to a TLS-enabled MongoDB Replica Set</a>.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-installing-on-debian-and-ubuntu">Installing on Debian and Ubuntu</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Download the <code>.deb</code> from the release you want, then install it with apt (which resolves the Java dependency):</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>curl -fL -o namazu-elements.deb \
  https://github.com/NamazuStudios/elements/releases/download/&lt;version&gt;/namazu-elements_&lt;version&gt;-1_all.deb
sudo apt install ./namazu-elements.deb</code></pre>
<!-- /wp:code -->

<!-- wp:code -->
<pre class="wp-block-code"><code>systemctl status namazu-elements</code></pre>
<!-- /wp:code -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-installing-on-amazon-linux-and-rhel-family">Installing on Amazon Linux and RHEL family</h2>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code>curl -fL -o namazu-elements.rpm \
  https://github.com/NamazuStudios/elements/releases/download/&lt;version&gt;/namazu-elements-&lt;version&gt;-1.noarch.rpm
sudo dnf install ./namazu-elements.rpm</code></pre>
<!-- /wp:code -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-configuring-the-deployment">Configuring the deployment</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Elements resolves configuration from the environment and from <code>/etc/elements/elements.properties</code>, and values in that file take precedence over environment variables. Because of that, keep deployment specific values (the MongoDB URI, public URLs, bootstrap admin credentials) out of the properties file and supply them through a systemd drop-in instead. Underscore-form environment keys translate to the dotted property names, for example <code>dev_getelements_elements_mongo_uri</code> sets <code>dev.getelements.elements.mongo.uri</code>.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Create <code>/etc/systemd/system/namazu-elements.service.d/deployment.conf</code>:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>[Service]
Environment=dev_getelements_elements_mongo_uri=mongodb://127.0.0.1:27017/?replicaSet=local-test
Environment=dev_getelements_elements_api_url=https://elements.example.com/api/rest</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>The bootstrap administrator account is configured the same way with <code>dev_getelements_elements_user_default_name</code>, <code>dev_getelements_elements_user_default_email</code>, and <code>dev_getelements_elements_user_default_password</code>. Put credentials in their own drop-in file with restrictive permissions (<code>chmod 600</code>), since systemd reads drop-ins as root.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Apply the change:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>sudo systemctl daemon-reload
sudo systemctl restart namazu-elements</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>The full list of properties is covered in <a href="properties">Properties</a>, and public URL configuration in <a href="configuring-external-urls-for-deployment">Configuring External URLs for Deployment</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-running-database-migrations">Running database migrations</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The launcher exposes the same migration tool the Docker distribution ships, so an installed host can migrate without a container:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>sudo -u elements namazu-elements migrate --help</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>Use <code>--status</code> to list applied and pending migrations without applying anything:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>sudo -u elements namazu-elements migrate --status</code></pre>
<!-- /wp:code -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-upgrading-and-removing">Upgrading and removing</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Upgrades install the new jars and restart the service only if it was running. Your configuration and data survive: <code>/etc/elements</code> and <code>/var/lib/namazu-elements</code> are operator-owned and are never overwritten or removed by an upgrade. Install the newer release asset the same way as the original install and the package manager handles the rest.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Removing the package (<code>apt remove namazu-elements</code> or <code>dnf remove namazu-elements</code>) stops the service and removes the application, leaving configuration and data in place. Purging (<code>apt purge</code>) additionally removes <code>/etc/elements/elements.properties</code>, and removes the state and log directories only when they are empty, so data is never deleted silently.</p>
<!-- /wp:paragraph -->
