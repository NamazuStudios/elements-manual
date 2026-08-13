<h1>Incorporating GMEXT-Elements into a GameMaker Project</h1>

<!-- wp:paragraph -->
<p>A walkthrough for adding YoYoGames' <a href="https://github.com/YoYoGames/GMEXT-Elements">GMEXT-Elements</a> extension to any GameMaker Studio project by vendoring the source directly from GitHub.</p>
<!-- /wp:paragraph -->

<!-- wp:quote -->
<blockquote class="wp-block-quote"><!-- wp:paragraph -->
<p>A Marketplace listing for GMEXT-Elements isn't available yet — vendoring from GitHub is the only option today. This doc should get a Marketplace-install alternative once that listing exists.</p>
<!-- /wp:paragraph --></blockquote>
<!-- /wp:quote -->

<!-- wp:heading {"anchor":"h-why-vendor-the-source"} -->
<h2 id="h-why-vendor-the-source" class="wp-block-heading">Why vendor the source</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Reasons to pull the source in directly rather than depending on a prebuilt drop-in:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>You need a fix or feature that's landed on <code>main</code> but hasn't been tagged in a stable point yet.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>You want to read/patch the actual source (a compiled-in version isn't easily diffable).</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>You're scripting project setup and want reproducible, version-pinned source rather than a manual install step.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>Trade-off: you're now responsible for tracking upstream changes yourself.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"anchor":"h-1-understand-what-youre-actually-importing"} -->
<h2 id="h-1-understand-what-youre-actually-importing" class="wp-block-heading">1. Understand what you're actually importing</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Don't just copy the whole GMEXT-Elements repo wholesale — most of it is the <em>demo project</em> GameMaker uses to showcase the extension, not the extension itself. Clone it first and look:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>git clone --depth 1 https://github.com/YoYoGames/GMEXT-Elements.git</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>The relevant subtree is <code>source/Elements_gml/</code>, which is itself a full <code>.yyp</code> demo project. Inside it:</p>
<!-- /wp:paragraph -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th>Path</th><th>What it is</th><th>Do you need it?</th></tr></thead><tbody><tr><td><code>extensions/Elements/Elements.yy</code></td><td>The extension resource — just holds config <strong>options</strong> (server URLs, ports, debug flag). No code lives here.</td><td>Yes</td></tr><tr><td><code>objects/obj_elements_core/</code></td><td>Singleton that owns the HTTP async event, auth token storage, request bookkeeping.</td><td>Yes</td></tr><tr><td><code>objects/obj_elements_crossfire/</code></td><td>Singleton that owns the WebSocket async event and dispatches Crossfire messages.</td><td>Yes, if you want realtime/matchmaking</td></tr><tr><td><code>scripts/elements_rest_api/</code>, <code>elements_rest_helpers/</code>, <code>elements_rest_schemas/</code></td><td>The generated REST client — every <code>elements_*</code> HTTP wrapper function and request/response schema constructor.</td><td>Yes</td></tr><tr><td><code>scripts/elements_crossfire_api/</code>, <code>elements_crossfire_client/</code>, <code>elements_crossfire_helpers/</code></td><td>The Crossfire WebSocket client — connect/matchmake/send/receive.</td><td>Yes, if you want realtime/matchmaking</td></tr><tr><td><code>extensions/Elements/docs/</code></td><td>A static HTML help viewer (fonts, CSS, JS) for GameMaker's in-IDE "view docs" button.</td><td>No — skip it, it's ~4 MB of dead weight for a game build. The real docs are the <a href="https://github.com/YoYoGames/GMEXT-Elements/wiki">GitHub wiki</a>.</td></tr><tr><td><code>objects/obj_game</code>, <code>objects/obj_gm_button</code>, <code>objects/obj_gm_text</code>, <code>objects/obj_gm_textbox</code>, <code>objects/obj_elements_crossfire_*</code> (the <code>Mouse_7</code>-only ones), <code>objects/obj_elements_rest_*</code>, <code>rooms/rm_main</code>, <code>fonts/fnt_gm_*</code></td><td>Demo-project UI: buttons and labels used to click through the sample.</td><td>No — these only exist to make the demo clickable.</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:paragraph -->
<p>The key realization: the "extension" resource itself (<code>Elements.yy</code>) is nearly empty — it's just a place for <code>server_rest_url</code>, <code>server_crossfire_url</code>, <code>server_crossfire_port</code>, and <code>debug_logging</code> to live as extension options. All the actual functionality is in plain GML scripts and two small controller objects, which you import as ordinary project resources.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"anchor":"h-2-copy-the-resources-into-your-project"} -->
<h2 id="h-2-copy-the-resources-into-your-project" class="wp-block-heading">2. Copy the resources into your project</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>GameMaker's <code>.yy</code>/<code>.yyp</code> files are JSON-with-trailing-commas. You can hand-edit them; the IDE re-normalizes on next save. Copy in this shape:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>your_project/
  extensions/Elements/Elements.yy
  objects/obj_elements_core/{obj_elements_core.yy, Create_0.gml, CleanUp_0.gml, Other_62.gml}
  objects/obj_elements_crossfire/{obj_elements_crossfire.yy, Create_0.gml, Other_68.gml}
  scripts/elements_rest_api/{elements_rest_api.yy, elements_rest_api.gml}
  scripts/elements_rest_helpers/{elements_rest_helpers.yy, elements_rest_helpers.gml}
  scripts/elements_rest_schemas/{elements_rest_schemas.yy, elements_rest_schemas.gml}
  scripts/elements_crossfire_api/{elements_crossfire_api.yy, elements_crossfire_api.gml}
  scripts/elements_crossfire_client/{elements_crossfire_client.yy, elements_crossfire_client.gml}
  scripts/elements_crossfire_helpers/{elements_crossfire_helpers.yy, elements_crossfire_helpers.gml}</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>Every <code>.yy</code> resource file has a <code>"parent"</code> field pointing at a folder, e.g.:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>"parent": { "name": "Rest", "path": "folders/Elements/Rest.yy" }</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p><strong>Folders in the <code>.yyp</code> format are virtual</strong> — <code>folders/Elements/Rest.yy</code> is not a real file on disk anywhere, it's just an identifier string. You don't need to create anything at that path; you only need a matching entry in the project's <code>.yyp</code> <code>Folders</code> array (see next step). Leave the copied <code>.yy</code> files' <code>parent</code> fields untouched — they already point at the right virtual paths.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"anchor":"h-3-register-everything-in-the-yyp"} -->
<h2 id="h-3-register-everything-in-the-yyp" class="wp-block-heading">3. Register everything in the <code>.yyp</code></h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Two arrays in your project's <code>.yyp</code> need new entries: <code>Folders</code> (so the IDE shows a folder in the asset tree) and <code>resources</code> (so the resources actually load).</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><strong><code>Folders</code></strong> — add the folder chain the copied resources expect:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>{"$GMFolder":"","%Name":"Elements","folderPath":"folders/Elements.yy","name":"Elements","resourceType":"GMFolder","resourceVersion":"2.0",},
{"$GMFolder":"","%Name":"Crossfire","folderPath":"folders/Elements/Crossfire.yy","name":"Crossfire","resourceType":"GMFolder","resourceVersion":"2.0",},
{"$GMFolder":"","%Name":"Rest","folderPath":"folders/Elements/Rest.yy","name":"Rest","resourceType":"GMFolder","resourceVersion":"2.0",},</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p><strong><code>resources</code></strong> — one entry per resource, <code>name</code> matching the resource's internal name and <code>path</code> matching where you put its <code>.yy</code> file:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>{"id":{"name":"Elements","path":"extensions/Elements/Elements.yy",},},
{"id":{"name":"obj_elements_core","path":"objects/obj_elements_core/obj_elements_core.yy",},},
{"id":{"name":"obj_elements_crossfire","path":"objects/obj_elements_crossfire/obj_elements_crossfire.yy",},},
{"id":{"name":"elements_rest_api","path":"scripts/elements_rest_api/elements_rest_api.yy",},},
{"id":{"name":"elements_rest_helpers","path":"scripts/elements_rest_helpers/elements_rest_helpers.yy",},},
{"id":{"name":"elements_rest_schemas","path":"scripts/elements_rest_schemas/elements_rest_schemas.yy",},},
{"id":{"name":"elements_crossfire_api","path":"scripts/elements_crossfire_api/elements_crossfire_api.yy",},},
{"id":{"name":"elements_crossfire_client","path":"scripts/elements_crossfire_client/elements_crossfire_client.yy",},},
{"id":{"name":"elements_crossfire_helpers","path":"scripts/elements_crossfire_helpers/elements_crossfire_helpers.yy",},},</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>Placement inside the <code>resources</code> array doesn't matter functionally — GameMaker doesn't care about ordering — but grouping by type (near other extensions, near other objects, near other scripts) keeps diffs readable.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><strong>Validate before opening the IDE.</strong> The <code>.yyp</code> is JSON except for trailing commas, so you can sanity-check your edit without launching GameMaker:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>import re, json
text = open("YourProject.yyp").read()
cleaned = re.sub(r',\s*([}\]])', r'\1', text)
data = json.loads(cleaned)  # raises if malformed
names = [r["id"]["name"] for r in data["resources"]]
assert len(names) == len(set(names)), "duplicate resource name"</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>A malformed <code>.yyp</code> or a duplicate resource name will cause GameMaker to fail to open the project or silently drop a resource — catching it before opening the IDE saves a round trip.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"anchor":"h-4-one-likely-fix-make-the-core-singleton-persistent"} -->
<h2 id="h-4-one-likely-fix-make-the-core-singleton-persistent" class="wp-block-heading">4. One likely fix: make the core singleton persistent</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Both <code>obj_elements_core</code> and <code>obj_elements_crossfire</code> are <strong>lazily instantiated singletons</strong> — you never place them in a room. The pattern (from <code>elements_rest_helpers.gml</code>):</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>function _elements_get_singleton(_where) {
    static instance = instance_create_depth(0, 0, 0, obj_elements_core);
    with (instance) return self;
}</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>The <code>static</code> keyword means this instance is created exactly once, the first time any <code>elements_*</code> function runs. <code>obj_elements_crossfire</code> ships <code>persistent: true</code> in the stock extension, but <strong><code>obj_elements_core</code> ships <code>persistent: false</code>.</strong> If your project has more than one room and the room changes after the singleton is created, the non-persistent instance is destroyed — but the <code>static</code> variable still holds a reference to it, so every subsequent call silently operates on a dead instance (auth tokens, in-flight request bookkeeping, all gone).</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>If your project is single-room, this doesn't matter. If it's multi-room (a login screen, a menu, gameplay rooms, etc.), set it persistent:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>// objects/obj_elements_core/obj_elements_core.yy
"persistent": true,</code></pre>
<!-- /wp:code -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"anchor":"h-5-configure-the-extension-options"} -->
<h2 id="h-5-configure-the-extension-options" class="wp-block-heading">5. Configure the extension options</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Open the project in GameMaker, find the <strong>Elements</strong> extension under Extensions in the asset tree, and fill in:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><code>server_rest_url</code> — your Elements REST API base URL</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>server_crossfire_url</code> — your Elements Crossfire WebSocket host</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>server_crossfire_port</code> — usually <code>443</code> for WSS</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>debug_logging</code> — <code>True</code> while integrating, so failed requests and Crossfire phase transitions print to the debug console</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>These are read via <code>extension_get_option_value("Elements", "...")</code> inside the vendored scripts — don't hardcode URLs elsewhere in your game code.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"anchor":"h-6-verify-it-works"} -->
<h2 id="h-6-verify-it-works" class="wp-block-heading">6. Verify it works</h2>
<!-- /wp:heading -->

<!-- wp:list {"ordered":true} -->
<ol class="wp-block-list"><!-- wp:list-item -->
<li>Open the project in GameMaker. It should load with no missing-resource errors and no red squiggles in the copied scripts.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Drop a test call somewhere reachable at startup, e.g. in a controller object's Create event:</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Run the game. <code>debug_logging = True</code> should print the outgoing request and response in the console. A <code>_code</code> of <code>200</code> (or a clear connection error if your server isn't running yet) confirms the plumbing — extension options, singleton creation, async event handling — is wired correctly.</li>
<!-- /wp:list-item --></ol>
<!-- /wp:list -->

<!-- wp:code -->
<pre class="wp-block-code"><code>elements_get_application("your-app-id", function(_code, _data, _request) {
    show_debug_message($"Elements reachable: {_code}");
});</code></pre>
<!-- /wp:code -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"anchor":"h-7-using-it-rest-and-crossfire-basics"} -->
<h2 id="h-7-using-it-rest-and-crossfire-basics" class="wp-block-heading">7. Using it: REST and Crossfire basics</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p><strong>REST</strong> — every endpoint is a thin async wrapper, callback-based:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>elements_create_username_password_session(_session_request, function(_code, _data, _request) {
    if (_code == 200) {
        _elements_request_auth_set_token("auth_bearer", _data.sessionSecret);
        // now every endpoint that declares "auth_bearer" as a security scheme is authenticated
    }
});</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p><strong>Crossfire</strong> — connect, then either find/create/join a match:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>elements_crossfire_set_identity(profile_id, session_secret);
elements_crossfire_events_on_connected_callback(function() {
    elements_crossfire_create_match("my_config");   // host path — server responds with a join code
});
elements_crossfire_events_on_created_callback(function(_msg) {
    show_debug_message("Room code: " + _msg.joinCode);
});
elements_crossfire_connect();</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>Guests call <code>elements_crossfire_join_match_by_code(_join_code)</code> instead of <code>elements_crossfire_create_match</code>, and listen for <code>elements_crossfire_events_on_matched_callback</code> instead of <code>_on_created_callback</code>.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Once connected, <code>elements_crossfire_send_string_broadcast(_text)</code> pushes a message to every participant in the match — this is the "push an event to all connected clients" mechanism for anything server-driven (e.g., "your generated content is ready").</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"anchor":"h-gotchas-learned-the-hard-way"} -->
<h2 id="h-gotchas-learned-the-hard-way" class="wp-block-heading">Gotchas learned the hard way</h2>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>Don't hand-patch a feature before checking upstream.</strong> GMEXT-Elements is under active development. A capability that looks missing from the version you last read the source of — whether that was an older commit you vendored from, or a plan doc written a few weeks ago — may already exist on <code>main</code> — always re-diff against the current GitHub source before writing a patch, or you'll duplicate work upstream already did.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>The extension resource itself has an empty <code>files</code> list.</strong> If you're used to native extensions with per-platform binaries, this one is disorienting — there's nothing to compile, it's pure GML. Don't go looking for a <code>.dll</code>/<code>.so</code>/<code>.dylib</code> to place; there isn't one.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Don't import the demo project's UI objects.</strong> It's tempting to copy everything under <code>source/Elements_gml/objects/</code> for completeness, but half of them (<code>obj_gm_button</code>, <code>obj_elements_rest_create_profile</code>, etc.) exist only to make the sample clickable and add dead weight and demo-specific coupling to your project.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Folder paths in <code>.yy</code> files don't need real files.</strong> If you see <code>"path": "folders/Elements/Rest.yy"</code> in a copied resource, that's a virtual identifier consumed only by the <code>.yyp</code>'s <code>Folders</code> array — don't go looking for (or creating) an actual file there.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->
