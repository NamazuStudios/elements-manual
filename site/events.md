<h1>Events</h1>

<!-- wp:paragraph -->
<p>Namazu Elements provides the ability to react to various events that are produced by either the core system, or other custom Elements. This allows for your code to react to things as they happen, such as setting startup code when an Element is loader, or configuring a Profile with default properties when a new Profile is created. Functions that receive the event are called Event Consumers, and functions that send the event are called Event Producers.<br></p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-determining-available-events">Determining Available Events</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>You can see which events are available to consume by looking under Produced Events in the Core Elements section of the CMS.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":22394,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://namazustudios.com/wp-content/uploads/2026/02/Screenshot-2026-02-03-at-4.09.11-PM-1024x466.png" alt="" class="wp-image-22394"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>DAO level events will always have two versions - one that occurs during the transaction, with a reference to the transaction itself, and one that occurs after the transaction. This is important, as sometimes you might want to communicate with the database and ensure that your event consumer has the same protections and retries that a transaction provides. In these cases, it's important to ensure that your code is idempotent, and that you use the transaction itself to get any DAO classes. For example:</p>
<!-- /wp:paragraph -->

<!-- wp:preformatted -->
<pre class="wp-block-preformatted">@ElementEventConsumer(ReceiptDao.RECEIPT_CREATED)<br>public void onReceiptCreated(Receipt receipt, Transaction transaction) {<br>    final User user = transaction.performAndCloseV(txn -> {<br>        final var dao = txn.getDao(UserDao.class);<br>        final var txUser = dao.getUser(receipt.getUser.getId());<br>        //Do something with the User here<br>        return txUser;<br>    });<br><br>    //The transaction has completed at this point<br>}</pre>
<!-- /wp:preformatted -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:paragraph -->
<p>You can also view the Produced Events for an individual Element by navigating to the Element Info screen for that Element.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":22395,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://namazustudios.com/wp-content/uploads/2026/02/Screenshot-2026-02-03-at-4.13.50-PM-1024x554.png" alt="" class="wp-image-22395"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>This information comes from the SystemElementsResource (GET /elements/system) and the ApplicationElementResource (GET /elements/application) respectively.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2 class="wp-block-heading" id="h-code-samples">Code Samples</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>To consume an event, be sure to place it in a service that has been registered in <code>package-info.java</code> like so:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>// Makes this discoverable by the event system
@ElementService(
        value = EventHandler.class,
        implementation = @ElementServiceImplementation(EventHandlerImpl.class)
)</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>where in this case, EventHandler is an interface that has been made public:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>@ElementPublic<br>public interface EventHandler {}</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>and EventHandlerImpl is the implementation class that looks like so:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>package com.namazustudios.events;

import dev.getelements.elements.sdk.ElementLoader;
import dev.getelements.elements.sdk.Event;
import dev.getelements.elements.sdk.annotation.ElementEventConsumer;
import dev.getelements.elements.sdk.dao.DistinctInventoryItemDao;
import dev.getelements.elements.sdk.dao.InventoryItemDao;
import dev.getelements.elements.sdk.dao.ItemDao;
import dev.getelements.elements.sdk.model.goods.Item;
import dev.getelements.elements.sdk.model.inventory.DistinctInventoryItem;
import dev.getelements.elements.sdk.model.inventory.InventoryItem;
import dev.getelements.elements.sdk.model.profile.Profile;
import dev.getelements.elements.sdk.service.profile.ProfileService;
import io.swagger.v3.oas.annotations.Hidden;
import jakarta.inject.Inject;

import static com.namazustudios.util.Constants.<em>STARTING_AVATAR_ITEM_NAME</em>;
import static com.namazustudios.util.Constants.<em>EXP_ITEM_NAME</em>;

@Hidden
public class EventHandlerImpl implements EventHandler {

    @Inject
    private InventoryItemDao inventoryItemDao;

    @Inject
    private DistinctInventoryItemDao distinctInventoryItemDao;

    private Item startingAvatarItem;

    private Item expItem;

    @Inject
    private void init(ItemDao itemDao) {
        startingAvatarItem = itemDao.getItemByIdOrName(<em>STARTING_AVATAR_ITEM_NAME</em>);
        expItem = itemDao.getItemByIdOrName(<em>EXP_ITEM_NAME</em>);
    }

    @ElementEventConsumer(ProfileService.<em>PROFILE_CREATED_EVENT</em>)
    public void onNewProfileCreatedEvent(final Profile profile) {
        assignStartingAvatarItem(profile);
        assignStartingExp(profile);
    }

    @ElementEventConsumer(ElementLoader.<em>SYSTEM_EVENT_ELEMENT_LOADED</em>)
    public void onElementLoadedEvent(Event event) {
        System.<em>out</em>.println("System event loaded");
    }

    private void assignStartingAvatarItem(final Profile profile) {


        final var distinctInventoryItem = new DistinctInventoryItem();
        distinctInventoryItem.setItem(startingAvatarItem);
        distinctInventoryItem.setProfile(profile);
        distinctInventoryItem.setUser(profile.getUser());


        distinctInventoryItemDao.createDistinctInventoryItem(distinctInventoryItem);
    }

    private void assignStartingExp(final Profile profile) {

        final var inventoryItem = new InventoryItem();
        inventoryItem.setItem(expItem);
        inventoryItem.setUser(profile.getUser());
        inventoryItem.setPriority(0);
        inventoryItem.setQuantity(0);

        inventoryItemDao.createInventoryItem(inventoryItem);
    }
}
</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>In this example, we have Event Consumers for when the Element is first loaded (only produced once), and when a new Profile is created (produced every time).</p>
<!-- /wp:paragraph -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"Warning","noticeBackgroundColor":"#ffdd57"} -->
<div style="color:#32373c;background-color:#ffdd57" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="0eaadb"><div class="gb-notice-title" style="color:#fff"><p>Warning</p></div><div class="gb-notice-text" style="border-color:#ffdd57"><!-- wp:paragraph -->
<p>If you use this event: <code>@ElementEventConsumer(ElementLoader.<em>SYSTEM_EVENT_ELEMENT_LOADED</em>)</code> in conjunction with Elements Core services, then you must lazily load the services instead of injecting them, as they are not made available yet when this event fires. Injecting DAO level classes is perfectly fine though. For example:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code><code>Element element = ElementSupplier
        .<em>getElementLocal</em>(ThisClass.class)
        .get();
<code>SomeService <span style="background-color: initial; font-family: inherit; font-size: inherit; text-align: initial;">someService = element</span></code>        .getServiceLocator()
        .getInstance(SomeService.class);</code></code></pre>
<!-- /wp:code --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->
