<h1>Elements Unity Plugin</h1>

<!-- wp:paragraph -->
<p>Github URL:<br><a href="https://github.com/NamazuStudios/unity-codegen-plugin">https://github.com/NamazuStudios/unity-codegen-plugin</a></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Asset Store URL:<br><a href="https://assetstore.unity.com/packages/tools/integration/namazu-elements-codegen-plugin-for-unity-cross-platform-gbaas-319085">https://assetstore.unity.com/packages/tools/integration/namazu-elements-codegen-plugin-for-unity-cross-platform-gbaas-319085</a></p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-overview">Overview</h2>
<!-- /wp:heading -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="requirements">Requirements<a href="https://github.com/NamazuStudios/unity-codegen-plugin/blob/main/README.md#requirements"></a></h3>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Unity 2018+</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>.Net 4.x enabled in project</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Elements running at an accessible URL</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>(Optional) To generate custom application code: Must also have an application created within Elements with the code already uploaded.&nbsp;<a href="https://namazustudios.com/docs/custom-code/preparing-for-code-generation/">See the docs here for more info</a>&nbsp;on how to prepare your Element code for client code generation.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading">Summary</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p><a href="https://github.com/NamazuStudios/unity-codegen-plugin/blob/main/README.md#summary"></a>Elements Codegen is a tool that will convert your Elements and application APIs and model definitions and into C# code that is immediately usable.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>In addition, there are some convenience classes generated so that you can hit the ground running. These are optional to use, however, so feel free to ignore them if you want to manage everything yourself.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Go to&nbsp;<strong>Window -&gt; Elements -&gt; Elements Codegen</strong>&nbsp;to get started.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>You'll need to enter the credentials of an admin/SUPERUSER. If you haven't changed the defaults yet, you should be able to just use root/example as the username/password.&nbsp;<a href="https://namazustudios.com/docs/getting-started/accessing-the-web-ui-cms/">See here</a>&nbsp;for more information on accessing the CMS and how to change the default SUPERUSER.</p>
<!-- /wp:paragraph -->

<!-- wp:quote -->
<blockquote class="wp-block-quote"><!-- wp:paragraph -->
<p>Important</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>You must have Elements running at the target root URL. If running locally, then by default this will be&nbsp;<code>http://localhost:8080</code></p>
<!-- /wp:paragraph --></blockquote>
<!-- /wp:quote -->

<!-- wp:quote -->
<blockquote class="wp-block-quote"><!-- wp:paragraph -->
<p>Warning</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>The tool might not be available if it is imported with active compiler errors. If this is the case, please resolve the errors and check again for the tool window.</p>
<!-- /wp:paragraph --></blockquote>
<!-- /wp:quote -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading">Generated Code Usage<a href="https://github.com/NamazuStudios/unity-codegen-plugin/blob/main/README.md#generated-code-usage"></a></h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>After generating your code, you can use {packageName}.Client.ElementsClient to initialize the API with the server URL root, and then make any API call. Most properties can be overridden if you prefer to write your own implementation, including object (de)serialization, credentials storage, etc. You can also use the APIs directly if you prefer to manage the requests yourself, or if you prefer a DI based architecture.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading">Initializing ElementsClient<a href="https://github.com/NamazuStudios/unity-codegen-plugin/blob/main/README.md#initializing-elementsclient"></a></h4>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>When you initialize, you will need to specify the root API path. Locally, this will be&nbsp;<code>http://localhost:8080/api/rest</code>. Optionally, you can tell ElementsClient to not cache the session if you prefer to do that yourself.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading">Session Caching<a href="https://github.com/NamazuStudios/unity-codegen-plugin/blob/main/README.md#session-caching"></a></h4>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>By default, the generated code will use JSON, and store the session in Application.PersistentDataPath. Encryption is not enabled by default.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading">Request Handling<a href="https://github.com/NamazuStudios/unity-codegen-plugin/blob/main/README.md#request-handling"></a></h4>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Another class will be generated for you to add extra handling to your requests and responses -&nbsp;<code>ApiClient.partial.cs</code>. This contains two methods -&nbsp;<code>InterceptRequest</code>&nbsp;and&nbsp;<code>InterceptResponse</code>. By default, session creating will be handled for you by checking if the response object is of type&nbsp;<code>SessionCreation</code>&nbsp;and if so, it will apply the session token to the appropriate request headers for subsequent requests.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>See ElementsCodegen/Tests/ElementsTest.cs for an example on how to log in and get the current user (this might be commented out to avoid compiler errors before you generate the Elements API code).</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-using-the-generated-code">Using the Generated Code</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>This section covers how to use the C# code generated by the Elements Codegen Tool. It assumes you have already run the tool at least once (<strong>Window → Elements → Elements Codegen</strong>) and that your&nbsp;<code>Assets/Generated</code>&nbsp;folder is populated.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="toc_2">Initialization</h2>
<!-- /wp:heading -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="toc_3">Singleton (<code>InitializeDefault</code>)</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The simplest way to get started is to call&nbsp;<code>InitializeDefault</code>&nbsp;once at application start. This creates a global instance accessible anywhere via&nbsp;<code>ElementsClient.Default</code>.</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>using Elements.Client;

public class AppStartup : MonoBehaviour
{
    void Awake()
    {
        // Pass the root URL of your Elements instance.
        // Locally this is typically http://localhost:8080
        ElementsClient.InitializeDefault("http://localhost:8080");
    }
}</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p><code>InitializeDefault</code>&nbsp;is a no-op if a default instance has already been created, so it is safe to call from multiple places.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>From any other script:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>var api = ElementsClient.Default.Api;  // ElementsCoreApi</code></pre>
<!-- /wp:code -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="toc_4">Session Caching</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>By default,&nbsp;<code>ElementsClient</code>&nbsp;persists the session token and profile to disk at<code>Application.persistentDataPath/ElementsSessionCache_default.json</code>&nbsp;and restores it on the next launch. You can disable this or use separate cache keys for multiple accounts:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>// No caching
ElementsClient.InitializeDefault("http://localhost:8080", cacheSession: false);

// Named cache key (useful if you support multiple local profiles)
var client = new ElementsClient("http://localhost:8080", cacheKey: "player2");</code></pre>
<!-- /wp:code -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="toc_5">Sign-up and Login</h2>
<!-- /wp:heading -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="toc_6">Registering a new user</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p><code>SignUpUserAsync</code>&nbsp;creates a new account. After sign-up you will typically want to log the user in immediately.</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>using Elements.Api;
using Elements.Model;
using Elements.Client;

public class AuthManager : MonoBehaviour
{
    async void SignUp(string email, string password)
    {
        var request = new UserCreateRequest(
            email: email,
            password: password
        );

        UserCreateResponse newUser = await ElementsClient.Default.Api.SignUpUserAsync(request);
        Debug.Log($"Created user: {newUser.Id} ({newUser.Email})");

        // Log in right after sign-up
        await Login(email, password);
    }
}</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p><code>UserCreateRequest</code>&nbsp;also accepts optional fields such as&nbsp;<code>name</code>,&nbsp;<code>firstName</code>,&nbsp;<code>lastName</code>, and&nbsp;<code>primaryPhoneNb</code>.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="toc_7">Logging in</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p><code>CreateUsernamePasswordSessionAsync</code>&nbsp;exchanges credentials for a session. The session token is automatically captured by&nbsp;<code>ApiClient.partial.cs</code>&nbsp;and applied to all subsequent requests - you do not need to set any headers manually.</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>async void Login(string userId, string password)
{
    var request = new UsernamePasswordSessionRequest(
        userId: userId,
        password: password
    );

    // SessionCreation is handled transparently; the return value is available
    // if you need to inspect the session directly.
    SessionCreation result = await ElementsClient.Default.Api.CreateUsernamePasswordSessionAsync(request);

    Debug.Log($"Logged in. Session expires: {result.Session.Expiry}");
}</code></pre>
<!-- /wp:code -->

<!-- wp:quote -->
<blockquote class="wp-block-quote"><!-- wp:paragraph -->
<p><strong>Note:</strong>&nbsp;The&nbsp;<code>userId</code>&nbsp;field accepts either the user's registered email address or their Elements user ID.</p>
<!-- /wp:paragraph --></blockquote>
<!-- /wp:quote -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="toc_8">Getting the current user</h3>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code>var user = await ElementsClient.Default.Api.GetCurrentUserAsync();
Debug.Log($"Hello, {user.Name}!");</code></pre>
<!-- /wp:code -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="toc_9">Session Management</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p><code>ElementsClient</code>&nbsp;tracks session state. You can query and clear it at any time:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>var client = ElementsClient.Default;

// Check whether there is a valid, non-expired session
if (client.IsSessionActive())
{
    Debug.Log("Session is active.");
}

// Access the current session and profile directly
Elements.Model.Session session = client.GetSession();
string token = client.GetSessionToken();

// Switch the active profile (updates the Elements-ProfileId header)
client.SetProfile(someProfile);

// Log out (clears session from memory and disk)
client.ClearSession();</code></pre>
<!-- /wp:code -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="toc_10">Using Custom Element APIs</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>When you generate code for a custom Element (using&nbsp;<strong>Generate Custom Element Code</strong>), an additional API class is created alongside&nbsp;<code>ElementsCoreApi</code>. In this example, the Element's serve prefix is&nbsp;<code>my-game</code>, and is named&nbsp;<code>MyGame</code>, so a&nbsp;<code>MyGameApi</code>&nbsp;class will be generated. See the&nbsp;<a href="https://namazustudios.com/docs/custom-code/preparing-for-code-generation/">Preparing for Code Generation documentation</a>&nbsp;for more details.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="toc_11">Registering the custom API</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Use&nbsp;<code>CreateAppApi&lt;T&gt;</code>&nbsp;to instantiate and register a custom API. The&nbsp;<code>appServePrefix</code>&nbsp;must match the&nbsp;<code>dev.getelements.elements.app.serve.prefix</code>&nbsp;property set in your Element - you can also find it in the&nbsp;<strong>Element Info</strong>&nbsp;panel in the Elements admin console.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><code>CreateAppApi</code>&nbsp;takes a factory delegate so that the constructor reference is visible to the linker on IL2CPP platforms (iOS, consoles), preventing it from being removed by managed code stripping.</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>using Elements.Api;
using Elements.Client;

public class AppStartup : MonoBehaviour
{
    void Awake()
    {
        ElementsClient.InitializeDefault("http://localhost:8080");

        // Register the custom Element API. Auth headers and session
        // handling are applied automatically, just like the core API.
        ElementsClient.Default.CreateAppApi&lt;MyGameApi&gt;("my-game", url =&gt; new MyGameApi(url));
    }
}</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p><code>CreateAppApi</code>&nbsp;both creates the instance and registers it, so you do not need to call&nbsp;<code>RegisterApi</code>&nbsp;separately.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="toc_12">Calling custom API methods</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Once registered, retrieve the API with&nbsp;<code>GetApi&lt;T&gt;</code>&nbsp;and call its methods:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>public class MatchmakingManager : MonoBehaviour
{
    async void FindMatch()
    {
        var myGameApi = ElementsClient.Default.GetApi&lt;MyGameApi&gt;();

        MultiplayerGameSummary game = await myGameApi.CreateOrFindMatchAsync();

        Debug.Log($"Joined game: {game.Id}");
    }

    async void ChallengePlayer(string opponentProfileId)
    {
        var myGameApi = ElementsClient.Default.GetApi&lt;MyGameApi&gt;();

        var request = new ChallengePlayerRequest { ProfileId = opponentProfileId };
        MultiplayerGameSummary game = await myGameApi.ChallengeProfileAsync(request);

        Debug.Log($"Challenge sent! Game ID: {game.Id}");
    }
}</code></pre>
<!-- /wp:code -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="toc_13">Registering a pre-constructed API instance</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>If you need to configure the API before registering it (e.g. to set a custom base URL or timeout), you can construct it manually and pass it to&nbsp;<code>RegisterApi</code>:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>var config = new Elements.Client.Configuration
{
    BasePath = "http://localhost:8080/app/rest/my-game",
    Timeout = TimeSpan.FromSeconds(30)
};

var MyGameApi = new MyGameApi(config);
ElementsClient.Default.RegisterApi(MyGameApi);</code></pre>
<!-- /wp:code -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="toc_14">Raw HTTP Responses</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Every generated method has a&nbsp;<code>WithHttpInfo</code>&nbsp;variant that returns&nbsp;<code>ApiResponse&lt;T&gt;</code>&nbsp;instead of&nbsp;<code>T</code>&nbsp;directly. This gives you access to the HTTP status code and response headers without any extra overhead.</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>ApiResponse&lt;UserCreateResponse&gt; response =
    await ElementsClient.Default.Api.SignUpUserWithHttpInfoAsync(request);

if (response.StatusCode == HTTPStatusCode.OK) // 200
{
    Debug.Log($"Sign-up succeeded: {response.Data.Id}");
}
else
{
    Debug.LogWarning($"Unexpected status: {response.StatusCode}");
}</code></pre>
<!-- /wp:code -->

<!-- wp:quote -->
<blockquote class="wp-block-quote"><!-- wp:paragraph -->
<p><strong>Note on exceptions:</strong>&nbsp;<code>ExceptionFactory</code>&nbsp;is set to&nbsp;<code>null</code>&nbsp;on all APIs registered through&nbsp;<code>ElementsClient</code>. This means failed requests will&nbsp;<strong>not</strong>&nbsp;throw an&nbsp;<code>ApiException</code>&nbsp;automatically - you should check&nbsp;<code>ApiResponse.StatusCode</code>&nbsp;when you need to handle error responses explicitly.</p>
<!-- /wp:paragraph --></blockquote>
<!-- /wp:quote -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="toc_15">Manual / DI-based Initialization</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>If you prefer to manage the&nbsp;<code>ElementsClient</code>&nbsp;instance yourself - for example, when using a dependency injection framework - skip&nbsp;<code>InitializeDefault</code>&nbsp;and construct the client directly:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>public class GameServices
{
    public ElementsClient Elements { get; }

    public GameServices()
    {
        Elements = new ElementsClient(
            instanceRootUrl: "http://localhost:8080",
            cacheSession: true
        );

        // Register any custom APIs at construction time
        Elements.CreateAppApi&lt;MyGameApi&gt;("my-game", url =&gt; new MyGameApi(url));
    }
}</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>Then inject&nbsp;<code>GameServices</code>&nbsp;(or&nbsp;<code>ElementsClient</code>&nbsp;directly) wherever you need it. All session handling, header injection, and caching behaviour is identical to the singleton path.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="support">Support</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>For questions or issues,&nbsp;open a GitHub issue with your Unity version,&nbsp;the relevant error logs,&nbsp;and a description of the steps to reproduce.</p>
<!-- /wp:paragraph -->
