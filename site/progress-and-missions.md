<h1>Progress and Missions</h1>

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"level":6} -->
<h6 class="wp-block-heading" id="h-elements-allows-the-creation-of-missions-in-which-users-can-make-progress-that-is-tracked-in-their-user-profile">Elements allows the creation of Missions, in which users can make progress that is tracked in their user profile.</h6>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Missions are structured tasks through which users can progress. Mission steps are tracked in the user's progress.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Common use cases for missions are a game's level structure or a game's achievements. Missions also allow for the user to earn rewards when steps are completed.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Missions serve as definitions. When a user begins his first mission, a copy of that mission definition is made in the progress attached to that user's profile for that application. The progress will then record the user's step completion, as well as rewards issued for step completion.</p>
<!-- /wp:paragraph -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"Warning","noticeBackgroundColor":"#ffdd57"} -->
<div style="color:#32373c;background-color:#ffdd57" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="0eaadb"><div class="gb-notice-title" style="color:#fff"><p>Warning</p></div><div class="gb-notice-text" style="border-color:#ffdd57"><!-- wp:paragraph -->
<p>A limitation of the current system is that when a mission definition is copied to the progress, it is locked in the state it was defined at that time. So, if developers modify that mission after progress is saved by an user, existing users who have started that mission won't see the changes - only users who have not yet started the mission will.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:paragraph -->
<p>This section will guide you on how to interact with the progress and missions APIs.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-mission-structure">Mission Structure <a href="#mission-structure" id="mission-structure"></a></h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Missions contain an arbitrary number of steps, followed by a single (optional) final repeat step. As the user of your application progresses through a mission, they will go through the steps in sequence.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Once all steps are completed, if there is a final repeat step, that step can be repeated again an unlimited number of times.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>For example, you can use the steps for a tiered set of achievements to be completed in sequence, each step with its own set of rewards:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li>Collect 5 Stars</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Collect 10 Stars</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>Collect 20 Stars</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="h-mission">Mission <a href="#mission" id="mission"></a></h4>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>name:</strong> This is a string representing the name of your mission. It must be unique and have no spaces.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>tags:</strong> You may include a list of tags as strings. Tags are an easy way to search for groups of missions or sort them among your applications.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>displayName:</strong> This is a string representing your mission's display name.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>description:</strong> This is a string that serves as a description for your mission.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>steps:</strong> Here, all the steps of the mission are represented. Steps have their own structure that is <a href="#mission-step">detailed below</a>, with their own corresponding fields.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>finalRepeatStep:</strong> This is the optional final repeat step of the mission, which has a similar structure to an individual step.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>metadata:</strong> Metadata is optional. It can include any number of named strings or integers. Metadata can have many uses. For example, secondary descriptions, asset paths, types, or any arbitrary values assigned to your mission.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="h-mission-steps">Mission Steps <a href="#mission-step" id="mission-step"></a></h4>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Mission steps define the actual content of the mission. The final repeat step has a structure identical to any other step.</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>displayName:</strong> This is a string representing your mission's display name.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>description:</strong> This is a string that serves as a description for your mission.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>count:</strong> This is an integer that defines how many times an action has to be executed for the step to be completed. For example, if the mission step was "collect 20 stars," you might have you application increment the user's progress on this step each time a star is collected, and then the step would be completed when the threshold is reached.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>rewards:</strong> Here, the rewards are what the player earns for the completion of the steps that are listed. Rewards are <a href="../digital-goods#items">items</a> accompanied by a quantity.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>name</strong>: A string naming the item reward. This must match the unique name of a defined <a href="../digital-goods#items">item</a>.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>quantity</strong>: This is an integer determining how many of each <a href="../digital-goods#items">item</a> will be rewarded.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>metadata:</strong> Metadata is optional. Steps can have their own metadata separate from the overarching mission. It can be any number of named strings or integers. Metadata can have many uses. For example, secondary descriptions, asset paths, types, or any arbitrary values assigned to your mission step.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>In the database, the mission will also have an <strong>_id</strong> field. This is automatically generated when the mission is created and serves as an internal reference for that mission.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-managing-missions-using-the-console">Managing Missions Using the Console <a href="#managing-missions-using-the-console" id="managing-missions-using-the-console"></a></h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Missions are managed in the Missions section of the admin console, which can be accessed from the upper nav bar or in the hamburger menu.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>The "Add Mission" button will open the new item panel.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Missions can be edited by tapping the <em>Edit</em> button next to that mission or they can be deleted by tapping the <em>Delete</em> button. Use the search function to easily find specific missions.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":21969} -->
<figure class="wp-block-image"><img src="https://namazustudios.com/wp-content/uploads/2025/08/mission_editor.png" alt="" class="wp-image-21969"/><figcaption class="wp-element-caption">Mission Editor</figcaption></figure>
<!-- /wp:image -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="h-editing-mission-metadata">Editing Mission Metadata <a href="#editing-mission-metadata" id="editing-mission-metadata"></a></h4>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Mission metadata is edited identically to how item metadata is edited. See <a href="../digital-goods#editing-item-metadata">Editing Item Metadata</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="h-editing-mission-steps">Editing Mission Steps <a href="#editing-mission-steps" id="editing-mission-steps"></a></h4>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Use the <em>Add Step</em> button to create a new step.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Step content can be displayed or hidden with the dropdown button on the right side of the header. If you have multiple steps, they can easily be reordered by dragging them in the list from the handle icon on the left of the header.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Step rewards are added by naming the reward item and adding a quantity, then using the "+" button to add additional rewards. <a href="../digital-goods#items">Item</a> names must be items that exist in your Elements instance.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Step metadata is edited just like general mission metadata, or <a href="../digital-goods#editing-item-metadata">item metadata</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Use the <em>Delete Step</em> button to delete the step.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Toggling the <em>Final Repeat Step</em> checkbox will set that step as the final repeat step and automatically move it to the end of the sequence.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":22051,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://namazustudios.com/wp-content/uploads/2025/08/Screenshot-2025-08-11-at-10.49.57-AM-1024x898.png" alt="" class="wp-image-22051"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"align":"center","style":{"typography":{"fontSize":"12px"}}} -->
<p class="has-text-align-center" style="font-size:12px">Mission Step Editor</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":4} -->
<h4 class="wp-block-heading" id="h-json-structure-of-missions">JSON Structure of Missions <a href="#json-structure-of-missions" id="json-structure-of-missions"></a></h4>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>This is a sample mission represented in JSON.</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>&#91;{
  "name": "missionName",
  "tags": &#91;"tag1", "tag2", "tag3"],
  "displayName": "Mission Name",
  "description": "This is the mission's description",
  "steps": &#91;
    {
      "displayName": "Step Name 1",
      "description": "This is step 1's description",
      "count": 1,
      "rewards": &#91;
        {
          "item": {
            "name": "itemName"
          },
          "quantity": 1
        }
      ],
      "metadata": {
        "metadata_string": "this is a string",
        "metadata_int": 1
      }
    },
    {
      "displayName": "Step Name 2",
      "description": "This is step 2's description",
      "count": 1,
      "rewards": &#91;
        {
          "item": {
            "name": "itemName"
          },
          "quantity": 1
        },
        {
        "item": {
          "name": "itemName2"
        },
        "quantity": 1
      }
      ],
      "metadata": {
        "metadata_string": "this is a string",
        "metadata_int": 1
      }
    }
  ],
  "finalRepeatStep": {
    "displayName": "Repeat Step Name",
    "description": "This is the repeat step's description",
    "count": 1,
    "rewards": &#91;
      {
        "item": {
          "name": "itemName"
        },
        "quantity": 5
      }
    ],
    "metadata": {
        "metadata_string": "this is a string",
        "metadata_int": 1
    }
  },
  "metadata": {
    "metadata_string": "this is a string",
    "metadata_int": 1
  }
}]</code></pre>
<!-- /wp:code -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"Note"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="3b0649"><div class="gb-notice-title" style="color:#fff"><p>Note</p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p>The <strong>_id</strong> field for the mission is not included in the above example. If you were to use a script to batch upload/update missions, your JSON would look like the above example. The <strong>_id</strong> field would not be included since that is autogenerated.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:paragraph -->
<p>Looking directly in the database, you'll notice that the rewards for a step are listed as below, using the object id for the item rather than its name.</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>"rewards" : &#91; 
            {
                "item" : {
                    "$ref" : "items",
                    "$id" : ObjectId("5cdb1204c44033001a6a2908")
                },
                "quantity" : 50
            }
            ]</code></pre>
<!-- /wp:code -->
