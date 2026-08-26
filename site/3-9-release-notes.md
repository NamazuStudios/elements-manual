<h1>3.9 Release Notes</h1>

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"Warning","noticeBackgroundColor":"#ffdd57"} -->
<div style="color:#32373c;background-color:#ffdd57" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="d29f31"><div class="gb-notice-title" style="color:#fff"><p>Warning</p></div><div class="gb-notice-text" style="border-color:#ffdd57"><!-- wp:paragraph -->
<p>Elements 3.9 is still under active development (current version: <code>3.9.0-SNAPSHOT</code>) and has not been released. The contents of this page are a draft and may change before the final release.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:heading {"anchor":"h-overview"} -->
<h2 id="h-overview" class="wp-block-heading">Overview</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Elements 3.9 adds per-application profile limits and automatic primary-profile creation, a new way to attach a user's profile to a session by naming an Application instead of an explicit profile, and account linking for the OIDC browser-redirect login flow.</p>
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
<li><strong>Session creation by Application, including auto-create</strong> — username/password, OAuth2, and OIDC session requests can now pass <code>applicationNameOrId</code> to attach the user's primary profile for that Application, instead of an explicit <code>profileId</code>/<code>profileSelector</code>. If no primary profile exists yet, it's now created automatically, subject to the Application's own <code>autoCreateProfile</code>/<code>maxProfiles</code> settings. See <a href="sessions">Sessions</a>.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Authoritative profile pictures and display-name validation</strong> — new <code>authoritativeProfilePicture</code> and <code>displayNameRegex</code> settings on Application. See <a href="applications">Applications</a>.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Account linking via the OIDC browser-redirect flow</strong> — starting a login attempt while already holding a session now links the resulting external identity to that user instead of creating a new one, with a new <code>confirmToken</code>-gated confirmation step to keep the mutation off the unauthenticated provider callback. See <a href="oidc-login-for-thick-clients-browser-redirect-flow">OIDC Login for Thick Clients</a>.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Progress API fixes and a new advance-progress endpoint</strong> — <code>POST /progress</code> and the superuser <code>PUT /progress/{id}</code> path are fixed, and a new <code>POST /progress/{progressId}/advance</code> endpoint lets a Mission opt in to client-driven progress advancement. Reported, diagnosed, and prototyped by community contributor <a href="https://github.com/hobolabsdigital">@hobolabsdigital</a> -- thank you!</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Stale Datastore/Mapper fix on Element (re)deploy</strong> — a singleton that captured Elements' shared Mongo <code>Datastore</code> could go stale on the next Element (re)deploy; see below.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Opt-in Guice PRODUCTION stage for injectors</strong> — every Guice injector in the platform (per-Element, jetty-ws itself, the <code>migrate</code>/<code>setup</code> tools, and more) can now be built with Guice's <code>Stage.PRODUCTION</code> instead of the default <code>Stage.DEVELOPMENT</code>, via a new system property/environment variable. This pairs with the Datastore/Mapper fix above: capturing the shared <code>Datastore</code> in an eager singleton is safe now, so there's no new risk from the earlier construction timing under <code>PRODUCTION</code>. See below.</li>
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
<p>Username/password, OAuth2, and OIDC session requests accept a new <code>applicationNameOrId</code> field (an application name or ID). If neither <code>profileId</code> nor <code>profileSelector</code> is specified, Elements resolves the user's primary profile for that application and attaches it to the session. If no primary profile exists yet, one is now created automatically, subject to the same <code>autoCreateProfile</code>/<code>maxProfiles</code> gating as signup-time auto-create, via the same <code>ProfileDao#createSlottedProfile</code> path; if the application can't be resolved, or auto-create isn't configured for it, the session is simply created without a profile.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3,"anchor":"h-account-linking-via-the-oidc-browser-redirect-flow"} -->
<h3 id="h-account-linking-via-the-oidc-browser-redirect-flow" class="wp-block-heading">Account Linking via the OIDC Browser-Redirect Flow</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Starting an OIDC browser-redirect login attempt (<code>POST /oidc/session</code>) while already holding a session now links the resulting external identity to that user, the same way the existing <a href="account-linking">Account Linking</a> endpoints do for a possessed <code>id_token</code>. No new request field is involved; whether an attempt links or creates a new user is decided purely by whether the caller had a session when the attempt was started.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Because the provider's callback that validates the external identity is always an unauthenticated redirect from the identity provider, with no way to confirm it's the same caller that started the attempt, it no longer performs the account-link mutation itself. A new <code>confirmToken</code>, returned only in the original <code>begin()</code> response, gates a new <code>POST /oidc/session/{id}/confirm</code> step that performs it. See <a href="oidc-login-for-thick-clients-browser-redirect-flow">OIDC Login for Thick Clients</a> for the full sequence. This closes a case where a leaked <code>state</code> value, which (unlike the poll <code>id</code>) necessarily passes through the browser and the identity provider, could otherwise have let an attacker permanently link their own external identity to a victim's account.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3,"anchor":"h-authoritative-profile-pictures-and-display-name-validation"} -->
<h3 id="h-authoritative-profile-pictures-and-display-name-validation" class="wp-block-heading">Authoritative Profile Pictures and Display-Name Validation</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p><code>Application</code> gains two more new fields: <code>authoritativeProfilePicture</code> (defaults to <code>false</code>), which when <code>true</code> blocks a user from editing their own profile picture for that application via the REST API (it must be set by backend/Element code instead), and <code>displayNameRegex</code> (optional), a Java regular expression a profile's display name must match for that application -- profile creates/updates with a non-matching display name are rejected. Leave it blank to skip the check.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3,"anchor":"h-progress-api-fixes-and-advance-progress-endpoint"} -->
<h3 id="h-progress-api-fixes-and-advance-progress-endpoint" class="wp-block-heading">Progress API Fixes and Advance-Progress Endpoint</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p><code>POST /progress</code> no longer 400s with <code>"profile - must not be null"</code> when a valid profile is supplied, and the superuser <code>PUT /progress/{id}</code> path no longer rejects every possible request body. Both were reported with full root-cause analysis by community contributor <a href="https://github.com/hobolabsdigital">@hobolabsdigital</a> in <a href="https://github.com/NamazuStudios/elements/issues/2">#2</a> and <a href="https://github.com/NamazuStudios/elements/issues/3">#3</a> -- thank you for the thorough repros and the <code>sequence</code>/<code>currentStep</code> data-model deep-dive.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Mission gains a new <code>authoritative</code> field (defaults to <code>true</code>). A new <code>POST /progress/{progressId}/advance</code> endpoint decrements a Progress's remaining actions, advancing Steps and issuing Rewards as needed -- superusers may always call it, and a regular user may only call it for their own Progress on a Mission explicitly marked <code>authoritative: false</code>. This is the client-driven progress advancement @hobolabsdigital originally prototyped in #3, now gated per-Mission so authoritative-integrity is preserved by default.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3,"anchor":"h-guice-spi-loading-strategy-escape-hatch"} -->
<h3 id="h-guice-spi-loading-strategy-escape-hatch" class="wp-block-heading">Guice SPI Loading-Strategy Escape Hatch</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The package-level <code>@GuiceOptions</code> annotation is now wired into <code>GuiceSpiModule</code>, giving third-party Element authors an opt-in escape hatch for the <code>[Guice/ExposedButNotBound]</code> crash that can occur when an exported service has no locally-discovered implementation. Elements that don't declare <code>@GuiceOptions</code> see no behavior change -- the existing bind/expose scanning remains the default <code>LEGACY</code> strategy. Authors can instead declare <code>GUICE_MODULE_ONLY</code> to defer every exported service to their own <code>@GuiceElementModule</code>(s), or <code>STRICT</code> to fail fast at startup with a clear error naming the unbound service instead of Guice's generic crash. See <a href="introduction-to-guice-and-jakarta-in-elements">Introduction to Guice and Jakarta in Elements</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3,"anchor":"h-eager-singleton-construction-for-elements"} -->
<h3 id="h-eager-singleton-construction-for-elements" class="wp-block-heading">Opt-In Eager Singleton Construction (Guice Stage.PRODUCTION)</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Every Guice injector in the platform can now be built with <code>Stage.PRODUCTION</code> instead of the default <code>Stage.DEVELOPMENT</code>, controlled by the <code>dev.getelements.elements.guice.stage</code> system property (or the <code>ELEMENTS_GUICE_STAGE</code> environment variable if the property isn't set). Leaving both unset keeps today's <code>DEVELOPMENT</code> behavior everywhere, including for Element injectors -- <code>PRODUCTION</code> is strictly opt-in. A server deployment that wants the benefits below should set <code>dev.getelements.elements.guice.stage=PRODUCTION</code> (or the equivalent environment variable) in its own launch configuration.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>When enabled, two things change for Element authors:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Any service class you mark <code>@Singleton</code> is constructed at Element-load time, not lazily on first use. Previously only bindings explicitly marked <code>.asEagerSingleton()</code> in a Guice module were built eagerly; a plain <code>@Singleton</code> class was left to first use.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Every binding in your Element's injector is validated up front at load time, so a misconfigured binding fails fast when the Element loads instead of surfacing later at first invocation.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>This is safe to rely on together with the Datastore/Mapper fix below: injecting the shared <code>Datastore</code> into an eager singleton no longer risks capturing a stale snapshot, since the <code>Datastore</code> you receive is a stable proxy regardless of when it's constructed. If your own service binds another shared, mutable dependency directly (not through a similar proxy or a <code>Provider</code>), review whether earlier eager construction under <code>PRODUCTION</code> stage could now capture a stale reference to it.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"anchor":"h-bug-fixes"} -->
<h2 id="h-bug-fixes" class="wp-block-heading">Bug Fixes</h2>
<!-- /wp:heading -->

<!-- wp:heading {"level":3,"anchor":"h-stale-datastore-mapper-state-on-element-redeploy"} -->
<h3 id="h-stale-datastore-mapper-state-on-element-redeploy" class="wp-block-heading">Stale Datastore/Mapper State on Element Redeploy</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Any singleton that captured the injected Morphia <code>Datastore</code> directly could go stale the moment an Element (re)deploy rebuilt and swapped the shared <code>Datastore</code>/Mapper -- most reliably on every redeploy, since an Element's eager singletons are constructed before its own entities are even registered. The injected <code>Datastore</code> is now a stable proxy that always forwards to whichever instance is live at call time, so holding a reference to it -- in Elements' own internal DAOs, or in a downstream Element's -- is safe by construction.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->
