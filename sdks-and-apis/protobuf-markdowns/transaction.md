# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/transaction.proto](#services_transaction-proto)
    - [AtomicBatchTransactionBody](#proto-AtomicBatchTransactionBody)
    - [Transaction](#proto-Transaction)
    - [TransactionBody](#proto-TransactionBody)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_transaction-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/transaction.proto
# Transaction
A (mostly legacy) wrapper around the bytes of a
serialized `SignedTransaction` message.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in
[RFC2119](https://www.ietf.org/rfc/rfc2119) and clarified in
[RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="proto-AtomicBatchTransactionBody"></a>

### AtomicBatchTransactionBody
A transaction body for handling a set of transactions atomically.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| transactions | [Transaction](#proto-Transaction) | repeated | &lt;&lt;&lt;pbj.java_package = &#34;com.hedera.hapi.node.util&#34;&gt;&gt;&gt; This comment is special code for setting PBJ Compiler java package |






<a name="proto-Transaction"></a>

### Transaction
A wrapper around signed transaction bytes.&lt;br/&gt;
This was originally a transaction with body, signatures, and/or bytes,
but is not only a wrapper around a byte array containing signed transction
bytes.

The `signedTransactionBytes` field is REQUIRED and MUST contain a valid,
serialized, `SignedTransaction` message.&lt;br/&gt;
All other fields are deprecated and MUST NOT be set.

#### Additional Notes
The four deprecated fields will be removed and reserved in a future release.

&lt;&lt;&lt;pbj.java_package = &#34;com.hedera.hapi.node.base&#34;&gt;&gt;&gt; This comment is special code for setting PBJ Compiler java package


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| body | [TransactionBody](#proto-TransactionBody) |  | **Deprecated.** Replaced with `signedTransactionBytes`.&lt;br/&gt; The body of the transaction. |
| sigs | [SignatureList](#proto-SignatureList) |  | **Deprecated.** Replaced with `signedTransactionBytes`.&lt;br/&gt; The signatures on the body. |
| sigMap | [SignatureMap](#proto-SignatureMap) |  | **Deprecated.** Replaced with `signedTransactionBytes`.&lt;br/&gt; The signatures on the body with a newer format. |
| bodyBytes | [bytes](#bytes) |  | **Deprecated.** Replaced with `signedTransactionBytes`.&lt;br/&gt; TransactionBody serialized into bytes. |
| signedTransactionBytes | [bytes](#bytes) |  | A valid, serialized, `SignedTransaction` message. &lt;p&gt; This field MUST be present. This field MUST NOT exceed the current network transaction size limit (currently 6144 bytes). |






<a name="proto-TransactionBody"></a>

### TransactionBody
A transaction body.

Every transaction is structured as a signed byte array. That byte array
is a serialized `TransactionBody`.  The transaction body contains the full
content of the transaction, while the `SignedTransaction` includes a
signature map for signatures authenticating that byte array, and that is
serialized and transmitted wrapped in a `Transaction` message.&lt;br/&gt;
The bulk of this message is a `oneof` block which offers the option for
any one of the transaction messages for the network.
This message also includes several additional fields to specify
various parameters required to process a transaction.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| transactionID | [TransactionID](#proto-TransactionID) |  | A transaction identifier.&lt;br/&gt; Each transaction is uniquely identified by its transaction identifier. &lt;p&gt; Each transaction identifier MUST be unique.&lt;br/&gt; Multiple transactions MAY be submitted with the same transaction identifier, but all except the first SHALL be rejected as duplicate transactions.&lt;br/&gt; This identifier MUST specify a `payer` account to be charged all fees associated with the transaction.&lt;br/&gt; This identifier MUST specify a &#34;valid start time&#34;.&lt;br/&gt; The &#34;valid start time&#34; MUST be strictly _earlier_ than the current network consensus time.&lt;br/&gt; The &#34;valid start time&#34; MUST NOT be more than the current network configuration value for `transaction.maxValidDuration` seconds before the current network consensus time.&lt;br/&gt; This identifier MUST NOT set the `scheduled` flag.&lt;br/&gt; This identifier MUST NOT set a nonce value. |
| nodeAccountID | [AccountID](#proto-AccountID) |  | A node account identifier. &lt;p&gt; This MUST identify the account of the consensus node to which this transaction is submitted. |
| transactionFee | [uint64](#uint64) |  | A maximum transaction fee, in tinybar. &lt;p&gt; The network SHALL NOT charge a transaction fee that exceeds this amount.&lt;br/&gt; The network MAY charge up to this amount, and reject the transaction, if the amount offered is insufficient to cover the required fees.&lt;br/&gt; The network MAY charge a minimum fee equal to 80% of the amount offered if the amount offered is much larger than the required fees. |
| transactionValidDuration | [Duration](#proto-Duration) |  | A maximum duration in which to execute this transaction. &lt;p&gt; This transaction SHALL be rejected as expired if the valid start time, extended by this duration, is less than the current network consensus time when the transaction is submitted.&lt;br/&gt; This transaction SHALL be rejected with an invalid duration if this value is greater than the current network configuration value for `transaction.maxValidDuration`. |
| generateRecord | [bool](#bool) |  | **Deprecated.** Records are always generated.&lt;br/&gt; Obsolete option to not generate a record. &lt;p&gt; This flag SHALL be ignored. Every transaction SHALL generate a record, or block stream equivalent. |
| memo | [string](#string) |  | A short description for this transaction. &lt;p&gt; This value, if set, MUST NOT exceed `transaction.maxMemoUtf8Bytes` (default 100) bytes when encoded as UTF-8. |
| batch_key | [Key](#proto-Key) |  | The &lt;b&gt;public key&lt;/b&gt; of the trusted batch assembler. |
| contractCall | [ContractCallTransactionBody](#proto-ContractCallTransactionBody) |  | Call a function defined on a smart contract. |
| contractCreateInstance | [ContractCreateTransactionBody](#proto-ContractCreateTransactionBody) |  | Create a smart contract. |
| contractUpdateInstance | [ContractUpdateTransactionBody](#proto-ContractUpdateTransactionBody) |  | Update a smart contract. |
| cryptoAddLiveHash | [CryptoAddLiveHashTransactionBody](#proto-CryptoAddLiveHashTransactionBody) |  | **Deprecated.** An obsolete, and unsupported, operation to add a &#34;live hash&#34; to an account. |
| cryptoCreateAccount | [CryptoCreateTransactionBody](#proto-CryptoCreateTransactionBody) |  | Create a new Hedera account. |
| cryptoDelete | [CryptoDeleteTransactionBody](#proto-CryptoDeleteTransactionBody) |  | Delete an Hedera account.&lt;br/&gt; This will mark the account as deleted, and transfer all remaining HBAR to a receiver account. |
| cryptoDeleteLiveHash | [CryptoDeleteLiveHashTransactionBody](#proto-CryptoDeleteLiveHashTransactionBody) |  | **Deprecated.** An obsolete, and unsupported, operation to remove a &#34;live hash&#34; from an account. |
| cryptoTransfer | [CryptoTransferTransactionBody](#proto-CryptoTransferTransactionBody) |  | Transfer HBAR between accounts. |
| cryptoUpdateAccount | [CryptoUpdateTransactionBody](#proto-CryptoUpdateTransactionBody) |  | Modify an Hedera account. |
| fileAppend | [FileAppendTransactionBody](#proto-FileAppendTransactionBody) |  | Append data to the end of a file. |
| fileCreate | [FileCreateTransactionBody](#proto-FileCreateTransactionBody) |  | Create a new file. |
| fileDelete | [FileDeleteTransactionBody](#proto-FileDeleteTransactionBody) |  | Delete a file.&lt;br/&gt; This will remove the content of the file, and mark the file as deleted. |
| fileUpdate | [FileUpdateTransactionBody](#proto-FileUpdateTransactionBody) |  | Modify a file.&lt;br/&gt; This may modify any metadata, and/or _replace_ the content. |
| systemDelete | [SystemDeleteTransactionBody](#proto-SystemDeleteTransactionBody) |  | Delete a file as an Hedera administrative function.&lt;br/&gt; This is a privileged operation. |
| systemUndelete | [SystemUndeleteTransactionBody](#proto-SystemUndeleteTransactionBody) |  | Restore a file deleted via `systemDelete`.&lt;br/&gt; This is a privileged operation. |
| contractDeleteInstance | [ContractDeleteTransactionBody](#proto-ContractDeleteTransactionBody) |  | Delete a smart contract and transfer remaining balance to a specified account. |
| freeze | [FreezeTransactionBody](#proto-FreezeTransactionBody) |  | Freeze the network.&lt;br/&gt; This is actually several possible operations, and the caller should examine the &#34;freeze service&#34; for more detail.&lt;br/&gt; This is a privileged operation. |
| consensusCreateTopic | [ConsensusCreateTopicTransactionBody](#proto-ConsensusCreateTopicTransactionBody) |  | Create a topic. |
| consensusUpdateTopic | [ConsensusUpdateTopicTransactionBody](#proto-ConsensusUpdateTopicTransactionBody) |  | Update a topic. |
| consensusDeleteTopic | [ConsensusDeleteTopicTransactionBody](#proto-ConsensusDeleteTopicTransactionBody) |  | Delete a topic. |
| consensusSubmitMessage | [ConsensusSubmitMessageTransactionBody](#proto-ConsensusSubmitMessageTransactionBody) |  | Submit a message to a topic.&lt;br/&gt; A message may be &#34;chunked&#34;, and submitted in parts, if the total message size exceeds the limit for a single transaction. |
| uncheckedSubmit | [UncheckedSubmitBody](#proto-UncheckedSubmitBody) |  | Unsupported system transaction. &lt;p&gt; This transaction MAY be implemented in testing networks, but SHALL NOT be enabled or supported in production environments.&lt;br/&gt; Clients MUST NOT call this method, and any such transaction SHALL be rejected.&lt;br/&gt; A network MAY choose to charge punitive fees for attempting to execute an `uncheckedSubmit`. |
| tokenCreation | [TokenCreateTransactionBody](#proto-TokenCreateTransactionBody) |  | Create a new Hedera token. |
| tokenFreeze | [TokenFreezeAccountTransactionBody](#proto-TokenFreezeAccountTransactionBody) |  | Freeze an account with respect to a token.&lt;br/&gt; A frozen account cannot transact in that token until unfrozen. |
| tokenUnfreeze | [TokenUnfreezeAccountTransactionBody](#proto-TokenUnfreezeAccountTransactionBody) |  | Unfreeze an account with respect to a token. |
| tokenGrantKyc | [TokenGrantKycTransactionBody](#proto-TokenGrantKycTransactionBody) |  | Grant KYC to an account with respect to a token.&lt;br/&gt; KYC is generally a &#34;know your customer&#34; assertion that a responsible entity has sufficient information to positively identify the account holder to relevant authorities. |
| tokenRevokeKyc | [TokenRevokeKycTransactionBody](#proto-TokenRevokeKycTransactionBody) |  | Revoke KYC from an account with respect to a token. |
| tokenDeletion | [TokenDeleteTransactionBody](#proto-TokenDeleteTransactionBody) |  | Delete an Hedera token.&lt;br/&gt; The token will be marked deleted. |
| tokenUpdate | [TokenUpdateTransactionBody](#proto-TokenUpdateTransactionBody) |  | Update an Hedera token.&lt;br/&gt; Depending on what fields are to be modified, the signature requirements will vary. See `TokenUpdateTransactionBody` for further detail. |
| tokenMint | [TokenMintTransactionBody](#proto-TokenMintTransactionBody) |  | Mint new tokens.&lt;br/&gt; All minted tokens will be delivered to the treasury account for the token type. The &#34;mint key&#34; for the token must sign this transaction. |
| tokenBurn | [TokenBurnTransactionBody](#proto-TokenBurnTransactionBody) |  | Burn tokens from the treasury account.&lt;br/&gt; The &#34;burn key&#34; for the token must sign this transaction. |
| tokenWipe | [TokenWipeAccountTransactionBody](#proto-TokenWipeAccountTransactionBody) |  | Wipe tokens from an account.&lt;br/&gt; This will remove a specified amount of fungible/common tokens or a specified list of non-fungible/unique serial numbered tokens of a given token type from an Hedera account. The removed tokens are _burned_ as if by a `tokenBurn` transaction.&lt;br/&gt; The &#34;wipe key&#34; for the token must sign this transaction. |
| tokenAssociate | [TokenAssociateTransactionBody](#proto-TokenAssociateTransactionBody) |  | Associate tokens to an account. |
| tokenDissociate | [TokenDissociateTransactionBody](#proto-TokenDissociateTransactionBody) |  | Dissociate tokens from an account. |
| scheduleCreate | [ScheduleCreateTransactionBody](#proto-ScheduleCreateTransactionBody) |  | Create a schedule.&lt;br/&gt; A schedule is a request to execute a specific transaction, included in the create body, in the future. The scheduled transaction may execute as soon as all signature requirements are met with the schedule create or a subsequent schedule sign transaction. A schedule may, alternatively, execute on expiration if long-term schedules are enabled and the schedule meets signature requirements at that time. |
| scheduleDelete | [ScheduleDeleteTransactionBody](#proto-ScheduleDeleteTransactionBody) |  | Delete a schedule.&lt;br/&gt; The schedule will be marked as deleted. |
| scheduleSign | [ScheduleSignTransactionBody](#proto-ScheduleSignTransactionBody) |  | Sign a schedule.&lt;br/&gt; Add one or more cryptographic keys to the list of keys that have signed a schedule, and which may serve to meet the signature requirements for the scheduled transaction. |
| token_fee_schedule_update | [TokenFeeScheduleUpdateTransactionBody](#proto-TokenFeeScheduleUpdateTransactionBody) |  | Update the custom fee schedule for a token.&lt;br/&gt; This transaction must be signed by the &#34;fee schedule key&#34; for the token. |
| token_pause | [TokenPauseTransactionBody](#proto-TokenPauseTransactionBody) |  | Pause a Token. &lt;p&gt; This transaction MUST be signed by the &#34;pause key&#34; for the token. |
| token_unpause | [TokenUnpauseTransactionBody](#proto-TokenUnpauseTransactionBody) |  | Unpause a Token. &lt;p&gt; This transaction MUST be signed by the &#34;pause key&#34; for the token. |
| cryptoApproveAllowance | [CryptoApproveAllowanceTransactionBody](#proto-CryptoApproveAllowanceTransactionBody) |  | Add one or more approved allowances for spenders to transfer the paying account&#39;s hbar or tokens. |
| cryptoDeleteAllowance | [CryptoDeleteAllowanceTransactionBody](#proto-CryptoDeleteAllowanceTransactionBody) |  | Delete one or more approvals for spenders to transfer the paying account&#39;s hbar or tokens. |
| ethereumTransaction | [EthereumTransactionBody](#proto-EthereumTransactionBody) |  | Perform an Ethereum encoded transaction. |
| node_stake_update | [NodeStakeUpdateTransactionBody](#proto-NodeStakeUpdateTransactionBody) |  | Update the staking information.&lt;br/&gt; This internal transaction is performed at the end of a staking period to complete staking calculations and indicate that new staking period has started. |
| util_prng | [UtilPrngTransactionBody](#proto-UtilPrngTransactionBody) |  | Provide a deterministic pseudorandom number based on network state. |
| token_update_nfts | [TokenUpdateNftsTransactionBody](#proto-TokenUpdateNftsTransactionBody) |  | Update one or more non-fungible/unique tokens.&lt;br/&gt; This will update metadata for one or more serial numbers within a collection (token type). |
| nodeCreate | [com.hedera.hapi.node.addressbook.NodeCreateTransactionBody](#com-hedera-hapi-node-addressbook-NodeCreateTransactionBody) |  | Create a new node in the network address book.&lt;br/&gt; This is a privileged operation. &lt;p&gt; This transaction SHALL create a new consensus node record and add that record to the network address book. |
| nodeUpdate | [com.hedera.hapi.node.addressbook.NodeUpdateTransactionBody](#com-hedera-hapi-node-addressbook-NodeUpdateTransactionBody) |  | Update a node in the network address book.&lt;br/&gt; This is a privileged operation. &lt;p&gt; This transaction SHALL update an existing consensus node record in the network address book. |
| nodeDelete | [com.hedera.hapi.node.addressbook.NodeDeleteTransactionBody](#com-hedera-hapi-node-addressbook-NodeDeleteTransactionBody) |  | Delete a node from the network address book.&lt;br/&gt; This is a privileged operation. &lt;p&gt; This transaction SHALL mark an existing consensus node record as deleted and remove that node from the network address book. |
| tokenReject | [TokenRejectTransactionBody](#proto-TokenRejectTransactionBody) |  | Reject and return a token to treasury.&lt;br/&gt; This transaction will transfer one or more tokens or token balances held by the requesting account to the treasury for each token type. &lt;p&gt; Each transfer MUST be one of the following: &lt;ul&gt; &lt;li&gt;A single non-fungible/unique token.&lt;/li&gt; &lt;li&gt;The full balance held for a fungible/common token type.&lt;/li&gt; &lt;/ul&gt; When complete, the requesting account SHALL NOT hold the rejected tokens.&lt;br/&gt; Custom fees and royalties defined for the tokens rejected SHALL NOT be charged for this transaction. |
| tokenAirdrop | [TokenAirdropTransactionBody](#proto-TokenAirdropTransactionBody) |  | &#34;Airdrop&#34; tokens.&lt;br/&gt; This transaction sends tokens from one or more &#34;sender&#34; accounts to one or more &#34;recipient&#34; accounts. &lt;p&gt; If a recipient account cannot immediately receive the token(s) sent, a &#34;pending&#34; airdrop SHALL be created and MUST be claimed. |
| tokenCancelAirdrop | [TokenCancelAirdropTransactionBody](#proto-TokenCancelAirdropTransactionBody) |  | Cancel one or more &#34;pending&#34; airdrops that are not yet claimed. |
| tokenClaimAirdrop | [TokenClaimAirdropTransactionBody](#proto-TokenClaimAirdropTransactionBody) |  | Claim one or more &#34;pending&#34; airdrops. |
| state_signature_transaction | [com.hedera.hapi.platform.event.StateSignatureTransaction](#com-hedera-hapi-platform-event-StateSignatureTransaction) |  | A transaction body for signature of a state root hash gossiped to other nodes |
| hints_preprocessing_vote | [com.hedera.hapi.services.auxiliary.hints.HintsPreprocessingVoteTransactionBody](#com-hedera-hapi-services-auxiliary-hints-HintsPreprocessingVoteTransactionBody) |  | A transaction body for voting on hinTS aggregation keys. |
| hints_key_publication | [com.hedera.hapi.services.auxiliary.hints.HintsKeyPublicationTransactionBody](#com-hedera-hapi-services-auxiliary-hints-HintsKeyPublicationTransactionBody) |  | A transaction body for publishing a node&#39;s hintTS key. |
| hints_partial_signature | [com.hedera.hapi.services.auxiliary.hints.HintsPartialSignatureTransactionBody](#com-hedera-hapi-services-auxiliary-hints-HintsPartialSignatureTransactionBody) |  | A transaction body for broadcasting a node&#39;s hintTS partial signature on a message. |
| history_proof_signature | [com.hedera.hapi.services.auxiliary.history.HistoryProofSignatureTransactionBody](#com-hedera-hapi-services-auxiliary-history-HistoryProofSignatureTransactionBody) |  | A transaction body for contributed a signature with a node&#39;s proof key to a history proof. |
| history_proof_key_publication | [com.hedera.hapi.services.auxiliary.history.HistoryProofKeyPublicationTransactionBody](#com-hedera-hapi-services-auxiliary-history-HistoryProofKeyPublicationTransactionBody) |  | A transaction body for publishing a node&#39;s metadata proof key. |
| history_proof_vote | [com.hedera.hapi.services.auxiliary.history.HistoryProofVoteTransactionBody](#com-hedera-hapi-services-auxiliary-history-HistoryProofVoteTransactionBody) |  | A transaction body for voting on a metadata proof descending from the ledger id. |
| crs_publication | [com.hedera.hapi.services.auxiliary.hints.CrsPublicationTransactionBody](#com-hedera-hapi-services-auxiliary-hints-CrsPublicationTransactionBody) |  | A transaction body for broadcasting a node&#39;s crs publication |
| atomic_batch | [AtomicBatchTransactionBody](#proto-AtomicBatchTransactionBody) |  | A transaction body for handling a set of transactions atomically. |
| max_custom_fees | [CustomFeeLimit](#proto-CustomFeeLimit) | repeated | A list of maximum custom fees that the users are willing to pay. &lt;p&gt; This field is OPTIONAL.&lt;br/&gt; If left empty, the users are accepting to pay any custom fee.&lt;br/&gt; If used with a transaction type that does not support custom fee limits, the transaction will fail. |





 

 

 

 



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

