<h1>Boolean Queries</h1>

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"level":6} -->
<h6 class="wp-block-heading" id="h-combining-base-queries-to-form-complex-queries">Combining Base Queries to Form Complex Queries</h6>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>In addition to Base Query Syntax, it is possible to mix and match multiple queries to perform complex lookups of objects in the database. The following operators allow for greater complexity of finding objects within Elements:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><code>AND</code> - Logical all-inclusive operation. Both sides of the AND operation must be true for the object to match.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>OR</code> - Logical operation which will match if the object matches either the left or right side of the operator</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>NOT</code> - Logical negation. The operator following NOT must be false to match the object in the query.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>( )</code> - Grouping Operator. Groups operations such that they are evaluated before the groups on the outside. Used to force order of operations or provide clarity where the syntax may be ambiguous.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-example-finding-legendary-swords">Example: Finding Legendary Swords</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>With complex Boolean queries, it is possible to perform advanced grouping of data. For example, it may be possible to search for all distinct items with certain metadata fields.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>In this case, we can query the whole database for legendary swords:</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><code>metadata.type:Sword AND metadata.rarity:Legendary</code></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>This will match all objects which match the following criteria:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Have a metadata string property named "Type"</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Have a metadata string property named "Rarity"</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Type equals the word "Sword"</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Rarity equals the word, "Legendary"</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-example-finding-legendary-low-level-swords">Example: Finding Legendary Low-Level Swords</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>We can further refine the query to include stats on the item itself:</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><code>metadata.type:Sword AND metadata.rarity:Legendary AND metadata.level:10 TO 20</code></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>This will match all objects which match the following criteria:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Have a metadata string property named "Type"</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Have a metadata string property named "Rarity"</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Type equals the word "Sword"</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Rarity equals the word, "Legendary"</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>The object's level is between Level 10 and Level 20.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->
