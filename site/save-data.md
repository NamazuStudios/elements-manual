<h1>Save Data</h1>

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"level":6} -->
<h6 class="wp-block-heading" id="h-elements-allows-saving-data-associated-with-users-and-profiles-with-multiple-options-to-prevent-conflicts-using-version-control">Elements allows saving data associated with Users and Profiles with multiple options to prevent conflicts using version control.</h6>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Save data allows for the storage of arbitrary data associated either with a <a href="/docs/users-and-profiles/">User</a> or a <a href="https://namazustudios.com/docs/users-and-profiles/#h-profile-properties">Profile</a>. This data may be any arbitrary string. There is no particular size limit, however, save documents exceeding a few kilobytes should be discouraged.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Elements organizes each Save Data Document in a slot identified by an integer, starting at zero. The number of slots per User or Profile is the maximum value of a signed 32-bit integer.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Each Save Data Document has the following properties:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><a href="../../general/general-concepts#id-property"><strong>id</strong></a> - The integer-based slot identifier for the save data document</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>user</strong> - The User that owns this Save Data, will always be set even if the Save Data Document is Profile scoped.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>profile</strong> - The Profile which owns the Save Data document. If User scoped, then this field will be null.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="../../general/general-concepts#timestamp-property"><strong>timestamp</strong></a> - Represents the last update time of the document.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>version</strong> - Indicates the last incremental revision of the contents of the save file. This is a hash or checksum of the Contents and must match when updating the data in the slot. This allows client code to detect and correct consistency for the slot.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>contents</strong> - A string representing the contents of the save data file.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-creating-save-data">Creating Save Data <a href="#creating-save-data" id="creating-save-data"></a></h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>When creating a new Save Data document, you must specify an empty slot, userId, and profileId. Elements will reject the request if the save data is already present in the supplied slot. When saving Contents for the first time, Elements will generate a hash of the contents and provide that in the <strong>version</strong> field of the response.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-updating-save-data">Updating Save Data <a href="#updating-save-data" id="updating-save-data"></a></h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>When updating Save Data, the client may specify one of two update modes: Forced or Checked update. The purpose of this process is to allow checking in the case that the client contains an outdated version of the save data document.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="h-checked-update">Checked Update <a href="#checked-update" id="checked-update"></a></h4>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>To preserve the integrity of your game or application's Save Data, we recommend attempting a checked update first. The basic process for performing a checked update is as follows:</p>
<!-- /wp:paragraph -->

<!-- wp:list {"ordered":true} -->
<ol class="wp-block-list"><!-- wp:list-item -->
<li>The first time the player saves data, issue a create request to save for the first time.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>As early as possible in the application lifecycle, fetch the save data from Elements.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Save both Version and Contents to local storage or in memory.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>When updating, supply the Version along with new Contents.</li>
<!-- /wp:list-item --></ol>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>A checked update detects a scenario in which the same User operates on multiple devices. For example:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>User logs in on Device A, creates a Save Data document, and successfully creates the Save Data in Elements.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>User logs in on Device B, reads the Save Data document from Elements, and stores locally.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>On Device B, the user makes changes to the Save Data and successfully updates Save Data.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>On Device A, user logs in, makes changes, and for some reason is unable to retrieve the data from Elements (e.g. - Airplane Mode).</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Once Device A has a connection again, the update will fail due to version mismatch.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>At this point the client can perform one of a few actions:</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Attempt to merge the data.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Present the option allowing the User to select which version of the data they wish to use.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="h-forced-update">Forced Update <a href="#forced-update" id="forced-update"></a></h4>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>When performing a forced update, Elements will ignore the version field and write the specified Contents directly to the database as supplied. Elements provides this as an option for when data consistency isn't terribly important or as a conflict resolution strategy (as outlined in the example above).</p>
<!-- /wp:paragraph -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"Note"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="3b0649"><div class="gb-notice-title" style="color:#fff"><p>Note</p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p>Using this option is dependent on the needs of your application.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->
