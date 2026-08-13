<h1>CMS Feature Overview</h1>

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-dashboard">Dashboard</h2>
<!-- /wp:heading -->

<!-- wp:image {"id":22385,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://namazustudios.com/wp-content/uploads/2026/02/Screenshot-2026-02-03-at-8.55.42-AM-1024x575.png" alt="" class="wp-image-22385"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>The dashboard is where you land right after logging in. It currently displays the results of the health check endpoint. This will also eventually be where analytics info lives.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-core-elements">Core Elements</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The Core Elements features are split into several categories, each with several pages. Each page will show a list view that allows your to view, modify, create, and delete objects related to that page. </p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>When creating a new object, you can navigate away from that window if need be, and it will save the draft in progress. This can be useful when you need to create a dependency of the object that you're working on. For example, you are defining a new Mission, but forgot to make the Item that represents the reward.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>There is also a Refresh button near the top of each page. For speed purposes, we cache the query results locally. However, in a live product, things can be constantly changing. If you need to force the page to fetch the latest results, you can do this via the Refresh button.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Each page is also paginated, and often only fetches a subset of the whole. This helps to ensure that you don't lock up the system when querying larger data sets. See <a href="#h-settings">Settings</a> for help on changing the number of results that you see per page.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-accounts">Accounts</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The accounts section is where the user account and application information are managed.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="h-users">Users</h4>
<!-- /wp:heading -->

<!-- wp:image {"id":22361,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://namazustudios.com/wp-content/uploads/2026/02/Screenshot-2026-02-02-at-2.46.11-PM-1024x516.png" alt="" class="wp-image-22361"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>Here, you can access the User management tools which will be useful for support operations such as manual password reset, inventory management, information correction, and more. </p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>The search bar will search for all Name and Email fields that contain the search string. For example, searching for "bag" would return users named "bagel" and "baguette@bread.com", but not "badminton".</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>When using a username + password to sign in, a name or email is required by the system. When using another method such as OIDC or OAuth2, Elements will link your user to the external account. These accounts are all listed under Linked Accounts. See <a href="https://namazustudios.com/docs/namazu-elements-core/user-authentication-sign-in/creating-a-user/">What is a User?</a> for more information on how Users are structured.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>The available actions are, from left to right: <strong>View Inventory, Edit User, Copy User, Delete User</strong></p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="h-applications">Applications</h4>
<!-- /wp:heading -->

<!-- wp:image {"id":22362,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://namazustudios.com/wp-content/uploads/2026/02/Screenshot-2026-02-02-at-3.05.13-PM-1024x371.png" alt="" class="wp-image-22362"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>The Applications page allows for you to create new and manage existing Applications. Creating an Application is a required first step for a variety of other actions, such as creating Profiles, and adding custom code to/extending Elements' functionality. See here for more information on <a href="https://namazustudios.com/docs/namazu-elements-core/features/applications/">Applications</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":22365,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://namazustudios.com/wp-content/uploads/2026/02/Screenshot-2026-02-02-at-3.18.14-PM-1024x774.png" alt="" class="wp-image-22365"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>Applications also have what are called Application Configurations to help you tie your Application to other services. This can include various things, such as:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>IAP platforms (such as Google Play and Apple) and how you map your IAP SKU to digital goods in Elements.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Firebase configuration for push notifications</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Matchmaking configuration for use with the <a href="https://namazustudios.com/docs/configuration/matchmaking/">Matchmaking API</a> or <a href="https://namazustudios.com/docs/add-ons/custom-elements/crossplay-gaming-backend/namazu-crossfire/">Namazu Crossfire</a></li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="h-profiles">Profiles</h4>
<!-- /wp:heading -->

<!-- wp:image {"id":22363,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://namazustudios.com/wp-content/uploads/2026/02/Screenshot-2026-02-02-at-3.12.23-PM-1024x466.png" alt="" class="wp-image-22363"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>Profiles are the Application specific info for a User. If you think of a User as the overarching account, then a Profile would be your character info for a game. See <a href="https://namazustudios.com/docs/namazu-elements-core/user-authentication-sign-in/creating-a-user/">What is a User?</a> for more information on how Profiles relate to Users.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>The available actions are, from left to right: <strong>View Inventory, Edit Profile, Copy <strong>Profile</strong>, Delete <strong>Profile</strong></strong></p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="h-receipts">Receipts</h4>
<!-- /wp:heading -->

<!-- wp:image {"id":22364,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://namazustudios.com/wp-content/uploads/2026/02/Screenshot-2026-02-02-at-3.15.45-PM-1024x341.png" alt="" class="wp-image-22364"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>This shows you the raw receipt data. Since receipts can come from a variety of sources and can be formatted differently based on that source, we store the receipt's raw data attached to our generic Receipt model. This is mainly used to verify purchases that the user made and help resolve any issues they may have had in redeeming an item, or restoring a purchase. Search will look through either the Original Transaction Id or the Schema.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-game">Game</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>This section contains pages more directly related to the game itself.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="h-items">Items</h4>
<!-- /wp:heading -->

<!-- wp:image {"id":22366,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://namazustudios.com/wp-content/uploads/2026/02/Screenshot-2026-02-02-at-3.37.28-PM-1024x469.png" alt="" class="wp-image-22366"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>The Items page is where all of the Item definitions are located. It is necessary to define an Item before you can set it as a reward for missions or add it to a User's inventory. See <a href="https://namazustudios.com/docs/namazu-elements-core/features/digital-goods/">Digital Goods</a> for more information.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="h-missions">Missions</h4>
<!-- /wp:heading -->

<!-- wp:image {"id":22367,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://namazustudios.com/wp-content/uploads/2026/02/Screenshot-2026-02-02-at-3.37.50-PM-1024x395.png" alt="" class="wp-image-22367"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>This is where you can define all of the information for a Mission in your game. Missions can get pretty detailed, allowing you to define multiple steps and rewards for each step along the way. A Final Repeat Step can also be assigned in the event that you want it to be repeatable by the user. Once a Mission is started by the User, it becomes a Progress object, which tracks the User's current Step. See <a href="https://namazustudios.com/docs/namazu-elements-core/features/progress-and-missions-3-4/">Missions and Progress</a> for more information.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="h-schedules">Schedules</h4>
<!-- /wp:heading -->

<!-- wp:image {"id":22368,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://namazustudios.com/wp-content/uploads/2026/02/Screenshot-2026-02-02-at-3.37.58-PM-1024x309.png" alt="" class="wp-image-22368"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>Schedules are ways to create timed or recurring events. Some games have special seasonal, monthly, weekly, and even daily missions to help incentivize users to return and re-engage with their game. Schedules are assigned to a User's Progress when they start a Mission.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="h-leaderboards">Leaderboards</h4>
<!-- /wp:heading -->

<!-- wp:image {"id":22369,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://namazustudios.com/wp-content/uploads/2026/02/Screenshot-2026-02-02-at-3.38.19-PM-1024x343.png" alt="" class="wp-image-22369"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>The Leaderboard section allows for you to create and modify Leaderboards. See here for more information on how to define a <a href="https://namazustudios.com/docs/namazu-elements-core/features/leaderboards/">Leaderboard</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="h-matchmaking">Matchmaking</h4>
<!-- /wp:heading -->

<!-- wp:image {"id":22370,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://namazustudios.com/wp-content/uploads/2026/02/Screenshot-2026-02-02-at-3.38.57-PM-1024x244.png" alt="" class="wp-image-22370"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>The Matchmaking section allows for you to see all current matches. You can also manually create matches, as well as delete one, or all matches. This can be useful for quick iterations when debugging your game, as well as player support operations.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-auth">Auth</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>This section allows you to define and edit various types of auth schemes to provide different ways to allow your users to log in and link accounts. See here for an overview on <a href="https://namazustudios.com/docs/namazu-elements-core/user-authentication-sign-in/auth-schemes/auth-schemes/">Auth Schemes</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="h-oidc">OIDC</h4>
<!-- /wp:heading -->

<!-- wp:image {"id":22371,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://namazustudios.com/wp-content/uploads/2026/02/Screenshot-2026-02-02-at-3.41.10-PM-1024x368.png" alt="" class="wp-image-22371"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>Open ID Connect, or OIDC, is an authentication flow used by many auth providers, such as Google, Apple, Meta, and more. Elements will configure several defaults for you, but you can add or modify others as you like. Be sure to follow the documentation for the provider that you're adding. See here for more information on <a href="https://namazustudios.com/docs/namazu-elements-core/user-authentication-sign-in/auth-schemes/oidc/">OIDC Auth Schemes</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="h-oauth2">OAuth2</h4>
<!-- /wp:heading -->

<!-- wp:image {"id":22372,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://namazustudios.com/wp-content/uploads/2026/02/Screenshot-2026-02-02-at-3.41.26-PM-1024x277.png" alt="" class="wp-image-22372"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>OAuth2 is the overarching methodoloy behind OIDC. While OIDC is a specific implementation, OAuth2 can be implemented in a variety of ways, and typically requires a server to server call. The OAuth2 Auth Schemes in Elements similarly have a lot of configuration options to be compatible with the requirements of different providers, such as Steam or Oculus. See here for more information on <a href="https://namazustudios.com/docs/namazu-elements-core/user-authentication-sign-in/auth-schemes/oauth2/">OAuth2 Auth Schemes</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="h-custom">Custom</h4>
<!-- /wp:heading -->

<!-- wp:image {"id":22373,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://namazustudios.com/wp-content/uploads/2026/02/Screenshot-2026-02-02-at-3.42.05-PM-1024x295.png" alt="" class="wp-image-22373"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>Custom Auth Schemes can be defined as well for use with any custom auth flows that you might want to author. This will generate a public/private key pair with encryption options and other metadata that you can configure.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-metadata">Metadata</h3>
<!-- /wp:heading -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="h-metadata-0">Metadata</h4>
<!-- /wp:heading -->

<!-- wp:image {"id":22374,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://namazustudios.com/wp-content/uploads/2026/02/Screenshot-2026-02-02-at-3.52.37-PM-1024x380.png" alt="" class="wp-image-22374"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>This page allows for you to define global Metadata objects. These Metadata objects can be as simple or complex as you want, from a simple version number to a full configuration. An access level can also be defined if you don't want your average user to be able to access specific information, for example a configuration that is only meant to be accessed via your custom Element code.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="h-metadata-spec">Metadata Spec</h4>
<!-- /wp:heading -->

<!-- wp:image {"id":22375,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://namazustudios.com/wp-content/uploads/2026/02/Screenshot-2026-02-02-at-3.54.06-PM-1024x445.png" alt="" class="wp-image-22375"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>This page allows for you to create and manage Metadata Specs. These allow you to define the structure that a Metadata object or a metadata field on another object (such as Items or Profiles). This can be very powerful as it lets you predefine how Metadata should be handled and what the various field names can be on different objects, which effectively allows for you to extend the functionality of existing object types within Elements. See <a href="https://namazustudios.com/docs/namazu-elements-core/features/metadata-3-4/">Metadata</a> for more information.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-web3">Web3</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>If you want to connect to a chain or specific contract and maintain a custodial system for your users, this is where you can define the smart contract info. Elements also has a system to secure user information (wallet/account info) in what are called Vaults.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="h-smart-contracts">Smart Contracts</h4>
<!-- /wp:heading -->

<!-- wp:image {"id":22377,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://namazustudios.com/wp-content/uploads/2026/02/Screenshot-2026-02-02-at-4.10.18-PM-1024x299.png" alt="" class="wp-image-22377"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>Here you can define contracts and set their address for reference in your custom code. Once defined, you can call into the contracts directly from your custom backend code to perform actions on behalf of the user. It is also possible to define a master wallet which can sponsor transaction fees on behalf of the user as well. See here for more information on <a href="https://namazustudios.com/docs/namazu-elements-core/features/web3/omni-chain-support/">Smart Contracts</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="h-vaults">Vaults</h4>
<!-- /wp:heading -->

<!-- wp:image {"id":22378,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://namazustudios.com/wp-content/uploads/2026/02/Screenshot-2026-02-02-at-4.10.33-PM-1024x271.png" alt="" class="wp-image-22378"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>Here you can view and manage user wallets for different networks, all under one vault for that user. See here for more information on <a href="https://namazustudios.com/docs/namazu-elements-core/features/web3/wallets/">Wallets</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-other">Other</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="h-large-object">Large Object</h4>
<!-- /wp:heading -->

<!-- wp:image {"id":22376,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://namazustudios.com/wp-content/uploads/2026/02/Screenshot-2026-02-02-at-4.02.17-PM-1024x407.png" alt="" class="wp-image-22376"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>Here is where you can upload and store arbitrary data. Depending on the type of data, you can also preview the image. This is useful for live ops, A/B testing, and other actions where you might want to store files and other game assets in Elements directly.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-api-explorer">API Explorer</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="h-core-api">Core API</h4>
<!-- /wp:heading -->

<!-- wp:image {"id":22379,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://namazustudios.com/wp-content/uploads/2026/02/Screenshot-2026-02-02-at-4.04.47-PM-1024x498.png" alt="" class="wp-image-22379"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>The Core API section allows you to look at the entirety of the Elements API and test out endpoints. It downloads a generated Open API Specification 3 (OAS3) file and creates UI dynamically. This can be useful for quick testing or reference. For most requests, it will use the session token that allows you to access the CMS. This can be overridden for some requests if you want to test endpoints as a different User.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="h-core-elements-0">Core Elements</h4>
<!-- /wp:heading -->

<!-- wp:image {"id":22381,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://namazustudios.com/wp-content/uploads/2026/02/Screenshot-2026-02-02-at-4.20.54-PM-1024x355.png" alt="" class="wp-image-22381"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>The Core Elements section gives you information on the various exposed features in the Core Elements platform, which is useful when writing your own custom Element. This includes all DAO (Data Access Object) level, service level, and mongo level interfaces, along with links to the javadocs for each of these. Additionally, all events that you can receive are listed here as well. See here for more information on building a <a href="https://namazustudios.com/docs/custom-code/element-structure/">Custom Element</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="h-custom-elements">Custom Elements</h4>
<!-- /wp:heading -->

<!-- wp:image {"id":22382,"width":"167px","height":"auto","sizeSlug":"full","linkDestination":"none"} -->
<figure class="wp-block-image size-full is-resized"><img src="https://namazustudios.com/wp-content/uploads/2026/02/Screenshot-2026-02-02-at-4.20.14-PM.png" alt="" class="wp-image-22382" style="width:167px;height:auto"/></figure>
<!-- /wp:image -->

<!-- wp:image {"id":22380,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://namazustudios.com/wp-content/uploads/2026/02/Screenshot-2026-02-02-at-4.19.57-PM-1024x638.png" alt="" class="wp-image-22380"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>This section lists all of the Custom Elements that you have installed. Theses can be Elements that you developed, or plugins from third party sources. Elements are installed on a per-Application basis. Each Application can have multiple Elements installed. </p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>In the sidebar, the Application names will be listed, with each of the detected Elements under it. If the Elements all loaded properly, a small green dot will be next to it. Otherwise, you may see a yellow or red dot. Clicking on the Element will give you information about that Element, including an API explorer. If the Element failed to load properly, the logs can be found here as well.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-settings">Settings</h2>
<!-- /wp:heading -->

<!-- wp:image {"id":22383,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://namazustudios.com/wp-content/uploads/2026/02/Screenshot-2026-02-02-at-4.26.08-PM-1024x805.png" alt="" class="wp-image-22383"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>The cog icon in the top right corner of the screen will give you access to the Settings page. From here, you can toggle the visual theme of the CMS, set the results per page for paginated results, and toggle the visibility of various other pages (for instance, if you know that you'll never user a particular feature and want it out of the way). After saving your settings changes, you may need to refresh the page to apply them.</p>
<!-- /wp:paragraph -->
