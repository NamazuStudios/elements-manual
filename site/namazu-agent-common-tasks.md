<h1>Common Agent Tasks</h1>

<!-- wp:paragraph -->
<p>This page walks through the everyday workflow of working with the <a href="namazu-agent">Namazu Agent</a>: starting a session in Namazu Cloud, connecting it to GitHub, setting up your workspace, previewing changes on a temporary instance, and finally moving your game to production. It is the practical companion to the overview, which explains what the agent is and why you would use one.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"anchor":"h-the-development-workflow-at-a-glance"} -->
<h2 id="h-the-development-workflow-at-a-glance" class="wp-block-heading">The Development Workflow at a Glance</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>You and the agent work in a loop. You do the parts that need taste and judgment, the agent does the parts that need code slinging, and GitHub is the handoff point between you.</p>
<!-- /wp:paragraph -->

<p><!-- TODO(docs): render source at images/namazu-agent-workflow.mmd, upload the SVG to the WP media library, then replace the src below with the hosted URL --></p>
<figure class="wp-block-image"><img src="images/namazu-agent-workflow.svg" alt="Namazu Agent development workflow"/></figure>

<!-- wp:list {"ordered":true} -->
<ol class="wp-block-list"><!-- wp:list-item -->
<li>Start a session in Namazu Cloud</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Authenticate with GitHub</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Have the agent set up the workspace so it can push and pull your game's code from GitHub</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Ask it to set up a temporary instance of Namazu Elements, a copy of your main production instance, to preview against</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Iterate: the agent pushes code changes to GitHub for you, you pull them locally, add art, review, test, and adjust, then push back</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>When you are satisfied, ask the agent to move your game to production</li>
<!-- /wp:list-item --></ol>
<!-- /wp:list -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"anchor":"h-starting-a-session"} -->
<h2 id="h-starting-a-session" class="wp-block-heading">Starting a Session</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Begin from the Namazu Cloud control panel by launching an agent session. Under the hood this dispatches a job via <a href="namazu-conductor">Namazu Conductor</a>, which starts the agent's execution environment in its own workspace and wires up the terminal you interact with in the dashboard. You do not need to provision anything yourself; starting the session is all it takes.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3,"anchor":"h-first-run-onboarding"} -->
<h3 id="h-first-run-onboarding" class="wp-block-heading">First-Run Onboarding</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>On its very first run in a fresh workspace, the agent walks you through a short onboarding flow instead of dropping you into a blank prompt. Expect a handful of questions, such as whether you are starting a new or existing project, which game engine or client framework you're using, and where the project's source lives. Answer them, and the agent sets up accordingly.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3,"anchor":"h-authenticate-with-github"} -->
<h3 id="h-authenticate-with-github" class="wp-block-heading">Authenticate with GitHub</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The agent pushes and pulls code for you, so it needs access to your GitHub account. It runs <code>gh auth login</code> as part of startup, and you complete the sign-in that it prompts you for, usually via a device code or a link to open. This is where the agent gets permission to clone your repos, commit, push, and open pull requests on your behalf. Because the running container is disposable (see Session Persistence below), you may need to sign in again at the start of a fresh session.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3,"anchor":"h-set-up-your-workspace"} -->
<h3 id="h-set-up-your-workspace" class="wp-block-heading">Set Up Your Workspace</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Ask the agent to pull your game's code into the workspace, or to scaffold a new project, and it sets up the whole structure: the server-side Elements project, the client or game code, and the reference material it bases its work on. From then on the workspace is the agent's working copy, with GitHub as the bridge to your own machine.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"anchor":"h-temporary-preview-instance"} -->
<h2 id="h-temporary-preview-instance" class="wp-block-heading">Temporary Preview Instance</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Before anything touches a real deployment, ask the agent to set up a temporary instance of Namazu Elements: a disposable preview running its own copy of your project's backend and database, separate from your main production instance. It gets its own externally browsable URL, so you and the agent can verify changes live without any risk to production. When you are done with it, the agent tears it down.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"anchor":"h-working-with-the-files-in-your-workspace"} -->
<h2 id="h-working-with-the-files-in-your-workspace" class="wp-block-heading">Working With the Files in Your Workspace</h2>
<!-- /wp:heading -->

<!-- wp:heading {"level":3,"anchor":"h-see-and-edit-the-workspace-files-in-your-browser"} -->
<h3 id="h-see-and-edit-the-workspace-files-in-your-browser" class="wp-block-heading">See and Edit the Workspace Files in Your Browser</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>You do not need a terminal to look at what the agent is working on. Every session runs a private web file browser over its own workspace, and the agent can hand you the keys in a few seconds. Just ask:</p>
<!-- /wp:paragraph -->

<!-- wp:quote -->
<blockquote class="wp-block-quote"><!-- wp:paragraph -->
<p>Can I see the files in my workspace?</p>
<!-- /wp:paragraph --></blockquote>
<!-- /wp:quote -->

<!-- wp:paragraph -->
<p>The agent replies with a link and a short-lived login token, usually putting the token on your clipboard along with a notification toast so it is ready to paste. Open the link; when the browser shows its login prompt, enter <strong>user</strong> as the username and paste the token as the password. Your own browser remembers the login for that site for the rest of the session, so it is a one-time paste.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>From there you can:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Browse the workspace and preview images, text, and code files, with search by name</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Edit code and text files right in the browser and save back to the workspace</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Upload files, like art or a config file you want the agent to work from, and download anything the agent has produced</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>Edits you make there are the same files the agent sees, so a quick typo fix or a dropped-in asset is immediately part of its world. Two guardrails are worth knowing: the agent's own credential files are hidden from the browser entirely, and the link only works with a token the agent minted for this session, so it cannot be shared or reused later.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3,"anchor":"h-asking-what-is-running"} -->
<h3 id="h-asking-what-is-running" class="wp-block-heading">Asking What Is Running</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>To get your bearings in a session, ask what is running:</p>
<!-- /wp:paragraph -->

<!-- wp:quote -->
<blockquote class="wp-block-quote"><!-- wp:paragraph -->
<p>What services are running in my agent pod, and how do I reach them?</p>
<!-- /wp:paragraph --></blockquote>
<!-- /wp:quote -->

<!-- wp:paragraph -->
<p>The agent reports each service by name, its externally reachable URL (when one has been reserved for the session), and how to authenticate. It is the same list that powers the walkthroughs on this page, so when in doubt, ask and the agent will point you at the right one.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3,"anchor":"h-using-the-local-model-from-your-own-tools"} -->
<h3 id="h-using-the-local-model-from-your-own-tools" class="wp-block-heading">Using the Local Model From Your Own Tools</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>On profiles that bundle a local model, that model is not locked away inside the agent. Ask the agent for access to it and it hands you a link and a token, the same exchange as the file browser above. Your tool then authenticates with the token: as an HTTP header for API clients, or as the password (username <strong>user</strong>) for anything that speaks standard login, like curl or an OpenAI-compatible client configured with a custom base URL.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>The token authorizes exactly one service for a short window you can ask the agent to size, so request the shortest one that covers what you are doing. If you paste a token back to the agent expecting it to use it, it will usually skip the token entirely and just use its own local access instead.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"anchor":"h-the-core-iteration-loop"} -->
<h2 id="h-the-core-iteration-loop" class="wp-block-heading">The Core Iteration Loop</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Day to day, you and the agent move in a loop that keeps the actual game-making on your machine:</p>
<!-- /wp:paragraph -->

<!-- wp:list {"ordered":true} -->
<ol class="wp-block-list"><!-- wp:list-item -->
<li>The agent writes or changes code and pushes it to GitHub for you</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>You pull that branch on your local machine</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>You add your art, review the code, test it locally, and make adjustments</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>You push your changes back to GitHub</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>The agent picks up the changes and continues</li>
<!-- /wp:list-item --></ol>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>You never hand a zip over a chat window, and the agent never works on a different copy than you do. GitHub is the single shared source of truth between you.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3,"anchor":"h-moving-to-production"} -->
<h3 id="h-moving-to-production" class="wp-block-heading">Moving to Production</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>When you are satisfied with a change, ask the agent to move your game to production. The agent deploys the verified work to your real, running instance as a live upgrade. Confirm that is what you want explicitly for that specific change; the agent treats a deploy to production as a one-way door and only does it on your clear go-ahead, not on a general okay from earlier in the conversation.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"anchor":"h-session-persistence"} -->
<h2 id="h-session-persistence" class="wp-block-heading">Session Persistence</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Two very different things survive between sessions:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>The project workspace persists.</strong> The workspace that holds your repos and working files is kept across separate sessions, so you do not have to rebuild your project each time you start a session.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>The running container is fresh every time.</strong> Each session runs in a new, disposable execution environment. Anything that lives only inside that container, such as a logged-in state stored in the container's own filesystem, does not survive between sessions. GitHub and other provider sign-ins are the practical case: you will usually sign in again at the start of each session rather than being silently carried over.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"anchor":"h-idle-timeout-and-notifications"} -->
<h2 id="h-idle-timeout-and-notifications" class="wp-block-heading">Idle Timeout and Notifications</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>A session that is dispatched but not actively used winds itself down automatically after a period of inactivity, so leaving a tab open does not leave the meter running. You can also stop a session manually from the control panel at any time. Each session is a per-job compute cost, not a standing service, so only start one when you actually need it.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>While a session is running, the agent does not just print text and hope you are watching. When it finishes a long task, gets blocked on a decision only you can make, or needs to hand over a link or a credential, it surfaces a toast notification in the dashboard to get your attention. Keep an eye on those toasts; they are how the agent asks you to weigh in between its own turns.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"anchor":"h-related-pages"} -->
<h2 id="h-related-pages" class="wp-block-heading">Related Pages</h2>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><a href="namazu-agent">Namazu Agent</a>: what the agent is, the agent variants, model options, and how sessions run</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="namazu-conductor">Namazu Conductor</a>: the orchestration engine that dispatches and manages agent sessions</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="deploying-an-element">Deploying an Element</a>: what a deployable Element looks like when you promote work to production</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->
