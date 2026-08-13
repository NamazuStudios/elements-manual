<h1>Reward Issuances</h1>

<!-- wp:paragraph -->
<p>Reward Issuances represent a promise to grant a user one or more digital goods at a later time. They are commonly used when a reward must be granted <em>as a result of an event</em> (for example, completing a quest or redeeming an in‑app purchase) but should not be immediately applied to the user’s inventory.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>A Reward Issuance can be created by either the client or the server and remains in an <strong>ISSUED</strong> state until it is redeemed. Upon redemption, the underlying Item(s) are applied to the user’s inventory and the issuance is marked <strong>REDEEMED</strong>.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-how-reward-issuances-fit-into-namazu-elements">How Reward Issuances Fit Into Namazu Elements</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Reward Issuances act as a bridge between <strong>events</strong> and <strong>inventory changes</strong>:</p>
<!-- /wp:paragraph -->

<!-- wp:list {"ordered":true} -->
<ol class="wp-block-list"><!-- wp:list-item -->
<li>An event occurs (quest completion, IAP receipt validation, leaderboard placement, etc.).</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>A Reward Issuance is created for a specific user, describing <em>what</em> they are entitled to receive.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>The client explicitly redeems the issuance.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>The associated Item quantity is applied to the user’s inventory.</li>
<!-- /wp:list-item --></ol>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>This flow provides strong guarantees against duplicate rewards, supports delayed redemption, and allows the client to present rewards to the user in a controlled way.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-reward-issuance-lifecycle">Reward Issuance Lifecycle</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>A Reward Issuance moves through a simple lifecycle:</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-1-issued">1. Issued</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>When first created, a Reward Issuance is placed into the <strong>ISSUED</strong> state. In this state:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>The reward has <em>not</em> yet affected the user’s inventory.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>The issuance is eligible to be redeemed by the client.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Only one ISSUED issuance may exist at a time for the same user and context (depending on type; see below).</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-2-redeemed">2. Redeemed</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>When redeemed:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>The issuance transitions to <strong>REDEEMED</strong>.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>The configured Item and quantity are applied to the user’s inventory.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>For <strong>NON_PERSISTENT</strong> issuances, the record may be automatically deleted shortly after redemption.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-core-fields">Core Fields</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>A Reward Issuance consists of the following key properties:</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-user">User</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The user who is entitled to redeem the reward.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-item-and-quantity">Item and Quantity</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Defines <em>what</em> will be granted upon redemption:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>Item</strong>: The digital good to grant.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Item Quantity</strong>: How many units are applied to the user’s inventory.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-state">State</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The current status of the issuance:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>ISSUED</strong> – Created and awaiting redemption.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>REDEEMED</strong> – Successfully redeemed and applied.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>The state cannot be modified directly; redemption must be performed through the appropriate redemption endpoint.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-context">Context</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>A context is a unique string that identifies <em>why</em> this issuance exists. Contexts are critical for preventing duplicate rewards.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Examples include:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>A specific quest step completion</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>An IAP transaction</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>A leaderboard payout</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>When creating an issuance from the client, the context <strong>must remain stable</strong> across retries. This ensures that if a request is retried (for example, due to a network failure), duplicate issuances are not created.</p>
<!-- /wp:paragraph -->

<!-- wp:quote -->
<blockquote class="wp-block-quote"><!-- wp:paragraph -->
<p>Contexts beginning with <code>SERVER.</code> are reserved for server‑generated issuances and should not be used by clients.</p>
<!-- /wp:paragraph --></blockquote>
<!-- /wp:quote -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-issuance-types">Issuance Types</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Reward Issuances support two types that control how duplicates are handled:</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-non-persistent-default">NON_PERSISTENT (Default)</h3>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Multiple issuances may occur over time for the same user and context.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Only one <strong>ISSUED</strong> issuance may exist at a time.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Once redeemed, a new issuance with the same context may be created.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>This is ideal for repeatable rewards such as daily quests or consumable purchases.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-persistent">PERSISTENT</h3>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Only one issuance may <em>ever</em> exist for a given user and context.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Once created (even if redeemed), future attempts with the same context will be rejected.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Expiration timestamps are ignored.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>This is ideal for one‑time rewards such as account‑level unlocks or permanent entitlements.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-expiration">Expiration</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Reward Issuances may optionally define an expiration timestamp:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>If the issuance is not redeemed before the expiration time, it becomes invalid and may be deleted.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Expiration can be extended by updating the timestamp.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Persistent issuances ignore expiration entirely.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>Expiration is useful for limited‑time rewards, promotional grants, or time‑boxed events.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-metadata-and-tags">Metadata and Tags</h2>
<!-- /wp:heading -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-metadata">Metadata</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Reward Issuances may include arbitrary metadata, either client‑defined or server‑generated. Metadata is commonly used to:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Record additional context about the source event</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Attach mission or progression identifiers</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Store platform‑specific purchase details</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-tags">Tags</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Tags provide a lightweight way to categorize or group issuances. They can be used for filtering, analytics, or debugging.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-redemption-results">Redemption Results</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>When redeeming a Reward Issuance, the API returns a <strong>RewardIssuanceRedemptionResult</strong>, which contains:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>The requested issuance ID</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>The updated Reward Issuance (on success)</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>The affected Inventory Item (on success)</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Error details (on failure)</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>This allows clients to reliably confirm whether redemption succeeded and what inventory changes occurred.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-code-samples">Code Samples</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Below are a few Java examples showing common Reward Issuance flows using the <code>RewardIssuanceDao</code>.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-create-or-fetch-an-issuance-idempotently">Create (or Fetch) an Issuance Idempotently</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>A key pattern is to treat issuance creation as <strong>idempotent</strong> by using a stable <code>context</code> string. If the same request is retried (network error, client restart, etc.), the same issuance should be returned rather than creating duplicates.</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>import dev.getelements.elements.sdk.dao.RewardIssuanceDao;
import dev.getelements.elements.sdk.model.goods.Item;
import dev.getelements.elements.sdk.model.reward.RewardIssuance;
import dev.getelements.elements.sdk.model.user.User;

public class RewardsService {

    private final RewardIssuanceDao rewardIssuanceDao;

    public RewardsService(RewardIssuanceDao rewardIssuanceDao) {
        this.rewardIssuanceDao = rewardIssuanceDao;
    }

    public RewardIssuance issueQuestReward(User user, Item item, int quantity,
                                          String questId, int stepSequence, int rewardIndex) {

        // For quest/progression-style rewards you typically want a server-generated context.
        // This context is stable and uniquely ties the issuance to a specific progression event.
        String context = RewardIssuance.buildMissionProgressContextString(
                questId,
                stepSequence,
                rewardIndex
        );

        RewardIssuance issuance = new RewardIssuance();
        issuance.setUser(user);
        issuance.setItem(item);
        issuance.setItemQuantity(quantity);
        issuance.setContext(context);
        issuance.setType(RewardIssuance.Type.NON_PERSISTENT);
        issuance.setSource("MISSION_PROGRESS");
        issuance.addTag("quest");
        issuance.addMetadata("questId", questId);
        issuance.addMetadata("sequence", stepSequence);
        issuance.addMetadata("rewardIndex", rewardIndex);

        // Creates the issuance if it doesn't exist, otherwise returns the existing one.
        // Newly-created issuances will be created in the ISSUED state.
        return rewardIssuanceDao.getOrCreateRewardIssuance(issuance);
    }
}
</code></pre>
<!-- /wp:code -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-create-an-issuance-for-an-iap-receipt">Create an Issuance for an IAP Receipt</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>For purchases, the goal is generally “<strong>never grant this twice</strong>.” Use a context derived from the purchase identifiers.</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>import dev.getelements.elements.sdk.model.reward.RewardIssuance;

public RewardIssuance issueAppleIapReward(User user, Item item, int quantity,
                                         String originalTransactionId, int skuOrdinal) {

    // Uses a hash of (originalTransactionId, itemId, skuOrdinal) to uniquely identify the purchase reward.
    String context = RewardIssuance.buildAppleIapContextString(
            originalTransactionId,
            item.getId(),
            skuOrdinal
    );

    RewardIssuance issuance = new RewardIssuance();
    issuance.setUser(user);
    issuance.setItem(item);
    issuance.setItemQuantity(quantity);
    issuance.setContext(context);

    // For most IAP rewards, PERSISTENT is a good default because you want a one-time grant.
    issuance.setType(RewardIssuance.Type.PERSISTENT);
    issuance.setSource("APPLE_IAP");
    issuance.addTag("iap");

    return rewardIssuanceDao.getOrCreateRewardIssuance(issuance);
}
</code></pre>
<!-- /wp:code -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-redeem-an-issuance">Redeem an Issuance</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Redemption applies the issuance to the user’s inventory and returns the <code>InventoryItem</code> that was modified.</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>import dev.getelements.elements.sdk.model.inventory.InventoryItem;
import dev.getelements.elements.sdk.model.reward.RewardIssuance;

public InventoryItem redeem(RewardIssuance issuance) {
    // Once redeemed, the issuance will be applied to the user's inventory.
    // Redemption must be safe to call multiple times without double-crediting.
    return rewardIssuanceDao.redeem(issuance);
}
</code></pre>
<!-- /wp:code -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-fetch-outstanding-issuances-then-redeem">Fetch Outstanding Issuances Then Redeem</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>A common client/server flow is to fetch all ISSUED issuances, present them to the user, and redeem them when the user accepts.</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>import dev.getelements.elements.sdk.model.Pagination;
import dev.getelements.elements.sdk.model.reward.RewardIssuance;
import dev.getelements.elements.sdk.model.reward.RewardIssuance.State;

import java.util.List;

public void redeemAllIssued(User user) {

    // Fetch a page of outstanding issuances.
    Pagination&lt;RewardIssuance&gt; page = rewardIssuanceDao.getRewardIssuances(
            user,
            0,
            50,
            List.of(State.ISSUED),
            List.of() // optionally filter by tags
    );

    for (RewardIssuance issuance : page.getItems()) {
        rewardIssuanceDao.redeem(issuance);
    }
}
</code></pre>
<!-- /wp:code -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-common-use-cases">Common Use Cases</h2>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>Quest Rewards</strong>: Issue rewards on quest completion and let the client redeem them when presenting a reward screen.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>In‑App Purchases</strong>: Issue rewards only after server‑side receipt validation.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Promotions</strong>: Grant time‑limited rewards with expirations.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>One‑Time Unlocks</strong>: Use persistent issuances to guarantee a reward is only ever granted once.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:paragraph -->
<p>Reward Issuances provide a flexible, reliable mechanism for granting digital goods while protecting against duplication and ensuring a clean, auditable reward flow.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-code-examples">Code Examples</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The following examples demonstrate common server‑side flows for creating and redeeming Reward Issuances using the RewardIssuanceDao. These examples assume server‑side execution and omit error handling for clarity.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-creating-a-reward-issuance">Creating a Reward Issuance</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Reward Issuances are typically created in response to a validated event, such as quest completion or a verified in‑app purchase.</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>RewardIssuance issuance = new RewardIssuance();

issuance.setUser(user);
issuance.setItem(item);
issuance.setItemQuantity(1);
issuance.setType(RewardIssuance.Type.NON_PERSISTENT);
issuance.setSource("QUEST");
issuance.setContext(
    RewardIssuance.buildContextString(
        "quest",
        questId,
        stepId
    )
);

RewardIssuance created = rewardIssuanceDao.create(issuance);
</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>Key points:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>The context must uniquely identify the event that caused the issuance.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Retrying the same create call with the same context is safe.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>The issuance is created in the <strong>ISSUED</strong> state automatically.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-redeeming-a-reward-issuance">Redeeming a Reward Issuance</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Redemption is an explicit action that applies the reward to the user’s inventory and transitions the issuance to <strong>REDEEMED</strong>.</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>RewardIssuanceRedemptionResult result = rewardIssuanceDao.redeem(rewardIssuanceId);

if (result.getErrorDetails() == null) {
    RewardIssuance redeemed = result.getRewardIssuance();
    InventoryItem inventoryItem = result.getInventoryItem();

    // Redemption succeeded
} else {
    // Redemption failed
}
</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>On successful redemption:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>The issuance state is updated to <strong>REDEEMED</strong>.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>The associated InventoryItem is created or updated.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>NON_PERSISTENT issuances may be deleted shortly after redemption.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-redeeming-multiple-issuances">Redeeming Multiple Issuances</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Some flows (such as bulk reward claiming) may require redeeming multiple issuances at once. This can be done from the RewardIssuanceService, instead of the DAO:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>List&lt;String> issuanceIds = List.of(id1, id2, id3);

List&lt;RewardIssuanceRedemptionResult> results =
    rewardIssuanceService.redeemRewardIssuances(issuanceIds);

for (RewardIssuanceRedemptionResult result : results) {
    if (result.getErrorDetails() == null) {
        // Success
    } else {
        // Handle failure
    }
}
</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>Each issuance is redeemed independently; a failure in one does not prevent others from succeeding.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-idempotency-and-safety">Idempotency and Safety</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Reward Issuances are designed to be safely retried:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Creating an issuance with the same user and context will not create duplicates.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Redeeming an already redeemed issuance will return an error without re‑applying inventory changes.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Persistent issuances provide strong guarantees for one‑time rewards.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>These guarantees make Reward Issuances ideal for distributed systems where retries and partial failures are expected.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->
