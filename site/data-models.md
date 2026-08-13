<h1>Data Models</h1>

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:paragraph -->
<p>Among the data models presented within Elements, several data models will contain the following properties. In all cases the properties represent the same consistent concept.</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><a href="#id-property"><strong>id</strong></a> is always the system-assigned unique ID of a particular object.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="#name-property"><strong>name</strong></a> represents a unique system-readable reference to the particular object.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="#metadata-property"><strong>metadata</strong></a> represents user editable metadata that Elements will store with each object for more information on effective using metadata.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="#tags-property"><strong>tags</strong></a> are a list or set of words used to categorize and filter content.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="#display-name-property"><strong>displayName</strong></a> represents a name used to refer to the object within the UI and potentially the final page or application.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="#timestamp-property"><strong>timestamp</strong></a> represents a time stamp or similar property.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-id-property">id Property <a href="#id-property" id="id-property"></a></h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p><strong>id</strong> always refers to the system-assigned unique ID of a particular object. Due to the server authoritative nature of Elements, the unique ID will always be the unique identifier for a particular object. In all cases, the identifier is generated server-side.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="h-tips-for-using-the-id-property">Tips for Using the id Property <a href="#tips-on-using-id-effectively" id="tips-on-using-id-effectively"></a></h4>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Always treat IDs as opaque values. For example:</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Do not assume any particular format of the ID. At the time of this writing, all server IDs are hex strings. There is not a strict requirement for future editions of Elements.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Some IDs may contain encoded data. Unless specifically documented, do not assume that any IDs will always follow the same encoding format.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>IDs are always handled as strings in request and response objects.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>The server will always generate IDs.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Some APIs will accept either an ID or a name for certain operations. Unless otherwise specified, always pass the ID.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-name-property">name Property <a href="#name-property" id="name-property"></a></h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p><strong>name</strong> represents a unique system-readable reference to a particular object. The name assigned to any object is always intended to be an alternative unique id for the object. Understanding this is crucial for integrating backend scripts and client-side application code.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Unless otherwise specified, the name can be used as a programmatically unique id in all parts of the code. Additionally, two unrelated items may share a name. For example an <a href="../../core-features/digital-goods#items">Item</a> and a <a href="../core-features/progress-and-missions">Mission</a> may share a name, however two Items may not share the same name. Names are case-sensitive and must be an alphanumeric string with no spaces.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="h-tips-for-using-the-name-property">Tips for Using the name Property <a href="#tips-on-using-names-effectively" id="tips-on-using-names-effectively"></a></h4>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Use a consistent convention. For example, all lowercase letters with underscore separation. If, for example, you wish to model a Paper Hat as an item. A good name would simply be <code>paper_hat</code>. All code may not refer to this when fetching Paper Hat Items from the database.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Use the name wherever possible, unless there is a specific need to use the database id. This ensures that the scripting engine code can refer to the specific object even if the object has been deleted and recreated, such as may be the case during migration between two separate instances of Elements.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Give all objects a clear name that is obvious to the reader. This will avoid problems with ambiguity when designing your system.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-metadata-property">metadata Property <a href="#metadata-property" id="metadata-property"></a></h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p><strong>metadata</strong> represents user editable metadata that Elements will store with each item. This data may be used for any purpose and will not be modified or touched by Elements in any way. Depending on the circumstance, this may or may not be editable by regular users. At the time of this writing, it is not possible to index by metadata. At the time of this writing, all metadata must be written in a server-authoritative way. In other words, only superusers and scripts running in the backend may modify metadata. This may be subject to future enhancements.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>In all cases, metadata is a multi-layered key-value object. The admin panel allows for simple editing of metadata as key-value strings, as well as editing inline JSON metadata. Some objects work with metadata schema, and full support for metadata schema is actively under development for all remaining system types.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="h-tips-for-using-the-metadata-property">Tips for Using the metadata Property <a href="#tips-for-using-metadata-effectively" id="tips-for-using-metadata-effectively"></a></h4>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Metadata is not indexed, so you should not make objects searchable via metadata.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Metadata is meant to be used to to add custom fields to objects that do not have sufficient support for data on their own. For example, when attached to a <a href="../../core-features/users-and-profiles#profile-metadata">Profile</a>, metadata may represent the current exp, level, or status of equipment slots.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>In general, the metadata should not include secure or sensitive information.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-tags-property">tags Property <a href="#tags-property" id="tags-property"></a></h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Tags are indexed for optimal querying from the database and should be used to quickly separate content when making API calls. Effectively using tags enables developers to quickly filter items as the tags are indexed and all APIs supporting tags support the querying of the tags. For example, items tagged with "armor" and "head" may represent helmets equipped to a character in a game.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="h-tips-for-using-the-tags-property">Tips for Using the tags Property <a href="#tips-for-using-tags-properly" id="tips-for-using-tags-properly"></a></h4>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>When querying tags, they are always all-inclusive. For example if <code>paper_hat</code> has tags, <code>armor</code> and <code>head</code> will show up in searches for both <code>armor</code> and <code>head</code> only <code>armor</code>, or only <code>head</code>. It will never appear if <code>weapon</code> is specified in the search terms.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>There is no specific limit on tags, and while too many tags may run up against database limitations, practically speaking, Elements will allow far more tags than what is necessary for a majority of use cases.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-displayname-property">displayName Property <a href="#display-name-property" id="display-name-property"></a></h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The display name is simply a name to use when displaying the object in the admin console or potentially to end users. The displayName should not be indexed and should never be used to make data decisions or in programmatic code. It's simply there for human readability and visual reference.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-timestamp-property">timestamp Property <a href="#timestamp-property" id="timestamp-property"></a></h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Elements represents timestamps and dates using the same notation used by the Java Virtual Machine. Timestamps are a 64-bit integer counting the number of milliseconds since the Unix Epoch (January 1, 1970). For more information see <a href="https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/System.html#currentTimeMillis()">System.currentTimeMillis()</a></p>
<!-- /wp:paragraph -->
