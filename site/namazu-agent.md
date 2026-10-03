<h1>Namazu Agent Overview</h1>

<!-- wp:paragraph -->
<p>The Namazu Agent is an AI coding agent that runs inside Namazu Cloud as a managed, on-demand job. It is purpose-built to help build and extend Namazu Elements projects, whether that work is server-side code, dashboard UI plugins, static web content, or the front-end game/client code that talks to them. Start a session from the Namazu Cloud control panel, and the agent gets to work against your project in its own workspace.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":22863,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://namazustudios.com/wp-content/uploads/2026/09/image-6-1024x531.png" alt="" class="wp-image-22863"/></figure>
<!-- /wp:image -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"anchor":"h-what-the-agent-does-for-you"} -->
<h2 id="h-what-the-agent-does-for-you" class="wp-block-heading">What the Agent Does for You</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The agent is a pair programmer, a deployment assistant, and an instance administrator in one. It can write code, commit and push changes, open pull requests, spin up disposable preview instances, and operate your running copy of Namazu Elements.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3,"anchor":"h-code-and-administer-in-one-place"} -->
<h3 id="h-code-and-administer-in-one-place" class="wp-block-heading">Code and Administer in One Place</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Because it runs within Namazu Cloud, the agent does more than edit files. With access to your running project it can administer your live instance, manage deployments, talk to the Elements REST API, and make changes to your game end to end without you touching a console.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3,"anchor":"h-diagnose-and-troubleshoot"} -->
<h3 id="h-diagnose-and-troubleshoot" class="wp-block-heading">Diagnose and Troubleshoot</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The same session that writes your code can diagnose live issues. It reads real logs, queries the actual databases and services, and works against your instance's own API to figure out what went wrong and fix it, rather than guessing from a stack trace alone.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3,"anchor":"h-temporary-preview-instance"} -->
<h3 id="h-temporary-preview-instance" class="wp-block-heading">Temporary Preview Instance</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>On demand, the agent can spin up (and tear down) its own temporary Namazu Elements + MongoDB instance, a disposable copy of your main setup for previewing changes safely. Anything you break there is broken only in the sandbox, never in your real deployment. See <a href="namazu-agent-common-tasks">Common Agent Tasks</a> for the full workflow.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3,"anchor":"h-services-that-come-with-your-agent"} -->
<h3 id="h-services-that-come-with-your-agent" class="wp-block-heading">Services That Come With Your Agent</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>A session is more than a terminal. The agent's pod runs a small set of purpose-built services alongside it, and every one of them is something you can ask the agent about by name:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>A workspace file browser.</strong> A private web file browser serves the agent's workspace, so you can browse, view, edit, upload, and download its working files (the checked-out code, generated assets, anything it is building) directly from your own browser, no terminal involved. When you ask for it, the agent hands you the link and a login token; the agent's own credential files are deliberately hidden from it.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>A local LLM.</strong> On profiles that bundle one (see Model Options below), the model runs in the same pod as the agent. It powers the agent itself, and on request the agent can also hand you a token so your own tools can call it.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Built-in automation tools.</strong> The agent carries its own MCP toolset for its operational tasks: starting, checking, and tearing down the temporary preview instance; reserving and releasing the on-demand subdomains that give your previews and services real, shareable URLs; and logging in to the Namazu Cloud control plane when a task needs it. These are the same tasks the agent performs for you conversationally; the tools are how it does them reliably.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>External access to these services is deliberately gated. They are all reached through one secure proxy that routes by hostname, and each one requires a short-lived login token that only the agent can mint for you. Nothing is reachable without a token, and every token dies with the session, so there is nothing long-lived to leak. See <a href="namazu-agent-common-tasks">Common Agent Tasks</a> for walkthroughs of asking the agent for each of these.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"anchor":"h-bring-your-own-agent"} -->
<h2 id="h-bring-your-own-agent" class="wp-block-heading">Bring Your Own Agent</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The Namazu Agent is not tied to a single vendor. It is built on top of three leading coding agents, selectable when you deploy, all working with the same custom configuration and MCP tools built into the execution environment:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>opencode</strong> (the default)</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>OpenAI Codex</strong> (the <code>codex</code> CLI)</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Anthropic Claude Code</strong> (the <code>claude</code> CLI)</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>Whichever you choose, the agent keeps the same workflow: an onboarding-aware workspace, a project clone from GitHub, a temporary preview instance, and the ability to push, pull, and open pull requests on your behalf.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3,"anchor":"h-model-options"} -->
<h3 id="h-model-options" class="wp-block-heading">Model Options</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Two ways to power an agent session, depending on the work and your budget:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>Bundled local/hosted models on GPU instances.</strong> A local model (for example <strong>Qwen 3.8B</strong>) runs on the GPU instance that hosts your session, with no API key or outside account needed. It is typically the best value for budget-friendly work, troubleshooting, and instance administration.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Your own API key or account.</strong> For deep and complex coding tasks, use the official opencode, Codex, or Claude Code tooling with your own API key. You can log in to your existing accounts through the agent, or supply an API key directly.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"anchor":"h-how-sessions-run"} -->
<h2 id="h-how-sessions-run" class="wp-block-heading">How Sessions Run</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Each agent session runs as a job dispatched and managed by <a href="namazu-conductor">Namazu Conductor</a>, the Elements orchestration engine. Conductor launches the session, gives it its workspace, and tracks its state. It is also how the session is visible from the Namazu Cloud control panel, where you can watch and stop it at any time (see the <a href="namazu-conductor-admin-api">Namazu Conductor Admin API</a> for the underlying job model).</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3,"anchor":"h-cost-and-lifecycle"} -->
<h3 id="h-cost-and-lifecycle" class="wp-block-heading">Cost and Lifecycle</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Agent sessions are not always on. Each one is a per-job compute cost, not a standing service, and an idle session winds itself down automatically after a period of inactivity, so leaving a session's tab open does not leave the meter running. You can also stop a session manually from the control panel whenever you are done with it.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"anchor":"h-related-pages"} -->
<h2 id="h-related-pages" class="wp-block-heading">Related Pages</h2>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><a href="namazu-agent-common-tasks">Common Agent Tasks</a>: the end-to-end workflow: launching a session, GitHub auth, workspace setup, a temporary preview instance, and the development loop through to production</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="namazu-conductor">Namazu Conductor</a>: the orchestration engine that dispatches and manages agent sessions</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="namazu-conductor-admin-api">Namazu Conductor Admin API</a>: the job model behind session state and control-panel visibility</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="deploying-an-element">Deploying an Element</a>: what a finished, deployable Element looks like</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->
