<h1>Section Statuses and Locks</h1>

<!-- wp:paragraph -->
<p>Every section of a page carries a status recording where it stands in the design's life. The status is the notebook's coordination signal: it tells teammates what a section needs next, and it is the primary instruction your agents receive about how they may touch it.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="the-status-lifecycle">The status lifecycle</h2>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>Draft.</strong> The default state. Freely editable.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Design review.</strong> Requesting team review. Notes only; factual corrections excepted.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Greenlit.</strong> Approved to build. Tickets get filed and asset lists made; edits are clarify-only.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Development in progress.</strong> Art and code underway. The section stays in sync with what is actually built.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Development review.</strong> Development done, awaiting final signoff. Notes only.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Shipped.</strong> Finalized and in players' hands. Lock it when it is truly frozen; shipped features can still change.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>The outline and the buffer shade each section with its status color, so the state of a whole page reads at a glance, and the section panel's own chrome carries the selected section's stage color.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="setting-a-status">Setting a status</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Select a section and open the panel's <strong>Status</strong> tab, or right-click the section and choose <strong>Set status…</strong>. A status change is held as pending (the card shows the target with a pending marker) and flushes with the next save, exactly like notes, so the status and the design it describes move together. A remarks field records why.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Approving a design for building is deliberate: <strong>Greenlit</strong> (and <strong>Shipped</strong>) can only be set by an owner or admin of the organization.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="locks">Locks</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The lock is a separate toggle on a section's status, and it applies immediately rather than at save. Setting <strong>Shipped</strong> locks the section automatically; you can lock or unlock any section explicitly from its status card. A locked section refuses edits outright: typing, pasting, and deleting inside it no-op in the editor, and a save that would change a locked section is rejected in full, naming the sections that blocked it.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Unlocking happens in the portal only. An agent cannot unlock a section, by design: if a locked section genuinely needs to change, the agent asks you, with its rationale, and you unlock it in the panel.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>One deliberate detail: a locked section protects its heading through its last content line, leaving the blank row below it writable. That row is the sanctioned place for content that follows a locked section, so a lock never blocks work elsewhere on the page.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="what-agents-may-do-at-each-stage">What agents may do at each stage</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The stage carries default instructions your agent receives for any section in that state, unless a more specific instruction overrides it (a section's own override, then the page's, then the notebook's). In outline: draft sections are freely rewritten with a remark on save; sections under design review take concerns and open questions as notes, not edits; greenlit sections get tickets and asset entries, with clarify-only wording changes; sections in development stay in sync with deviations reported as notes; sections in development review take notes and open questions only; and shipped sections are not to be edited while locked, with the agent asking you to unlock when a change is truly needed.</p>
<!-- /wp:paragraph -->
