<h1>3.7 Release Notes</h1>

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-overview">Overview</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Elements 3.7 is the biggest release yet. Elements can now be packaged, distributed, and hot-loaded at runtime — no server restart required. The new <code>.elm</code> archive format gives developers a single self-contained artifact to ship, and the new Deployments UI makes managing the full lifecycle a first-class experience in the admin console.<br>Highlights</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-highlights">Highlights</h2>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>Hot-deploy Elements at runtime</strong> — load, update, and unload Elements dynamically via the new deployment API and runtime/container services.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>ELM package format</strong> — distribute Elements as self-contained <code>.elm</code> archives, loadable from the filesystem or MongoDB GridFS, with nested JAR and SPI support.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Deployments UI</strong> — brand-new admin UI with a guided wizard for creating and managing deployments, runtimes, and containers.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Maven archetypes</strong> — new standard Java and Kotlin archetypes (<code>sdk-element-standard</code>, <code>sdk-element-kotlin</code>) to scaffold a production-ready Element in seconds.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>SDK Bill of Materials</strong> — <code>sdk-bom</code> makes dependency management across Element projects consistent and effortless.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Security hardening</strong> — fixed OAuth2 account linking vulnerabilities, anonymous user collision bugs, and tightened classloader isolation.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Windows support</strong> — full build and test compatibility on Windows, including path-length mitigations.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-coming-soon">Coming Soon</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The deployment and packaging work in 3.7 is laying the foundation for something bigger. Namazu Cloud will be a fully managed hosting solution for Namazu Elements; spin up and manage your own instances with one-click deploys, automated backups, and self-service scaling, all without touching infrastructure. Stay tuned.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-new-features">New Features</h2>
<!-- /wp:heading -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-dynamic-deployment">Dynamic Deployment</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Introduced a full hot-deploy system for Elements,&nbsp;enabling Elements to be loaded,&nbsp;updated,&nbsp;and unloaded at runtime without restarting the server.</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>New <code>sdk-deployment</code> module extracts deployment services as a first-class SDK concern.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>ElementDeployment</code> model supports multiple Elements per deployment, full CRUD, status tracking, and lifecycle events.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>New REST API endpoints for managing deployments (<code>/api/element/deployment</code>).</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>ElementRuntimeService</code> and <code>ElementContainerService</code> now support lifecycle events and handler cleanup on unmount.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>LoadConfiguration</code> with <code>AttributesLoader</code> for customizable Element initialization.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>ElementDeployment</code> can now inject the hosting <code>Application</code> into the Element's <code>Attributes</code>.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>SPI directory support and flat element loading architecture.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>New <code>AttributesLoader</code> and <code>SpiLoader</code> interfaces for external configuration.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-elm-package-format-and-local-sdk">ELM Package Format and Local SDK</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Full support for distributing and loading Elements as self-contained&nbsp;<code>.elm</code>&nbsp;archive files.</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Load Elements from <code>.elm</code> packages stored in MongoDB GridFS or from the local filesystem.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Nested JAR classloading within <code>.elm</code> packages.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>New <code>sdk-element-standard</code> Maven archetype for scaffolding standard Elements.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>New <code>sdk-bom</code> Bill of Materials for consistent SDK dependency management across Element projects.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>ELM Inspector: new REST endpoint and UI for introspecting a deployed <code>.elm</code> package.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>PermittedTypesClassLoader</code> with <code>TypeRequest</code>/<code>PackageRequest</code> (literal, regex, wildcard) for fine-grained classloader isolation.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>ElementDependencyMetadata</code> DTO for reporting deployed element dependencies.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>OpenAPI spec integration test suite.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Local SDK improvements: simplified <code>ElementsLocal</code> API, Maven-based local builder, abstract base class for local tests.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>sdk-bom</code> fleshed out as a proper BOM encompassing all SDK modules.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Features endpoint added to expose server capabilities.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-signup-creates-session">Signup Creates Session</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The signup API now creates an authenticated session immediately after account creation,&nbsp;matching the behavior users expect from a signup flow.</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>New endpoint path added to avoid breaking existing login integrations.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Legacy endpoint preserved and deprecated.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-deployments-ui">Deployments UI</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>New web UI for managing Element deployments.</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Deployment wizard with Runtimes and Containers pages.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Features dialog on the Deployments page.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>File upload support to pre-fill deployment fields.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Edit element flow aligned with the create wizard.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Search filter presets to hide large <code>.elm</code> objects from general object lists.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Updated Container and Runtime detail views for better screen fit.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-kotlin-archetype">Kotlin Archetype</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Added a Kotlin Maven archetype (<code>sdk-element-kotlin</code>) for developers who prefer Kotlin when building Elements.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-bug-fixes">Bug Fixes</h2>
<!-- /wp:heading -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-obsolete-fields">Obsolete Fields</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Removed obsolete application fields from the admin UI. Updated tests to reflect that metadata name changes are now permitted.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-codegen-creating-duplicate-methods">Codegen Creating Duplicate Methods</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Fixed code generation producing duplicate method names in OpenAPI-generated clients. Resources without an explicit tag are now automatically tagged with <code>ElementsCore</code>.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-elements-api-cleanup">Elements API Cleanup</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Normalized REST API path conventions across applications, leaderboards, progress, and mission endpoints. Added <code>@Deprecated</code> annotations to all renamed methods. Renamed methods referencing "active" or "inactive" applications for consistency.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-shrinkwrap-module-loader">ShrinkWrap Module Loader</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Added a ShrinkWrap-based module loader for test harnesses, enabling more reliable module assembly in integration tests.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-windows-build-and-path-length-issues">Windows Build and Path Length Issues</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Fixed failures building and running tests on Windows caused by path lengths exceeding the Windows filesystem limit. Added cleanup on exception to avoid leaving temporary state behind.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-classloader-memory-leak">ClassLoader Memory Leak</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Fixed a resource leak in <code>ElementImplementationClassLoader</code> where native resources were not released when a deployment was unloaded.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="error-hiding-in-directoryelementpathloader">Error Hiding in DirectoryElementPathLoader</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Improved error handling in&nbsp;<code>DirectoryElementPathLoader</code>&nbsp;so that load failures are surfaced rather than silently swallowed.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="elements-not-loading-after-deployment-regression">Elements Not Loading After Deployment (Regression)</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Fixed a regression where Elements failed to load after deployment due to incorrect attribute loading hierarchy.&nbsp;Reworked&nbsp;<code>PropertiesAttributes</code>&nbsp;to fix a&nbsp;<code>NullPointerException</code>&nbsp;and corrected the attribute merge order so system attributes take proper precedence.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="oauth2-account-linking">OAuth2 Account Linking</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Fixed a bug where the JWK cache was considered out of date on the first authentication attempt,&nbsp;causing the first OIDC login to fail.&nbsp;Fixed related issues in the account linking flow.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="anonymous-user-collision-and-oauth2-security">Anonymous User Collision and OAuth2 Security</h3>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Fixed a soft-deleted anonymous user being returned as a live user under certain conditions.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Fixed a security issue where an OAuth2 identity could be linked to a second account under the same scheme.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Prevented duplicate linking when the same identity provider scheme is used more than once.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Added additional guard rails and expanded test coverage.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="missing-slf4j-dependency-on-container-b">Missing SLF4J Dependency on Container B</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Added a missing&nbsp;<code>slf4j</code>&nbsp;dependency that caused startup failures in certain deployment configurations.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="codegen--oas3-integration-tests">Codegen / OAS3 Integration Tests</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Added integration tests validating OpenAPI 3 code generation output.&nbsp;Hardened related codegen logic to prevent regressions.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="other-changes">Other Changes</h2>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>Application configuration hotfix</strong>: Fixed an error when creating an application configuration with no product bundles; <code>description</code> field is no longer required to be non-null.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Dependency updates</strong>: Updated Jackson and Swagger to their latest versions. Migrated Jetty, Jersey, and Swagger to BOM-managed versions.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Branding cleanup</strong>: Replaced outdated references to "Elemental-Computing" with current project naming.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>CI improvements</strong>: Reduced double-builds in Bitbucket Pipelines; added Maven version as a Surefire system property; fixed Makefile syntax.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Javadoc</strong>: Added Javadoc generation to all builds; fixed misplaced Javadoc tags across multiple modules.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->
