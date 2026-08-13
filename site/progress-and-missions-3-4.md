<h1>Progress and Missions (3.4+)</h1>

<!-- wp:paragraph -->
<p>Missions and quests form a key part of a game’s metagame design. They provide structured, rewarding goals that encourage players to return, progress, and engage with your game’s core systems. Properly designed missions drive retention by offering incremental achievements and tangible rewards that reinforce player investment over time.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>This guide refers to version of Namazu Elements 3.4 and above.</p>
<!-- /wp:paragraph -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"\u003cmark style=\u0022background-color:rgba(0, 0, 0, 0)\u0022 class=\u0022has-inline-color has-black-color\u0022\u003e\u003cstrong\u003e📝Notes on Missions and Progress\u003c/strong\u003e\u003c/mark\u003e"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="bf9709"><div class="gb-notice-title" style="color:#fff"><p><mark style="background-color:rgba(0, 0, 0, 0)" class="has-inline-color has-black-color"><strong>📝Notes on Missions and Progress</strong></mark></p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p>Namazu Elements missions serve a similar purpose to PlayFab’s <em>Quests</em> or <em>Event-based Tasks</em>. Both systems define multi-step objectives that track player progress and grant rewards upon completion. This section explains how to manage missions in Namazu Elements using the admin console.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:paragraph -->
<p>This guide explains how to create, update and delete missions using the Namazu Elements CMS. A mission is a set of steps that players can complete to earn rewards. Each step has a completion threshold and grants rewards when finished. Missions are defined in the admin console and copied to each user’s profile when they begin the mission, so modifying a mission after users have started it does <strong>not</strong> affect their existing progress.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-creating-a-multi-step-mission">Creating a Multi-Step Mission</h2>
<!-- /wp:heading -->

<!-- wp:list {"ordered":true} -->
<ol class="wp-block-list"><!-- wp:list-item -->
<li>From the navigation menu, choose <strong>Progress → Missions</strong>. The missions list appears with an <strong>Add Mission</strong> button.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Click <strong>Add Mission</strong>. A blank mission editor opens. Fill in the mission-level fields: Field Description and guidance <strong>Display Name</strong> Human-readable title shown in the UI. <strong>Name</strong> Unique identifier used in APIs and scripts. <strong>Description</strong> Summary of what the mission requires. <strong>Tags</strong> Comma-separated labels for organisation or filtering (optional). <strong>Metadata</strong> Key–value pairs for additional data, such as image paths or categories.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>In the <strong>Steps</strong> section, click <strong>Add Step</strong> to create the first mission step. For each step, provide: Field Purpose <strong>Display Name</strong> Name of the step visible to players. <strong>Description</strong> Brief description of the objective. <strong>Count</strong> Number of times the player must perform an action to complete the step. For example, <code>5</code> means the action must be performed five times. <strong>Rewards</strong> Add rewards by selecting an existing item (e.g. <code>sample_reward</code>) and specifying the quantity. Use the <strong>+</strong> button to add multiple reward items. <strong>Metadata</strong> Optional key–value pairs for step-specific data.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Repeat step 3 for additional steps. Increase the <code>count</code> threshold and reward quantity for each successive step to encourage continued engagement. Example: Step Count Reward Item Quantity Description 1 5 sample_reward 10 Collect 5 stars 2 10 sample_reward 20 Collect 10 stars 3 15 sample_reward 30 Collect 15 stars</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>(Optional) Check <strong>Final Repeat Step</strong> on the last step if it should repeat indefinitely.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Click <strong>Save</strong> or <strong>Create</strong>. The mission definition is stored and appears in the missions list.</li>
<!-- /wp:list-item --></ol>
<!-- /wp:list -->

<!-- wp:image {"id":22245,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://namazustudios.com/wp-content/uploads/2025/11/image-2-1024x836.png" alt="" class="wp-image-22245"/></figure>
<!-- /wp:image -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-updating-a-mission">Updating a Mission</h2>
<!-- /wp:heading -->

<!-- wp:list {"ordered":true} -->
<ol class="wp-block-list"><!-- wp:list-item -->
<li>In the missions list, locate the mission you created. Use the search bar or filter tags as needed.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Click the <strong>Edit</strong> button next to the mission name. The mission editor opens with current values.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Update mission-level fields (display name, description, tags, metadata) or modify existing steps:</li>
<!-- /wp:list-item --></ol>
<!-- /wp:list -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Change a step’s <strong>Count</strong> to alter the completion threshold.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Adjust reward quantities or add another reward item.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Use the drag handle to reorder steps.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Click <strong>Add Step</strong> to insert new steps or <strong>Delete Step</strong> to remove one.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:list {"ordered":true} -->
<ol class="wp-block-list"><!-- wp:list-item -->
<li>After making changes, click <strong>Save</strong>. The mission definition updates; however, remember that changes do <strong>not</strong> affect users who already started the mission.</li>
<!-- /wp:list-item --></ol>
<!-- /wp:list -->

<!-- wp:image {"id":22246,"sizeSlug":"full","linkDestination":"none"} -->
<figure class="wp-block-image size-full"><img src="https://namazustudios.com/wp-content/uploads/2025/11/image-3.png" alt="" class="wp-image-22246"/></figure>
<!-- /wp:image -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-deleting-a-mission">Deleting a Mission</h2>
<!-- /wp:heading -->

<!-- wp:list {"ordered":true} -->
<ol class="wp-block-list"><!-- wp:list-item -->
<li>In the missions list, find the mission you wish to delete.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Click the <strong>Delete</strong> button (trash-can icon) next to the mission.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Confirm the deletion when prompted. The mission definition is removed from the database.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Deleting a mission prevents new users from starting it, but it does not remove progress data from users who already started the mission.</li>
<!-- /wp:list-item --></ol>
<!-- /wp:list -->

<!-- wp:image {"id":22247,"width":"840px","height":"auto","sizeSlug":"full","linkDestination":"none"} -->
<figure class="wp-block-image size-full is-resized"><img src="https://namazustudios.com/wp-content/uploads/2025/11/image-4.png" alt="" class="wp-image-22247" style="width:840px;height:auto"/></figure>
<!-- /wp:image -->

<!-- wp:image {"id":22248,"sizeSlug":"full","linkDestination":"none"} -->
<figure class="wp-block-image size-full"><img src="https://namazustudios.com/wp-content/uploads/2025/11/image-5.png" alt="" class="wp-image-22248"/></figure>
<!-- /wp:image -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-best-practices">Best Practices</h2>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>Plan your mission structure.</strong> Because mission definitions are copied to user profiles when a mission starts, editing a mission later will not retroactively update existing players’ progress. Make sure the steps, rewards, and metadata are correct before activating the mission.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Use meaningful names and descriptions</strong> so administrators and players can easily identify missions and steps.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Scale rewards thoughtfully.</strong> Increasing reward quantities for later steps motivates players, but avoid making rewards so large that they destabilise the game economy.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Organise with tags and metadata.</strong> Tags help you filter missions in the console, and metadata allows you to attach custom data like images or categories.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-integrating-missions-with-analytics-and-retention">Integrating Missions with Analytics and Retention</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Mission completion data is an effective tool for measuring player engagement and progression. By tracking which missions are started, completed, or abandoned, developers can identify trends in player bahaior and adjust content accordingly. Integrating mission data into your analytics pipeline allows you to measure retention, tune difficulty pacing, and optimize rewards. Over time, this feedback loop helps improve the overall player experience and supports long-term engagement strategies.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading">Using Schedules</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Schedules control <strong>when missions are active</strong> and which missions should be assigned to players at a given time. They’re ideal for rotating, seasonal, or limited-time content.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>A schedule consists of:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>Schedule</strong> – a named container (e.g. <code>weekly_quests</code>, <code>season_1</code>)</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>ScheduleEvent</strong> – associates a mission with a start/end time</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>At runtime, schedules are used to <strong>assign and unassign Progress records</strong> for the missions that are currently active.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading">Typical schedule flow</h3>
<!-- /wp:heading -->

<!-- wp:image {"id":22392,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://namazustudios.com/wp-content/uploads/2025/11/Screenshot-2026-02-03-at-12.23.56-PM-1024x459.png" alt="" class="wp-image-22392"/></figure>
<!-- /wp:image -->

<!-- wp:list {"ordered":true} -->
<ol class="wp-block-list"><!-- wp:list-item -->
<li>Create a <code>Schedule</code> (via the CMS or programatically)</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Create one or more <code>ScheduleEvent</code> entries referencing missions</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Resolve which events are active “now”</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Assign progress for active missions</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Optionally unassign progress for inactive missions</li>
<!-- /wp:list-item --></ol>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>This flow is safe to re-run repeatedly (for example, on player login).</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-creating-missions-programatically">Creating Missions Programatically</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>A <strong>Mission</strong> defines a sequence of steps and rewards. Mission definitions are global and are copied into player progress when started.</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>import com.google.inject.Inject;
import dev.getelements.elements.sdk.dao.MissionDao;
import dev.getelements.elements.sdk.model.mission.Mission;
import dev.getelements.elements.sdk.model.mission.Step;

import java.util.List;

public class MissionsService {

    @Inject
    private MissionDao missionDao;

    public Mission createMission() {
        Mission mission = new Mission();
        mission.setName("collect_stars");
        mission.setTitle("Collect Stars");
        mission.setRepeatable(false);

        Step step = new Step();
        step.setTitle("Collect 10 stars");
        step.setActionCountRequired(10);

        mission.setSteps(List.of(step));

        return missionDao.createMission(mission);
    }
}
</code></pre>
<!-- /wp:code -->

<!-- wp:quote -->
<blockquote class="wp-block-quote"><!-- wp:paragraph -->
<p>Once a mission is copied into player progress, later edits to the mission definition do <strong>not</strong> affect existing players.</p>
<!-- /wp:paragraph --></blockquote>
<!-- /wp:quote -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading">Creating or Fetching Progress</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>A <strong>Progress</strong> object tracks a player’s state for a specific mission.</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>import com.google.inject.Inject;
import dev.getelements.elements.sdk.dao.ProgressDao;
import dev.getelements.elements.sdk.model.mission.Progress;
import dev.getelements.elements.sdk.model.profile.Profile;

public class ProgressService {

    @Inject
    private ProgressDao progressDao;

    public Progress ensureProgress(Profile profile, String missionNameOrId) {
        Progress progress = new Progress();
        progress.setProfile(profile);
        progress.setMission(missionNameOrId);

        return progressDao.createOrGetExistingProgress(progress);
    }
}
</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>This operation is <strong>idempotent</strong>—existing progress is returned instead of creating duplicates.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading">Advancing Progress</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>When a player performs an action tracked by a mission step, advance their progress:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>import com.google.inject.Inject;
import dev.getelements.elements.sdk.dao.ProgressDao;
import dev.getelements.elements.sdk.model.mission.Progress;

public class ProgressActions {

    @Inject
    private ProgressDao progressDao;

    public Progress advance(Progress progress, int actionsPerformed) {
        return progressDao.advanceProgress(progress, actionsPerformed);
    }
}
</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>Advancing progress automatically:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Updates the current step</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Completes steps when requirements are met</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Issues rewards tied to completed steps</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading">Schedules: End-to-End Example</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>This example shows how schedules are typically used to assign missions to players.</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>import com.google.inject.Inject;
import dev.getelements.elements.sdk.dao.ScheduleDao;
import dev.getelements.elements.sdk.dao.ScheduleEventDao;
import dev.getelements.elements.sdk.dao.ScheduleProgressDao;
import dev.getelements.elements.sdk.model.mission.Schedule;
import dev.getelements.elements.sdk.model.mission.ScheduleEvent;
import dev.getelements.elements.sdk.model.mission.Progress;

import java.time.Instant;
import java.util.List;

public class ScheduledMissionsService {

    @Inject
    private ScheduleDao scheduleDao;

    @Inject
    private ScheduleEventDao scheduleEventDao;

    @Inject
    private ScheduleProgressDao scheduleProgressDao;

    public List&lt;Progress&gt; syncWeeklyMissions(String profileId) {

        // 1) Create or fetch the schedule
        Schedule schedule;
        try {
            schedule = scheduleDao.createSchedule(new Schedule()
                    .setName("weekly_quests")
                    .setTitle("Weekly Quests"));
        } catch (Exception e) {
            schedule = scheduleDao.getScheduleByNameOrId("weekly_quests");
        }

        // 2) Create a schedule event for a mission
        ScheduleEvent event = new ScheduleEvent();
        event.setSchedule(schedule.getId());
        event.setMission("collect_stars");
        event.setStartTimestamp(Instant.now().toEpochMilli());
        event.setEndTimestamp(
                Instant.now().plusSeconds(7 * 24 * 60 * 60).toEpochMilli()
        );

        scheduleEventDao.createScheduleEvent(event);

        // 3) Resolve active events (defaults to \"now\")
        List&lt;ScheduleEvent&gt; activeEvents =
                scheduleEventDao.getAllScheduleEvents(schedule.getId());

        // 4) Assign progress for active missions
        List&lt;Progress&gt; assigned =
                scheduleProgressDao.assignProgressesForMissionsIn(
                        schedule.getId(),
                        profileId,
                        activeEvents
                );

        // 5) Optional cleanup
        // scheduleProgressDao.unassignProgressesForMissionsNotIn(
        //         schedule.getId(), profileId, activeEvents
        // );

        return assigned;
    }
}
</code></pre>
<!-- /wp:code -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading">Key guarantees</h3>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Assignment is safe to run repeatedly (no duplicate progress)</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Missions remain assigned as long as at least one active schedule references them</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Progress is only removed when explicitly unassigned</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading">When to use Schedules vs Manual Progress</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Use <strong>Schedules</strong> when:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Missions rotate over time</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Content is seasonal or event-driven</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>You want centralized control over mission availability</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>Use <strong>manual progress creation</strong> when:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Missions are unlocked permanently</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Progress is tied to explicit player actions (e.g. tutorial completion)</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->
