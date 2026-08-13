<h1>Windows Setup</h1>

<!-- wp:paragraph -->
<p>This guide walks you through installing the required tools to run the Elements Local SDK on a Windows machine.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>You can either follow the&nbsp;<strong>manual step-by-step instructions</strong>&nbsp;to understand each component being installed, or use the&nbsp;<strong>full setup script</strong>&nbsp;at the bottom for a one-shot install.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-manual-step-by-step-installation">Manual Step-by-Step Installation</h2>
<!-- /wp:heading -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-1-install-chocolatey">1. Install Chocolatey</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p><a href="https://chocolatey.org/install" target="_blank" rel="noreferrer noopener nofollow">Chocolatey</a> is a Windows package manager used to install other dependencies easily.</p>
<!-- /wp:paragraph -->

<!-- wp:list {"ordered":true} -->
<ol class="wp-block-list"><!-- wp:list-item -->
<li>Open&nbsp;<strong>PowerShell as Administrator</strong></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Run the following command:<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><code><code>Set-ExecutionPolicy Bypass -Scope Process -Force [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072 iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))</code></code></li>
<!-- /wp:list-item --></ul>
<!-- /wp:list --></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Close and reopen PowerShell after installation.</li>
<!-- /wp:list-item --></ol>
<!-- /wp:list -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-2-install-openjdk">2. Install OpenJDK</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Install OpenJDK using Chocolatey:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>choco install openjdk -y</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>Verify the installation:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>java -version</code></pre>
<!-- /wp:code -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-3-install-maven">3. Install Maven</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Install Maven using Chocolatey:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>choco install maven -y</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>Verify the installation:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>mvn -version</code></pre>
<!-- /wp:code -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-4-install-intellij-idea">4. Install IntelliJ IDEA</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Download and install IntelliJ IDEA from:</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><a href="https://www.jetbrains.com/idea/download/">https://www.jetbrains.com/idea/download/</a></p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Community Edition is sufficient</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Ensure Java and Maven plugins are enabled (usually enabled by default)</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-5-clone-and-import-the-example-project">5. Clone and Import the Example Project</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Clone the Elements example project:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>git clone https://github.com/NamazuStudios/element-example.git</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>Then:</p>
<!-- /wp:paragraph -->

<!-- wp:list {"ordered":true} -->
<ol class="wp-block-list"><!-- wp:list-item -->
<li>Open IntelliJ IDEA</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Click&nbsp;<strong>Open</strong>&nbsp;and select the&nbsp;<code>element-example</code>&nbsp;directory</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>If prompted, import as a Maven project</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>If not prompted, right-click&nbsp;<code>pom.xml</code>&nbsp;and select&nbsp;<strong>Add as Maven Project</strong></li>
<!-- /wp:list-item --></ol>
<!-- /wp:list -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-6-run-the-local-sdk">6. Run the Local SDK</h3>
<!-- /wp:heading -->

<!-- wp:list {"ordered":true} -->
<ol class="wp-block-list"><!-- wp:list-item -->
<li>In IntelliJ, navigate to the&nbsp;<code>dev.getelements.sdk.local</code>&nbsp;package</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Locate and run the&nbsp;<code>LocalSdkMain</code>&nbsp;class (or the appropriate main class)</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>The local server should boot up and be accessible for testing</li>
<!-- /wp:list-item --></ol>
<!-- /wp:list -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-full-setup-script-for-convenience">Full Setup Script (For Convenience)</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>To install everything in one shot, open&nbsp;<strong>PowerShell as Administrator</strong>&nbsp;and paste the following:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code># Set execution policy and install Chocolatey
Set-ExecutionPolicy Bypass -Scope Process -Force
&#91;System.Net.ServicePointManager]::SecurityProtocol = &#91;System.Net.ServicePointManager]::SecurityProtocol -bor 3072
iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))

# Refresh environment
refreshenv

# Install OpenJDK and Maven
choco install openjdk -y
choco install maven -y

# Verify installations
java -version
mvn -version
</code></pre>
<!-- /wp:code -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-you-re-ready">You’re Ready</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Once you’ve completed the setup:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>You can build and run the Elements example project locally</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>You’re now ready to begin backend development using the Elements SDK</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->
