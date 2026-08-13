<h1>Wallets</h1>

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"level":6} -->
<h6 class="wp-block-heading" id="h-elements-supports-implementation-of-a-custodial-wallet-system-to-facilitate-user-transactions-with-your-game-or-application"><br>Elements supports implementation of a custodial Wallet system to facilitate user transactions with your game or application.</h6>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Wallets are at the heart of any blockchain. A <a href="https://en.wikipedia.org/wiki/Cryptocurrency_wallet">Wallet</a> typically contains multiple accounts, which is typically a public/private key pair. The wallet identifies the user or organization on the blockchain and uses the information to generate the digital signature required to execute transactions.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Elements provides a custodial wallet system that permits you to store user's crypto assets within Elements. In addition to full custodial wallets, Elements also allows the storage of the public address only, permitting users to receive crypto assets, but not to sign transactions. This can be useful for users who wish to bring their own wallets.</p>
<!-- /wp:paragraph -->

<!-- wp:embed {"url":"https://www.youtube.com/watch?v=YQcuRY6hAuM","type":"video","providerNameSlug":"youtube","responsive":true,"className":"wp-embed-aspect-16-9 wp-has-aspect-ratio"} -->
<figure class="wp-block-embed is-type-video is-provider-youtube wp-block-embed-youtube wp-embed-aspect-16-9 wp-has-aspect-ratio"><div class="wp-block-embed__wrapper">
https://www.youtube.com/watch?v=YQcuRY6hAuM
</div></figure>
<!-- /wp:embed -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-wallet-properties">Wallet Properties <a href="#wallet-properties" id="wallet-properties"></a></h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The general Wallet properties are as follows.</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><a href="../../general/general-concepts#id-property">id</a></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="../../general/general-concepts#display-name-property">displayName</a></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>user</strong> - The user which owns the Wallet</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>vault</strong> - The <a href="vaults">Vault</a> which owns the Wallet</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>api</strong> - The API with which this wallet is compatible. See <a href="../omni-chain-support#supported-blockchain-apis">Supported Blockchain APIs</a> for more information.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>networks</strong> - A set of networks with which this wallet is compatible. See <a href="../omni-chain-support#supported-blockchain-networks">Supported Blockchain Networks</a> for a list of supported values.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>preferredAccount</strong> - As a single Wallet may contain multiple accounts, this indicates the preferred account. In cases where a Wallet may</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>accounts</strong> - A list of all accounts contained within the Wallet.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"Warning","noticeBackgroundColor":"#ffdd57"} -->
<div style="color:#32373c;background-color:#ffdd57" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="0eaadb"><div class="gb-notice-title" style="color:#fff"><p>Warning</p></div><div class="gb-notice-text" style="border-color:#ffdd57"><!-- wp:paragraph -->
<p>Blockchain APIs and Blockchain Networks must be compatible in all cases. For example, it is not possible to specify a Wallet that uses the Ethereum API on the Solana Network, as they are incompatible networks.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-wallet-accounts">Wallet Accounts <a href="#wallet-accounts" id="wallet-accounts"></a></h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>A Wallet may contain several accounts, each of which is a separate identity on the blockchain. Most blockchains support multiple accounts per Wallet. Practically speaking, most users will have a single account per wallet. However, it is possible to support multiple accounts per wallet.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>The wallet account properties are as follows:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>address</strong> - the public address of the account. This field is mandatory.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>privateKey</strong> - the private key of the wallet. This field is optional. However, if left unspecified, then the wallet may not be used to sign transactions. Rather it may be only used to receive currency and NFTs.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>encrypted</strong> - a boolean value to indicate whether or not the private key in the wallet is encrypted.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->
