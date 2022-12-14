# ExampleGameAssetsContractERC1155
Example game asset contract with in game currency, buy function, rartity based status values and fragments to mint the game asset.

The metadata uploads were updated for the tutorial after the contract creation, so addresses or links could be obsolete.

## Requirements
### Require 00
A contract workspace according to previous examples.

### Require 01
Hosted metadata according to previous examples.

## The contract / Implemented tokenomics
### Items
There are 11 predefined items. The currency, 5 rarities of game asset and 5 rarities of token. 

### Pricing
The values defined are just an example to represent some ideas of mechanics in the game asset. Further the resulting price adjustment functions are not implemented yet. These steps will follow after game contract creation.

| Variable | About | Value |
| --- | --- | --- |
| _FeePerAsset | The price in native curreny to pay mint a single asset. | 0.001 ether |
| _FragmentsPerAsset | The number of fragments to burn, to mint a defined rarity with default charge. | 100 |
| _CashPerAssetToBurn | The number of cash to burn, to mint a random asset. | 100 |
| _CashPerAssetFee | The number of cash to pay to the owner, to mint a random asset. | 10 |

### Constructor, receive, fallback and withdraw

Additional to the contract creation the consturctor mints specified default amount of each token to the owner wallet. 

Further it needs the default functions receive and fallback to handle incoming ether or other transactions. The receive function checks if the message value is bigger than the fee per asset. If this requirement is met, a random asset is minted to the message sender.

The withdraw function is necessary to withdraw the ether in the contract to the owner wallet.

### Modifier
The following modifiers are implemented:

| Name | Calls | Requirements | 
| --- | --- | --- |
| mintNativeCompliance | mintWithNative | Message value lower than sender balance |
|   |   | Message value higher than fee per asset |
| mintCashCompliance | mintWithCash | Message value lower than sender balance |
|   |   | Cash balance higher than (cash per asset to burn + cash per asset fee) |
| mintFragmentCompliance | mintWithFragment | Message value lower than sender balance |
|   |   | Rarity in range ( >= 0) |
|   |   | Rarity in range ( <= 4) |
|   |   | Balance of fragments enough to fusion |
| isAsset | getStatsURI | Id in range ( > 0) |
|   |  mintWithFragment | Id in range ( < 6) |
| isCash |  | Id in range ( == 0) |
| isFragment |  | Id in range ( < 11) |
| isFragment |  | Id in range ( > 5) |

### Maintenance functions

| Name | Params | Modifier | Fuction | 
| --- | --- | --- |--- |
| setBaseStatsURI | string _newURI | onlyOwner | Set URI to basic stats values |
| setURI | string _newURI | onlyOwner | Set general token URI |

### Reading functions

| Name | Params | Modifier | Fuction | 
| --- | --- | --- |--- |
| getStatsURI | uint256 _tokenRarity | isAsset | Get URI to basic stats values |
| getURI | string _newURI |   | Get general token URI |
| tokenURI | uint256 _qId |   | Get general token URI |

### Minting functions

| Name | Params | Modifier | Fuction | 
| --- | --- | --- |--- |
| mintOwner | uint256 id, uint256 amount | onlyOwner | Mint specified token with specified amount to owner |
| mintFromOwnerTo | address to, uint256 id, uint256 amount | onlyOwner | Mint specified token with specified amount to other address |
| mintWithNative |   | mintNativeCompliance | Mint random rarity asset for native fee |
| mintWithFragment | uint256 _tokenRarity | isAsset mintFragmentCompliance | Mint specified rarity asset for native fee |
| mintWithCash |   | mintCashCompliance | Mint random rarity asset for native fee |

### Other functions

| Name | Params | Modifier | Fuction | 
| --- | --- | --- | --- |
| _createRandomNum | uint256 _mod |   | Returns random number in the specified mod range |
| burn | address from, uint256 id, uint256 amount |   | Returns random number in the specified mod range |

## Proof of functionality
### Contract Creation

Contract Address:
0x7975A3B87f2986f42993e5De201089DCAfD3Eeff
https://testnet.bscscan.com/address/0x7975a3b87f2986f42993e5de201089dcafd3eeff

TX hash contract creation:
0xe85e1db9eee8b2e2448376d3dd0606558ff27ea748ec37e0e7ad0751b863903b
https://testnet.bscscan.com/tx/0xe85e1db9eee8b2e2448376d3dd0606558ff27ea748ec37e0e7ad0751b863903b

![ScreenShot](/img/TestGfnIcoDice_TX_ContractCreation.PNG)

### Metadata and statsURI

Token URI returns link with replaceable id:

![ScreenShot](/img/TestGfnIcoDice_TokenURI.PNG)

Stats URI function returns a link to the json file which contains the stats object.

![ScreenShot](/img/TestGfnIcoDice_StatsURI.PNG)



### Mint with native

Native mint via mint function:

![ScreenShot](/img/TestGfnIcoDice_TX_MintWithNative.PNG)

### Mint with cash

Mint with token 0 from ERC1155 contract:

![ScreenShot](/img/TestGfnIcoDice_TX_MintWithCash.PNG)

### Mint with fragment

Mint with specific rarity fragments:

![ScreenShot](/img/TestGfnIcoDice_TX_MintWithFragments.PNG)

### Mint on ether receive

Mint by sending the fee in ether directly to the contract:

![ScreenShot](/img/TestGfnIcoDice_TX_MintWithEtherTransaction.PNG)

![ScreenShot](/img/TestGfnIcoDice_TXdetails_MintWithEtherTransaction.PNG)

### Withdraw

Enter 0 in the payable field when calling withdraw function:

![ScreenShot](/img/TestGfnIcoDice_Call_Withdraw.PNG)

The collected ether will be send to the contract owner:

![ScreenShot](/img/TestGfnIcoDice_TXdetails_Withdraw.PNG)
