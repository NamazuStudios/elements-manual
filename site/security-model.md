<h1>Security Model</h1>

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"level":6} -->
<h6 class="wp-block-heading" id="h-elements-offers-a-proven-security-model-with-three-levels-of-access-for-users-take-advantage-of-sso-with-facebook-and-apple-accounts-or-native-account-login-all-with-profile-management">Elements offers a proven security model with three levels of access for users. Take advantage of SSO with Facebook and Apple accounts, or native account login - all with profile management.</h6>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Elements uses a three-tiered security model through a single API. When accessing the API a user may access at one of three access levels. Briefly, these include:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>Anonymous</strong> - the user is not logged in or has not provided any valid access credentials.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>User</strong> - the user is a regular user and can access some APIs intended for the general purpose users.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Superuser</strong> - the user can access full APIs which generally includes access to all records in the system.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-user-access-levels-detailed-overview">User Access Levels (Detailed Overview) <a href="#levels-of-access-for-users" id="levels-of-access-for-users"></a></h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Currently, a user may have one of the three following access levels. This applies to the User's entire account. Across all of Elements, the security model applies as follows:</p>
<!-- /wp:paragraph -->

<!-- wp:list {"ordered":true} -->
<ol class="wp-block-list"><!-- wp:list-item -->
<li><strong>Anonymous</strong> (or Unprivileged) provides access to information that is considered public. Few APIs will supply information when used without any kind of credentials. Some APIs may only return limited sets of data at this level. Anonymous is the default access level and tends to grant very little access. If the client supplies no credentials, Elements will process all requests at this level. It is possible to greatly restrict a logged-in User's access by assigning this level. This will give the user the same access level as if they were not logged in. It may, however, still allow for valid session creation.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>User</strong> is the level of access for all authenticated users. This is what the average user will use when accessing your applications. When creating new accounts, the system will automatically assign users to this level of access. User is the access level for normal users. Additionally, it is the default level for making new accounts. Only a Superuser may escalate a user's access level allowing for access to the whole system. In general, users of your application should always be assigned this level.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Superuser</strong> allows for complete control over the system. APIs may return all information requested. Admins may perform operations such editing user accounts, resetting passwords for other users in the system, or adjust the application parameters. Using the <a href="https://namazustudios.com/elementsmanual/quickstart/console/WebConsole">admin console</a> requires superuser access.</li>
<!-- /wp:list-item --></ol>
<!-- /wp:list -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"Note"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="3b0649"><div class="gb-notice-title" style="color:#fff"><p>Note</p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p>Important:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Elements intercepts incoming requests as soon as possible, reads credentials information, and applies scope based on the user-supplied credentials.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>For a majority of endpoints, including those defined as cloud functions in the scripting engine, the user scope will be set before the presentation layer receives the code.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Elements will instantiate the specific version of a <a href="https://namazustudios.com/elementsmanual/general/NTieredArchitecture/#service-layer">Service</a> based on the access level.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:paragraph -->
<p>Additionally, when troubleshooting, it is often times useful to fetch the version endpoint which prints out the revision, build time, and version number by visiting this link in your local browser <a href="http://localhost:8080/api/rest/version">http://localhost:8080/api/rest/version</a>. This information should also be visible upon logging in to the admin console.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>At the time of this writing, Elements does not support full user segmentation by group and permission scheme. This is a feature slated for future releases. However, future versions will still operate against group-based access.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-signing-in-from-your-client-code">Signing in from your client code</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Username/password sign in is the default method of authentication within Elements which we'll use the for the example below. <a href="https://namazustudios.com/docs/namazu-elements-core/user-authentication-sign-in/user-authentication-in-elements/">See here</a> for more information on OIDC/OAuth2 sign in. Regardless of the method, you will get back a session token, as described below.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-create-new-user">Create new User <a href="#create-new-user" id="create-new-user"></a></h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>When someone signs up for Elements (via the <code>UserSignupApi</code>), they must provide a user name, a password, and an email address. This will provide you with the new User object in the response (but does not create a session token yet!), which will be needed for the next step.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-sign-in-with-existing-user">Sign in with existing User <a href="#sign-in-with-existing-user" id="sign-in-with-existing-user"></a></h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>After creating the new User, or if someone has already created a User in Elements, they can retrieve a session token with just the Username and Password via the <strong>UsernamePasswordSessionApi</strong>. This will return a <strong>SessionCreation</strong> object that contains the <strong>SessionSecret</strong> property, which is your session token.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Once the session token has been retrieved, you can then authenticate User related requests by adding the session token to the singleton client configuration in your generated code. For example:</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>C#
Headers&#91;"Elements-SessionSecret"] = &lt;SessionSecret&gt;;</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"Note"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="3b0649"><div class="gb-notice-title" style="color:#fff"><p>Note</p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p>There will likely be two configurations, one for Elements and one for your application. It will be necessary to add the same session secret to both of these.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:paragraph -->
<p>Adding this will automatically add the session token to any requests that require auth.</p>
<!-- /wp:paragraph -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"Warning","noticeBackgroundColor":"#ffdd57"} -->
<div style="color:#32373c;background-color:#ffdd57" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="0eaadb"><div class="gb-notice-title" style="color:#fff"><p>Warning</p></div><div class="gb-notice-text" style="border-color:#ffdd57"><!-- wp:paragraph -->
<p>Most requests are profile related, so we are not quite done with the login process yet.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:paragraph -->
<p>Now that the session secret for the User has been acquired, we can either retrieve all of the Profiles for that User, or create a new Profile.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-create-new-profile">Create New Profile <a href="#create-new-profile" id="create-new-profile"></a></h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Creating a new Profile will require a reference to the Application that it's being created for. See <a href="https://namazustudios.com/elementsmanual/web2/UsersAndProfiles/">Users and Profiles</a> for more information on the relationship structure. The <strong>CreateProfileRequest</strong> object used to create a new Profile contains the following properties:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>UserId (Required)</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>ApplicationId (Required)</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>DisplayName (Required, can be changed later)</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>ImageUrl (Optional, can be changed later)</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>The <strong>CreateProfile</strong> request will give you the created Profile object if the request was successful.</p>
<!-- /wp:paragraph -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"Note"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="3b0649"><div class="gb-notice-title" style="color:#fff"><p>Note</p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p>It is possible to create a Profile with the signup request. If you have done this, and you only ever intend for the User to have one Profile for your Application, then it's perfectly reasonable to cache the profileId somewhere and reuse it when assigning the session token to the headers. However, it's still a good idea to implement the next section as a backup in case the cache is cleared or the user signs in from a different device.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-fetch-existing-profiles">Fetch Existing Profiles <a href="#fetch-existing-profiles" id="fetch-existing-profiles"></a></h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>If someone has already created a Profile and simply wishes to log back in to that Profile, then you can retrieve all profiles for a User and Application using the <strong>GetProfiles</strong> via the <strong>ProfilesApi</strong>. At this point it's up to you to decide whether to let someone choose their Profile (for example, if you have a game that allows for multiple characters) or to choose for them.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-add-profile-id-to-headers">Add Profile Id to Headers <a id="add-profile-id-to-session-key" href="#add-profile-id-to-session-key"></a></h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Once the Profile has been acquired, to authenticate any Profile specific requests (e.g. Progress), the Profile Id can be added as a separate header:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>C#
Headers&#91;"Elements-ProfileId"] = &lt;Profile Id&gt;</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>and with this, the login process is complete!</p>
<!-- /wp:paragraph -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"Note"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="3b0649"><div class="gb-notice-title" style="color:#fff"><p>Note</p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p>Please review the <a href="../../core-features/sessions#scoping-rules">Sessions &gt; Scoping Rules</a> documentation to ensure you've provided the proper access levels to new users.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->
