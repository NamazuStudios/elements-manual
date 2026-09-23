<h1>3.9 Release Notes</h1>

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"Warning","noticeBackgroundColor":"#ffdd57"} -->
<div style="color:#32373c;background-color:#ffdd57" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="0eaadb"><div class="gb-notice-title" style="color:#fff"><p>Warning</p></div><div class="gb-notice-text" style="border-color:#ffdd57"><!-- wp:paragraph -->
<p>Elements 3.9 is in the release-candidate stage (current version: <code>3.9.0-rc-10</code>) and has not been finally released. The contents of this page are a draft and may change before the final release.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:heading {"anchor":"h-overview"} -->
<h2 id="h-overview" class="wp-block-heading">Overview</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Elements 3.9 adds per-application profile limits and automatic primary-profile creation, a new way to attach a user's profile to a session by naming an Application instead of an explicit profile, and account linking for the OIDC browser-redirect login flow. It also introduces editable, database-backed email templates with a CMS editor, single sign-on for the admin console via any configured OIDC provider, an operator-run database migration tool, and a system-wide password policy.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"anchor":"h-highlights"} -->
<h2 id="h-highlights" class="wp-block-heading">Highlights</h2>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>Per-application profile limits</strong>: a new <code>maxProfiles</code> setting on <a href="applications">Application</a> bounds how many profiles a user may create for it.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Automatic primary profile creation</strong>: a new <code>autoCreateProfile</code> setting on Application, combined with a new <code>autoCreateProfileApplicationNameOrId</code> field on the user-create/signup request, lets Elements create a user's primary profile for an Application automatically at signup time. See <a href="creating-a-user">Creating a User</a>.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Session creation by Application, including auto-create</strong>: username/password, OAuth2, and OIDC session requests (including the OIDC browser-redirect and account-linking flows) can now pass <code>applicationNameOrId</code> to attach the user's primary profile for that Application, instead of an explicit <code>profileId</code>/<code>profileSelector</code>. If no primary profile exists yet, it's now created automatically, subject to the Application's own <code>autoCreateProfile</code>/<code>maxProfiles</code> settings. See <a href="sessions">Sessions</a>.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Authoritative profile pictures and display-name validation</strong>: new <code>authoritativeProfilePicture</code> and <code>displayNameRegex</code> settings on Application. See <a href="applications">Applications</a>.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Account linking via the OIDC browser-redirect flow</strong>: starting a login attempt while already holding a session now links the resulting external identity to that user instead of creating a new one, with a new <code>confirmToken</code>-gated confirmation step to keep the mutation off the unauthenticated provider callback. See <a href="oidc-login-for-thick-clients-browser-redirect-flow">OIDC Login for Thick Clients</a>.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Editable email templates</strong>: transactional email subjects and bodies are now persistent, editable records with a full editor in the CMS, replacing hardcoded defaults. See <a href="email-templates">Email Templates</a>.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>OIDC single sign-on for the admin console</strong>: the admin panel's login page can now offer sign-in via any configured OIDC provider. See below.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Operator-run database migration tool</strong>: a tracked, idempotent <code>migrate</code> CLI ships on the server Docker image for schema migrations and index upkeep, starting with a profile-slot backfill for older deployments. See below.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Progress API fixes and a new advance-progress endpoint</strong>: <code>POST /progress</code> and the superuser <code>PUT /progress/{id}</code> path are fixed, and a new <code>POST /progress/{progressId}/advance</code> endpoint lets a Mission opt in to client-driven progress advancement. Reported, diagnosed, and prototyped by community contributor <a href="https://github.com/hobolabsdigital">@hobolabsdigital</a>, thank you!</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>System-wide password policy</strong>: a new deploy-time regex config gates every password a client submits (signup, password reset, admin-set password, account linking), paired with a human-readable description shown in the validation error. See below.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Stale Datastore/Mapper fix on Element (re)deploy</strong>: a singleton that captured Elements' shared Mongo <code>Datastore</code> could go stale on the next Element (re)deploy; see below.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>CRUD events across nearly every DAO</strong>: create/update/delete events, previously only produced by a handful of DAOs, now cover nearly all of them, making it practical to build audit logging and similar cross-cutting Elements. See <a href="events">Events</a>.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Guice SPI loading-strategy escape hatch</strong>: the package-level <code>@GuiceOptions</code> annotation lets third-party Element authors opt out of legacy bind/expose scanning in favor of their own Guice modules, with an optional strict mode that fails fast at load time. See below.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"anchor":"h-new-features"} -->
<h2 id="h-new-features" class="wp-block-heading">New Features</h2>
<!-- /wp:heading -->

<!-- wp:heading {"level":3,"anchor":"h-per-application-profile-limits-and-auto-create"} -->
<h3 id="h-per-application-profile-limits-and-auto-create" class="wp-block-heading">Per-Application Profile Limits and Auto-Create</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p><code>Application</code> gains two new fields: <code>maxProfiles</code> (defaults to <code>1</code>) caps how many profiles a user may create for that application, and <code>autoCreateProfile</code> (defaults to <code>true</code>) governs whether a user's primary profile is created automatically when requested via <code>autoCreateProfileApplicationNameOrId</code> on user creation. Lowering <code>maxProfiles</code> never affects profiles that already exist, only new profile creations are gated. Existing applications with no value set for these fields behave as if they were set to the defaults.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3,"anchor":"h-session-creation-by-application"} -->
<h3 id="h-session-creation-by-application" class="wp-block-heading">Session Creation by Application</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Username/password, OAuth2, and OIDC session requests accept a new <code>applicationNameOrId</code> field (an application name or ID). If neither <code>profileId</code> nor <code>profileSelector</code> is specified, Elements resolves the user's primary profile for that application and attaches it to the session. If no primary profile exists yet, one is now created automatically, subject to the same <code>autoCreateProfile</code>/<code>maxProfiles</code> gating as signup-time auto-create, via the same <code>ProfileDao#createSlottedProfile</code> path; if the application can't be resolved, or auto-create isn't configured for it, the session is simply created without a profile.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>The same field is now honored across the rest of the OIDC machinery. The browser-redirect flow's begin request (<code>POST /oidc/session</code>) persists <code>applicationNameOrId</code> on the login attempt and uses it at callback time, and account-linking attempts do the same, falling back to the <code>id_token</code>'s own <code>aud</code> claim when the attempt didn't name an application explicitly. A bug where the OAuth2 auto-create path built a profile with no display name (and therefore always failed validation) is also fixed; auto-created profiles now get a generated display name, matching the OIDC path.</p>
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
<p><code>Application</code> gains two more new fields: <code>authoritativeProfilePicture</code> (defaults to <code>false</code>), which when <code>true</code> blocks a user from editing their own profile picture for that application via the REST API (it must be set by backend/Element code instead), and <code>displayNameRegex</code> (optional), a Java regular expression a profile's display name must match for that application. Profile creates/updates with a non-matching display name are rejected. Leave it blank to skip the check.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3,"anchor":"h-progress-api-fixes-and-advance-progress-endpoint"} -->
<h3 id="h-progress-api-fixes-and-advance-progress-endpoint" class="wp-block-heading">Progress API Fixes and Advance-Progress Endpoint</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p><code>POST /progress</code> no longer 400s with <code>"profile - must not be null"</code> when a valid profile is supplied, and the superuser <code>PUT /progress/{id}</code> path no longer rejects every possible request body. Both were reported with full root-cause analysis by community contributor <a href="https://github.com/hobolabsdigital">@hobolabsdigital</a> in <a href="https://github.com/NamazuStudios/elements/issues/2">#2</a> and <a href="https://github.com/NamazuStudios/elements/issues/3">#3</a>, thank you for the thorough repros and the <code>sequence</code>/<code>currentStep</code> data-model deep-dive.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Mission gains a new <code>authoritative</code> field (defaults to <code>true</code>). A new <code>POST /progress/{progressId}/advance</code> endpoint decrements a Progress's remaining actions, advancing Steps and issuing Rewards as needed. Superusers may always call it, and a regular user may only call it for their own Progress on a Mission explicitly marked <code>authoritative: false</code>. This is the client-driven progress advancement @hobolabsdigital originally prototyped in #3, now gated per-Mission so authoritative integrity is preserved by default.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3,"anchor":"h-system-wide-password-policy"} -->
<h3 id="h-system-wide-password-policy" class="wp-block-heading">System-Wide Password Policy</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Two new deploy-time configuration values, <code>dev.getelements.elements.password.policy.regex</code> and <code>dev.getelements.elements.password.policy.description</code>, let an operator require passwords to match a regular expression before Elements will accept them. The regex is enforced everywhere a password is accepted or changed by a client: signup, password reset completion, admin-set password, self-service change-password, and email/username-password account linking. It is not applied to server-generated passwords, such as the bootstrap default superuser account or mock/test accounts. See <a href="properties">Properties</a> for the exact keys.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>The description value is a plain-text, human-readable explanation of the requirement (e.g. "Password must be at least 4 characters."), returned as part of the error message when a submitted password fails the regex, so a client can surface it directly without parsing the regex itself. The default regex is <code>.{4,}</code> (minimum length only, matching the length of the built-in default superuser password), which is a change from Elements' previous behavior of accepting any non-blank password. Operators are responsible for keeping the regex and its description in sync, Elements does not attempt to derive one from the other. This is system-wide only; there is no per-application or per-tenant override.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3,"anchor":"h-editable-email-templates"} -->
<h3 id="h-editable-email-templates" class="wp-block-heading">Editable Email Templates</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Transactional emails, such as password resets and email verification, are no longer hardcoded. <code>EmailTemplateService</code> is a new first-class platform service that stores each email's subject and HTML body as a persistent record addressed by a unique <code>key</code>, so operators can change wording and styling without a redeploy, and custom Elements can register their own templates without any core platform changes. Password Reset and Email Verification both use it, replacing the older approach of hardcoding subject and body as element attribute defaults.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Two core templates ship with the platform under a reserved <code>dev.getelements.elements.</code> prefix: they can be edited but not created or deleted through the API or CMS, and they are seeded automatically with their built-in defaults the first time they are listed or requested. The CMS gains a full editor under <strong>Other &gt; Email Templates</strong>, including a preview that renders the body with sample values substituted for the template's documented variables. See <a href="email-templates">Email Templates</a> for the template variables and the REST API, and <a href="email-verification">Email Verification</a> for the verification flow itself.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3,"anchor":"h-oidc-single-sign-on-for-the-admin-console"} -->
<h3 id="h-oidc-single-sign-on-for-the-admin-console" class="wp-block-heading">OIDC Single Sign-On for the Admin Console</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The admin console's login page can now offer single sign-on via OIDC. Any <a href="oidc-login-for-thick-clients-browser-redirect-flow">OidcProviderConfiguration</a> can set the new <code>adminLoginEnabled</code> flag (plus an optional <code>displayName</code> and <code>iconUrl</code> for the button), and the unauthenticated login page then lists it as a sign-in option. The listing deliberately exposes only the provider's name, display name, and icon, never its client ID, secret, or URLs. Signing in runs the same browser-redirect OIDC login flow used elsewhere, and the admin console still only admits SUPERUSER-level accounts: a successfully authenticated non-superuser session is turned away with a clear message, and an existing administrator must elevate the account first. See <a href="accessing-the-web-ui-cms">Accessing the Web UI CMS</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3,"anchor":"h-operator-run-database-migration-tool"} -->
<h3 id="h-operator-run-database-migration-tool" class="wp-block-heading">Operator-Run Database Migration Tool</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Elements 3.9 ships a lightweight, tracked, idempotent database migration mechanism: a standalone <code>migrate</code> CLI packaged onto the existing server Docker image (there is no second container to run) that connects to the configured MongoDB instance and runs each pending migration exactly once, recording its progress in the database. It wires only the Mongo/DAO layer, no web server, so it can run as a one-shot ops task during an upgrade, and it remains runnable outside Docker for self-managed installations.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>It ships with the first real migration: a backfill that creates slot-0 <code>MongoProfileSlot</code> documents for profiles that predate 3.9's per-application profile slots, so existing deployments pick up the new profile-limit machinery without manual data surgery. The tool also self-heals stale index-option conflicts (Mongo error 86, <code>IndexKeySpecsConflict</code>): if an on-disk index's options no longer match the current index definition, as happens for the OAuth2/OIDC auth-scheme indexes after this release adds <code>sparse: true</code> to them, the tool drops and recreates just the conflicting index instead of aborting. Run it when upgrading an existing deployment.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3,"anchor":"h-guice-spi-loading-strategy-escape-hatch"} -->
<h3 id="h-guice-spi-loading-strategy-escape-hatch" class="wp-block-heading">Guice SPI Loading-Strategy Escape Hatch</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The package-level <code>@GuiceOptions</code> annotation is now wired into <code>GuiceSpiModule</code>, giving third-party Element authors an opt-in escape hatch for the <code>[Guice/ExposedButNotBound]</code> crash that can occur when an exported service has no locally-discovered implementation. Elements that don't declare <code>@GuiceOptions</code> see no behavior change, the existing bind/expose scanning remains the default <code>LEGACY</code> strategy. Authors can instead declare <code>GUICE_MODULE_ONLY</code> to defer every exported service to their own <code>@GuiceElementModule</code>(s), ignoring <code>ElementService#implementation()</code> so the annotation and the module never double-define the same binding. Independently of the strategy, a separate <code>strict = true</code> flag verifies at load time that every exported service relying on a <code>@GuiceElementModule</code> is actually bound by one, failing fast with a clear error naming the unbound services instead of Guice's generic crash. See <a href="introduction-to-guice-and-jakarta-in-elements">Introduction to Guice and Jakarta in Elements</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3,"anchor":"h-crud-events-across-nearly-every-dao"} -->
<h3 id="h-crud-events-across-nearly-every-dao" class="wp-block-heading">CRUD Events Across Nearly Every DAO</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Before 3.9, only three DAOs (<code>ElementDeploymentDao</code>, <code>MultiMatchDao</code>, and <code>ReceiptDao</code>) published create/update/delete events. That coverage now extends to nearly every DAO in the system, including profiles, sessions, applications and their configurations, auth schemes, OIDC login attempts and provider configurations, inventory items and item ledger entries, missions and progress, schedules, leaderboards and scores, reward issuances, save data documents, large objects, followers and friends, FCM registrations, smart contracts, vaults and wallets, and more.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Each new event follows the same two-variant pattern already used by the pre-3.9 DAOs and described in <a href="events">Events</a>: a transactional variant carrying a <code>Transaction</code> argument, published immediately as part of the write, and a plain variant published only once the enclosing transaction commits (and dropped entirely if it rolls back). A handful of DAOs expose fewer variants where it matches the entity's actual lifecycle, for example, <code>ScoreDao</code> fires a single <code>SCORE_CREATED_OR_UPDATED</code> event since scores are always upserted, <code>ItemLedgerDao</code> only fires a created event since ledger entries are immutable, and <code>FriendDao</code> only fires a deleted event since friendships are formed implicitly through mutual follows rather than a direct create call.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>As with all Element events, the authoritative list of event names and their argument types for a given DAO is discoverable at runtime via the CMS's Produced Events screens, or the underlying <code>GET /elements/system</code> and <code>GET /elements/application</code> endpoints; see <a href="events">Events</a> for details. The new <a href="event-reference">Event Reference</a> page catalogs every built-in event with its payload, the release that introduced it, and links to the Javadoc for each declaring interface.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"anchor":"h-bug-fixes"} -->
<h2 id="h-bug-fixes" class="wp-block-heading">Bug Fixes</h2>
<!-- /wp:heading -->

<!-- wp:heading {"level":3,"anchor":"h-stale-datastore-mapper-state-on-element-redeploy"} -->
<h3 id="h-stale-datastore-mapper-state-on-element-redeploy" class="wp-block-heading">Stale Datastore/Mapper State on Element Redeploy</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Any singleton that captured the injected Morphia <code>Datastore</code> directly could go stale the moment an Element (re)deploy rebuilt and swapped the shared <code>Datastore</code>/Mapper, most reliably on every redeploy, since an Element's eager singletons are constructed before its own entities are even registered. The injected <code>Datastore</code> is now a stable proxy that always forwards to whichever instance is live at call time, so holding a reference to it, in Elements' own internal DAOs or in a downstream Element's, is safe by construction.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3,"anchor":"h-oidc-signature-verification-and-jwk-key-support"} -->
<h3 id="h-oidc-signature-verification-and-jwk-key-support" class="wp-block-heading">OIDC Signature Verification and JWK Key Support</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>A security bug in OIDC <code>id_token</code> verification is fixed: on the direct-key-match path, the result of the signature check was discarded, so any JWT whose <code>kid</code> header matched a cached key (a public, non-secret value) passed verification regardless of whether its signature was actually valid. A failed signature check now throws <code>ForbiddenException</code>, matching the already-correct fetch-on-miss path.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Algorithm handling is also corrected. Verification previously hardcoded RSA256 regardless of the key's declared <code>alg</code>, so RS384/RS512 tokens were silently verified as RS256. It now dispatches on both the JWK's key type and algorithm, supporting RS256/384/512 and ES256/384/512. The JWK model previously had only RSA fields, so an EC-signed identity provider could never be represented; it now carries the EC public-key members (<code>crv</code>/<code>x</code>/<code>y</code> per RFC 7518) as well.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3,"anchor":"h-oauth2-oidc-auth-scheme-soft-delete"} -->
<h3 id="h-oauth2-oidc-auth-scheme-soft-delete" class="wp-block-heading">OAuth2/OIDC Auth-Scheme Soft-Delete</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Soft-deleting an OAuth2 auth scheme left it permanently visible with null name and validation URL, and deleting it a second time was a silent no-op. Both the OAuth2 and OIDC scheme DAOs are now filtered to skip soft-deleted rows, and a repeated delete on an already-deleted scheme throws <code>AuthSchemeNotFoundException</code> instead of silently succeeding. The unique indexes backing those fields also gained <code>sparse: true</code> to match the unset-based soft-delete pattern (deployments upgrading an existing database should run the migration tool, which reconciles the index options as described above). Separately, auto-provisioning an OIDC auth scheme for a new provider configuration no longer fails validation with unset keys, and schemes with null keys no longer crash verification; keys are treated as an empty list, matching the existing JWKS refresh behavior.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3,"anchor":"h-jakarta-rs-loader-openapi-context-leak"} -->
<h3 id="h-jakarta-rs-loader-openapi-context-leak" class="wp-block-heading">Jakarta RS Loader OpenAPI Context Leak</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Unloading an Element leaked its OpenAPI contexts and, with them, the Element's classloader, via swagger's static <code>OpenApiContextLocator</code>. The loader now clears those contexts on unload, so repeated (re)deploys no longer accumulate classes and native memory.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3,"anchor":"h-java-sql-visibility-for-elements"} -->
<h3 id="h-java-sql-visibility-for-elements" class="wp-block-heading">java.sql Visibility for Elements</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p><code>java.sql</code> types (such as <code>Timestamp</code>) are now visible to Element code the same way <code>java.base</code> types already were, instead of failing with a bare <code>NoClassDefFoundError</code>. Classloader denial errors in general now name which mechanism denied the class and how to permit it, so a failure several frames removed from the loader is diagnosable without already knowing about the visibility gate.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"anchor":"h-other-changes"} -->
<h2 id="h-other-changes" class="wp-block-heading">Other Changes</h2>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>Element-load performance</strong>: Element loading previously ran roughly 11 independent ClassGraph scans per Element (one per metadata category, each triggering its own probe scan); these are consolidated into three, and Mongo entity registration now batches its index/collection mutations per deployment instead of applying them one at a time.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Configurable Maven artifact cache</strong>: the Maven/Aether local-repository cache used to resolve Maven-sourced Elements (default <code>~/.m2/repository</code>) can now be redirected via the <code>dev.getelements.elements.maven.repo.local</code> system property or the <code>ELEMENTS_MAVEN_REPO_LOCAL</code> environment variable, e.g. to an NFS-mounted, cluster-shared directory. An explicitly set <code>maven.repo.local</code> JVM property is never overridden.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Admin console fixes</strong>: the Endpoints links no longer hardcode <code>localhost:8080</code>; the OIDC provider editor gains a structured JWK editor and fixes a table-rendering bug that could collapse a table to zero columns; and the main dashboard gains call-to-action links.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>RootElementRegistry.unregister() fixes</strong>: two latent bugs (an always-true self-comparison and an <code>iterator.remove()</code> call on a <code>CopyOnWriteArrayList</code>) in this currently-unused API are fixed, and unregistering an Element now also detaches it from further events and close.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Published API docs</strong>: Javadoc and the OpenAPI spec are now published per release to <a href="https://apidocs.namazustudios.com/elements/" target="_blank">apidocs.namazustudios.com/elements/</a>.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->
