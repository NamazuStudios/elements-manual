<h1>User Workspaces</h1>

<!-- wp:paragraph -->
<p>A <strong>user workspace</strong> is your own private, sandboxed environment inside your organization's Namazu Cloud cluster. It is provisioned automatically: when you create an organization you become its owner and a workspace is set up for you; when you accept an invitation and join an organization, a workspace is set up for you there too. When you leave an organization, or the organization is deleted, the workspace is torn down with it. If you later rejoin, you get a fresh one.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Concretely, a workspace is a dedicated <em>namespace</em> in the cluster, named after an opaque identifier unique to you and that organization. Inside it live the AI coding agent catalog (OpenCode, Codex, Claude profiles, as configured by your organization), the compute capacity those agents run on, and anything the agents create while working for you: files, terminals, temporary workloads, and throwaway test deployments.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-what-sandboxed-means">What "Sandboxed" Means</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The agent does not run on your laptop, and it does not run loose in the cluster. It runs inside your workspace, and the impact of everything it does is limited to the resources inside that workspace:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>Namespace isolation.</strong> Kubernetes namespaces are hard boundaries. The agent's service account is scoped to your workspace's namespace and is structurally unable to read or write anything in any other namespace, no matter what it (or a prompt injection, or a buggy script) asks it to do. Other tenants' workspaces and workloads are invisible to it.</li>
<!-- /wp:list-item -->
<!-- wp:list-item -->
<li><strong>Node isolation.</strong> Workloads in your workspace are pinned to the nodes dedicated to your workspace. No other tenant's workload can land on your nodes, and yours cannot land on theirs.</li>
<!-- /wp:list-item -->
<!-- wp:list-item -->
<li><strong>No production reach.</strong> The workspace has no wiring into your organization's production instances: no shared database, no shared storage, no DNS. An agent working in your workspace cannot modify a production instance's data or configuration, because from inside the workspace those things do not exist.</li>
<!-- /wp:list-item -->
<!-- wp:list-item -->
<li><strong>Disposable by design.</strong> Everything in the workspace is replaceable. Destroying the workspace destroys its contents and its cost; a fresh one is provisioned on the next join. Nothing in it is expected to outlive your membership.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>The same containment applies to cost and auditing. All compute the workspace uses is labeled to you and your organization, so agent-driven usage rolls up to the right org's bill and can be attributed to you within it.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-why-a-controlled-agent-environment">Why a Controlled Agent Environment</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Coding agents are powerful and mostly reliable, and occasionally they do exactly what they were told and it was the wrong thing to have asked. A controlled environment is how you get the productivity of letting an agent act autonomously without betting production on its judgment:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>Bounded blast radius.</strong> A destructive command, a bad migration, a runaway loop that fills a disk: the worst outcome is contained in a disposable sandbox you can destroy and re-create in minutes. Nothing outside the workspace can be affected by it.</li>
<!-- /wp:list-item -->
<!-- wp:list-item -->
<li><strong>Least privilege by default.</strong> The agent never needs your production credentials. It never sees your organization's production instance configuration, subdomains, or data. Every capability it has is scoped to the sandbox, which means there is nothing to leak.</li>
<!-- /wp:list-item -->
<!-- wp:list-item -->
<li><strong>Honest testing.</strong> Because the workspace looks and behaves like the real platform (the same agent catalog, the same mechanisms, real compute and DNS), what you validate there is meaningful, without any of it touching shared infrastructure.</li>
<!-- /wp:list-item -->
<!-- wp:list-item -->
<li><strong>Parallel and personal.</strong> Every member gets their own workspace. Your experiments, your agent sessions, and your half-finished work never contend with anyone else's, and nobody else's experiments can break yours.</li>
<!-- /wp:list-item -->
<!-- wp:list-item -->
<li><strong>Attribution.</strong> Usage is labeled per user, so organizations can see what agentic work actually costs, per person and per workspace, instead of it blurring into a shared bill.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-best-practice-develop-and-test-in-your-workspace">Best Practice: Develop and Test in Your Workspace</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Your organization's production instances are shared, long-lived, and serve real workloads. Your workspace is private, disposable, and serves exactly one person: you. Treat them accordingly.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Use the workspace for everything experimental: letting an agent refactor or explore a codebase, running schema migrations against a throwaway database, deploying experimental builds, previewing changes, trying a new agent profile, or just asking "what happens if". The workspace's agent can spin up <em>temporary instances</em>: short-lived, agent-managed deployments (including an Elements stack with its own database) that exist only inside the sandbox and vanish when you are done with them.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Production is for the things that have earned it: the reviewed, tested, intended-to-stay work. Deploy to a production instance only after the change has already run in a workspace (or a test environment) and been reviewed. This keeps production stable, keeps shared costs predictable, and keeps agent-driven work somewhere it cannot hurt anything.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>It is also simply faster to be wrong in a workspace. There is no ceremony in throwing one away and starting over, so the cost of an experiment is the time it took, not a recovery project.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-lifecycle-reference">Lifecycle Reference</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>You can see and manage your workspaces under <strong>Workspace</strong> in the Namazu Cloud portal, one per organization. Once a workspace is ready, its namespace also appears in the <strong>Agents</strong> drawer under <em>My Workspace</em>, alongside your organizations' production instances under <em>Production Workspace</em>. Agent jobs available in a workspace are the catalog profiles deployed into it. See <a href="namazu-conductor">Namazu Conductor</a> for how agent jobs run.</p>
<!-- /wp:paragraph -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th>State</th><th>Meaning</th></tr></thead><tbody>
<tr><td><code>CREATE_PENDING</code></td><td>Deployment was requested; a job is queued but has not reported starting yet.</td></tr>
<tr><td><code>CREATING</code></td><td>The deploy job is actively running (Terraform apply in flight).</td></tr>
<tr><td><code>READY</code></td><td>The workspace is deployed and usable.</td></tr>
<tr><td><code>REFRESH_PENDING</code> / <code>REFRESHING</code></td><td>A refresh (re-running the workspace's Terraform to pick up catalog or configuration updates) was requested, then is actively running.</td></tr>
<tr><td><code>DESTROY_PENDING</code> / <code>DESTROYING</code></td><td>Teardown was requested (by you, by an owner, or automatically on leaving), then is actively running.</td></tr>
<tr><td><code>DESTROYED</code></td><td>Teardown finished. The workspace is gone; joining again provisions a fresh one.</td></tr>
<tr><td><code>ERROR_CREATING</code> / <code>ERROR_REFRESHING</code> / <code>ERROR_DESTROYING</code></td><td>The operation failed. Retry from the Workspace page; the page also carries a per-workspace history showing every attempted operation, when, and its outcome. If failures persist, the page points you at support.</td></tr>
</tbody></table></figure>
<!-- /wp:table -->

<!-- wp:paragraph -->
<p>Every operation on a workspace is recorded in its history, including what was attempted, when, by whom (you, an organization owner, or the system), and the outcome. Destructive actions always ask for confirmation first, and the automatic provisioning of workspaces on organization events can be disabled for a deployment, in which case members deploy their own workspaces from the Workspace page.</p>
<!-- /wp:paragraph -->
