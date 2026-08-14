<h1>Sessions</h1>

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"level":6} -->
<h6 class="wp-block-heading" id="h-elements-utilizes-sessions-in-order-for-the-client-application-to-securely-communicate-with-elements-apis">Elements utilizes Sessions in order for the client application to securely communicate with Elements APIs.</h6>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>All HTTP requests between the client application and Elements APIs can have a session key. Creating a Session may be completed through several API calls. There are multiple ways to create a session as detailed in this document.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Elements recognizes the following session headers:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><code>Authorization: [bearer] {secret}</code></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>secret</code> <strong>(Required):</strong> The Session secret (See <a href="#creation-methods">Creation Methods</a> below for more information.)</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>Elements-SessionSecret: {secret} [u{UserId}] [p{ProfileId}]</code></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>secret</code> <strong>(Required):</strong> The Session secret (See <a href="#creation-methods">Creation Methods</a> below for more information.)</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>userId</code> <strong>(Optional):</strong> The id of a User. If the secret matches a <a href="../users-and-profiles#user-properties">Super User</a> then this may be any other non Super User in Elements. This will cause the subsequent request to be executed as the specified User. For all other Users, this must be the identity associated with the session making the request. This will override the User <a href="#scoping-rules">Scoping Rules</a>.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>profileId</code> <strong>(Optional):</strong> The id of a Profile. If the secret matches a <a href="../users-and-profiles#user-properties">Super User</a> then this may be any Profile in Elements. For all other Users, this must be a Profile associated with the User making the request. Ths will override the <a href="#profile">Profile Scoping Rules</a>.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-creation-methods">Creation Methods <a href="#creation-methods" id="creation-methods"></a></h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>There are several ways to create sessions within Elements. Each section here describes the supported creation methods and how to implement each one of them. In all cases, Elements performs the account verification authoritatively, ensuring that the server has verified the credentials supplied to it.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="h-username-and-password">Username and Password <a href="#username-and-password" id="username-and-password"></a></h4>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>This the most basic method of creating a session and requires no third-party integration. When creating a User, the client supplies the desired username and password. Elements securely hashes each User's password in the database.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>To attach a Profile to the session, the request may specify one of <code>profileId</code>, <code>profileSelector</code>, or (<em>Elements 3.9+</em>) <code>applicationNameOrId</code> -- the name or ID of an <a href="applications">Application</a> whose primary profile should be attached. <code>applicationNameOrId</code> is only consulted if neither <code>profileId</code> nor <code>profileSelector</code> is specified, and if the Application or the user's primary profile for it can't be resolved, the session is simply created without a profile rather than failing the request. The same <code>applicationNameOrId</code> field is also accepted on OAuth2 session creation (see below).</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="h-sso-using-oidc-oauth2">SSO using OIDC / OAuth2</h4>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Elements provides the ability to create and customize Single Sign On (SSO) options for a variety of services, and even connect a single User to multiple external ids. These services must follow the OIDC or OAuth2 specifications. See <a href="auth-schemes">Auth Schemes</a> for more details on how this works. Note that logging in through one of these services never merges into a session you already hold — to attach an additional identity to an account you're already signed into, see <a href="account-linking">Account Linking</a> instead.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-scoping-rules">Scoping Rules <a href="#scoping-rules" id="scoping-rules"></a></h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Within the Elements <a href="../general/security-model">Security Model</a>, sessions may have two scopes: User or Profile. Some APIs require a session with a current Profile and not just a User. Generally these APIs are for application specific operations, such as Matchmaking.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="h-user">User <a href="#user" id="user"></a></h4>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>All sessions will have a User associated with them. Once created, they will always be valid for that User until it expires or the user changes their password.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="h-profile">Profile <a href="#profile" id="profile"></a></h4>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>A Session may, optionally, be scoped by a User and a Profile. The Profile's owner must match the User who created the session. APIs that require Profile scoping rules are as follows:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Fetching current Profile (eg GET /profile/current)</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Processing IAPs</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Leaderboards and Rankings</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Push Notifications</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Progress and Missions</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->
