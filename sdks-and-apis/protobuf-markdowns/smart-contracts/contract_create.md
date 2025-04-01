# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/contract_create.proto](#services_contract_create-proto)
    - [ContractCreateTransactionBody](#proto-ContractCreateTransactionBody)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_contract_create-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/contract_create.proto
# Smart Contract Create

Create a new smart contract.

## General Comments
 - A smart contract normally enforces rules, so &#34;the code is law&#34;.&lt;br/&gt;
   For example, an ERC-20 contract prevents a transfer from being undone
   without a signature by the recipient of the transfer. This characteristic
   is generally true if the contract instance was created without a value
   for the `adminKey` field. For some uses, however, it may be desirable to
   create something like an ERC-20 contract that has a specific group of
   trusted individuals who can act as a &#34;supreme court&#34; with the ability to
   override the normal operation, when a sufficient number of them agree to
   do so. If `adminKey` is set to a valid Key (which MAY be complex), then a
   transaction that can change the state of the smart contract in arbitrary
   ways MAY be signed with enough signatures to activate that Key. Such
   transactions might reverse a transaction, change the code to close an
   unexpected loophole, remove an exploit, or adjust outputs in ways not
   covered by the code itself. The admin key MAY also be used to change the
   autoRenewPeriod, and change the adminKey field itself (for example, to
   remove that key after a suitable testing period). The API currently does
   not implement all relevant capabilities. But it does allow the `adminKey`
   field to be set and queried, and MAY implement further administrative
   capability in future releases.
 - The current API ignores shardID, realmID, and newRealmAdminKey, and
   creates everything in shard 0 and realm 0. Future versions of the system
   MAY support additional shards and realms.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in
[RFC2119](https://www.ietf.org/rfc/rfc2119) and clarified in
[RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="proto-ContractCreateTransactionBody"></a>

### ContractCreateTransactionBody
Create a new smart contract.

If this transaction succeeds, the `ContractID` for the new smart contract
SHALL be set in the transaction receipt.&lt;br/&gt;
The contract is defined by the initial bytecode (or `initcode`). The
`initcode` SHALL be stored either in a previously created file, or in the
transaction body itself for very small contracts.

As part of contract creation, the constructor defined for the new smart
contract SHALL run with the parameters provided in the
`constructorParameters` field.&lt;br/&gt;
The gas to &#34;power&#34; that constructor MUST be provided via the `gas` field,
and SHALL be charged to the payer for this transaction.&lt;br/&gt;
If the contract _constructor_ stores information, it is charged gas for that
storage. There is a separate fee in HBAR to maintain that storage until the
expiration, and that fee SHALL be added to this transaction as part of the
_transaction fee_, rather than gas.

### Block Stream Effects
A `CreateContractOutput` message SHALL be emitted for each transaction.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| fileID | [FileID](#proto-FileID) |  | The source for the smart contract EVM bytecode. &lt;p&gt; The file containing the smart contract initcode. A copy of the contents SHALL be made and held as `bytes` in smart contract state.&lt;br/&gt; The contract bytecode is limited in size only by the network file size limit. |
| initcode | [bytes](#bytes) |  | The source for the smart contract EVM bytecode. &lt;p&gt; The bytes of the smart contract initcode. A copy of the contents SHALL be made and held as `bytes` in smart contract state.&lt;br/&gt; This value is limited in length by the network transaction size limit. This entire transaction, including all fields and signatures, MUST be less than the network transaction size limit. |
| adminKey | [Key](#proto-Key) |  | Access control for modification of the smart contract after it is created. &lt;p&gt; If this field is set, that key MUST sign this transaction.&lt;br/&gt; If this field is set, that key MUST sign each future transaction to update or delete the contract.&lt;br/&gt; An updateContract transaction that _only_ extends the topic expirationTime (a &#34;manual renewal&#34; transaction) SHALL NOT require admin key signature. &lt;p&gt; A contract without an admin key SHALL be immutable, except for expiration and renewal. |
| gas | [int64](#int64) |  | A maximum limit to the amount of gas to use for the constructor call. &lt;p&gt; The network SHALL charge the greater of the following, but SHALL NOT charge more than the value of this field. &lt;ol&gt; &lt;li&gt;The actual gas consumed by the smart contract constructor call.&lt;/li&gt; &lt;li&gt;`80%` of this value.&lt;/li&gt; &lt;/ol&gt; The `80%` factor encourages reasonable estimation, while allowing for some overage to ensure successful execution. |
| initialBalance | [int64](#int64) |  | The amount of HBAR to use as an initial balance for the account representing the new smart contract. &lt;p&gt; This value is presented in tinybar (10&lt;sup&gt;&lt;strong&gt;-&lt;/strong&gt;8&lt;/sup&gt; HBAR).&lt;br/&gt; The HBAR provided here will be withdrawn from the payer account that signed this transaction. |
| proxyAccountID | [AccountID](#proto-AccountID) |  | **Deprecated.** Proxy account staking is handled via `staked_id`. &lt;p&gt; Former field to designate a proxy account for HBAR staking. This field MUST NOT be set. |
| autoRenewPeriod | [Duration](#proto-Duration) |  | The initial lifetime, in seconds, for the smart contract, and the number of seconds for which the smart contract will be automatically renewed upon expiration. &lt;p&gt; This value MUST be set.&lt;br/&gt; This value MUST be greater than the configured MIN_AUTORENEW_PERIOD.&lt;br/&gt; This value MUST be less than the configured MAX_AUTORENEW_PERIOD.&lt;br/&gt; |
| constructorParameters | [bytes](#bytes) |  | An array of bytes containing the EVM-encoded parameters to pass to the smart contract constructor defined in the smart contract init code provided. |
| shardID | [ShardID](#proto-ShardID) |  | &lt;blockquote&gt;Review Question&lt;br/&gt; &lt;blockquote&gt;Should this be deprecated?&lt;br/&gt; It&#39;s never been used and probably never should be used...&lt;br/&gt; Shard should be determined by the node the transaction is submitted to. &lt;/blockquote&gt;&lt;/blockquote&gt; &lt;p&gt; The shard in which to create the new smart contract.&lt;br/&gt; This value is currently ignored. |
| realmID | [RealmID](#proto-RealmID) |  | &lt;blockquote&gt;Review Question&lt;br/&gt; &lt;blockquote&gt;Should this be deprecated?&lt;br/&gt; It&#39;s never been used and probably never should be used...&lt;br/&gt; Realm should be determined by node and network parameters. &lt;/blockquote&gt;&lt;/blockquote&gt; &lt;p&gt; The shard/realm in which to create the new smart contract.&lt;br/&gt; This value is currently ignored. |
| newRealmAdminKey | [Key](#proto-Key) |  | &lt;blockquote&gt;Review Question&lt;br/&gt; &lt;blockquote&gt;Should this be deprecated?&lt;br/&gt; It&#39;s never been used and probably never should be used...&lt;br/&gt; If a realm is used, it must already exist; we shouldn&#39;t be creating it without a separate transaction.&lt;/blockquote&gt;&lt;/blockquote&gt; &lt;p&gt; This was intended to provide an admin key for any new realm created during the creation of the smart contract.&lt;br/&gt; This value is currently ignored. a new realm SHALL NOT be created, regardless of the value of `realmID`. |
| memo | [string](#string) |  | A short memo for this smart contract. &lt;p&gt; This value, if set, MUST NOT exceed `transaction.maxMemoUtf8Bytes` (default 100) bytes when encoded as UTF-8. |
| max_automatic_token_associations | [int32](#int32) |  | The maximum number of tokens that can be auto-associated with this smart contract. &lt;p&gt; If this is less than or equal to `used_auto_associations` (or 0), then this contract MUST manually associate with a token before transacting in that token.&lt;br/&gt; Following HIP-904 This value may also be `-1` to indicate no limit.&lt;br/&gt; This value MUST NOT be less than `-1`. |
| auto_renew_account_id | [AccountID](#proto-AccountID) |  | The id of an account, in the same shard and realm as this smart contract, that has signed this transaction, allowing the network to use its balance, when needed, to automatically extend this contract&#39;s expiration time. &lt;p&gt; If this field is set, that key MUST sign this transaction.&lt;br/&gt; If this field is set, then the network SHALL deduct the necessary fees from the designated auto renew account, if that account has sufficient balance. If the auto renew account does not have sufficient balance, then the fees for contract renewal SHALL be deducted from the HBAR balance held by the smart contract.&lt;br/&gt; If this field is not set, then all renewal fees SHALL be deducted from the HBAR balance held by this contract. |
| staked_account_id | [AccountID](#proto-AccountID) |  | An account ID. &lt;p&gt; This smart contract SHALL stake its HBAR via this account as proxy. |
| staked_node_id | [int64](#int64) |  | The ID of a network node. &lt;p&gt; This smart contract SHALL stake its HBAR to this node. &lt;p&gt; &lt;blockquote&gt;Note: node IDs do fluctuate as node operators change. Most contracts are immutable, and a contract staking to an invalid node ID SHALL NOT participate in staking. Immutable contracts MAY find it more reliable to use a proxy account for staking (via `staked_account_id`) to enable updating the _effective_ staking node ID when necessary through updating the proxy account.&lt;/blockquote&gt; |
| decline_reward | [bool](#bool) |  | A flag indicating that this smart contract declines to receive any reward for staking its HBAR balance to help secure the network. &lt;p&gt; If set to true, this smart contract SHALL NOT receive any reward for staking its HBAR balance to help secure the network, regardless of staking configuration, but MAY stake HBAR to support the network without reward. |





 

 

 

 



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

