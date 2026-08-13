<h1>Object Graph Navigation</h1>

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"level":6} -->
<h6 class="wp-block-heading" id="h-understanding-object-graphs">Understanding Object Graphs</h6>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Objects in Elements are not flat, but typically represented as hierarchical JSON objects with nested information. Due to internal storage requirements, sometimes those objects are embedded in the object and other times they are tracked internally as references. It takes some special consideration when querying for objects to find the ones you need.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>The dot-syntax notation is used to specify nested objects. For example:</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><code>foo.bar</code></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Would specify the "bar" field of the following nested object:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>{
  "foo" : { 
    "bar" : 42.0,
    "baz" : "Hello"
  }
}</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>Therefore, to match the example object, the following queries would match the object:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><code>foo.bar = 42</code></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>foo.baz:Hello</code></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>foo.bar:0 TO 100</code></li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->
