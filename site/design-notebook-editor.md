<h1>Writing a Page: the GDD Editor</h1>

<!-- wp:paragraph -->
<p>Selecting a page opens the editor: a free-form Markdown buffer with an outline on the left, a rendered reading view, and a section panel on the right. The document is yours to write however you like; the notebook tracks the structure as you go.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="the-buffer-and-the-outline">The buffer and the outline</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The editor is a plain Markdown buffer. Heading lines are syntax-accented so structure stands out while you write. The outline beside it lists every section, follows your cursor (selecting a section in the outline, or clicking into the buffer, keeps both in sync), and shades each section with its status color. A locked section carries a lock marker.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>The outline marks unsaved structure as you edit: modified sections, moved or renamed ones (with an arrow showing where the content now lives), removed ones, and newly added ones. Saved moves and deletions hide behind the <strong>Show moved</strong> and <strong>Show deleted</strong> toggles so the outline stays readable; live edits always show.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="document-mode">Document mode</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Toggle <strong>Document</strong> to read the page as one rendered document: headings, prose, tables, and media all rendered. Click a heading to edit that section in place, or right-click it for the same section menu the outline offers. Design file links (see below) render as the actual image, video, or audio player in document mode, so a page with embedded reference art reads like a finished document.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Dialog blocks render screenplay-styled, and a section containing one gains a <strong>Cast</strong> tab in the panel listing its characters (with line counts) and scenes.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="the-right-click-menu">The right-click menu</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Right-click inside the buffer for the editing menu. It offers <strong>New Section</strong> and <strong>New Subsection</strong> (placed structurally, after fenced blocks rather than inside them), <strong>Add note on line…</strong> for an anchored note on the selection, emphasis and link wrapping, <strong>Insert dialog…</strong> for a Fountain fence, <strong>Insert file…</strong> to link a notebook file, and <strong>Upload file…</strong> to upload a new file and link it in one step. With text selected, the menu also offers <strong>Send section to agent</strong> and <strong>Send selection to agent</strong>, which type a drift-free reference into your agent terminal. See <a href="design-notebook-agents">Working with agents</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Right-clicking a section in the outline or a heading in document mode opens that section's own menu: edit in place, add a note, set status, view history, delete the section, and send it to an agent.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Fenced blocks the editor understands structurally (Fountain dialog, code, math) offer <strong>Edit … structurally</strong>, an in-line editor tailored to the block.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="saving">Saving</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Save with <strong>Ctrl+S</strong> (or the Save button); <strong>Ctrl+Shift+S</strong> opens a remarks-first dialog. Saving is composite and atomic: the Markdown, every pending note, and every pending status change land together in one transaction, with a remark recorded for the revision.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>New notes and status changes are held as pending (marked in the panel with a pending dot) and flush with the next save, so the notes you leave refer to the exact revision of the design they were left on. The editor refuses nothing you type: your edits live in a per-page draft that survives switching pages or closing the tab, and the tree's amber dot marks the page until you save. <strong>⟳ Refresh</strong> reloads the page from the server; it refuses to run while you have unsaved edits rather than dropping them.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><strong>Export .md</strong> downloads the page as the server renders it: the latest revision of every section, in canonical form.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="section-revision-history">Section revision history</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Every save writes new immutable rows only for the sections that changed; untouched sections keep their history clean. Select a section and open the panel's <strong>History</strong> tab to list its revisions, recall any older revision read-only, or compare a revision against the current text side by side. The panel's lineage view follows a section across moves: when content is renamed or re-parented, history says where it lives now.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Section identities follow their heading path. Rename a section and the next save records the move, carrying notes, statuses, and history with it; delete a section and its rows are archived, so restoring or viewing history stays possible.</p>
<!-- /wp:paragraph -->
