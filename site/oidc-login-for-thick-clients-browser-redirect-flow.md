<h1>OIDC Login for Thick Clients (Browser Redirect Flow)</h1>

<!-- wp:paragraph -->
<p>Elements handles the entire OIDC handshake on the server, so a native client logs a user in with two HTTP calls and one call to open a URL. That works from any engine or language that can POST JSON and launch the system browser: Unity, Unreal, GameMaker, a console build, a command-line tool.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"anchor":"h-what-this-gives-you"} -->
<h2 id="h-what-this-gives-you" class="wp-block-heading">What this gives you.</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p><strong>One integration covers every provider.</strong> The client sends a provider name and gets back a URL. Google, Apple, Twitch, and any provider an administrator registers all use the identical client-side code path.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><strong>Providers are a server-side configuration change.</strong> Adding Twitch support after ship is an admin API call, not a client rebuild and store resubmission.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><strong>Credentials stay on the server.</strong> Client secrets, code exchange, and <code>id_token</code> validation all live in Elements. Your game binary carries a provider name and nothing else worth extracting.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><strong>Users authenticate in their real browser.</strong> Existing provider sessions, password managers, passkeys, and 2FA prompts all work the way the user expects, which usually means the login is a single tap rather than a typed password.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><strong>The client needs only outbound HTTP.</strong> Elements owns the registered redirect URI and receives the provider's callback directly, so the client requires no listening socket, no port allocation, and no firewall exception.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Clients that already hold an <code>id_token</code> from a native platform SDK can skip the browser entirely; see the shortcut at the end of this page.<br></p>
<!-- /wp:paragraph -->

<!-- wp:heading {"anchor":"h-linking-an-existing-account"} -->
<h2 id="h-linking-an-existing-account" class="wp-block-heading">Linking an existing account</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>If the client already holds an Elements session (for example, from an anonymous <a href="user-authentication-in-elements">signup</a>) when it calls <code>POST /oidc/session</code>, the resulting attempt links the external identity to that user instead of creating a new one, on the same terms as the explicit linking endpoints covered in <a href="account-linking">Account Linking</a>. No extra request field is needed: whether the attempt links or creates a new user is decided purely by whether the caller's session was present on that call. Everything else in the sequence below works the same way for both cases, except that a linking attempt requires one additional step, <code>POST /oidc/session/{id}/confirm</code>, described after the poll step.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"anchor":"h-full-request-sequence"} -->
<h2 id="h-full-request-sequence" class="wp-block-heading">Full Request Sequence</h2>
<!-- /wp:heading -->

<!-- wp:image {"id":22571,"width":"705px","height":"auto","aspectRatio":"0.9607686148919136","sizeSlug":"full","linkDestination":"none"} -->
<figure class="wp-block-image size-full is-resized"><img src="https://namazustudios.com/wp-content/uploads/2026/08/oidc-thick-client-login.mermaid.svg" alt="" class="wp-image-22571" style="aspect-ratio:0.9607686148919136;width:705px;height:auto"/></figure>
<!-- /wp:image -->

<!-- wp:heading {"anchor":"h-endpoint-reference"} -->
<h2 id="h-endpoint-reference" class="wp-block-heading">Endpoint reference</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Three endpoints appear in the flow above, but the client calls only two of them for an anonymous attempt. <code>POST /oidc/session</code> starts the attempt and <code>GET /oidc/session/{id}</code> polls it. The callback in between is provider-facing and is documented here so you can recognize it in logs, not because your client will ever invoke it. A linking attempt needs a fourth call, <code>POST /oidc/session/{id}/confirm</code>, covered as step 4 below.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3,"anchor":"h-1-post-oidc-session-begin-the-attempt"} -->
<h3 id="h-1-post-oidc-session-begin-the-attempt" class="wp-block-heading">1. <code>POST /oidc/session</code>, begin the attempt</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Requests, the client must begin the OIDC auth process. Each of the </p>
<!-- /wp:paragraph -->

<!-- wp:betterdocs/code-snippet {"blockId":"betterdocs-code-snippet-f8995eef","blockMeta":{"desktop":" .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-f8995eef { border-width: 0px !important; border-radius: 0px !important; } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-f8995eef .betterdocs-code-snippet-header.betterdocs-file-preview-header { border-bottom-width: 1px !important; border-bottom-style: solid !important; } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-f8995eef .betterdocs-code-snippet-header .betterdocs-file-name .file-name-text { font-size: 14px; } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-f8995eef .betterdocs-code-snippet-header .betterdocs-code-snippet-copy-button { } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-f8995eef .betterdocs-code-snippet-content .betterdocs-code-snippet-line-numbers { border-right-width: 1px !important; border-right-style: solid !important; } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-f8995eef .betterdocs-code-snippet-content .betterdocs-code-snippet-line-numbers .line-number { } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-f8995eef .betterdocs-code-snippet-code { } ","tab":" ","mobile":" "},"codeContent":"{ \u0022provider\u0022: \u0022twitch\u0022 }","language":"json","fileName":"Request Body"} /-->

<!-- wp:betterdocs/code-snippet {"blockId":"betterdocs-code-snippet-f5911d0b","blockMeta":{"desktop":" .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-f5911d0b { border-width: 0px !important; border-radius: 0px !important; } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-f5911d0b .betterdocs-code-snippet-header.betterdocs-file-preview-header { border-bottom-width: 1px !important; border-bottom-style: solid !important; } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-f5911d0b .betterdocs-code-snippet-header .betterdocs-file-name .file-name-text { font-size: 14px; } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-f5911d0b .betterdocs-code-snippet-header .betterdocs-code-snippet-copy-button { } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-f5911d0b .betterdocs-code-snippet-content .betterdocs-code-snippet-line-numbers { border-right-width: 1px !important; border-right-style: solid !important; } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-f5911d0b .betterdocs-code-snippet-content .betterdocs-code-snippet-line-numbers .line-number { } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-f5911d0b .betterdocs-code-snippet-code { } ","tab":" ","mobile":" "},"codeContent":"{\n  \u0022id\u0022: \u0022opaque-poll-id\u0022,\n  \u0022authorizeUrl\u0022: \u0022https://id.twitch.tv/oauth2/authorize?...\u0022,\n  \u0022expiresAt\u0022: 1234567890,\n  \u0022confirmToken\u0022: \u0022opaque-confirm-token\u0022\n}","language":"json","fileName":"Response Body - 201 Created"} /-->

<!-- wp:paragraph -->
<p>The client opens <code>authorizeUrl</code> in the system browser and retains <code>id</code> for polling. <code>expiresAt</code> bounds how long the attempt stays valid; once it passes, the id stops resolving and the client should start a fresh attempt.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><code>confirmToken</code> is returned for every attempt, but only a linking attempt (see <a href="#h-linking-an-existing-account">above</a>) needs it, to finalize the link in step 4 below. Treat it with the same care as a session token: it is returned only in this response, over the same authenticated channel that requested it, and it never appears anywhere in the browser-redirect leg of the flow.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3,"anchor":"h-2-browser-completes-the-provider-s-login-flow"} -->
<h3 id="h-2-browser-completes-the-provider-s-login-flow" class="wp-block-heading">2. Browser completes the provider's login flow</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>This step happens entirely between the user's browser and the provider. The client application is not involved and does not receive the redirect. The provider redirects the browser to the server's registered callback:</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><code>GET /oidc/{provider}/callback?code=...&amp;state=...</code></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>This endpoint is provider-facing only, is never called by the game client, and always returns a <code>200</code> HTML page regardless of outcome. The actual result is observable only via the poll endpoint, which keeps the outcome on an authenticated channel the client controls.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3,"anchor":"h-3-get-oidc-session-id-poll-for-completion"} -->
<h3 id="h-3-get-oidc-session-id-poll-for-completion" class="wp-block-heading">3. <code>GET /oidc/session/{id}</code>, poll for completion</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Response, one of:</p>
<!-- /wp:paragraph -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th><code>status</code></th><th>Meaning</th></tr></thead><tbody><tr><td><code>PENDING</code></td><td>Still waiting on the user; poll again after a short delay.</td></tr><tr><td><code>COMPLETE</code></td><td>Returned <strong>exactly once</strong>, on the poll that first observes completion. Includes <code>session</code>, the completed Elements session.</td></tr><tr><td><code>FAILED</code></td><td>Login failed or was denied. Includes a human-readable <code>reason</code>.</td></tr><tr><td><code>404</code></td><td>The <code>id</code> is unknown, already consumed, or has expired.</td></tr><tr><td><code>403</code></td><td>This is a linking attempt and its external identity has already been validated; poll no longer applies to it. Call <code>POST /oidc/session/{id}/confirm</code> instead, described next.</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:paragraph -->
<p>Because <code>COMPLETE</code> is returned exactly once, the session is delivered to a single caller and cannot be picked up by a replayed poll. Store the returned <code>session</code> on receipt; a second poll against the same <code>id</code> will <code>404</code>.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>For an anonymous attempt, once <code>COMPLETE</code> is observed, the client has its Elements session and the flow is done. A linking attempt never reaches <code>COMPLETE</code> via this endpoint; once its external identity is validated, it moves straight to the <code>403</code> case above instead, and the client must call the confirm endpoint below to actually obtain the session.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3,"anchor":"h-4-post-oidc-session-id-confirm-finalize-a-linking-attempt"} -->
<h3 id="h-4-post-oidc-session-id-confirm-finalize-a-linking-attempt" class="wp-block-heading">4. <code>POST /oidc/session/{id}/confirm</code>, finalize a linking attempt</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Only needed for a linking attempt; an anonymous attempt never reaches this step. The provider's callback (step 2) is a bare, unauthenticated redirect, so it deliberately never performs the account-link itself, even once the external identity is validated; it only records that the identity is ready to be linked. This step performs the actual link, gated on presenting <code>confirmToken</code> from step 1, the one piece of the flow the callback never sees and an attacker intercepting the browser-redirect leg never gets access to.</p>
<!-- /wp:paragraph -->

<!-- wp:betterdocs/code-snippet {"blockId":"betterdocs-code-snippet-a3c1f0e2","blockMeta":{"desktop":" .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-a3c1f0e2 { border-width: 0px !important; border-radius: 0px !important; } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-a3c1f0e2 .betterdocs-code-snippet-header.betterdocs-file-preview-header { border-bottom-width: 1px !important; border-bottom-style: solid !important; } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-a3c1f0e2 .betterdocs-code-snippet-header .betterdocs-file-name .file-name-text { font-size: 14px; } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-a3c1f0e2 .betterdocs-code-snippet-header .betterdocs-code-snippet-copy-button { } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-a3c1f0e2 .betterdocs-code-snippet-content .betterdocs-code-snippet-line-numbers { border-right-width: 1px !important; border-right-style: solid !important; } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-a3c1f0e2 .betterdocs-code-snippet-content .betterdocs-code-snippet-line-numbers .line-number { } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-a3c1f0e2 .betterdocs-code-snippet-code { } ","tab":" ","mobile":" "},"codeContent":"{ \u0022confirmToken\u0022: \u0022opaque-confirm-token\u0022 }","language":"json","fileName":"Request Body"} /-->

<!-- wp:paragraph -->
<p>Response, one of:</p>
<!-- /wp:paragraph -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th><code>status</code></th><th>Meaning</th></tr></thead><tbody><tr><td><code>COMPLETE</code></td><td>The link succeeded. Includes <code>session</code>, the completed Elements session for the linked user. Like the poll endpoint, this is returned <strong>exactly once</strong>.</td></tr><tr><td><code>404</code></td><td>The <code>id</code> is unknown, not awaiting confirmation, already claimed, or expired.</td></tr><tr><td><code>403</code></td><td><code>confirmToken</code> is missing or doesn't match. Not consumed; retry with the correct token.</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:heading {"level":4,"anchor":"h-shortcut-client-already-holds-an-id-token"} -->
<h4 id="h-shortcut-client-already-holds-an-id-token" class="wp-block-heading">Shortcut: client already holds an <code>id_token</code></h4>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>If the client already has a valid <code>id_token</code> from a native provider SDK, such as a platform Sign-In SDK, it can skip the browser and poll steps entirely:</p>
<!-- /wp:paragraph -->

<!-- wp:betterdocs/code-snippet {"blockId":"betterdocs-code-snippet-8b3e0829","blockMeta":{"desktop":" .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-8b3e0829 { border-width: 0px !important; border-radius: 0px !important; } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-8b3e0829 .betterdocs-code-snippet-header.betterdocs-file-preview-header { border-bottom-width: 1px !important; border-bottom-style: solid !important; } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-8b3e0829 .betterdocs-code-snippet-header .betterdocs-file-name .file-name-text { font-size: 14px; } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-8b3e0829 .betterdocs-code-snippet-header .betterdocs-code-snippet-copy-button { } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-8b3e0829 .betterdocs-code-snippet-content .betterdocs-code-snippet-line-numbers { border-right-width: 1px !important; border-right-style: solid !important; } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-8b3e0829 .betterdocs-code-snippet-content .betterdocs-code-snippet-line-numbers .line-number { } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-8b3e0829 .betterdocs-code-snippet-code { } ","tab":" ","mobile":" "},"codeContent":"{ \u0022provider\u0022: \u0022twitch\u0022, \u0022idToken\u0022: \u0022\u003cid_token\u003e\u0022 }","fileName":""} /-->

<!-- wp:paragraph -->
<p><code>POST /oidc/session</code> with <code>idToken</code> set returns <code>200</code> synchronously with the completed session, sharing the same token-validation logic as the callback path:</p>
<!-- /wp:paragraph -->

<!-- wp:betterdocs/code-snippet {"blockId":"betterdocs-code-snippet-cef4c00f","blockMeta":{"desktop":" .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-cef4c00f { border-width: 0px !important; border-radius: 0px !important; } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-cef4c00f .betterdocs-code-snippet-header.betterdocs-file-preview-header { border-bottom-width: 1px !important; border-bottom-style: solid !important; } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-cef4c00f .betterdocs-code-snippet-header .betterdocs-file-name .file-name-text { font-size: 14px; } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-cef4c00f .betterdocs-code-snippet-header .betterdocs-code-snippet-copy-button { } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-cef4c00f .betterdocs-code-snippet-content .betterdocs-code-snippet-line-numbers { border-right-width: 1px !important; border-right-style: solid !important; } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-cef4c00f .betterdocs-code-snippet-content .betterdocs-code-snippet-line-numbers .line-number { } .betterdocs-code-snippet-wrapper.betterdocs-code-snippet-cef4c00f .betterdocs-code-snippet-code { } ","tab":" ","mobile":" "},"codeContent":"{ \u0022session\u0022: { \u0022...\u0022: \u0022...\u0022 } }"} /-->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->
