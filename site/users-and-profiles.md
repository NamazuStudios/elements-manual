<h1>Users and Profiles</h1>

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"level":6} -->
<h6 class="wp-block-heading" id="h-elements-fully-supports-users-and-profiles-that-allow-users-of-your-application-or-game-to-store-their-profile-data-and-access-a-network-of-applications-from-a-single-login-profile">Elements fully supports Users and Profiles that allow users of your application or game to store their profile data and access a network of applications from a single login profile.</h6>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Elements uses a one-to-many model for the User / Profile relationship. A Profile is linked directly to both one single Application and one single User.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>As Elements is a multi-tenant system, there can be many Applications that allow for many Profiles per Application.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>This structure gives you the flexibility to create a network of Applications under the same roof, with any User level information, such as Inventory, accessible in every Application should you so choose.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-user-properties">User Properties</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>A <strong>User</strong> has the following properties:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>id</strong></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>name</strong></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>email</strong></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>level</strong><!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>UNPRIVILEGED - An unprivileged/anonymous user.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>USER - A basic user.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>SUPERUSER - An administrator/super user, who can do all of the above as well as delete/create users.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list --></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>linkedAccounts</strong><!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>A list of account types that the User is linked to, such as name, email, steam, google, apple, etc.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list --></li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="h-managing-users-in-the-console">Managing Users in the Console <a href="#managing-users-in-the-console" id="managing-users-in-the-console"></a></h4>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Users are managed in the Users section of the admin console, which can be accessed from the upper nav bar or in the hamburger menu.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>The "Add User" button will open the new user panel.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Users can be edited by tapping the "Edit" button next to that user, or can be deleted by tapping the "Delete" button. Use the search function to more easily find specific users.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Passwords can be set or changed using the admin console as well. In the database, they are salted and hashed and can't be manipulated directly.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":22354,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://namazustudios.com/wp-content/uploads/2025/08/Screenshot-2026-01-30-at-4.37.05-PM-1024x848.png" alt="" class="wp-image-22354"/></figure>
<!-- /wp:image -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="h-json-structure-of-users">JSON Structure of Users <a href="#json-structure-of-users" id="json-structure-of-users"></a></h4>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Here is a sample of a user JSON entry from the database:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>{
    "_id" : ObjectId("5cdb0aa4e96c3c4f2bfe16ea"),
    "active" : true,
    "name" : "jsmith",
    "email" : "jsmith@elements.com",
    "level" : "SUPERUSER",
    "salt" : { "$binary" : "/thisissalt", "$type" : "00" },
    "passwordHash" : { "$binary" : "thisisapasswordhash=", "$type" : "00" },
    "hashAlgorithm" : "SHA-256"
}</code></pre>
<!-- /wp:code -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-profile-properties">Profile Properties</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>A <strong>Profile</strong> has the following properties:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Id</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>User - The User that this Profile is linked to.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Application - The Application that this Profile is linked to.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>ImageUrl - Any associated avatar image.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>DisplayName - The name that should be displayed on the client side.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Metadata - Key/Value store that can be modified.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>LastLogin - The time that the current session token was created (MS since epoch)</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>Once created, only the DisplayName and ImageUrl can be modified from the client side (via <strong>ProfilesApi</strong>). However, other properties, such as the Metadata, can be modified from the server side.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="h-profile-metadata">Profile Metadata <a href="#profile-metadata" id="profile-metadata"></a></h4>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>As mentioned in the previous section, the Profile metadata is a key/value store for just about anything that you'd want to be publicly facing (as Inventory is private) in a Profile. This is a great place for storing info that you might want different Profiles to know about each other. For example, say you have a game where someone can earn points to increase a numeric rank. In order to see that information, it needs to be stored in a publicly accessible place. Take this wireframe image for example:</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":21963,"width":"297px","height":"auto","aspectRatio":"0.4637483483227484","align":"center"} -->
<figure class="wp-block-image aligncenter is-resized"><img src="https://namazustudios.com/wp-content/uploads/2025/08/profile_metadata.png" alt="" class="wp-image-21963" style="aspect-ratio:0.4637483483227484;width:297px;height:auto"/><figcaption class="wp-element-caption">FBPanelExample</figcaption></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>All of the stats listed in this screen would be housed by the Profile metadata, so the JSON representation might look something like this:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>{
    "id" : &lt;id string&gt;, 
    "user" : &lt;redacted&gt;,
    "application" : &lt;Application JSON&gt;,
    "image_url" : &lt;path to image&gt;,
    "display_name" : &lt;display name string&gt;,
    "last_login" : &lt;last login time&gt;,
    "metadata": {
        "ranking" : 1300,
        "wins" : 999,
        "losses" : 999,
        "total_games" : 999,
        "current_win_streak" : 999,
        etc...
    }
}</code></pre>
<!-- /wp:code -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="h-managing-profiles-using-the-console">Managing Profiles Using the Console <a href="#managing-profiles-using-the-console" id="managing-profiles-using-the-console"></a></h4>
<!-- /wp:heading -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"Note"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="3b0649"><div class="gb-notice-title" style="color:#fff"><p>Note</p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p>Profile metadata is edited identically to how item metadata is edited. See <a href="../digital-goods#editing-item-metadata">Editing Item Metadata</a>.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:paragraph -->
<p>Profiles can be added, edited, or deleted from the admin console in the Profiles section, accessible from the top nav bar or the hamburger menu.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Since Profiles must be connected to a user, when making a new profile you can use the "Select User" button to find the user that will own this profile. If you are editing an existing profile, you can use the "Edit User" button to open the Edit User panel for that user. Existing profiles cannot be reassigned to another user.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>When creating a new profile, you must also select an Application. A dropdown menu will open with the available applications. If editing an existing profile, the Application may not be changed.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":22355,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://namazustudios.com/wp-content/uploads/2025/08/Screenshot-2026-01-30-at-4.39.05-PM-1024x795.png" alt="" class="wp-image-22355"/></figure>
<!-- /wp:image -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="h-json-structure-of-profile">JSON Structure of Profile <a href="#json-structure-of-profile" id="json-structure-of-profile"></a></h4>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>This is a sample profile represented in JSON:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>&#91;{
    "_id" : ObjectId("5cdb92e7e96c3c4f2b01106f"),
    "active" : true,
    "application" : {
        "$ref" : "application",
        "$id" : ObjectId("5cdb1088e96c3c4f2bfe1da7")
    },
    "user" : {
        "$ref" : "user",
        "$id" : ObjectId("5cdb92e5e96c3c4f2b0107ca")
    },
    "imageUrl" : "url/to/image",
    "displayName" : "DisplayName",
    "lastLogin" : ISODate("2019-05-15T04:17:47.127Z"),
    "metadata" : {
        "metadata_string" : "string"
        "metadata_int" : 1
    }
}]</code></pre>
<!-- /wp:code -->
