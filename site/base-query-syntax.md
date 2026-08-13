<h1>Base Query Syntax</h1>

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"level":6} -->
<h6 class="wp-block-heading" id="h-base-query-syntax">Base Query Syntax</h6>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The query system support two major types, Strings and Numbers. When searching for objects, it will be necessary to distinguish between those two types.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-text-queries">Text Queries</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>When dealing with text types, the <code>:</code> operator is used for textual equality. The full syntax is as follows:</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><code>{property name}:{property value}</code></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>This will perform a query which matches objects with the following criteria:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><code>property name</code> must exist on the object</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>The value of the object in the database must be a string</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>The value must match exactly <code>property value</code></li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>If those conditions are not met, then no objects will match.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Additionally, it is possible to encapsulate the property in quotes to ensure that the parser will properly parse the values. For example:</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><code>displayName:"Jack Ryan 1984"</code></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Will match exactly the string "Jack Ryan 1984"</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-numeric-queries">Numeric Queries</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Similar text queries, numeric queries exist allowing for complex selection of numeric values. Unlike text queries, Elements supports multiple operators:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><code>&lt;</code> - Less Than</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>></code> - Greater Than</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>&lt;=</code> - Less Than or Equal To</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>>=</code> - Greater Than or Equal To</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>=</code> - Equal to</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>!=</code> - Not Equal or Logical Xor</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>:</code> and <code>TO</code> - Range Operator</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>Numeric queries typically look a little bit different than their string-based counterparts and, with the exception of the range operator, follow the following syntax:</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><code>{property name} {numeric operator} {property value}</code></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>This will perform a query which matches objects with the following criteria:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><code>property name</code> must exist on the object</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>The <code>numeric operator</code> must be a valid operator one of:</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>&lt;</code>, <code>></code>, <code>&lt;=</code>, <code>>=</code>, <code>=</code>, <code>!=</code></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>For range queries, see the section <a href="#range-query">#range-query</a> for more information.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>The value must match <code>property value</code> based on the rules of the operator</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>For example, it would be possible to find all scores greater than 100 using the following query:</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><code>score &gt; 100</code></p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-range-query">Range Query</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The Range Query is a special case designed to reduce the need to define ranges using a combination of boolean operators. Range Queries look similar to String queries, with the important distinction that they are able to operate ranges of numbers. All ranges are inclusive and use the following syntax:</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><code>{property name}:{property lower bound} TO {property upper bound}</code></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>This will perform a query which matches objects with the following criteria:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><code>property name</code> must exist on the object</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>The value of the object in the database must be a number</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>The value must be …</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Greater than or equal to <code>property lower bound</code></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Less than or equal to <code>property upper bound</code></li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->
