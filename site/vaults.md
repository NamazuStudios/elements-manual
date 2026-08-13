<h1>Vaults</h1>

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"level":6} -->
<h6 class="wp-block-heading" id="h-vaults-store-encrypted-wallet-information-in-elements-using-a-public-private-key-pair">Vaults store encrypted Wallet information in Elements using a public/private key pair.</h6>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Vaults are the heart of the custodial wallet system provided by Elements. A Vault securely stores multiple <a href="wallets">wallets</a> and consists of a public/private key pair.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>The contents of the vault's private key can be optionally secured using <a href="https://en.wikipedia.org/wiki/Advanced_Encryption_Standard">AES-256</a>. If enabled on a vault, the user must supply their secret passphrase on each request to unlock the contents of the vault.</p>
<!-- /wp:paragraph -->

<!-- wp:genesis-blocks/gb-notice {"noticeTitle":"Warning","noticeBackgroundColor":"#ffdd57"} -->
<div style="color:#32373c;background-color:#ffdd57" class="wp-block-genesis-blocks-gb-notice gb-font-size-18 gb-block-notice" data-id="0eaadb"><div class="gb-notice-title" style="color:#fff"><p>Warning</p></div><div class="gb-notice-text" style="border-color:#ffdd57"><!-- wp:paragraph -->
<p>When designing an application, we strongly recommend that all vaults are encrypted with a passphrase.</p>
<!-- /wp:paragraph --></div></div>
<!-- /wp:genesis-blocks/gb-notice -->

<!-- wp:paragraph -->
<p>Because the Vault uses private key encryption, it is possible to generate or insert new custodial wallets without needing to unlock the vault first. In this case, Elements simply uses the public key to insert the wallet into the Vault.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-vault-properties">Vault Properties <a href="#vault-properties" id="vault-properties"></a></h3>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><a href="../../general/general-concepts#id-property">id</a></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="../../general/general-concepts#display-name-property">displayName</a></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>user</strong> - The user which owns the vault</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>key</strong> - The key pair which Elements uses to store the wallets in the Vault</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-vault-key-properties">Vault Key Properties <a href="#vault-key-properties" id="vault-key-properties"></a></h3>
<!-- /wp:heading -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><strong>algorithm</strong> - this is the encryption algorithm Elements uses to store the wallets in the vault. The available algorithms are as follows:</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="https://en.wikipedia.org/wiki/Elliptic-curve_cryptography">Elliptic Curve 256</a></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="https://en.wikipedia.org/wiki/Elliptic-curve_cryptography">Elliptic Curve 384</a></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="https://en.wikipedia.org/wiki/Elliptic-curve_cryptography">Elliptic Curve 512</a></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="https://en.wikipedia.org/wiki/RSA_(cryptosystem)">RSA 256</a></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="https://en.wikipedia.org/wiki/RSA_(cryptosystem)">RSA 384</a></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="https://en.wikipedia.org/wiki/RSA_(cryptosystem)">RSA 512</a></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>publicKey</strong> - This is the public key portion of the vault. This is always stored unencrypted.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>privateKey</strong> - This is the private key portion of the vault. This is either encrypted or stored as plain text.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>encrypted</strong> - A boolean value indicating whether the vault private key is encrypted</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>encryption</strong> - An arbitrary key-value object which contains encryption metadata. Elements uses this internally to perform various operations against the private key itself.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->
