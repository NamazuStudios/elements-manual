<h1>Shaping Workloads for Your Dedicated Nodes</h1>

<!-- wp:paragraph -->
<p>Every workspace and every instance in Namazu Cloud runs in its own Kubernetes namespace, and its workloads run on nodes dedicated to it — nodes carry exactly one tenant's work, and other tenants cannot land there. To make that work, a pod must <em>say who it is</em>: it carries a small scheduling annotation (an identity pin) that ties it to its own namespace, plus a toleration that lets it onto tenant nodes in the first place. This page documents that contract — when you must know about it, and when the platform handles it for you.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>In the common case you don't hand-write any of this: jobs dispatched through the platform (job and service templates, agent sessions) are shaped automatically. The contract matters when you bring your own workload definitions — raw Kubernetes manifests, CI pipelines, Helm charts, or anything else that creates pods directly.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"anchor":"h-workspaces-vs-instances"} -->
<h2 id="h-workspaces-vs-instances" class="wp-block-heading">Workspaces vs Instances</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Namazu Cloud has two deployment types, and they differ in what they are for and in which identity label they use. Both follow the same rule: <strong>the namespace name <em>is</em> the identity</strong>.</p>
<!-- /wp:paragraph -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th></th><th>Workspace</th><th>Instance</th></tr></thead><tbody><tr><td>What it is</td><td>A development sandbox where <a href="namazu-agent">agent sessions</a> and project work happen</td><td>A dedicated, hosted run of <a href="namazu-cloud-instances">Elements</a> serving your game</td></tr><tr><td>Created</td><td>When you start working in Namazu Cloud</td><td>From a product version and a SKU, per organization</td></tr><tr><td>Namespace name</td><td>The workspace id</td><td>The instance id</td></tr><tr><td>Identity label key</td><td><code>cloud.namazustudios.com/workspace_id</code></td><td><code>cloud.namazustudios.com/instance_id</code></td></tr><tr><td>Typical workloads</td><td>Agent sessions, builds, scratch jobs, preview stacks</td><td>Your live game's services, your own job and service templates</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:paragraph -->
<p>Everything below applies to both; only the label key differs. Where the examples say <code>&lt;namespace&gt;</code>, substitute your workspace id or instance id — it is the same string as the namespace your workload runs in.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"anchor":"h-the-scheduling-contract"} -->
<h2 id="h-the-scheduling-contract" class="wp-block-heading">The Scheduling Contract</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>A compliant pod carries two things:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>An identity pin</strong> — a <code>nodeSelector</code> (or a required node-affinity term) on your identity label key, whose value equals your own namespace name. This is what puts the pod on a node dedicated to you, and it is what your nodes are tagged with for billing.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>A toleration for the dedicated taint</strong> — tenant nodes carry <code>cloud.namazustudios.com/dedicated=tenant:NoSchedule</code>, so only pods that tolerate it can land there.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>Both are checked when the pod is created. A pod that pins <em>another</em> namespace's identity, or that uses a bare <code>Exists</code> (which would match any tenant's node), is rejected outright — the pin must be present and must be yours.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3,"anchor":"h-workspace-example"} -->
<h3 id="h-workspace-example" class="wp-block-heading">Workspace example</h3>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code>apiVersion: v1
kind: Pod
metadata:
  name: my-batch-job
  namespace: my-workspace            # your workspace id
spec:
  nodeSelector:
    # must equal metadata.namespace, exactly
    cloud.namazustudios.com/workspace_id: my-workspace
  tolerations:
    - key: cloud.namazustudios.com/dedicated
      operator: Equal
      value: tenant
      effect: NoSchedule
  containers:
    - name: main
      image: registry.example.com/my-job:latest</code></pre>
<!-- /wp:code -->

<!-- wp:heading {"level":3,"anchor":"h-instance-example"} -->
<h3 id="h-instance-example" class="wp-block-heading">Instance example</h3>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code>apiVersion: v1
kind: Pod
metadata:
  name: my-worker
  namespace: my-instance             # your instance id
spec:
  nodeSelector:
    cloud.namazustudios.com/instance_id: my-instance
  tolerations:
    - key: cloud.namazustudios.com/dedicated
      operator: Equal
      value: tenant
      effect: NoSchedule
  containers:
    - name: main
      image: registry.example.com/my-worker:latest</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>A required node-affinity term is accepted in place of <code>nodeSelector</code>, as long as it is an <code>In</code> constraint on the identity key with your namespace as the only value. Prefer the <code>nodeSelector</code> form — it says the same thing in three lines.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"anchor":"h-how-strict"} -->
<h2 id="h-how-strict" class="wp-block-heading">How Strict Is It?</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Enforcement is per namespace and can escalate as a tenant is moved onto the shared cluster capacity. The level is recorded on your namespace as a label and is managed by the platform, not by you:</p>
<!-- /wp:paragraph -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th>Level</th><th>What happens to a non-compliant pod</th></tr></thead><tbody><tr><td><code>audit</code></td><td>Admitted. The result is recorded on the API server's audit log.</td></tr><tr><td><code>warn</code> (default)</td><td>Admitted, and the pod's creator receives a warning describing the fix.</td></tr><tr><td><code>deny</code></td><td>Rejected at creation with a clear message.</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:paragraph -->
<p>Workloads dispatched through the platform are always compliant, so the practical effect of escalation is on self-managed workloads: under <code>warn</code> you will see warnings; under <code>deny</code> a non-compliant pod fails to create. The rejection message names the missing piece — a pin, a correct pin value, or the dedicated-taint toleration.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"anchor":"h-three-ways-to-run"} -->
<h2 id="h-three-ways-to-run" class="wp-block-heading">Three Ways to Run a Workload</h2>
<!-- /wp:heading -->

<!-- wp:heading {"level":3,"anchor":"h-job-and-service-templates"} -->
<h3 id="h-job-and-service-templates">1. Job and service templates (nothing to do)</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p><a href="namazu-cloud-job-and-service-templates">Job and service templates</a> are the platform's way to run extra workloads inside your instance: jobs run to completion, services keep running. Templates you define on the instance page are dispatched by the platform with the identity pin and toleration already applied — you never see the scheduling details.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3,"anchor":"h-ask-the-agent"} -->
<h3 id="h-ask-the-agent">2. Ask the agent (nothing to do)</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>In a <a href="namazu-agent">workspace</a>, you can simply ask the agent to run something — a one-off script, a build, a preview stack, a long-running service. The agent dispatches it through the same platform machinery, so the workload lands on your workspace's nodes and is compliant by construction. If you need the workload shaped differently (a bigger instance type, spot vs on-demand), say so in the same breath and the agent sets it. GPUs are the exception: they must be <a href="#h-gpu-workloads">requested from Namazu</a> first — the agent can run GPU workloads for you once the request is approved, but it cannot grant the capacity itself.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3,"anchor":"h-bring-your-own"} -->
<h3 id="h-bring-your-own">3. Bring your own manifests (carry the contract yourself)</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>If you create pods directly — <code>kubectl</code>, a CI pipeline, Helm, a controller you run — include the pin and toleration from the examples above. The minimal checklist:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>The pod's <code>metadata.namespace</code> is your workspace id or instance id (it always is — that's where you are).</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>nodeSelector</code> carries the matching identity key with that same namespace as the value.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><code>tolerations</code> includes <code>cloud.namazustudios.com/dedicated=tenant:NoSchedule</code>.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>If your tenant still runs on its own dedicated pool from before the shared-capacity migration, you may also see a second toleration keyed on the identity label itself — the platform's templates tolerate both during the transition; copying the examples above is sufficient either way.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"anchor":"h-gpu-workloads"} -->
<h2 id="h-gpu-workloads" class="wp-block-heading">GPU Workloads</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>GPU nodes are granted per tenant <strong>on request</strong> and carry an additional taint, <code>nvidia.com/gpu=present:NoSchedule</code>. A GPU workload therefore requests the GPU resource <em>and</em> tolerates the GPU taint — both together:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>apiVersion: v1
kind: Pod
metadata:
  name: my-inference
  namespace: my-instance
spec:
  nodeSelector:
    cloud.namazustudios.com/instance_id: my-instance
  tolerations:
    - key: cloud.namazustudios.com/dedicated
      operator: Equal
      value: tenant
      effect: NoSchedule
    - key: nvidia.com/gpu
      operator: Equal
      value: present
      effect: NoSchedule
  containers:
    - name: main
      image: registry.example.com/my-inference:latest
      resources:
        limits:
          nvidia.com/gpu: 1</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>Three rules are enforced for GPU pods, and a violation is rejected with a message that says which one:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>A pod that requests GPUs must tolerate the GPU taint (otherwise it could never schedule).</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>A pod that tolerates the GPU taint must actually request GPUs — CPU work cannot park on GPU nodes.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>A single pod may not request more GPUs than the tenant's per-pod cap; the aggregate ceiling is enforced by the tenant's GPU pool configuration.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>GPUs are <strong>not self-serve</strong>. GPU capacity must be requested from Namazu: open a request describing what the workload is, how many GPUs per pod it needs, and how long it runs. Once approved, the platform enables GPU capacity for the tenant and the examples above work as written. Until then, GPU requests are rejected — that rejection is the guardrail doing its job, not something to work around, and no scheduling trick (extra tolerations, different instance types) will land a GPU pod on a tenant that has not been granted capacity.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"anchor":"h-troubleshooting"} -->
<h2 id="h-troubleshooting" class="wp-block-heading">Troubleshooting</h2>
<!-- /wp:heading -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th>Symptom</th><th>Meaning</th><th>Fix</th></tr></thead><tbody><tr><td>Pod creation rejected: "must pin its own tenant identity"</td><td>No <code>nodeSelector</code>/affinity pin on the identity key</td><td>Add the pin from the example above</td></tr><tr><td>Rejected: "must not pin a tenant identity other than its own namespace"</td><td>The pin's value is a different namespace</td><td>Set the value to <code>metadata.namespace</code> exactly</td></tr><tr><td>Rejected: "operator In with this namespace as the only value"</td><td>An affinity term uses <code>Exists</code> or extra values</td><td>Use <code>operator: In</code> with only your namespace</td></tr><tr><td>Rejected: "must tolerate the dedicated taint"</td><td>Missing <code>dedicated=tenant</code> toleration</td><td>Add it from the example above</td></tr><tr><td>Rejected: "tolerates the GPU capability taint but requests no nvidia.com/gpu"</td><td>GPU toleration without a GPU request</td><td>Request <code>nvidia.com/gpu</code> in <code>resources.limits</code>, or drop the toleration</td></tr><tr><td>Rejected: "more than the per-pod GPU cap"</td><td>One pod asking for the tenant's whole GPU budget</td><td>Split the workload, or request a cap raise from Namazu</td></tr><tr><td>Rejected: pod requests GPUs on a tenant without GPU capacity</td><td>GPUs were never granted for this tenant</td><td><a href="#h-gpu-workloads">Request GPU access from Namazu</a> — it is granted on approval, not self-serve</td></tr><tr><td>Warning on create, pod runs anyway</td><td>The namespace is at the <code>warn</code> level</td><td>Fix it before the level escalates to <code>deny</code></td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:paragraph -->
<p>Platform-dispatched workloads (job and service templates, agent sessions) are always compliant — if one of those is rejected, that is a platform bug and worth reporting rather than working around.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"anchor":"h-related"} -->
<h2 id="h-related" class="wp-block-heading">Related</h2>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><a href="namazu-cloud-instances">Namazu Cloud Instances</a>: creating and managing the hosted Elements deployment</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="namazu-cloud-job-and-service-templates">Job and Service Templates</a>: the no-configuration way to run workloads inside an instance</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="namazu-agent">Namazu Agent</a> and <a href="namazu-agent-common-tasks">Common Agent Tasks</a>: delegating workload runs to the agent in a workspace</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="where-your-authoritative-code-runs">Where Your Authoritative Code Runs</a>: how Elements itself is deployed and scaled</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->
