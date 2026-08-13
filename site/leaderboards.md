<h1>Leaderboards</h1>

<!-- wp:paragraph -->
<p>Elements supports <strong>Leaderboards</strong> for tracking player scores and displaying ranked results. A leaderboard defines the rules for <em>how scores are tracked over time</em> and <em>how new scores are applied</em>.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>There are three primary resources involved:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>Leaderboard</strong> – the configuration (name, reset cadence, score strategy, etc.)</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Score</strong> – a player’s score entry on a leaderboard</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Rank / RankRow</strong> – a ranked view of scores (position + score/user info)</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:quote -->
<blockquote class="wp-block-quote"><!-- wp:paragraph -->
<p>Before working with Scores or Ranks, you must create a Leaderboard.</p>
<!-- /wp:paragraph --></blockquote>
<!-- /wp:quote -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-leaderboard-properties">Leaderboard Properties</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>A <strong>Leaderboard</strong> has the following properties:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>id</strong> – Database ID assigned when created.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>name</strong> – Unique identifier for the leaderboard.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>title</strong> – Player-facing title shown in UI.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>scoreUnits</strong> – Units label for the score (“points”, “coins”, etc.).</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>timeStrategyType</strong> – Controls whether the leaderboard resets:<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>ALL_TIME</strong> – does not reset</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>EPOCHAL</strong> – resets on a fixed interval</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list --></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>scoreStrategyType</strong> – Controls how new scores update existing ones:<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>OVERWRITE_IF_GREATER</strong> – keeps <code>max(old, new)</code></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>ACCUMULATE</strong> – keeps <code>old + new</code></li>
<!-- /wp:list-item --></ul>
<!-- /wp:list --></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>firstEpochTimestamp</strong> (EPOCHAL only) – epoch start time (milliseconds since epoch).</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>epochInterval</strong> (EPOCHAL only) – epoch duration in milliseconds.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>If either <code>firstEpochTimestamp</code> or <code>epochInterval</code> is set during creation, the other must also be provided.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-creating-and-managing-leaderboards-server-side">Creating and Managing Leaderboards (Server-Side)</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>On the server, you can manage leaderboards using the <code>LeaderboardDao</code>.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-create-a-leaderboard">Create a Leaderboard</h3>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code>import dev.getelements.elements.sdk.dao.LeaderboardDao;
import dev.getelements.elements.sdk.model.leaderboard.Leaderboard;

public class LeaderboardsService {

    @Inject
    private LeaderboardDao leaderboardDao;

    public Leaderboard createAllTimeHighScoreLeaderboard() {
        Leaderboard lb = new Leaderboard();
        lb.setName("global_high_score");
        lb.setTitle("Global High Score");
        lb.setScoreUnits("points");

        lb.setTimeStrategyType(Leaderboard.TimeStrategyType.ALL_TIME);
        lb.setScoreStrategyType(Leaderboard.ScoreStrategyType.OVERWRITE_IF_GREATER);

        return leaderboardDao.createLeaderboard(lb);
    }
}

    public Leaderboard createAllTimeHighScoreLeaderboard() {
        Leaderboard lb = new Leaderboard();
        lb.setName("global_high_score");
        lb.setTitle("Global High Score");
        lb.setScoreUnits("points");

        lb.setTimeStrategyType(Leaderboard.TimeStrategyType.ALL_TIME);
        lb.setScoreStrategyType(Leaderboard.ScoreStrategyType.OVERWRITE_IF_GREATER);

        return leaderboardDao.createLeaderboard(lb);
    }
}
</code></pre>
<!-- /wp:code -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-create-an-epochal-resetting-leaderboard">Create an Epochal (Resetting) Leaderboard</h3>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code>import java.time.Instant;

public Leaderboard createWeeklyLeaderboard() {
    Leaderboard lb = new Leaderboard();
    lb.setName("weekly_points");
    lb.setTitle("Weekly Points");
    lb.setScoreUnits("points");

    lb.setTimeStrategyType(Leaderboard.TimeStrategyType.EPOCHAL);
    lb.setScoreStrategyType(Leaderboard.ScoreStrategyType.ACCUMULATE);

    // Example: start epochs at a known boundary.
    long firstEpochMs = Instant.parse("2026-01-01T00:00:00Z").toEpochMilli();
    long oneWeekMs = 7L * 24 * 60 * 60 * 1000;

    lb.setFirstEpochTimestamp(firstEpochMs);
    lb.setEpochInterval(oneWeekMs);

    return leaderboardDao.createLeaderboard(lb);
}
</code></pre>
<!-- /wp:code -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-list-and-fetch-leaderboards">List and Fetch Leaderboards</h3>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code>import dev.getelements.elements.sdk.model.Pagination;

public Pagination&lt;Leaderboard&gt; listLeaderboards(int offset, int count) {
    return leaderboardDao.getLeaderboards(offset, count);
}

public Leaderboard getLeaderboard(String nameOrId) {
    return leaderboardDao.getLeaderboard(nameOrId);
}
</code></pre>
<!-- /wp:code -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-update-and-delete-a-leaderboard">Update and Delete a Leaderboard</h3>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code>public Leaderboard renameTitle(String nameOrId, String newTitle) {
    Leaderboard lb = leaderboardDao.getLeaderboard(nameOrId);
    lb.setTitle(newTitle);
    return leaderboardDao.updateLeaderboard(nameOrId, lb);
}

public void deleteLeaderboard(String nameOrId) {
    leaderboardDao.deleteLeaderboard(nameOrId);
}
</code></pre>
<!-- /wp:code -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-end-to-end-example-server-side">End-to-End Example (Server-Side)</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Below is a consolidated example that shows a typical server-side flow:</p>
<!-- /wp:paragraph -->

<!-- wp:list {"ordered":true} -->
<ol class="wp-block-list"><!-- wp:list-item -->
<li>Create (or fetch) a leaderboard</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Post a score for a profile</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Fetch ranked results (global + relative)</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Optionally fetch a UI-friendly tabular view</li>
<!-- /wp:list-item --></ol>
<!-- /wp:list -->

<!-- wp:code -->
<pre class="wp-block-code"><code>import dev.getelements.elements.sdk.dao.LeaderboardDao;
import dev.getelements.elements.sdk.dao.RankDao;
import dev.getelements.elements.sdk.dao.ScoreDao;
import dev.getelements.elements.sdk.model.Pagination;
import dev.getelements.elements.sdk.model.Tabulation;
import dev.getelements.elements.sdk.model.leaderboard.Leaderboard;
import dev.getelements.elements.sdk.model.leaderboard.Rank;
import dev.getelements.elements.sdk.model.leaderboard.RankRow;
import dev.getelements.elements.sdk.model.leaderboard.Score;
import dev.getelements.elements.sdk.model.profile.Profile;

public class LeaderboardFlow {

    @Inject
    private LeaderboardDao leaderboardDao;

    @Inject
    private ScoreDao scoreDao;

    @Inject
    private RankDao rankDao;

    public void runExample(Profile profile, double points) {

        // 1) Create (or fetch) the leaderboard configuration.
        // If your deployment provisions leaderboards up-front, you can skip creation and just call getLeaderboard(...).
        Leaderboard lb = new Leaderboard();
        lb.setName("global_high_score");
        lb.setTitle("Global High Score");
        lb.setScoreUnits("points");
        lb.setTimeStrategyType(Leaderboard.TimeStrategyType.ALL_TIME);
        lb.setScoreStrategyType(Leaderboard.ScoreStrategyType.OVERWRITE_IF_GREATER);

        try {
            leaderboardDao.createLeaderboard(lb);
        } catch (Exception ignored) {
            // Leaderboard likely already exists.
        }

        Leaderboard resolved = leaderboardDao.getLeaderboard("global_high_score");

        // 2) Post a score for this profile.
        Score score = new Score();
        score.setProfile(profile);
        score.setPointValue(points);

        scoreDao.createOrUpdateScore(resolved.getId(), score);

        long epoch = 0L;

        // 3a) Fetch global ranks (top N).
        Pagination&lt;Rank&gt; global = rankDao.getRanksForGlobal(resolved.getId(), 0, 25, epoch);

        // 3b) Fetch ranks relative to the current profile.
        Pagination&lt;Rank&gt; relative = rankDao.getRanksForGlobalRelative(
                resolved.getId(),
                profile.getId(),
                0,
                25,
                epoch
        );

        // 4) Optional: fetch a UI-friendly table of rows.
        Tabulation&lt;RankRow&gt; tabular = rankDao.getRanksForGlobalTabular(resolved.getId(), epoch);
    }
}
</code></pre>
<!-- /wp:code -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-posting-scores">Posting Scores</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>A <strong>Score</strong> records a player’s entry on a leaderboard.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Typical usage:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Post a score when a match ends or an achievement occurs.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Let the leaderboard’s <strong>scoreStrategyType</strong> decide whether to overwrite or accumulate.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-example-server-side-create-or-update-a-score">Example (Server-Side) – Create or Update a Score</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>On the server, use <code>ScoreDao#createOrUpdateScore(...)</code> to write a player’s score for a leaderboard. If a score already exists for the same <strong>leaderboard + profile</strong>, the existing row is updated rather than creating a duplicate.</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>import dev.getelements.elements.sdk.dao.ScoreDao;
import dev.getelements.elements.sdk.model.leaderboard.Score;
import dev.getelements.elements.sdk.model.profile.Profile;

public class ScoresService {

    @Inject
    private ScoreDao scoreDao;

    public Score postScore(String leaderboardNameOrId, Profile profile, double points) {
        Score score = new Score();
        score.setProfile(profile);
        score.setPointValue(points);

        // Server assigns creationTimestamp + leaderboardEpoch.
        return scoreDao.createOrUpdateScore(leaderboardNameOrId, score);
    }
}

    public Score postScore(String leaderboardNameOrId, Profile profile, double points) {
        Score score = new Score();
        score.setProfile(profile);
        score.setPointValue(points);

        // Server assigns creationTimestamp + leaderboardEpoch.
        return scoreDao.createOrUpdateScore(leaderboardNameOrId, score);
    }
}
</code></pre>
<!-- /wp:code -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-reading-ranks">Reading Ranks</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>A <strong>Rank</strong> pairs a <code>position</code> (1st, 2nd, 3rd, …) with a <code>Score</code> object. In many UIs you’ll display either:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>a <strong>global</strong> leaderboard page (top N)</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>a <strong>relative</strong> page (N entries centered around the current user)</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>a <strong>friends/followers</strong> page (filtered to a social graph)</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>On the server, use <code>RankDao</code> to retrieve ranked views of scores.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-example-get-global-ranks">Example – Get Global Ranks</h3>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code>import dev.getelements.elements.sdk.dao.RankDao;
import dev.getelements.elements.sdk.model.Pagination;
import dev.getelements.elements.sdk.model.leaderboard.Rank;

public class RanksService {

    @Inject
    private RankDao rankDao;

    public Pagination&lt;Rank&gt; getGlobalRanks(String leaderboardNameOrId, int offset, int count, long epoch) {
        return rankDao.getRanksForGlobal(leaderboardNameOrId, offset, count, epoch);
    }
}

    public Pagination&lt;Rank&gt; getGlobalRanks(String leaderboardNameOrId, int offset, int count, long epoch) {
        return rankDao.getRanksForGlobal(leaderboardNameOrId, offset, count, epoch);
    }
}
</code></pre>
<!-- /wp:code -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-example-get-ranks-relative-to-a-profile">Example – Get Ranks Relative to a Profile</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>This pattern is useful for “show me around me” UI, where the player always sees themselves in the list.</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>import dev.getelements.elements.sdk.model.Pagination;
import dev.getelements.elements.sdk.model.leaderboard.Rank;

public Pagination&lt;Rank&gt; getRelativeGlobalRanks(String leaderboardNameOrId,
                                              String profileId,
                                              int offset, int count,
                                              long epoch) {
    return rankDao.getRanksForGlobalRelative(leaderboardNameOrId, profileId, offset, count, epoch);
}
</code></pre>
<!-- /wp:code -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-example-friends-and-followers-filters">Example – Friends and Followers Filters</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Depending on your social model, you can query ranks filtered to:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>Friends</strong> via <code>getRanksForFriends(...)</code> or <code>getRanksForFriendsRelative(...)</code></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Mutual followers</strong> via <code>getRanksForMutualFollowers(...)</code> or <code>getRanksForMutualFollowersRelative(...)</code></li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:code -->
<pre class="wp-block-code"><code>public Pagination&lt;Rank&gt; getFriendsRanks(String leaderboardNameOrId,
                                       String profileId,
                                       int offset, int count,
                                       long epoch) {
    return rankDao.getRanksForFriends(leaderboardNameOrId, profileId, offset, count, epoch);
}
</code></pre>
<!-- /wp:code -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-tabular-ui-friendly-output">Tabular (UI-Friendly) Output</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>If you want to render a leaderboard without additional profile lookups, you can request a tabular view of <code>RankRow</code>, which includes profile display fields (id, display name, image URL, last login) alongside the score and position.</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>import dev.getelements.elements.sdk.model.Tabulation;
import dev.getelements.elements.sdk.model.leaderboard.RankRow;

public Tabulation&lt;RankRow&gt; getGlobalRanksTabular(String leaderboardNameOrId, long epoch) {
    return rankDao.getRanksForGlobalTabular(leaderboardNameOrId, epoch);
}
</code></pre>
<!-- /wp:code -->

<!-- wp:quote -->
<blockquote class="wp-block-quote"><!-- wp:paragraph -->
<p>For <strong>ALL_TIME</strong> leaderboards, <code>leaderboardEpoch</code> is typically <code>0</code> by convention.</p>
<!-- /wp:paragraph --></blockquote>
<!-- /wp:quote -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-end-to-end-example">End-to-End Example</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Below is a complete server-side flow showing how leaderboards are typically used together:</p>
<!-- /wp:paragraph -->

<!-- wp:list {"ordered":true} -->
<ol class="wp-block-list"><!-- wp:list-item -->
<li>Create (or fetch) a leaderboard</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Post a score for a player</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Read back ranked results</li>
<!-- /wp:list-item --></ol>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>This mirrors the most common production setup.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-example-create-leaderboard-post-score-fetch-ranks">Example – Create Leaderboard, Post Score, Fetch Ranks</h3>
<!-- /wp:heading -->

<!-- wp:code -->
<pre class="wp-block-code"><code>import jakarta.inject.Inject;
import dev.getelements.elements.sdk.dao.LeaderboardDao;
import dev.getelements.elements.sdk.dao.ScoreDao;
import dev.getelements.elements.sdk.dao.RankDao;
import dev.getelements.elements.sdk.model.Pagination;
import dev.getelements.elements.sdk.model.leaderboard.Leaderboard;
import dev.getelements.elements.sdk.model.leaderboard.Score;
import dev.getelements.elements.sdk.model.leaderboard.Rank;
import dev.getelements.elements.sdk.model.profile.Profile;

public class LeaderboardFlowService {

    private static final Logger logger = LoggerFactory.getLogger(LeaderboardFlowService.class);

    @Inject
    private final LeaderboardDao leaderboardDao;

    @Inject
    private final ScoreDao scoreDao;

    @Inject
    private final RankDao rankDao;

    public void runExample(Profile profile) {

        // 1. Create or fetch leaderboard
        Leaderboard leaderboard = leaderboardDao.getLeaderboard("weekly_points");

        if (leaderboard == null) {
            leaderboard = new Leaderboard();
            leaderboard.setName("weekly_points");
            leaderboard.setTitle("Weekly Points");
            leaderboard.setScoreUnits("points");
            leaderboard.setTimeStrategyType(Leaderboard.TimeStrategyType.EPOCHAL);
            leaderboard.setScoreStrategyType(Leaderboard.ScoreStrategyType.ACCUMULATE);

            leaderboard = leaderboardDao.createLeaderboard(leaderboard);
        }

        // 2. Post a score for the player
        Score score = new Score();
        score.setProfile(profile);
        score.setPointValue(250);

        scoreDao.createOrUpdateScore(leaderboard.getId(), score);

        // 3. Fetch top 10 ranks for the current epoch
        long currentEpoch = score.getLeaderboardEpoch();

        Pagination&lt;Rank> ranks = rankDao.getRanksForGlobal(
                leaderboard.getId(),
                0,
                10,
                currentEpoch
        );

        for (Rank rank : ranks.getItems()) {
            logger.debug(rank.getPosition() + ": " +
                               rank.getScore().getProfile().getId() +
                               " : " + rank.getScore().getPointValue());
        }
    }
}
</code></pre>
<!-- /wp:code -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-notes-and-best-practices">Notes and Best Practices</h2>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>Use unique, stable leaderboard names</strong>; treat them as durable identifiers.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Pick <strong>OVERWRITE_IF_GREATER</strong> for “high score” style boards, and <strong>ACCUMULATE</strong> for “total points” style boards.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Prefer <strong>EPOCHAL</strong> leaderboards for seasonal ladders and weekly/monthly competitions.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>When building UI, consider using <strong>relative rank queries</strong> so players always see themselves in the list.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->
