<h1>Ubuntu Linux Setup</h1>

<!-- wp:paragraph -->
<p>This guide will walk you through setting up the Elements Local SDK on an Ubuntu-based system. You’ll install the required dependencies manually or use the provided setup script for convenience.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-manual-step-by-step-installation">Manual Step-by-Step Installation</h2>
<!-- /wp:heading -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-1-install-openjdk">1. Install OpenJDK</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Install OpenJDK 21(or later):</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>sudo apt update
sudo apt install openjdk-21-jdk -y</code></pre>
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

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-2-install-maven">2. Install Maven</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Install Maven using APT:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>sudo apt install maven -y</code></pre>
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
<h3 class="wp-block-heading" id="h-3-install-git-if-not-already-installed">3. Install Git (if not already installed)</h3>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code>sudo apt install git -y</code></pre>
<!-- /wp:code -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-4-install-intellij-idea-optional-but-recommended">4. Install IntelliJ IDEA (Optional but Recommended)</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>You can install IntelliJ IDEA via Snap:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>sudo snap install intellij-idea-community --classic</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>Or download from:<br><a href="https://www.jetbrains.com/idea/download/">https://www.jetbrains.com/idea/download/</a></p>
<!-- /wp:paragraph -->

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
<p>To install everything in one shot, run the following in a terminal:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>sudo apt update
sudo apt install openjdk-21-jdk maven git -y</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>To install IntelliJ (optional):</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>sudo snap install intellij-idea-community --classic</code></pre>
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
