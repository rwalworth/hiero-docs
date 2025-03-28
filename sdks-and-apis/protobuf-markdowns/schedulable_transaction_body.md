# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/schedulable_transaction_body.proto](#services_schedulable_transaction_body-proto)
    - [SchedulableTransactionBody](#proto-SchedulableTransactionBody)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_schedulable_transaction_body-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/schedulable_transaction_body.proto
# Schedulable Transaction Body
A message that replicates the `TransactionBody` message, with slight
changes to exclude fields that cannot be scheduled via a `scheduleCreate`
transaction.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in
[RFC2119](https://www.ietf.org/rfc/rfc2119) and clarified in
[RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="proto-SchedulableTransactionBody"></a>

### SchedulableTransactionBody
A schedulable transaction.

The network configuration `scheduling.whitelist` limits which of these
transaction types may actually be scheduled. As of version `0.50.0` of the
consensus node software this list contains only `CryptoTransfer`,
`ConsensusSubmitMessage`, `TokenBurn`, `TokenMint`,
and `CryptoApproveAllowance`.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| transactionFee | [uint64](#uint64) |  | A limit for the transaction fee the client is willing to pay. &lt;p&gt; The network SHALL NOT charge fees greater than this value. |
| memo | [string](#string) |  | A short description of the schedulable transaction. &lt;p&gt; This value, if set, MUST NOT exceed `transaction.maxMemoUtf8Bytes` (default 100) bytes when encoded as UTF-8. |
| contractCall | [ContractCallTransactionBody](#proto-ContractCallTransactionBody) |  | Call a function defined on a smart contract. |
| contractCreateInstance | [ContractCreateTransactionBody](#proto-ContractCreateTransactionBody) |  | Create a smart contract. |
| contractUpdateInstance | [ContractUpdateTransactionBody](#proto-ContractUpdateTransactionBody) |  | Update a smart contract. |
| contractDeleteInstance | [ContractDeleteTransactionBody](#proto-ContractDeleteTransactionBody) |  | Delete a smart contract and transfer remaining balance to a specified account. |
| cryptoCreateAccount | [CryptoCreateTransactionBody](#proto-CryptoCreateTransactionBody) |  | Create a new Hedera account. |
| cryptoDelete | [CryptoDeleteTransactionBody](#proto-CryptoDeleteTransactionBody) |  | Delete an Hedera account.&lt;br/&gt; This will mark the account as deleted, and transfer all remaining HBAR to a receiver account. |
| cryptoTransfer | [CryptoTransferTransactionBody](#proto-CryptoTransferTransactionBody) |  | Transfer HBAR between accounts. |
| cryptoUpdateAccount | [CryptoUpdateTransactionBody](#proto-CryptoUpdateTransactionBody) |  | Modify an Hedera account. |
| fileAppend | [FileAppendTransactionBody](#proto-FileAppendTransactionBody) |  | Append data to the end of a file. |
| fileCreate | [FileCreateTransactionBody](#proto-FileCreateTransactionBody) |  | Create a new file. |
| fileDelete | [FileDeleteTransactionBody](#proto-FileDeleteTransactionBody) |  | Delete a file.&lt;br/&gt; This will remove the content of the file, and mark the file as deleted. |
| fileUpdate | [FileUpdateTransactionBody](#proto-FileUpdateTransactionBody) |  | Modify a file.&lt;br/&gt; This may modify any metadata, and/or _replace_ the content. |
| systemDelete | [SystemDeleteTransactionBody](#proto-SystemDeleteTransactionBody) |  | Delete a file as an Hedera administrative function.&lt;br/&gt; This is a privileged operation. |
| systemUndelete | [SystemUndeleteTransactionBody](#proto-SystemUndeleteTransactionBody) |  | Restore a file deleted via `systemDelete`.&lt;br/&gt; This is a privileged operation. |
| freeze | [FreezeTransactionBody](#proto-FreezeTransactionBody) |  | Freeze the network.&lt;br/&gt; This is actually several possible operations, and the caller should examine the &#34;freeze service&#34; for more detail.&lt;br/&gt; This is a privileged operation. |
| consensusCreateTopic | [ConsensusCreateTopicTransactionBody](#proto-ConsensusCreateTopicTransactionBody) |  | Create a topic. |
| consensusUpdateTopic | [ConsensusUpdateTopicTransactionBody](#proto-ConsensusUpdateTopicTransactionBody) |  | Update a topic. |
| consensusDeleteTopic | [ConsensusDeleteTopicTransactionBody](#proto-ConsensusDeleteTopicTransactionBody) |  | Delete a topic. |
| consensusSubmitMessage | [ConsensusSubmitMessageTransactionBody](#proto-ConsensusSubmitMessageTransactionBody) |  | Submit a message to a topic.&lt;br/&gt; A message may be &#34;chunked&#34;, and submitted in parts, if the total message size exceeds the limit for a single transaction. |
| tokenCreation | [TokenCreateTransactionBody](#proto-TokenCreateTransactionBody) |  | Create a new Hedera token. |
| tokenFreeze | [TokenFreezeAccountTransactionBody](#proto-TokenFreezeAccountTransactionBody) |  | Freeze an account with respect to a token.&lt;br/&gt; A frozen account cannot transact in that token until unfrozen. |
| tokenUnfreeze | [TokenUnfreezeAccountTransactionBody](#proto-TokenUnfreezeAccountTransactionBody) |  | Unfreeze an account with respect to a token. |
| tokenGrantKyc | [TokenGrantKycTransactionBody](#proto-TokenGrantKycTransactionBody) |  | Grant KYC to an account with respect to a token.&lt;br/&gt; KYC is generally a &#34;know your customer&#34; assertion that a responsible entity has sufficient information to positively identify the account holder to relevant authorities. |
| tokenRevokeKyc | [TokenRevokeKycTransactionBody](#proto-TokenRevokeKycTransactionBody) |  | Revoke KYC from an account with respect to a token. |
| tokenDeletion | [TokenDeleteTransactionBody](#proto-TokenDeleteTransactionBody) |  | Deletes an Hedera token.&lt;br/&gt; The token will be marked deleted. |
| tokenUpdate | [TokenUpdateTransactionBody](#proto-TokenUpdateTransactionBody) |  | Update an Hedera token.&lt;br/&gt; Depending on what fields are to be modified, the signature requirements will vary. See `TokenUpdateTransactionBody` for further detail. |
| tokenMint | [TokenMintTransactionBody](#proto-TokenMintTransactionBody) |  | Mint new tokens.&lt;br/&gt; All minted tokens will be delivered to the treasury account for the token type. The &#34;mint key&#34; for the token must sign this transaction. |
| tokenBurn | [TokenBurnTransactionBody](#proto-TokenBurnTransactionBody) |  | Burn tokens from the treasury account.&lt;br/&gt; The &#34;burn key&#34; for the token must sign this transaction. |
| tokenWipe | [TokenWipeAccountTransactionBody](#proto-TokenWipeAccountTransactionBody) |  | Wipe tokens from an account.&lt;br/&gt; This will remove a specified amount of fungible/common tokens or a specified list of non-fungible/unique serial numbered tokens of a given token type from an Hedera account. The removed tokens are _burned_ as if by a `tokenBurn` transaction.&lt;br/&gt; The &#34;wipe key&#34; for the token must sign this transaction. |
| tokenAssociate | [TokenAssociateTransactionBody](#proto-TokenAssociateTransactionBody) |  | Associate tokens to an account. |
| tokenDissociate | [TokenDissociateTransactionBody](#proto-TokenDissociateTransactionBody) |  | Dissociate tokens from an account. |
| scheduleDelete | [ScheduleDeleteTransactionBody](#proto-ScheduleDeleteTransactionBody) |  | Delete a schedule.&lt;br/&gt; The schedule will be marked as deleted. |
| token_pause | [TokenPauseTransactionBody](#proto-TokenPauseTransactionBody) |  | Pause a Token.&lt;br/&gt; This transaction must be signed by the &#34;pause key&#34; for the token. |
| token_unpause | [TokenUnpauseTransactionBody](#proto-TokenUnpauseTransactionBody) |  | Unpause a Token.&lt;br/&gt; This transaction must be signed by the &#34;pause key&#34; for the token. |
| cryptoApproveAllowance | [CryptoApproveAllowanceTransactionBody](#proto-CryptoApproveAllowanceTransactionBody) |  | Add one or more approved allowances for spenders to transfer the paying account&#39;s hbar or tokens. |
| cryptoDeleteAllowance | [CryptoDeleteAllowanceTransactionBody](#proto-CryptoDeleteAllowanceTransactionBody) |  | Delete one or more approvals for spenders to transfer the paying account&#39;s hbar or tokens. |
| token_fee_schedule_update | [TokenFeeScheduleUpdateTransactionBody](#proto-TokenFeeScheduleUpdateTransactionBody) |  | Update the custom fee schedule for a token.&lt;br/&gt; This transaction must be signed by the &#34;fee schedule key&#34; for the token. |
| util_prng | [UtilPrngTransactionBody](#proto-UtilPrngTransactionBody) |  | Provide a deterministic pseudorandom number based on network state. |
| token_update_nfts | [TokenUpdateNftsTransactionBody](#proto-TokenUpdateNftsTransactionBody) |  | Update one or more non-fungible/unique tokens.&lt;br/&gt; This will update metadata for one or more serial numbers within a collection (token type). |
| nodeCreate | [com.hedera.hapi.node.addressbook.NodeCreateTransactionBody](#com-hedera-hapi-node-addressbook-NodeCreateTransactionBody) |  | Create a new node in the network address book.&lt;br/&gt; This is a privileged operation. |
| nodeUpdate | [com.hedera.hapi.node.addressbook.NodeUpdateTransactionBody](#com-hedera-hapi-node-addressbook-NodeUpdateTransactionBody) |  | Update a node in the network address book.&lt;br/&gt; This is a privileged operation. |
| nodeDelete | [com.hedera.hapi.node.addressbook.NodeDeleteTransactionBody](#com-hedera-hapi-node-addressbook-NodeDeleteTransactionBody) |  | Delete a node from the network address book.&lt;br/&gt; This will mark the node as deleted.&lt;br/&gt; This is a privileged operation. |
| tokenReject | [TokenRejectTransactionBody](#proto-TokenRejectTransactionBody) |  | &#34;Reject&#34; undesired tokens.&lt;br/&gt; This transaction will transfer one or more tokens or token balances held by the requesting account to the treasury for each token type. &lt;p&gt; Each transfer MUST be one of the following: &lt;ul&gt; &lt;li&gt;A single non-fungible/unique token.&lt;/li&gt; &lt;li&gt;The full balance held for a fungible/common token type.&lt;/li&gt; &lt;/ul&gt; When complete, the requesting account SHALL NOT hold the rejected tokens.&lt;br/&gt; Custom fees and royalties defined for the tokens rejected SHALL NOT be charged for this transaction. |
| tokenCancelAirdrop | [TokenCancelAirdropTransactionBody](#proto-TokenCancelAirdropTransactionBody) |  | Cancel an &#34;airdrop&#34;.&lt;br/&gt; This transaction cancels a pending airdrop for one or more recipients. &lt;p&gt; The airdrop(s) to cancel MUST be pending, and not claimed.&lt;br/&gt; |
| tokenClaimAirdrop | [TokenClaimAirdropTransactionBody](#proto-TokenClaimAirdropTransactionBody) |  | Claim an &#34;airdrop&#34;. This transaction &#34;claims&#34; one or more pending &#34;airdrops&#34;. &lt;p&gt; The airdrop(s) to claim MUST be pending, and not already claimed.&lt;br/&gt; |
| tokenAirdrop | [TokenAirdropTransactionBody](#proto-TokenAirdropTransactionBody) |  | Send an &#34;airdrop&#34; of tokens to one or more recipients. &lt;p&gt; This transaction unilaterally &#34;gifts&#34; tokens by transferring them from a &#34;sender&#34; account to the &#34;recipient&#34; account(s). If any recipient is not already associated to the token to airdrop, or has set a &#34;reciever signature required&#34; flag, then that recipient is recorded as a &#34;pending&#34; airdrop which must be &#34;claimed&#34;. All other recipients receive the &#34;airdropped&#34; tokens immediately. |





 

 

 

 



## Scalar Value Types

| .proto Type | Notes | C++ | Java | Python | Go | C# | PHP | Ruby |
| ----------- | ----- | --- | ---- | ------ | -- | -- | --- | ---- |
| <a name="double" /> double |  | double | double | float | float64 | double | float | Float |
| <a name="float" /> float |  | float | float | float | float32 | float | float | Float |
| <a name="int32" /> int32 | Uses variable-length encoding. Inefficient for encoding negative numbers – if your field is likely to have negative values, use sint32 instead. | int32 | int | int | int32 | int | integer | Bignum or Fixnum (as required) |
| <a name="int64" /> int64 | Uses variable-length encoding. Inefficient for encoding negative numbers – if your field is likely to have negative values, use sint64 instead. | int64 | long | int/long | int64 | long | integer/string | Bignum |
| <a name="uint32" /> uint32 | Uses variable-length encoding. | uint32 | int | int/long | uint32 | uint | integer | Bignum or Fixnum (as required) |
| <a name="uint64" /> uint64 | Uses variable-length encoding. | uint64 | long | int/long | uint64 | ulong | integer/string | Bignum or Fixnum (as required) |
| <a name="sint32" /> sint32 | Uses variable-length encoding. Signed int value. These more efficiently encode negative numbers than regular int32s. | int32 | int | int | int32 | int | integer | Bignum or Fixnum (as required) |
| <a name="sint64" /> sint64 | Uses variable-length encoding. Signed int value. These more efficiently encode negative numbers than regular int64s. | int64 | long | int/long | int64 | long | integer/string | Bignum |
| <a name="fixed32" /> fixed32 | Always four bytes. More efficient than uint32 if values are often greater than 2^28. | uint32 | int | int | uint32 | uint | integer | Bignum or Fixnum (as required) |
| <a name="fixed64" /> fixed64 | Always eight bytes. More efficient than uint64 if values are often greater than 2^56. | uint64 | long | int/long | uint64 | ulong | integer/string | Bignum |
| <a name="sfixed32" /> sfixed32 | Always four bytes. | int32 | int | int | int32 | int | integer | Bignum or Fixnum (as required) |
| <a name="sfixed64" /> sfixed64 | Always eight bytes. | int64 | long | int/long | int64 | long | integer/string | Bignum |
| <a name="bool" /> bool |  | bool | boolean | boolean | bool | bool | boolean | TrueClass/FalseClass |
| <a name="string" /> string | A string must always contain UTF-8 encoded or 7-bit ASCII text. | string | String | str/unicode | string | string | string | String (UTF-8) |
| <a name="bytes" /> bytes | May contain any arbitrary sequence of bytes. | string | ByteString | str | []byte | ByteString | string | String (ASCII-8BIT) |

