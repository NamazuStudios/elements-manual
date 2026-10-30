<h1>Files, Assets, and Tickets</h1>

<!-- wp:paragraph -->
<p>A notebook holds more than its pages. Design files are the reference material (art, audio, documents) that pages link into their prose; assets are the deliverables a design calls for, with their reference art; and tickets are the GitHub issues that track the implementation work.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="design-files">Design files</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Under a notebook's <strong>Files</strong> heading, upload anything the design needs: concept art, mockups, audio, spreadsheets, PDFs. A file's row shows its kind (with an icon), its current revision, and its upload count. The Files list filters by kind (image, video, audio, or downloadable) and by name.</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>Preview</strong> clicks open a rendering right in the tree: images, video, and audio play in place.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Replace</strong> uploads a new version. Replacing never overwrites: each upload appends a new revision, and the file's current content is always its latest revision while every historical revision stays downloadable.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Click a file to expand its <strong>revision history</strong>: one row per upload, in order, each with its own download.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="linking-files-into-a-page">Linking files into a page</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>In the editor, <strong>Insert file…</strong> (right-click) links a file already in the notebook; <strong>Upload file…</strong> uploads and links in one step. The inserted link points at the file by identity, and in document mode the link renders as the thing itself: the image, the video player, the audio player. A page with its reference art embedded reads like a finished design document.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Because links reference the file, not a particular upload, a replaced file updates everywhere it renders. When a page needs to cite an exact version, the asset list below is the tool for that.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="the-asset-list">The asset list</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>An asset is a deliverable: "title splash", "combat theme", "box art". Each entry carries a name, a description for whoever delivers it, an optional link to the GDD section it belongs to (the tree shows the link as a page and section path), and a delivered flag.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>An asset's <strong>reference art</strong> entries pin an exact revision of a notebook file: the revision current at attach time. A later upload never silently changes what an asset asked for; updating the reference is an explicit "update to latest" act. Reference art is downloadable at its pinned revision, so the version that informed the request is always recoverable.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Mark an asset delivered when it lands (and reopen it if it comes back).</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="github-tickets">GitHub tickets</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The design calls for implementation work; tickets track it where the work happens. <strong>+ add</strong> under a notebook's <strong>Tickets</strong> heading records a GitHub issue as owner and number (for example, a repository slug plus issue number), and the list renders it as a clickable reference to that issue.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Tickets recorded in the notebook are the ones agents file from the design (greenlit sections tell them to), keeping the notebook's list real rather than aspirational. Like every other item, a ticket carries its own details modal with editable agent instructions.</p>
<!-- /wp:paragraph -->
