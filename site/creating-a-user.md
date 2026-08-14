<h1>What is a User?</h1>

<!-- wp:paragraph -->
<p>The User object is the primary representation of a person's account in Elements. The User is what helps connect Profiles, Inventory, Progress, and other collections inside of Elements.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="h-user-uid">User UID</h4>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Each authentication method maps to a User UID internally, which in turn keeps a reference to the User. There can be any number of these User UIDs, but each scheme can only exist once per User. In other words, there can be any number of different ways to access the User, but that User can only have a single Google Id, Apple Id, Steam Id, etc.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":21954} -->
<figure class="wp-block-image"><img src="https://namazustudios.com/wp-content/uploads/2025/08/spaces2FSwaCRaceHc67DqQUJ3ZN2Fuploads2FGSbhhPYZbLGpqJDZGbDY2Fimage-scaled.png" alt="" class="wp-image-21954"/></figure>
<!-- /wp:image -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="h-profiles">Profiles</h4>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Many of the core features are associated directly with a Profile. Elements uses a one-to-many model for the User / Profile relationship. A Profile is linked directly to both one single Application and one single User.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>One example might be a game where you might have your account login (User) and several characters that you can create with individual character attributes and story progress (Profile).</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":21953} -->
<figure class="wp-block-image"><img src="https://namazustudios.com/wp-content/uploads/2025/08/spaces2FSwaCRaceHc67DqQUJ3ZN2Fuploads2FPqkxzQjU0zBFVQhkSCae2Fimage.png" alt="" class="wp-image-21953"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>As Elements is a multi-tenant system, there can be many Applications that allow for many Profiles per Application.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Some collections, such as Inventory, are associated directly with the User object, and can be accessed across multiple Profiles. However, other collections reference the individual Profile directly.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-so-how-do-i-make-a-user">So how do I make a user…?</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Ah right! There are a few different ways to make a user, but without diving into a custom Element, here are the three simplest.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>First, we can use the admin console as a SUPERUSER to create new users (see <a href="../core-features/users-and-profiles">Users and Profiles</a>).</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Second, we can use the <a href="https://namazustudios.com/docs/namazu-elements-core/user-authentication-sign-in/user-authentication-in-elements/#h-authentication-methods">SignUp API </a>from our frontend code for basic user id / password auth. Every field on the signup request is optional — <code>name</code>, <code>email</code>, and <code>password</code> can all be omitted, in which case Elements generates them for you (a random name, an <code>&lt;name&gt;@anonymous.invalid</code> placeholder email, and a random password). Because a generated password can't be seen anywhere else, it's returned once in the signup response. You can also pass a <code>profiles</code> array on the same request to create one or more Profiles for the new User in the same call, rather than creating them separately afterward.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><em>(Elements 3.9+)</em> Instead of (or in addition to) an explicit <code>profiles</code> entry, you can pass <code>autoCreateProfileApplicationId</code> (an application name or ID) on the user-create/signup request to have Elements automatically create the user's primary profile for that <a href="applications">Application</a> -- but only if the Application is configured for it (see <code>autoCreateProfile</code> and <code>maxProfiles</code> in <a href="applications">Applications</a>); otherwise the field is a no-op. Naming the same application in both <code>autoCreateProfileApplicationId</code> and <code>profiles</code> is a <code>400 Bad Request</code>, since the two mechanisms conflict for that application.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Lastly, we can use any <a href="../core-features/auth-schemes/oidc">OIDC</a>, <a href="../core-features/auth-schemes/oauth2">OAuth2</a>, or <a href="../core-features/auth-schemes/custom-auth-schemes">Custom Auth</a> to sign in, which will automatically create a user with the info in the token if one doesn't exist yet.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><a href="../docs/core-features/user-authentication-sign-in/user-authentication-in-elements">Check out here for more info on User creation and authentication</a></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->
