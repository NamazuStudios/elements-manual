<h1>Metadata</h1>

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:paragraph -->
<p>Metadata in Elements provides a flexible way to define and store structured information.<br>It consists of two parts:</p>
<!-- /wp:paragraph -->

<!-- wp:list {"ordered":true} -->
<ol class="wp-block-list"><!-- wp:list-item -->
<li><strong>Metadata Specs</strong> – define the <em>schema</em> (types and structure).</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Metadata Objects</strong> – store actual data, validated against a spec.</li>
<!-- /wp:list-item --></ol>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>This lets you enforce consistency across different parts of your game/application, while still allowing for flexible definitions.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-metadata-specs">Metadata Specs</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>A <strong>Metadata Spec</strong> is a schema that describes what a metadata object should look like.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-structure">Structure</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>A spec has:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>name</strong>: A unique identifier for the spec.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>properties</strong>: A list of fields, each with a type and optional values.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-property-types">Property Types</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Each property has a <strong>name</strong> and a <strong>type</strong>. Four types are supported:</p>
<!-- /wp:paragraph -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th>Type</th><th>Description</th><th>Extras</th></tr></thead><tbody><tr><td><strong>STRING</strong></td><td>A text field</td><td>Optional placeholder value</td></tr><tr><td><strong>NUMBER</strong></td><td>A numeric field (int or float)</td><td>Optional placeholder value</td></tr><tr><td><strong>BOOLEAN</strong></td><td>A true/false value</td><td>–</td></tr><tr><td><strong>OBJECT</strong></td><td>A nested structure containing its own properties</td><td>Properties can be any of the four types</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:paragraph -->
<p><strong>Example Spec</strong></p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>{
  "name": "WeaponSpec",
  "properties": &#91;
    { "name": "damage", "type": "NUMBER", "placeholder": 10 },
    { "name": "rarity", "type": "STRING", "placeholder": "common" },
    { "name": "isLegendary", "type": "BOOLEAN" },
    {
      "name": "stats",
      "type": "OBJECT",
      "properties": &#91;
        { "name": "range", "type": "NUMBER", "placeholder": 100 },
        { "name": "weight", "type": "NUMBER" }
      ]
    }
  ]
}
</code></pre>
<!-- /wp:code -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-metadata-objects">Metadata Objects</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>A <strong>Metadata Object</strong> is an actual piece of data created based on a Metadata Spec.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-structure-0">Structure</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>A metadata object has:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>name</strong>: A human-readable identifier.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>access level</strong>: Controls who can see this object:<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>Unprivileged</strong> – visible to anyone.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>User</strong> – visible only to logged-in users.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Superuser</strong> – visible only to administrators.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list --></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>spec</strong>: The Metadata Spec that defines its structure.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>data</strong>: A map of values that follows the rules from the spec.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-example-object">Example Object</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Using the <code>WeaponSpec</code> defined earlier:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>{
  "name": "Excalibur",
  "accessLevel": "User",
  "spec": "WeaponSpec",
  "data": {
    "damage": 50,
    "rarity": "legendary",
    "isLegendary": true,
    "stats": {
      "range": 150,
      "weight": 5
    }
  }
}
</code></pre>
<!-- /wp:code -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-scoping">Scoping</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Metadata objects can be attached <strong>globally</strong> or <strong>scoped</strong> to specific domains within Elements:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>Global</strong> – accessible across the entire application.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>User Profile</strong> – metadata specific to a given user.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Application</strong> – metadata tied to the app instance.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Items</strong> – metadata attached to inventory/game objects.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>This makes metadata a flexible tool for storing structured information at different levels of your system.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-access-control">Access Control</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>When querying metadata objects, Elements automatically filters results based on the <strong>user’s access level</strong>:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>Unprivileged</strong> users only see <code>Unprivileged</code> metadata.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Users</strong> see both <code>Unprivileged</code> and <code>User</code> metadata.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Superusers</strong> see everything.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>This ensures sensitive or internal metadata is never leaked to unintended clients.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-common-use-cases">Common Use Cases</h2>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Defining <strong>item properties</strong> (weapons, gear, consumables).</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Creating <strong>application-level configs</strong>.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Attaching <strong>custom profile attributes</strong> to users.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Storing <strong>dynamic game data</strong> (quests, NPC definitions, world states).</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-summary">Summary</h2>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>Specs</strong> define <em>what</em> metadata looks like.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Objects</strong> hold the actual <em>data</em>, validated against a spec.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Access levels</strong> control visibility.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Scoping</strong> lets you attach metadata where it’s needed — global, user, app, or items.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->
