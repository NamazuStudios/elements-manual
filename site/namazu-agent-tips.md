<h1>Tips for Using the Namazu Agent</h1>

<!-- wp:paragraph -->
<p>The <a href="namazu-agent">Namazu Agent</a> works well out of the box, but a little technique goes a long way. This page collects the habits that get the best results out of every session: how to steer the agent before it touches your code, what to do when something goes wrong, and how to pick the right model for the job. For the day-to-day workflow itself, see <a href="namazu-agent-common-tasks">Common Agent Tasks</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><strong>Namazu Cloud is currently in closed beta.</strong> You can <a href="https://cloud.namazustudios.com">sign up for Namazu Cloud</a> today, and once you have, a member of our team can activate the agent in your instance.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-plan-before-you-code">Plan Before You Code</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Before letting the agent write code, make sure it is in <strong>plan mode</strong>. In plan mode the agent reads your workspace, researches the problem, and proposes an approach for your approval before it changes anything. You review the plan, correct its course while it is still cheap, and only then let it start editing.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>This matters because the most expensive mistakes an agent makes are the ones it makes confidently, across many files, before you have said a word. A few minutes of plan review is far cheaper than a large refactor in the wrong direction. It also gives you a natural place to add constraints the agent could not have guessed: naming conventions, parts of the codebase to leave alone, or an approach you already know you want.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>For small, well-defined tasks (a one-line fix, a quick script), plan mode is overkill and you can just let the agent work. For anything that touches multiple files or that you could not describe fully in a sentence, plan first.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-when-you-hit-a-problem-ask-the-agent">When You Hit a Problem, Ask the Agent</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>If you run into a problem, ask the agent. Generally it will help. Unlike a plain chat assistant, the agent is sitting in an environment with real access: your project's code, the logs of your running instance, its database, and the instance's own API. Describe the symptom in plain language ("the login screen hangs after submitting", "matches stop being created after about a minute") and let it investigate.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>This is one of the agent's real strengths. It can correlate what it finds in the code with what it sees in a live instance, which is exactly the kind of cross-referencing that is tedious to do by hand. The same applies to administrative work: if something about your instance seems off, ask the agent to look at it rather than clicking through the admin panel on your own.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-still-stuck-ask-it-to-file-an-issue">Still Stuck? Ask It to File an Issue</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>If you get stuck using the agent even after asking the agent, ask it to file an issue with us. The agent can open a well-formed issue that captures what you were trying to do, what failed, what you already tried, and how to reproduce it, which is exactly the report a human would need to jump in. This is the escalation path for problems that look like they are in Namazu itself rather than in your game's code: the agent hands the failure to the people who can actually fix it.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-ask-us-on-discord">Ask Us on Discord</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>You can always join <a href="https://fly.conncord.com/match/hubspot?hid=21130957&amp;cid=%7B%7B%20personalization_token%28%27contact.hs_object_id%27%2C%20%27%27%29%20%7D%7D">Discord</a> and ask us for help. Whether it is a question the agent could not answer, feedback on the workflow, or just wanting to compare notes with other developers building on Elements, the Discord is the fastest way to reach the team.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-pick-the-right-model">Pick the Right Model</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The agent can run on many different models, and the difference matters. A small, fast model is great for administering an instance, troubleshooting, or working on a budget. A frontier model is the better choice for deep, complex coding tasks, where its higher quality more than pays for the higher price. You can bring your own agent (opencode, Codex, or Claude Code) and supply your own API key or log in to your existing accounts, or use the bundled local models that run on GPU instances for budget-friendly work.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>The table below breaks down the major models: what each is good at, and what it is not. Model generations move quickly, so treat this as a snapshot of the current landscape rather than a permanent reference.</p>
<!-- /wp:paragraph -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th>Model</th><th>Provider</th><th>Good At</th><th>Not Great At</th></tr></thead><tbody>
<tr><td>Claude Fable 5</td><td>Anthropic</td><td>The hardest long-horizon coding: overnight refactors, large migrations, deep debugging. Tops coding and reasoning benchmarks.</td><td>Price. It is the most expensive Claude and overkill for everyday work.</td></tr>
<tr><td>Claude Opus 4.8</td><td>Anthropic</td><td>Quality-first, multi-file refactoring, architecture, code review, and long autonomous runs that stay coherent across dozens of steps.</td><td>The priciest mainstream option and slower to respond than smaller models.</td></tr>
<tr><td>Claude Sonnet 5</td><td>Anthropic</td><td>The daily driver: near-flagship coding quality at a much lower price, strong on code generation and reviews.</td><td>The very hardest, longest-horizon problems, where Opus and Fable still pull ahead.</td></tr>
<tr><td>Claude Haiku 4.5</td><td>Anthropic</td><td>Quick edits, lint fixes, simple well-specified tasks, and fast sub-agent work at the lowest Claude price.</td><td>Complex multi-step reasoning and anything that needs sustained context.</td></tr>
<tr><td>GPT-6 Sol</td><td>OpenAI</td><td>The newest OpenAI flagship tier for demanding coding, science, and security work, with efficient token use.</td><td>Premium pricing and a very new release; most daily work does not need it.</td></tr>
<tr><td>GPT-5.6 Sol</td><td>OpenAI</td><td>Frontier coding and cybersecurity-adjacent work; strong scores on coding-agent benchmarks with fewer tokens.</td><td>Overkill for routine edits; costs well above mid-tier models.</td></tr>
<tr><td>GPT-5.5</td><td>OpenAI</td><td>Terminal-native and DevOps-style work: shell, git, CI/CD debugging. The strongest pick for command-line-heavy sessions.</td><td>Large, repo-scale refactoring, where Claude's long-context coherence wins.</td></tr>
<tr><td>GPT-5.4</td><td>OpenAI</td><td>High-volume workhorse coding with a good speed/price balance; quick at finding edge cases and bugs.</td><td>Consistency on subtle, multi-step logic, where it trails Claude's quality.</td></tr>
<tr><td>GPT-5.4 Mini</td><td>OpenAI</td><td>Cheap, fast handling of simple tasks, classification, and summarization.</td><td>Hard reasoning and complex code; it is a utility model, not a builder.</td></tr>
<tr><td>Gemini 3.1 Pro</td><td>Google</td><td>Value flagship: near-frontier coding at roughly half the price, a 1M-token context window, and native image/video/PDF input.</td><td>Consistency on complex stateful logic; it needs more human oversight on the tricky cases.</td></tr>
<tr><td>Gemini 3.5 Flash</td><td>Google</td><td>Extremely fast (several times quicker than typical frontier models) and competitive on coding benchmarks; ideal for iterative loops and CI.</td><td>The deepest long-horizon work, where flagship reasoning models still win.</td></tr>
<tr><td>Gemini 3.5 Flash Lite</td><td>Google</td><td>The budget end: high volumes of simple, well-specified calls for very little money.</td><td>Anything complex; keep it away from real feature work.</td></tr>
<tr><td>Grok 4.7</td><td>xAI</td><td>Affordable long-context reasoning and general coding assistance at a competitive price.</td><td>Long autonomous multi-file coding sessions, where it is less proven than Claude or GPT flagships.</td></tr>
<tr><td>DeepSeek V4 Pro</td><td>DeepSeek</td><td>Open-weight frontier-class coding (MIT licensed): near-flagship benchmark scores you can self-host for data sovereignty.</td><td>Getting the most from it takes infrastructure; as a managed API its edge shrinks.</td></tr>
<tr><td>DeepSeek V4 Flash</td><td>DeepSeek</td><td>The price floor: extremely cheap output with a 1M-token context, ideal for batch jobs and CI bots.</td><td>Hard agentic work; it is built for volume, not depth.</td></tr>
<tr><td>Qwen3.7 Max</td><td>Alibaba</td><td>Open-weight frontier coding at low cost, competitive with much pricier proprietary models.</td><td>The absolute hardest tasks and the most polished agent ecosystems.</td></tr>
<tr><td>Kimi K2.7 Code</td><td>Moonshot AI</td><td>Coding-focused work and web-browsing agents, with cheap long-context support.</td><td>A smaller ecosystem and less mature tooling than the major labs' models.</td></tr>
<tr><td>GLM-5.2</td><td>Zhipu</td><td>Budget-friendly agentic coding with a 1M-token context and self-hosting options.</td><td>Name recognition and the frontier ceiling; it trades peak quality for price.</td></tr>
<tr><td>MiniMax M3</td><td>MiniMax</td><td>Some of the cheapest frontier-class output available, with a 1M-token context.</td><td>Ecosystem maturity and peak quality on the hardest problems.</td></tr>
<tr><td>Qwen 3.8B (bundled)</td><td>Local, on GPU</td><td>Budget-friendly local model included with the agent: private, no API key needed, good for instance administration, troubleshooting, and quick questions.</td><td>Deep, complex coding tasks; it is far below frontier models and is not meant to replace them.</td></tr>
</tbody></table></figure>
<!-- /wp:table -->

<!-- wp:paragraph -->
<p>As a rule of thumb: use the bundled local model (or a small, cheap model) for administration, troubleshooting, and budget-friendly work, and reserve frontier models, used through your own opencode, Codex, or Claude Code account or API key, for deep and complex coding tasks.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-get-started">Get Started</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Namazu Cloud is currently in closed beta. <a href="https://cloud.namazustudios.com">Sign up for Namazu Cloud</a>, and a member of our team can activate the agent in your instance so you can start building.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-related-pages">Related Pages</h2>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><a href="namazu-agent">Namazu Agent</a>: what the agent is, the agent variants, model options, and how sessions run</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="namazu-agent-common-tasks">Common Agent Tasks</a>: the end-to-end workflow, from starting a session to moving your game to production</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->