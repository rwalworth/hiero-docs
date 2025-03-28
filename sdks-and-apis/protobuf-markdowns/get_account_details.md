# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/get_account_details.proto](#services_get_account_details-proto)
    - [GetAccountDetailsQuery](#proto-GetAccountDetailsQuery)
    - [GetAccountDetailsResponse](#proto-GetAccountDetailsResponse)
    - [GetAccountDetailsResponse.AccountDetails](#proto-GetAccountDetailsResponse-AccountDetails)
    - [GrantedCryptoAllowance](#proto-GrantedCryptoAllowance)
    - [GrantedNftAllowance](#proto-GrantedNftAllowance)
    - [GrantedTokenAllowance](#proto-GrantedTokenAllowance)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_get_account_details-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/get_account_details.proto
# Get Account Details
A standard query to inspect the full detail of an account.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in
[RFC2119](https://www.ietf.org/rfc/rfc2119) and clarified in
[RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="proto-GetAccountDetailsQuery"></a>

### GetAccountDetailsQuery
Request detail information about an account.

The returned information SHALL include balance and allowances.&lt;br/&gt;
The returned information SHALL NOT include a list of account records.

#### Important
This query is a _privileged_ query. Only &#34;system&#34; accounts SHALL be
permitted to submit this query.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| header | [QueryHeader](#proto-QueryHeader) |  | Standard information sent with every query operation.&lt;br/&gt; This includes the signed payment and what kind of response is requested (cost, state proof, both, or neither). |
| account_id | [AccountID](#proto-AccountID) |  | An account ID for which information is requested &lt;p&gt; This value SHALL identify the account to be queried.&lt;br/&gt; This value MUST identify a valid account.&lt;br/&gt; This field is REQUIRED. |






<a name="proto-GetAccountDetailsResponse"></a>

### GetAccountDetailsResponse
A response to a `GetAccountDetailsQuery`.

This SHALL contain the account details if requested and successful.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| header | [ResponseHeader](#proto-ResponseHeader) |  | The standard response information for queries.&lt;br/&gt; This includes the values requested in the `QueryHeader` (cost, state proof, both, or neither). |
| account_details | [GetAccountDetailsResponse.AccountDetails](#proto-GetAccountDetailsResponse-AccountDetails) |  | Details of the account. &lt;p&gt; A state proof MAY be generated for this field. |






<a name="proto-GetAccountDetailsResponse-AccountDetails"></a>

### GetAccountDetailsResponse.AccountDetails
Information describing a single Account in the Hedera distributed ledger.

#### Attributes
Each Account may have a unique three-part identifier, a Key, and one or
more token balances. Accounts also have an alias, which has multiple
forms, and may be set automatically. Several additional items are
associated with the Account to enable full functionality.

#### Expiration
Accounts, as most items in the network, have an expiration time, recorded
as a `Timestamp`, and must be &#34;renewed&#34; for a small fee at expiration.
This helps to reduce the amount of inactive accounts retained in state.
Another account may be designated to pay any renewal fees and
automatically renew the account for (by default) 30-90 days at a time as
a means to optionally ensure important accounts remain active.

### Staking
Accounts may participate in securing the network by &#34;staking&#34; the account
balances to a particular network node, and receive a portion of network
fees as a reward. An account may optionally decline these rewards but
still stake its balances.

#### Transfer Restrictions
An account may optionally require that inbound transfer transactions be
signed by that account as receiver (in addition to any other signatures
required, including sender).


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| account_id | [AccountID](#proto-AccountID) |  | The unique ID of this account. &lt;p&gt; An account ID, when assigned to this field, SHALL be of the form `shard.realm.number`.&lt;br/&gt; Transactions MAY reference the account by alias, but the account itself MUST always have a purely numeric identifier. This numeric ID is the value used to reference the account in query responses, transaction receipts, transaction records, and the block stream. |
| contract_account_id | [string](#string) |  | A Solidity ID.&lt;br/&gt; This identifies the contract instance, and the `Account` associated with that contract instance. &lt;p&gt; This SHALL be populated if this account is a smart contract, and SHALL NOT be populated otherwise.&lt;br/&gt; This SHALL be formatted as a string according to Solidity ID standards. |
| deleted | [bool](#bool) |  | A boolean indicating that this account is deleted. |
| proxy_account_id | [AccountID](#proto-AccountID) |  | **Deprecated.** Replaced by StakingInfo.&lt;br/&gt; ID of the account to which this account is staking its balances. If this account is not currently staking its balances, then this field, if set, SHALL be the sentinel value of `0.0.0`. |
| proxy_received | [int64](#int64) |  | The total amount of tinybar proxy staked to this account. |
| key | [Key](#proto-Key) |  | The key to be used to sign transactions from this account, if any. &lt;p&gt; This key SHALL NOT be set for hollow accounts until the account is finalized.&lt;br/&gt; This key SHALL be set on all other accounts, except for certain immutable accounts (0.0.800 and 0.0.801) necessary for network function and otherwise secured by the governing council. |
| balance | [uint64](#uint64) |  | The HBAR balance of this account, in tinybar (10&lt;sup&gt;-8&lt;/sup&gt; HBAR). &lt;p&gt; This value SHALL always be a whole number. |
| receiver_sig_required | [bool](#bool) |  | A boolean indicating that the account requires a receiver signature for inbound token transfer transactions. &lt;p&gt; If this value is `true` then a transaction to transfer tokens to this account SHALL NOT succeed unless this account has signed the transfer transaction. |
| expiration_time | [Timestamp](#proto-Timestamp) |  | The current expiration time for this account. &lt;p&gt; This account SHALL be due standard renewal fees when the network consensus time exceeds this time.&lt;br/&gt; If rent and expiration are enabled for the network, and automatic renewal is enabled for this account, renewal fees SHALL be charged after this time, and, if charged, the expiration time SHALL be extended for another renewal period.&lt;br/&gt; This account MAY be expired and removed from state at any point after this time if not renewed.&lt;br/&gt; An account holder MAY extend this time by submitting an account update transaction to modify expiration time, subject to the current maximum expiration time for the network. |
| auto_renew_period | [Duration](#proto-Duration) |  | A duration to extend this account&#39;s expiration. &lt;p&gt; The network SHALL extend the account&#39;s expiration by this duration, if funds are available, upon automatic renewal.&lt;br/&gt; This SHALL NOT apply if the account is already deleted upon expiration.&lt;br/&gt; If this is not provided in an allowed range on account creation, the transaction SHALL fail with INVALID_AUTO_RENEWAL_PERIOD. The default values for the minimum period and maximum period are currently 30 days and 90 days, respectively. |
| token_relationships | [TokenRelationship](#proto-TokenRelationship) | repeated | As of `HIP-367`, which enabled unlimited token associations, the potential scale for this value requires that users consult a mirror node for this information. Only the top `maxRelsPerInfoQuery` (default 1000) relationships will be returned by this query.&lt;br/&gt; A list of tokens to which this account is &#34;associated&#34;, enabling the transfer of that token type by this account. |
| memo | [string](#string) |  | A short description of this account. &lt;p&gt; This value, if set, MUST NOT exceed `transaction.maxMemoUtf8Bytes` (default 100) bytes when encoded as UTF-8. |
| owned_nfts | [int64](#int64) |  | The total number of non-fungible/unique tokens owned by this account. |
| max_automatic_token_associations | [int32](#int32) |  | The maximum number of tokens that can be auto-associated with the account. &lt;p&gt; If this is less than or equal to `used_auto_associations` (or 0), then this account MUST manually associate with a token before transacting in that token.&lt;br/&gt; Following HIP-904 This value may also be `-1` to indicate no limit.&lt;br/&gt; This value MUST NOT be less than `-1`. |
| alias | [bytes](#bytes) |  | An account EVM alias.&lt;br/&gt; This is a value used in some contexts to reference an account when the tripartite account identifier is not available. &lt;p&gt; This field, when set to a non-default value, is immutable and SHALL NOT be changed. |
| ledger_id | [bytes](#bytes) |  | The ledger ID of the network that generated this response. &lt;p&gt; This value SHALL identify the distributed ledger that responded to this query. |
| granted_crypto_allowances | [GrantedCryptoAllowance](#proto-GrantedCryptoAllowance) | repeated | A list of crypto (HBAR) allowances approved by this account. &lt;p&gt; If this is not empty, each allowance SHALL permit a specified &#34;spender&#34; account to spend this account&#39;s HBAR balance, up to a designated limit.&lt;br/&gt; This field SHALL permit spending only HBAR balance, not other tokens the account may hold.&lt;br/&gt; Allowances for other tokens SHALL be listed in the `token_allowances` field or the `approve_for_all_nft_allowances` field. |
| granted_nft_allowances | [GrantedNftAllowance](#proto-GrantedNftAllowance) | repeated | A list of non-fungible token (NFT) allowances approved by this account. &lt;p&gt; If this is not empty, each allowance SHALL permit a specified &#34;spender&#34; account to transfer _all_ of this account&#39;s non-fungible/unique tokens from a particular collection.&lt;br/&gt; Allowances for a specific serial number MUST be directly associated with that specific non-fungible/unique token, rather than the holding account. |
| granted_token_allowances | [GrantedTokenAllowance](#proto-GrantedTokenAllowance) | repeated | A list of fungible token allowances approved by this account. &lt;p&gt; If this is not empty, each allowance SHALL permit a specified &#34;spender&#34; to spend this account&#39;s fungible tokens, of the designated type, up to a designated limit. |






<a name="proto-GrantedCryptoAllowance"></a>

### GrantedCryptoAllowance
Permission granted by one account (the &#34;funding&#34; account) to another
account (the &#34;spender&#34; account) that allows the spender to spend a
specified amount of HBAR owned by the funding account.

An allowance SHALL NOT transfer any HBAR directly, it only permits
transactions signed only by the spender account to transfer HBAR, up
to the amount specified, from the funding account.

Once the specified amount is spent, the allowance SHALL be consumed
and a new allowance SHALL be required before that spending account
may spend additional HBAR from the funding account.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| spender | [AccountID](#proto-AccountID) |  | The identifier for the spending account associated with this allowance. &lt;p&gt; This account SHALL be permitted to sign transactions to spend HBAR from the funding/allowing account.&lt;br/&gt; This permission SHALL be limited to no more than the specified `amount`. |
| amount | [int64](#int64) |  | The maximum amount that the spender account may transfer within the scope of this allowance. &lt;p&gt; This allowance SHALL be consumed if any combination of transfers authorized via this allowance meet this value in total.&lt;br/&gt; This value MUST be specified in tinybar (i.e. 10&lt;sup&gt;-8&lt;/sup&gt; HBAR). |






<a name="proto-GrantedNftAllowance"></a>

### GrantedNftAllowance
Permission granted by one account (the &#34;funding&#34; account) to another
account (the &#34;spender&#34; account) that allows the spender to transfer
all serial numbers of a specific non-fungible/unique token (NFT)
collection owned by the funding account.&lt;br/&gt;
This is a broad permission, as it does not matter how many NFTs of the
specified collection the funding account owns, the spender MAY dispose
of any or all of them with this allowance.&lt;br/&gt;
Each token type (typically a collection of NFTs) SHALL require
a separate allowance.&lt;br/&gt;
Allowances for a specific serial number MUST be directly associated
with that specific non-fungible/unique token, rather than
the holding account.

An allowance SHALL NOT transfer any tokens directly, it only permits
transactions signed only by the spender account to transfer any
non-fungible/unique tokens of the specified type owned by
the funding account.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| token_id | [TokenID](#proto-TokenID) |  | The identifier for the token associated with this allowance. &lt;p&gt; This token MUST be a non-fungible/unique token. |
| spender | [AccountID](#proto-AccountID) |  | The identifier for the spending account associated with this allowance. &lt;p&gt; This account SHALL be permitted to sign transactions to spend tokens of the associated token type from the funding/allowing account. |






<a name="proto-GrantedTokenAllowance"></a>

### GrantedTokenAllowance
Permission granted by one account (the &#34;funding&#34; account) to another
account (the &#34;spender&#34; account) that allows the spender to spend a
specified amount of a specific non-HBAR fungible token from the
balance owned by the funding account.

An allowance SHALL NOT transfer any tokens directly, it only permits
transactions signed only by the spender account to transfer tokens
of the specified type, up to the amount specified, from the funding account.

Once the specified amount is spent, the allowance SHALL be consumed
and a new allowance SHALL be required before that spending account
may spend additional tokens from the funding account.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| token_id | [TokenID](#proto-TokenID) |  | The identifier for the token associated with this allowance. &lt;p&gt; This token MUST be a fungible/common token. |
| spender | [AccountID](#proto-AccountID) |  | The identifier for the spending account associated with this allowance. &lt;p&gt; This account SHALL be permitted to sign transactions to spend tokens of the associated token type from the funding/allowing account.&lt;br/&gt; This permission SHALL be limited to no more than the specified `amount`. |
| amount | [int64](#int64) |  | The maximum amount that the spender account may transfer within the scope of this allowance. &lt;p&gt; This allowance SHALL be consumed if any combination of transfers authorized via this allowance meet this value in total.&lt;br/&gt; This value MUST be specified in the smallest units of the relevant token (i.e. 10&lt;sup&gt;-decimals&lt;/sup&gt; whole tokens). |





 

 

 

 



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

