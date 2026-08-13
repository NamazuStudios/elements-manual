<h1>Unity Crossfire Plugin</h1>

<!-- wp:paragraph -->
<p>Github URL:<br><a href="https://github.com/NamazuStudios/crossfire">https://github.com/NamazuStudios/crossfire</a></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>A peer-to-peer networking plugin for Unity built on Unity Netcode for GameObjects and Unity WebRTC.&nbsp;It handles WebRTC connection negotiation,&nbsp;signaling,&nbsp;host election,&nbsp;and real-time connection monitoring through a single&nbsp;<code>NetworkSessionManager</code>&nbsp;component.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>This plugin requires a running instance of&nbsp;<a href="https://github.com/namazuStudios/crossfire">Crossfire</a>&nbsp;on&nbsp;<a href="https://github.com/NamazuStudios/elements">Elements</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="requirements">Requirements</h2>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Unity 2022.3 or later</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="https://docs-multiplayer.unity3d.com/netcode/current/about/">Unity Netcode for GameObjects</a> via Package Manager (<code>com.unity.netcode.gameobjects</code>)</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="https://docs.unity3d.com/Packages/com.unity.webrtc@3.0/manual/index.html">Unity WebRTC</a> via Package Manager (<code>com.unity.webrtc</code>)</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="https://www.newtonsoft.com/json">Newtonsoft Json</a> via Package Manager (<code>com.unity.nuget.newtonsoft-json</code>)</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>WebSocket Sharp (pre-compiled, included in the Plugins folder)</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="installation">Installation</h2>
<!-- /wp:heading -->

<!-- wp:list {"ordered":true} -->
<ol class="wp-block-list"><!-- wp:list-item -->
<li>Import the required packages via the Unity Package Manager.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Place the <code>NetworkSessionManager</code> prefab in your scene, or add a <code>NetworkSessionManager</code> component to a GameObject manually.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Assign a <code>NetworkManager</code> to the <code>NetworkSessionManager</code> in the Inspector.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Set the <code>serverHost</code> field in the <code>NetworkSessionConfig</code> to the base URL of your Crossfire server (for example, <code>ws://localhost:8080/app/ws/crossfire</code>).</li>
<!-- /wp:list-item --></ol>
<!-- /wp:list -->

<!-- wp:quote -->
<blockquote class="wp-block-quote"><!-- wp:paragraph -->
<p><strong>Warning:</strong>&nbsp;The&nbsp;<code>NetworkSessionManager</code>&nbsp;calls&nbsp;<code>DontDestroyOnLoad</code>&nbsp;on startup.&nbsp;To avoid duplicates when reloading scenes,&nbsp;either place it only in a one-time initialization scene,&nbsp;or check for an existing instance before creating one.</p>
<!-- /wp:paragraph --></blockquote>
<!-- /wp:quote -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="quick-start">Quick Start</h2>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code><code>public class GameManager : MonoBehaviour
{
    &#91;SerializeField] private NetworkSessionManager sessionManager;

    void Start()
    {
        sessionManager.OnPlayerJoined += player =>
            Debug.Log($"{player.profileId} joined");

        sessionManager.OnAllPlayersConnected += () =>
            Debug.Log("All players ready - start gameplay");

        sessionManager.OnConnectionError += error =>
            Debug.LogError($"Network error: {error}");

        // profileId and sessionToken come from your auth flow (e.g. Elements login)
        sessionManager.StartSession("player123", "auth-token");
        sessionManager.FindOrCreateMatch("default");
    }
</code>}</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>A fuller example using the <a href="https://namazustudios.com/docs/add-ons/game-engines/unity/elements-codegen/">Elements Codegen client</a> is in <code>Scripts/Test/NetworkTestViewController.cs</code>.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="documentation">Documentation</h2>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><a href="file:///Users/keithhudnall/Documents/workspace/Elements/github/unity-crossfire-plugin/Assets/Namazu%20Studios/Assets/Namazu%20Studios/Crossfire/docs/events.md">Events Reference</a> - all events, parameter semantics, and usage examples</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="file:///Users/keithhudnall/Documents/workspace/Elements/github/unity-crossfire-plugin/Assets/Namazu%20Studios/Assets/Namazu%20Studios/Crossfire/docs/configuration.md">Configuration</a> - Inspector fields, CrossfireConstants, and tuning</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="file:///Users/keithhudnall/Documents/workspace/Elements/github/unity-crossfire-plugin/Assets/Namazu%20Studios/Assets/Namazu%20Studios/Crossfire/docs/architecture.md">Architecture</a> - state machine, component responsibilities, and local testing</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="file:///Users/keithhudnall/Documents/workspace/Elements/github/unity-crossfire-plugin/Assets/Namazu%20Studios/Assets/Namazu%20Studios/Crossfire/docs/extending.md">Extending the Plugin</a> - custom transports, custom signaling, and best practices</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="support">Support</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>For questions or issues,&nbsp;open a GitHub issue with your Unity version,&nbsp;the relevant error logs,&nbsp;and a description of the steps to reproduce.</p>
<!-- /wp:paragraph -->
