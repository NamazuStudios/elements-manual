<h1>Namazu Cloud Organizations</h1>

<!-- wp:paragraph -->
<p>An organization is the ownership and billing boundary in Namazu Cloud. Every instance, subdomain, add-on, and backup belongs to an organization, and every Namazu Cloud account can belong to many of them. Organizations are where you add teammates, and only members of an organization can see or manage the resources inside it.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"anchor":"h-roles"} -->
<h2 id="h-roles" class="wp-block-heading">Roles</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Every member of an organization has one of three roles:</p>
<!-- /wp:paragraph -->

<!-- wp:table -->
<figure class="wp-block-table"><table class="has-fixed-layout"><thead><tr><th>Role</th><th>What it can do</th></tr></thead><tbody><tr><td><code>OWNER</code></td><td>Everything an administrator can do, plus renaming or deleting the organization, removing members, changing roles, transferring ownership, and managing billing</td></tr><tr><td><code>ADMIN</code></td><td>Invite members (as the <code>MEMBER</code> role only) and view or cancel invitations</td></tr><tr><td><code>MEMBER</code></td><td>View the organization and use the resources inside it: create and manage instances, subdomains, backups, and templates</td></tr></tbody></table></figure>
<!-- /wp:table -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"Note"} -->
<div style="color:#32373c;background-color:#00d1b2" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="3b0649"><div class="gb-notice-title" style="color:#fff"><p>Note</p></div><div class="gb-notice-text" style="border-color:#00d1b2"><!-- wp:paragraph -->
<p>The role that matters most day to day is <code>OWNER</code>. Only owners can rename or delete the organization, invite members, change roles, transfer ownership, and manage billing. Administrators can invite members, and members can use the organization's resources.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"anchor":"h-creating-an-organization"} -->
<h2 id="h-creating-an-organization" class="wp-block-heading">Creating an Organization</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>From the portal, open the Organizations section and choose to create an organization, giving it a name. The account that creates the organization becomes its sole owner. There is no limit on the number of organizations an account can own or belong to, so you can keep separate organizations for separate projects or customers.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>An organization you already belong to appears in the listing with its member roles. After you create an organization it is ready for a reserved subdomain and, once billing is set up, its first instance. See <a href="namazu-cloud-billing">Namazu Cloud Billing</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"anchor":"h-members"} -->
<h2 id="h-members" class="wp-block-heading">Members</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The members list shows every account in the organization with its display name, email, and role. Owners can:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>Remove a member</strong>: a member is removed entirely from the organization. You cannot remove the last owner; give the organization another owner first or delete it.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Change a member's role</strong>: move a member between <code>MEMBER</code>, <code>ADMIN</code>, and <code>OWNER</code>.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Transfer ownership</strong>: hand ownership to another member. The current owner is demoted to <code>MEMBER</code>, so this is one-way until the new owner transfers it back.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"anchor":"h-invitations"} -->
<h2 id="h-invitations" class="wp-block-heading">Invitations</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Teammates join through email invitations. An owner creates an invitation for an email address, optionally pre-setting the role the invitee will get on acceptance. Namazu Cloud emails the invitee a link, and accepting it adds the account to the organization.</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>Pending invites</strong>: an owner sees every outstanding invitation for the organization and can cancel one at any time.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Resend</strong>: resending emails a fresh invitation to the same address and resets the 7-day expiry. The invite token stays the same.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Accept or decline</strong>: the invited account accepts or declines from its own portal. Acceptance brings the account into the organization with the invited role, and requires a <a href="namazu-cloud-account">verified email address</a> on the account.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>Your pending invites</strong>: the portal surfaces every invitation pending for your own account across all organizations, so nothing gets lost in an inbox.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"anchor":"h-renaming-and-deleting"} -->
<h2 id="h-renaming-and-deleting" class="wp-block-heading">Renaming and Deleting</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>An owner can rename the organization at any time. Deleting an organization is final: it removes the organization and is only available to its sole owner. Destroy any instances first, since deleting an organization while it still has resources leaves them behind.</p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"anchor":"h-what-you-can-do-inside"} -->
<h2 id="h-what-you-can-do-inside" class="wp-block-heading">What You Can Do Inside</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>An organization is the context for nearly every other Namazu Cloud feature. Each is documented on its own page:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><a href="namazu-cloud-instances">Instances</a>: dedicated Elements deployments owned by the organization</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="namazu-cloud-subdomains">Subdomains</a>: hostnames reserved in the organization's pool</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="namazu-cloud-backups">Backups</a>: snapshots of the organization's instances</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="namazu-cloud-addons">Add-ons</a>: extras purchased against the organization's billing</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="namazu-cloud-billing">Billing</a>: the payment method on file for the organization</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"anchor":"h-related-pages"} -->
<h2 id="h-related-pages" class="wp-block-heading">Related Pages</h2>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><a href="namazu-cloud-overview">Namazu Cloud Overview</a></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="namazu-cloud-account">Namazu Cloud Account</a>. Your own sign-in, sessions, and email verification</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->
