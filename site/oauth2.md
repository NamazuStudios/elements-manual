<h1>OAuth2</h1>

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"level":6} -->
<h6 class="wp-block-heading" id="h-when-creating-a-new-session-you-have-the-option-to-authenticate-using-a-predefined-oauth2-auth-scheme">When creating a new Session, you have the option to authenticate using a predefined OAuth2 Auth Scheme.</h6>
<!-- /wp:heading -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-oauth2-auth-scheme-anatomy">OAuth2 Auth Scheme Anatomy</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>OAuth2 Auth Schemes are comprised of:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>the Scheme name</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>a Validation URL, which Elements will use to validate the token it receives</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>the Response Id Mapping, which Elements will use to get the user id from the validation request (see below for example)</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>a list of Headers and a list of Parameters for the validation request, which can be predefined, or sent from the client and filled in by Elements before making the validation request</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:image {"id":22307,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://namazustudios.com/wp-content/uploads/2025/08/Screenshot-2026-01-23-at-3.49.36-PM-1024x270.png" alt="" class="wp-image-22307"/></figure>
<!-- /wp:image -->

<!-- wp:image {"id":22305,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://namazustudios.com/wp-content/uploads/2025/08/Screenshot-2026-01-23-at-3.48.07-PM-1024x889.png" alt="" class="wp-image-22305"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>Since the requirements for OAuth2 vary by provider, we've created a system that is flexible enough for any spec. For instance, some systems, such as Oculus, require that you send the user id from the client alongside the validation token. We've added a User Id flag that allows you to flag either a header, parameter, or body property as the User Id (only one is allowed) in these instances. In other cases, the user id comes back in the response when validating the token. In these cases, we've provided an id mapper. In any case, it's very important to look at the docs for the oauth provider and understand exactly what they're expecting.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Once the validation request comes back successfully, Elements will link the provided/returned user id to the internal Elements user id.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-auth-scheme-properties">Auth Scheme Properties</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Here's a breakdown of the auth scheme properties and what they represent:<br></p>
<!-- /wp:paragraph -->

<!-- wp:group {"layout":{"type":"constrained"}} -->
<div class="wp-block-group"><!-- wp:table {"hasFixedLayout":false} -->
<figure class="wp-block-table"><table><tbody><tr><td><strong>Id</strong></td><td>The Elements database id.</td></tr><tr><td><strong>Name</strong></td><td>The unique name of this scheme in the Elements system.</td></tr><tr><td><strong>Validation Url</strong></td><td>The URL that the server to server validation request will be sent to.</td></tr><tr><td><strong>Headers</strong></td><td>Any headers that the validation request might be expecting. Can mark as fromClient, indicating that  this will be sent from the client. Otherwise, it assumes that this was predefined in the auth scheme.</td></tr><tr><td><strong>Params</strong></td><td>Any query parameters that the validation request might be expecting. Can mark as fromClient, indicating that  this will be sent from the client. Otherwise, it assumes that this was predefined in the auth scheme.</td></tr><tr><td><strong>Body</strong></td><td>(POST requests only) Any body parameters that the validation request might be expecting. Can mark as fromClient, indicating that  this will be sent from the client. Otherwise, it assumes that this was predefined in the auth scheme.</td></tr><tr><td><strong>Response Id Mapping</strong></td><td>Determines how to map the user id in the response. Will search the response for the corresponding key. For example, if the response is structured like: <br>{"response": { "params" : { "steamid" : &lt;id&gt; } } }<br>then you only need to input "steamid". Ignored if one of the parameters is marked as user id.</td></tr><tr><td><strong>Response Valid Mapping</strong></td><td>Optional key in the response whose value indicates whether the token/user is valid.<br>For example: "is_valid", "active", or "success".<br>If not set, only the HTTP status code is used.</td></tr><tr><td><strong>Response Valid Expected Value</strong></td><td>Optional expected value for the validation field.<br>If null:<br>- boolean true is treated as success<br>- non-empty/non-null for non-boolean values is treated as success.<br>If set, the field's string value must equal this.</td></tr><tr><td><strong>Valid Status Codes</strong></td><td>HTTP status codes that are considered "processable" for validation.<br>Any other status is treated as failure before inspecting the body.<br>Defaults to [200].</td></tr><tr><td><strong>Method</strong></td><td>HTTP method for the validation request (GET or POST).</td></tr><tr><td><strong>Body Type</strong></td><td>How to encode the request body when using POST.<br>FORM_URL_ENCODED corresponds to application/x-www-form-urlencoded.</td></tr></tbody></table></figure>
<!-- /wp:table --></div>
<!-- /wp:group -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-steam-example">Steam Example</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>One of the default OAuth2 Auth Schemes is for authenticating via Steam. We'll use this as an example as to how these can be used.</p>
<!-- /wp:paragraph -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"Note"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="3b0649"><div class="gb-notice-title" style="color:#fff"><p>Note</p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p>It will be necessary to have a registered application on Steam, as this process will require an application id.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:paragraph -->
<p>The goal here is to have our client application retrieve an auth ticket from Steam, then to send that to Elements, which in turn will use the ticket to verify the corresponding user id.</p>
<!-- /wp:paragraph -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"Note"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="3b0649"><div class="gb-notice-title" style="color:#fff"><p>Note</p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p>This assumes that you already have a process set up to retrieve the auth ticket, either manually as depicted in the <a href="https://partner.steamgames.com/doc/features/auth#client_to_backend_webapi">Steam documentation</a>, or through a client library, such as <a href="https://steamworks.github.io/">Steamworks.net</a>.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:paragraph -->
<p>First let's look at the auth scheme to determine what Elements is expecting:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>{
  "id": "683a2311bc53452f7ea04b33",
  "name": "Steam",
  "validationUrl": "https://api.steampowered.com/ISteamUserAuth/AuthenticateUserTicket/v1/",
  "headers": &#91;
    {
      "key": "x-webapi-key",
      "value": "169AA59968B1FDBA0E303C311C525B8F",
      "fromClient": false,
      "userId": false
    },
    {
      "key": "Content-Type",
      "value": "application/x-www-form-urlencoded",
      "fromClient": false,
      "userId": false
    }
  ],
  "params": &#91;
    {
      "key": "appid",
      "value": "370140",
      "fromClient": false,
      "userId": false
    },
    {
      "key": "ticket",
      "value": "Ticket from GetAuthSessionTicket (Sent from frontend)",
      "fromClient": true,
      "userId": false
    },
    {
      "key": "identity",
      "value": "nebtest",
      "fromClient": false,
      "userId": false
    }
  ],
  "responseIdMapping": "steamid",
  "validStatusCodes": &#91;
    200
  ]
}</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>We're basically telling it how to format the request that Elements will send to the authenticating server. In this case, we're telling Elements to expect the ticket to be sent from the client (the only header or parameter with <code>fromClient</code> as true), and that all other values will be preset in the scheme and stored within Elements.</p>
<!-- /wp:paragraph -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"Note"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="3b0649"><div class="gb-notice-title" style="color:#fff"><p>Note</p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p>When you make the call to GetAuthTicketForWebApi to retrieve the auth ticket, you pass in an identity value. This can be anything (or nothing), but must match on the client and server side.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:paragraph -->
<p>When we make a request against the <a href="../../restful-apis/api-specification/auth_scheme/oauth2">/auth/oauth2</a> endpoint, Elements will format a new request using the <code>headers</code> and <code>params</code> information in the scheme, and will send the request to the <code>validationUrl</code> defined in the scheme. In this case, we only told the scheme to expect one parameter, <code>"ticket"</code>, and no headers from the client, so our request would look like this:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>Example: 
POST http://localhost:8080/api/rest/auth/oauth2

Body:
{
    "schemeId":"Steam",
    "requestParameters" : {
        "ticket":"&lt;Your Steam ticket from GetAuthTicketForWebApi call. Will be very long, around 5120 characters.&gt;""
    }
}</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>When Elements receives the response, it will use the <code>responseIdMapping</code> to look for a corresponding key, and will assume the value of this key is the user's id.</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>Example response from Steam (handled internally in Elements):
{
    "response": {
        "params": {
            "result": "OK",
            "steamid": "&lt;steam user id&gt;",
            "ownersteamid": "&lt;steam user id&gt;",
            "vacbanned": false,
            "publisherbanned": false
        }
    }
}</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>If Elements retrieves the user id successfully, it will automatically associate this Steam user id with an Elements user, and return the <a href="../sessions">Elements Session</a> object back to you.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Once you have the Session object, just add the <code>sessionSecret</code> to the <code>Elements-SessionSecret</code> header for subsequent requests, and you're all set!</p>
<!-- /wp:paragraph -->
