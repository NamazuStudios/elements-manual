<h1>Friends</h1>

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"level":6} -->
<h6 class="wp-block-heading" id="h-elements-supports-the-concept-of-friends-between-two-users-which-establishes-a-relationship-that-offers-more-engagement-such-as-viewing-leaderboard-rankings-or-viewing-each-other-s-profile-details">Elements supports the concept of Friends between two users, which establishes a relationship that offers more engagement, such as viewing Leaderboard rankings or viewing each other's profile details.</h6>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Elements supports the concept of Friends between two Users. Friends are created at the User level and not at the Profile level. When two users are Friends, they have the following privileges.</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Friends may view <a href="leaderboards">Leaderboard</a> rankings relative to other friends.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Friends may view each other's <a href="users-and-profiles">Profiles</a> associated with their account.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>When one user views a User through a Friend, Elements will intentionally hide personal information.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Cloud Functions may perform friend checks and alter the behavior as the Friend API is fully exposed to the scripting engine.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>Elements models Friends as a bi-directional relationship between two <a href="users-and-profiles">Users</a>. The Friend model contains the following properties:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><a href="../../general/general-concepts#id-property"><strong>id</strong></a></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>user</strong>: The <a href="users-and-profiles">User</a> which is the other friend. This contains limited information on the User as some of it may be personally identificable.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>profiles</strong>: The <a href="users-and-profiles">Profiles</a> of the other User.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>friendship</strong>: Indicates the kind of friendship between the Users, based on any of these options below.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>NONE</strong>: There is no association between the users. This is used internally and APIs should almost never use this value.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>OUTGOING: T</strong>he logged-in User has requested friendship from the other user</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>INCOMING</strong>: The other User has requested friendship from the currently logged-in user</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>MUTUAL</strong>: Both users have accepted friendship.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-automatic-facebook-friends">Automatic Facebook Friends <a href="#automatic-facebook-friends" id="automatic-facebook-friends"></a></h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>When two Users connect with Facebook, Elements will automatically scan their friends list and mirror that association within Elements. This behavior is hardcoded in the <a href="applications/facebook-application-configuration">Facebook</a> workflow and cannot be disabled. Each new Facebook session forces a refresh of the friends list.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-associating-friends">Associating Friends <a href="#associating-friends" id="associating-friends"></a></h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>You must use authoritative code to make Friend associations. This ensures that your game or application can make friend associations using the rules of your application.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->
