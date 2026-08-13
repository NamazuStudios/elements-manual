<h1>OIDC</h1>

<!-- wp:paragraph -->
<p>When creating a new Session, you have the option to authenticate using a predefined OIDC Auth Scheme.</p>
<!-- /wp:paragraph -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"Note"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="3b0649"><div class="gb-notice-title" style="color:#fff"><p>Note</p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p>This page covers the older, static OIDC Auth Scheme, which validates an <code>id_token</code> you already possess against a fixed issuer/keys configuration. If you want Elements to drive the full authorization-code flow — opening a provider's login page, handling the redirect, and exchanging the code for tokens on your behalf — see <a href="oidc-login-for-thick-clients-browser-redirect-flow">OIDC Login for Thick Clients (Browser Redirect Flow)</a> and <a href="setting-up-twitch-oidc-login-backend">Setting Up Twitch OIDC Login (Backend)</a> for the newer, provider-configuration-driven approach.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-how-does-it-work">How does it work?</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>An OIDC Auth Scheme is comprised of:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>an issuer, which is generally the URL of the service that provided the authentication token.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>the keys URL, which is where the public keys are stored in the form of JWKs.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>the media type for the request to fetch the keys, which will almost always be <code>application/json</code></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>the keys, which are used to validate the signature of the auth token</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>To authenticate using an OIDC Auth Scheme, you must have a scheme defined with an issuer matching the value of the <code>iss</code> key in the JWT that you are sending. Here's an example decoded JWT from Apple:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>Header
{
  "kid": "fh6Bs8C",
  "alg": "RS256"
}

Payload
{
  "iss": "https://appleid.apple.com",
  "aud": "com.mycompany.myapplication",
  "exp": 1703881696,
  "iat": 1703795296,
  "sub": "x.y.z",
  "nonce": "c182a093d0a21f15282a1701feabd9ffbdff318de5f52046ce1e093f16e74f43",
  "c_hash": "t2_yMK6paDfgGiNPROjYKw",
  "email": "someid@privaterelay.appleid.com",
  "email_verified": "true",
  "is_private_email": "true",
  "auth_time": 1703795296,
  "nonce_supported": true
}</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>This would automatically attempt to use the scheme defined for <code>https://appleid.apple.com</code></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>To send an <a href="../../restful-apis/api-specification/auth">authentication request</a>, you will need to first fetch a Json Web Token (JWT) from the issuer. The JWT will then need to be sent to Elements to verify the signature using the cached Json Web Key (JWK).</p>
<!-- /wp:paragraph -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"Note"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="3b0649"><div class="gb-notice-title" style="color:#fff"><p>Note</p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p>When sending the JWT to Elements, it must be Base64 encoded. This is also typically how the JWT is received from the issuer.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-managing-auth-schemes">Managing Auth Schemes</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Elements will create several default schemes for common SSO providers. However, it is possible to create new schemes.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>In the Auth section of the admin console, under the OIDC tab, you can create a new scheme or manage existing schemes.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":22308,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://namazustudios.com/wp-content/uploads/2025/08/Screenshot-2026-01-23-at-4.08.41-PM-1024x351.png" alt="" class="wp-image-22308"/></figure>
<!-- /wp:image -->

<!-- wp:image {"id":22309,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://namazustudios.com/wp-content/uploads/2025/08/Screenshot-2026-01-23-at-4.08.54-PM-1024x891.png" alt="" class="wp-image-22309"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>An OIDC Auth Scheme follows the following structure:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>{
    "id": "67d673f5131dde00b60e230b",
    "issuer": "https://accounts.google.com",
    "keys": &#91;
        {
            "alg": "RS256",
            "kid": "d9740a70b0972dccf75fa88bc529bd16a30573bd",
            "kty": "RSA",
            "use": "sig",
            "e": "AQAB",
            "n": "oeS547_9wjr2KSN8kA8shy-1arjHHxrx8QeARyWQ9tjQZ8xuF62y-2Ffz0J9F8A_vjrtWCv-ApD1m2v86qs6ZhCXYjvFOPzu7eehcSIojxqgjcN8rqMmhOloPVll_xsc1XXs3djFYL4cGaozJ4b7C5HWQqCJwkKqDTUPAfNTgQG-CSFlGVMM9Yu5ZElsiQIvP_DHfmyMsSIfmi5xxJD_xIBxumh9C8pOOcarw2oi8eLqtyj9jnnjEJncm51PsjkyATCzcMKSFIGFr-UPVnH4-4mYpeqwwYzcvb95DH-exQANjYLANFiSbyRU0SxzJ39yKPAPIBwqrA37BVwsD5AJvw"
        },
        {
            "alg": "RS256",
            "kid": "3628258601113e6576a45337365fe8b8973d1671",
            "kty": "RSA",
            "use": "sig",
            "e": "AQAB",
            "n": "vHJNSdOKUAG53oCGHbEp2PJFX-NksFDrw1_TEzK8yF72Jbp8cYebwkoZpCkr2THVAmRuvDe8GuuXYyRih9w7APwAH0aNy8og4Q1rqPuX-q1TAqO9KXYJNd2VIaICwY2IvY3IgQNu0r9GKouSBeeaXGBlUYi2IR74T4ICOwcpJYTQOE2GWcWeri7iaeFzMfqKa0NJrv6f7paGA0DNu0PggNpgOQMbZoriWc7-PGa7lP4QrStpGikgNOcbGfEw53LeB6dbw72uCCpGbd1iuhzv6M6B-7gLQEp4188mAgjSkmr4TruyZ36Nn4gK_FTOFI44QNMvAGUBJ1L7M49V0KyELQ"
        }
    ],
    "keysUrl": "https://www.googleapis.com/oauth2/v3/certs",
    "mediaType": "application/json"
}</code></pre>
<!-- /wp:code -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"Note"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="3b0649"><div class="gb-notice-title" style="color:#fff"><p>Note</p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p>It is not necessary to define the JWKs ahead of time. The keys listed here are cached keys that have been fetched from the Keys URL. When attempting to authenticate, the JWT will contain the Key Id (kid). If the cached keys do not contain a key with this id, then Elements will fetch the keys from the Keys URL and retry the authentication attempt with the new keys. If authentication is successful, then the new keys will be cached.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->
