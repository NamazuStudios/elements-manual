<h1>Working with Agents</h1>

<!-- wp:paragraph -->
<p>The Design Notebook is built agent-first: your agents read and write the notebook through the same organization-scoped surface you use, guided by the written design and by instructions you control. This page covers what agents do with a notebook, how they are guided, and how you hand them context.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="what-your-agent-can-see-and-do">What your agent can see and do</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>An agent working your notebook can, within your organization's notebooks:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>list and search notebooks, and read their pages and files;</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>read any section, at its latest revision or from history;</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>save a page (full Markdown, parsed and diffed into sections, with pending notes and status changes flushed atomically);</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>leave notes, ask open questions, and reply to yours;</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>set section statuses (greenlit and shipped are owner- and admin-gated, enforced per call);</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>read the notebook's design files, manage the asset list, and record GitHub tickets;</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>Everything an agent does is organization-scoped: an agent acts with the authority of the account it authenticates as and sees only that account's organizations' notebooks.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="how-agents-are-guided">How agents are guided</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Every notebook, page, file, asset, ticket, and section can carry <strong>agent instructions</strong>: standing guidance edited from that item's details modal (the ⓘ in the tree). An agent resolves the most specific one that applies: a section's own override, then its page's, then its notebook's. The section panel's instructions view shows the effective guidance for the selected section.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Below the per-item chain sit the <strong>stage defaults</strong>: the standing instructions every agent receives for a section in each status. They encode the collaboration rules: rewrite drafts freely with a remark on save; record concerns and open questions as notes (questions as question notes) on sections under review; file tickets and asset entries for greenlit work and keep wording changes clarify-only; keep sections in development synchronized with the build and report deviations as notes; never edit a locked shipped section, and ask you to unlock it instead, with rationale.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>The standing directive across all of it: when the agent has an open question about the design, it asks, as a question note on the relevant section, rather than summarizing or burying the question in prose. Question notes surface in the page's Questions view where you answer them in place. See <a href="design-notebook-notes">Notes and question notes</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="sending-design-context-to-an-agent">Sending design context to an agent</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Two directions of handoff keep you and the agent talking about the same thing:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>You to the agent.</strong> From the editor's right-click menu, <strong>Send section to agent</strong> and <strong>Send selection to agent</strong> type a drift-free reference (the notebook, page, section identity, and heading path, plus your selected text) into an agent terminal. The reference is resolvable by the agent's own notebook tools, so no context is guessed.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>The agent to you.</strong> An agent can ask the portal to open a notebook, a specific page, or a specific section, landing you exactly where it wants your eyes, and it can leave notes and questions that the tree surfaces.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="agent-access">Agent access</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Agents authenticate to the notebook's MCP surface as a user of your organization: through your portal session (the embedded terminal and ADE flows do this for you), or with an MCP API key issued from the portal's MCP API Keys page, which carries its own optional expiry. A key is a credential: treat it like a password.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="what-agents-cannot-do">What agents cannot do</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The guardrails are deliberate. An agent cannot unlock a section (locks are portal-only), cannot set greenlit or shipped statuses (owner and admin only), cannot purge anything, and cannot archive notebooks, pages, or files. When an agent hits one of these walls it says so and asks you, which is the intended workflow: the design's approvals and destructive acts stay in human hands.</p>
<!-- /wp:paragraph -->
