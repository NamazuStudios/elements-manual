<h1>3.9 Release Notes</h1>

<!-- wp:heading {"anchor":"h-overview"} -->
<h2 id="h-overview" class="wp-block-heading">Overview</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Elements 3.9 adds per-application profile limits and automatic primary-profile creation, plus a new way to attach a user's profile to a session by naming an Application instead of an explicit profile.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"anchor":"h-highlights"} -->
<h2 id="h-highlights" class="wp-block-heading">Highlights</h2>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>Per-application profile limits</strong> — a new <code>maxProfiles</code> setting on <a href="applications">Application</a> bounds how many profiles a user may create for it.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Automatic primary profile creation</strong> — a new <code>autoCreateProfile</code> setting on Application, combined with a new <code>autoCreateProfileApplicationNameOrId</code> field on the user-create/signup request, lets Elements create a user's primary profile for an Application automatically at signup time. See <a href="creating-a-user">Creating a User</a>.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Session creation by Application</strong> — username/password and OAuth2 session requests can now pass <code>applicationNameOrId</code> to attach the user's primary profile for that Application, instead of an explicit <code>profileId</code>/<code>profileSelector</code>. See <a href="sessions">Sessions</a>.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"anchor":"h-new-features"} -->
<h2 id="h-new-features" class="wp-block-heading">New Features</h2>
<!-- /wp:heading -->

<!-- wp:heading {"level":3,"anchor":"h-per-application-profile-limits-and-auto-create"} -->
<h3 id="h-per-application-profile-limits-and-auto-create" class="wp-block-heading">Per-Application Profile Limits and Auto-Create</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p><code>Application</code> gains two new fields: <code>maxProfiles</code> (defaults to <code>1</code>) caps how many profiles a user may create for that application, and <code>autoCreateProfile</code> (defaults to <code>true</code>) governs whether a user's primary profile is created automatically when requested via <code>autoCreateProfileApplicationNameOrId</code> on user creation. Lowering <code>maxProfiles</code> never affects profiles that already exist -- only new profile creations are gated. Existing applications with no value set for these fields behave as if they were set to the defaults.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3,"anchor":"h-session-creation-by-application"} -->
<h3 id="h-session-creation-by-application" class="wp-block-heading">Session Creation by Application</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Username/password and OAuth2 session requests accept a new <code>applicationNameOrId</code> field (an application name or ID). If neither <code>profileId</code> nor <code>profileSelector</code> is specified, Elements resolves the user's primary profile for that application and attaches it to the session; if the application or the primary profile can't be resolved, the session is simply created without a profile.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->
