<h1>Common Issues with Docker</h1>

<!-- wp:paragraph -->
<p>Running Namazu Elements locally is best done in Docker. We provide <a href="https://github.com/NamazuStudios/docker-compose/">Community Edition</a> as a pre-configured environment using <a href="https://docs.docker.com/compose/">Docker Compose</a>. If you are running into trouble using Docker, this page has a few common problems and solutions.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-not-in-the-correct-working-directory">Not in the Correct Working Directory</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Docker relies on the current working directory for almost all operations. Typically you will see errors about missing configuration if you do not <code>cd</code> to the correct directory first.</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>ptwohig@ryzen:~$ docker compose up -d
no configuration file provided: not found</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>If you are using <a href="https://desktop.github.com/download/">Github Desktop</a>, then look for your project in these directories:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Windows - <code>C:\Users\&lt;your-username>\Documents\GitHub</code></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Mac OS - <code>~/Documents/GitHub</code></li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>Refer to the usage for your specific git client when locating the directory on disk. Before running docker, ensure you are in the correct directory.</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>cd<code>C:\Users\username\&lt;your-user\GitHub\docker-compose</code></code></pre>
<!-- /wp:code -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-docker-service-not-running">Docker Service Not Running</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Not all operating systems immediately run Docker or the Docker service can be shut down indvertently. Typically errors like this will appear:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?</code></pre>
<!-- /wp:code -->

<!-- wp:code -->
<pre class="wp-block-code"><code>Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>To fix this start Docker using the instructions for your operating system.</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Windows  <!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>Desktop App:</strong> Search for "Docker Desktop" in the <strong>Start menu</strong> and launch it.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>CLI:</strong> Use the <a href="https://docs.docker.com/desktop/features/desktop-cli/" target="_blank" rel="noreferrer noopener">Docker Desktop CLI</a> by running <code>docker desktop start</code> in PowerShell or Command Prompt (requires Docker Desktop 4.37+).</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Auto-start:</strong> In settings, check <strong>"Start Docker Desktop when you log in"</strong> to have it run on boot. </li>
<!-- /wp:list-item --></ul>
<!-- /wp:list --></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Mac<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>Desktop App:</strong> Open <strong>Docker.app</strong> from your <strong>Applications</strong> folder or use Spotlight (<code>Cmd + Space</code>).</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>CLI:</strong> Run <code>open -a Docker</code> in the Terminal. For newer versions (4.37+), you can also use <code>docker desktop start</code>.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Headless/Alternative:</strong> If using Colima, use the command <code>colima start</code>. </li>
<!-- /wp:list-item --></ul>
<!-- /wp:list --></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Linux<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>Systemd (Standard Engine):</strong> Start the daemon with <code>sudo systemctl start docker</code>. To ensure it starts on every boot, use <code>sudo systemctl enable docker</code>.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list --></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Docker Desktop for Linux<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>GUI:</strong> Launch "Docker Desktop" from your application menu (Gnome/KDE).</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>CLI:</strong> Use <code>systemctl --user start docker-desktop</code> to start the desktop-managed engine.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list --></li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->
