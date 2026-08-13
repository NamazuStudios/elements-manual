<h1>.ref</h1>

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"level":6} -->
<h6 class="wp-block-heading" id="h-specifies-and-object-reference">Specifies and Object Reference</h6>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>When a type references another type, it is not possible to directly query that object. The <code>.ref</code> operator enables support for such an operation. It is similar to a JOIN operation in SQL. This is used to reference other types which have their own <code>id</code> field and are first-class database objects.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-example-user-and-profile">Example: User and Profile</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Given the following example of a User and a Profile:</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>The User object has fields such as name, and ID.</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>{
  "id" : "abcd",
  "name" : "earl1",
  "email" : "earl1@example.com"
}</code></pre>
<!-- /wp:code -->

<!-- wp:code -->
<pre class="wp-block-code"><code>{
  "id" : "defg",
  "displayName" : "Earl 1",
  "user" : {
    "id" : "abcd",
    "name" : "earl1",
    "email" : "earl1@example.com"
  }
  "application" : { 
    "id" : "hijk",
    "name" : "EXAMPLE"
  }
}</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>It would, at a glance, make sense to attempt to query a Profile this way:</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><code>user.id:abcd</code></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>However, as User is a reference that will return no results. Therefore it is necessary to use the reference operator to perform the query:</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><code>.ref.user:abcd</code></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>This tells the query engine to execute the following:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Identify the client is requesting a reference to another object.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Identify the reference as the <code>user</code> field.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Find a user with <code>id = "abcd"</code></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Find all Profiles matching that user.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->
