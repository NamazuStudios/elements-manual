<h1>Smart Contracts</h1>

<!-- wp:separator -->
<hr class="wp-block-separator has-alpha-channel-opacity"/>
<!-- /wp:separator -->

<!-- wp:heading {"level":6} -->
<h6 class="wp-block-heading" id="h-elements-fully-supports-blockchain-smart-contracts-allowing-developers-to-create-fully-custom-applications-performing-advanced-operations-on-multiple-blockchains-at-once">Elements fully supports blockchain Smart Contracts, allowing developers to create fully custom applications performing advanced operations on multiple blockchains at once.</h6>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Most blockchains use are programmable, allowing developers to customize the operations performed by the nodes on the chain. Common tasks for smart contracts include simple fungible token transfer, sale or transfer of <a href="https://en.wikipedia.org/wiki/Non-fungible_token">non-fungible tokens (NFTs)</a>, and <a href="https://en.wikipedia.org/wiki/Decentralized_finance">Decentralized Finance (DeFi)</a> operations.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Unfortunately, execution and management of Smart Contracts for your web3 game or application requires complex configuration, attention to security detail, and careful management of infrastructure. What makes this more challenging is that there are multiple competing standards for the development and execution of smart contracts leaving developers to deal with multiple desperate SDKs and tools.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Elements solves this problem by providing a single point to track and manage smart contracts across multiple chains and SDKs. With tie-ins to the lua based Scripting Engine.</p>
<!-- /wp:paragraph -->

<!-- wp:embed {"url":"https://www.youtube.com/watch?v=3aD9gWK83fM","type":"video","providerNameSlug":"youtube","responsive":true,"className":"wp-embed-aspect-16-9 wp-has-aspect-ratio"} -->
<figure class="wp-block-embed is-type-video is-provider-youtube wp-block-embed-youtube wp-embed-aspect-16-9 wp-has-aspect-ratio"><div class="wp-block-embed__wrapper">
https://www.youtube.com/watch?v=3aD9gWK83fM
</div></figure>
<!-- /wp:embed -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-data-model">Data Model <a href="#data-model" id="data-model"></a></h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The Smart Contract management system allows you to track the metadata necessary to operate against smart contracts deployed to the blockchain of your choice. A single smart contract consists of the following general properties:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul class="wp-block-list"><!-- wp:list-item -->
<li><a href="../../general/general-concepts#id-property">id</a></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="../../general/general-concepts#name-property">name</a></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="../../general/general-concepts#metadata-property">metadata</a></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><a href="../../general/general-concepts#display-name-property">display name</a></li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>addresses</strong> - A mapping of blockchain network identifiers to the smart contract addresses. The specific semantics of a smart contract address vary per chain and API. See <a href="#smart-contract-addresses">Smart Contract Addresses</a> below for specifics on each network.</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li><strong>vault</strong> - Reference to the <a href="vaults">Omni Vault</a> which Elements will use to operate against the contract.</li>
<!-- /wp:list-item --></ul>
<!-- /wp:list -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-smart-contract-addresses">Smart Contract Addresses <a href="#smart-contract-addresses" id="smart-contract-addresses"></a></h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Each Blockchain API has specific requirements for addresses. Though some chains refer to a smart contract as having an address, this is not universally true across all chains. Elements uses the term "address" to refer to the singular identifier of a particular contract. The following subsections define the address semantics of each supported API.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3 class="wp-block-heading" id="h-vault-s-relationship-to-smart-contracts">Vault's Relationship to Smart Contracts <a href="#vaults-relationship-to-smart-contracts" id="vaults-relationship-to-smart-contracts"></a></h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>In order to sign new transactions, which is essentially executing smart contract code which writes to the blockchain, the signing wallet must pay a fee to the node which wins the opportunity to execute the transaction. Ethereum coined the term "<a href="https://en.wikipedia.org/wiki/Ethereum#Gas">gas fee</a>" to describe this concept, and often times the term is colloquially applied to other unrelated networks as well.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>As the developer of a web3 application or game, you will likely have to pay transaction (or "gas fees") to perform write operations against the chain. In order to accomplish this automatically, you must fund and supply the private key of the wallet associated with your smart contract. Elements' secure vault system ensures that the funds stored in the wallet are safe, while providing the convenience of not having to manually verify each transaction against the chain.</p>
<!-- /wp:paragraph -->
