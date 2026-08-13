<h1>Digital Goods</h1>

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"level":6} -->
<h6 class="wp-block-heading" id="h-elements-fully-supports-digital-goods-which-are-items-that-can-be-owned-by-users-in-their-inventory-or-purchased-with-in-app-purchases-such-as-potions-or-gold-coins-in-a-video-game">Elements fully supports Digital Goods, which are items that can be owned by users in their inventory or purchased with In-App Purchases, such as potions or gold coins in a video game.</h6>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Elements supports traditional digital goods. Digital goods are items that can be owned by a user in their inventory. User inventories are independent of applications. Examples of digital goods may be consumable items such as potions or gold coins in a video game. Alternatively, they may be part of equipment sets such as swords, helmets, or weapon skins.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Digital goods are not limited to games and can be used to represent other content such as boosts in a dating app. Essentially, digital goods can be used to represent any type of content in an application that can be bought directly from your business.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>The Digital Goods system in Elements manages three things:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>Items</strong> represent the core metadata of the items offered for sale.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Inventory</strong> entries provide the association between a user and the Item they own.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Purchase Verification</strong> ties In-App Purchases to specific Items and Quantities which Elements will deposit upon successful validation of purchases tied to IAP bundles</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"Note"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="3b0649"><div class="gb-notice-title" style="color:#fff"><p>Note</p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p>Elements can verify purchases for Apple App Store and Google Play applications. First, you must set up <a href="../applications#application-configurations">Application Configurations</a> for <a href="applications/ios-application-configuration">iOS</a> or <a href="applications/android-application-configuration">Android</a> and then add the relevant info for your app to Elements to interact with the App Store or Google Play.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-items">Items <a href="#items" id="items"></a></h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Items are digital goods in the Elements database. These exist independent of <a href="applications">Applications</a>, so any application in your instance can interact with any item. Core APIs allow for the reading of items for sale.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>These are the various fields that make up an item.</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>name:</strong> This is a string representing the name of your item. It must be unique and have no spaces.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>tags:</strong> You may include a list of tags, as strings. Tags are an easy way to search for groups of items or sort them in your applications.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>displayName:</strong> This is a string representing your item's display name.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>description:</strong> This is a string that serves as a description for your item. Client code may opt to show this directly to the user.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>metadata:</strong> Metadata is optional. It can be any number of named strings or integers. This can have many uses, for example secondary descriptions, asset paths, types, or any arbitrary values assigned to your item.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Category:</strong> Describes one of two particular inventory item values: <a href="#fungible-inventory-item">Fungible</a> and <a href="#distinct-inventory-item">Distinct</a><strong>.</strong></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>_id:</strong> In the database, the item will also have an <strong>_id</strong> field. This is automatically generated when the item is created and serves as an internal reference for that item.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"Note"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="3b0649"><div class="gb-notice-title" style="color:#fff"><p>Note</p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p>See <a href="#managing-items-using-the-console">Managing Items</a> below for more details about how to manage and modify items.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-inventory">Inventory <a href="#inventory" id="inventory"></a></h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Inventory is tied to each user, listing the items that user owns and the quantity. A user's inventory is accessible across all applications. Once an Item exists, you may use Cloud Functions to assign inventory items to users in the system. Additionally, you may use SUPERUSER APIs to award items to users for administrative purposes.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="h-fungible-inventory-item">Fungible Inventory Item <a href="#fungible-inventory-item" id="fungible-inventory-item"></a></h4>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Fungible inventory items exist as stacks in the user's inventory. Common examples for fungible items may be coins or potions in a game, credits for something else redeemable, or simply points accrued for playing a game.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Fungible items exist at he USER level only and will be visible to all applications within Elements. See <a href="https://namazustudios.com/elementsmanual/web2/Application/">Application</a> scoping rules for more details.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="h-distinct-inventory-item">Distinct Inventory Item <a href="#distinct-inventory-item" id="distinct-inventory-item"></a></h4>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Distinct inventory items exist as single objects in the database. Unlike Fungible items, Distinct items may exist in multiples in the inventory. Additionally, they are not stackable. Distinct items would be useful to represent digital goods such as a sword, gun, or other in-game inventory item. In addition to the base Metadata, Distinct Inventory Items have their own metadata. This is useful, for example, if your game has degradation logic for particular items. For example, a sword can eventually wear out until it needs repair, or can gain experience as you defeat enemies with it.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-managing-items-using-the-console">Managing Items using the Console <a href="#managing-items-using-the-console" id="managing-items-using-the-console"></a></h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Items are managed in the Digital Goods section of the admin console, which can be accessed from the upper nav bar or in the hamburger menu.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>The "Add Item" button will open the new item panel.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Items can be edited by tapping the "Edit" button next to that item, or can be deleted by tapping the "Delete" button. Use the search function to more easily find specific items.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":22357,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://namazustudios.com/wp-content/uploads/2025/08/Screenshot-2026-02-01-at-3.57.36-PM-1024x821.png" alt="" class="wp-image-22357"/><figcaption class="wp-element-caption">Item Editor</figcaption></figure>
<!-- /wp:image -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="h-editing-item-metadata">Editing Item Metadata <a href="#editing-item-metadata" id="editing-item-metadata"></a></h4>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>There are several to edit metadata on items. One is to add it free-form, adding key/value pairs directly. Simple click Add Entry and then add the values that you want. </p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>The other way is to assign a Metadata Spec, which would have to be defined beforehand. Metadata Specs are a great way to pre-format what you want the metadata to look for for a set of items, and force the metadata to adhere to that structure.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>You can also switch to the JSON Editor tab, where you can edit metadata directly in a JSON format.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":22358,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://namazustudios.com/wp-content/uploads/2025/08/Screenshot-2026-02-01-at-3.59.25-PM-1024x353.png" alt="" class="wp-image-22358"/></figure>
<!-- /wp:image -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="h-json-structure-of-items">JSON Structure of Items <a href="#json-structure-of-items" id="json-structure-of-items"></a></h4>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>This is a sample item represented in JSON:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>&#91;{
        "name": "name",
        "tags": &#91;"tag1","tag2"],
        "displayName": "Display Name",
        "description": "This is a description.",
        "metadata": {
            "metadata_string": "This is a string",
            "metadata_int": 1
        }
}]</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->
