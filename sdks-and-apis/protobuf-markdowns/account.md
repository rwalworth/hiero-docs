# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/state/token/account.proto](#services_state_token_account-proto)
    - [Account](#proto-Account)
    - [AccountApprovalForAllAllowance](#proto-AccountApprovalForAllAllowance)
    - [AccountCryptoAllowance](#proto-AccountCryptoAllowance)
    - [AccountFungibleTokenAllowance](#proto-AccountFungibleTokenAllowance)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_state_token_account-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/state/token/account.proto
# Account.
This is a single account within the Hedera network. An Account is the
primary entity representing ownership of assets tracked on the ledger.
Account Allowances are also described here, and these represent permission
granted to an account to transfer assets owned by a different account which
granted the &#34;allowance&#34;. Allowances specify the assets and amounts which may
be transferred.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in
[RFC2119](https://www.ietf.org/rfc/rfc2119) and clarified in
[RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="proto-Account"></a>

### Account
A single Account in the Hedera distributed ledger.

Each Account SHALL have a unique three-part identifier, a Key, and one
or more token balances.&lt;br/&gt;
Each Account SHALL have an alias, which has multiple forms, and MAY be set automatically.&lt;br/&gt;
Several additional items SHALL be associated with the Account to enable
full functionality.&lt;br/&gt;
Assets SHALL be represented as linked-lists with only the &#34;head&#34; item
referenced directly in the Account, and the remaining items SHALL be
accessible via the token relation or unique tokens maps.&lt;br/&gt;
Accounts, as most items in the network, SHALL have an expiration time,
recorded as seconds since the epoch, and MUST be &#34;renewed&#34; for a small fee
at expiration. This helps to reduce the amount of inactive accounts retained
in state.&lt;br/&gt;
Another account MAY be designated to pay any renewal fees and automatically
renew an account for (by default) 30-90 days at a time as a means to
optionally ensure important accounts remain active.&lt;br/&gt;
Accounts MAY participate in securing the network by &#34;staking&#34; the account
balances to a particular network node, and receive a portion of network
fees as a reward. An account MAY optionally decline these rewards but still
stake its balances.&lt;br/&gt;
An account MAY optionally require that inbound transfer transactions be
signed by that account as receiver
(in addition to the sender&#39;s signature).&lt;br/&gt;
As with all network entities, Account ID SHALL be represented as
shard.realm.X.&lt;br/&gt;
Alias and contractId SHALL be additional identifiers used to connect accounts
to transactions before the account is fully enabled,
or in EVM contracts.&lt;br/&gt;

---

#### Alias
There is considerable complexity with `alias` (aka `evm_address`) for
Accounts. Much of this comes from the existence of a &#34;hidden&#34; alias for
almost all accounts, and the reuse of the alias field for both EVM reference
and &#34;automatic&#34; account creation.

For the purposes of this specification, we will use the following terms for
clarity.
  - `key_alias` is the account public key as a protobuf serialized message
    and used for auto-creation and subsequent lookup. This is only valid if
    the account key is a
    single `primitive` key, either ED25519 or ECDSA_SECP256K1.
  - `evm_address` exists for every account and is one of
     - `contract_address`, which is the 20 byte EVM contract address per
       EIP-1014
     - `evm_key_address`, which is the keccak-256 hash of a ECDSA_SECP256K1
       `primitive` key.
        - This is for accounts lazy-created from EVM public keys, when the
          corresponding ECDSA_SECP256K1 public key is presented in a
          transaction signed by the private key for that public key, the
          account is created that key assigned, and the protobuf-serialized
          form is set as the account alias.
     - `long_zero`, is a synthetic 20 byte address inferred for &#34;normally&#34;
       created accounts. It is constructed from the &#34;standard&#34; AccountID as
       follows.
        - 4 byte big-endian shard number
        - 8 byte big-endian realm number
        - 8 byte big-endian entity number

The `alias` field in the `Account` message SHALL contain one of four values
for any given account.
  - The `key_alias`, if the account was created by transferring HBAR to the
    account referenced by `key_alias`.
  - The `evm_key_address` if the account was created from an EVM public key
  - The `contract_address` if the account belongs to an EVM contract
  - Not-Set/null/Bytes.EMPTY (collectively `null`) if the account was
    created normally

If the `alias` field of an `Account` is any form of `null`, then the account
MAY be referenced by `alias` in an `AccountID` by using the `long_zero`
address for the account. This &#34;hidden default&#34; alias SHALL NOT be stored,
but is synthesized by the node software as needed, and may be synthesized by
an EVM contract or client software as well.

An AccountID in a transaction MAY reference an `Account` with
`shard`.`realm`.`alias`.&lt;br/&gt;
If the account `alias` field is set for an Account, that value SHALL be the
account alias.&lt;br/&gt;
If the account `alias` field is not set for an Account, the `long_zero`
alias SHALL be the account alias.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| account_id | [AccountID](#proto-AccountID) |  | The unique ID of this account. &lt;p&gt; An account ID, when assigned to this field, SHALL be of the form `shard.realm.number`.&lt;br/&gt; Transactions MAY reference the account by alias, but the account itself MUST always have a purely numeric identifier. This numeric ID is the value used to reference the account in query responses, transaction receipts, transaction records, and the block stream. |
| alias | [bytes](#bytes) |  | An account EVM alias. &lt;p&gt; This is a value used in some contexts to reference an account when the numeric account identifier is not available.&lt;br/&gt; This field, when set to a non-default value, is immutable and SHALL NOT be changed. |
| key | [Key](#proto-Key) |  | The key to be used to sign transactions from this account, if any. &lt;p&gt; This key SHALL NOT be set for hollow accounts until the account is finalized.&lt;br/&gt; This key SHALL be set on all other accounts, except for certain immutable accounts (0.0.800 and 0.0.801) necessary for network function and otherwise secured by the governing council. |
| expiration_second | [int64](#int64) |  | The current expiration time of this account, in seconds since the epoch. &lt;p&gt; For this purpose, `epoch` SHALL be the UNIX epoch with 0 at `1970-01-01T00:00:00.000Z`.&lt;br/&gt; This account SHALL be due standard renewal fees when the network consensus time exceeds this time.&lt;br/&gt; If rent and expiration are enabled for the network, and automatic renewal is enabled for this account, renewal fees SHALL be charged after this time, and, if charged, the expiration time SHALL be extended for another renewal period.&lt;br/&gt; This account MAY be expired and removed from state at any point after this time if not renewed.&lt;br/&gt; An account holder MAY extend this time by submitting an account update transaction to modify expiration time, subject to the current maximum expiration time for the network. |
| tinybar_balance | [int64](#int64) |  | The HBAR balance of this account, in tinybar (10&lt;sup&gt;-8&lt;/sup&gt; HBAR). &lt;p&gt; This value is a signed integer for efficiency, but MUST always be a whole number. |
| memo | [string](#string) |  | A short description of this account. &lt;p&gt; This value, if set, MUST NOT exceed `transaction.maxMemoUtf8Bytes` (default 100) bytes when encoded as UTF-8. |
| deleted | [bool](#bool) |  | A boolean indicating that this account is deleted. |
| staked_to_me | [int64](#int64) |  | The amount of HBAR staked to this account by others. |
| stake_period_start | [int64](#int64) |  | If this account stakes to another account, this value SHALL be set to the time when the current period for staking and reward calculations began. |
| staked_account_id | [AccountID](#proto-AccountID) |  | An identifier for the account to which this account is staking its balances as a proxy. &lt;p&gt; If this account is not currently staking its balances, then this field, if set, SHALL be the sentinel value of `0.0.0`. |
| staked_node_id | [int64](#int64) |  | An identifier for the node this account is staked to. &lt;p&gt; If this account is not currently staking its balances, then this field, if set, SHALL be the sentinel value of `-1`. Wallet software SHOULD surface staking issues to users and provide a simple mechanism to update staking to a new node ID in the event the prior staked node ID ceases to be valid. &lt;p&gt; &lt;blockquote&gt;Note: node IDs do fluctuate as node operators change. The Account owner MUST submit a new transaction to change this value if the current node ID changes or ceases to operate as a node. An account with an invalid `staked_node_id` SHALL NOT participate in staking until the `staked_node_id` is updated to a valid node ID. &lt;/blockquote&gt; |
| decline_reward | [bool](#bool) |  | A boolean indicating that this account has chosen to decline rewards for staking its balances. &lt;p&gt; This account MAY still stake its balances, but SHALL NOT receive reward payments for doing so. |
| receiver_sig_required | [bool](#bool) |  | A boolean indicating that the account requires a receiver signature for inbound token transfer transactions. &lt;p&gt; If this value is `true` then a transaction to transfer tokens to this account SHALL NOT succeed unless this account has signed the transfer transaction. |
| head_token_id | [TokenID](#proto-TokenID) |  | A token ID at the head of the linked list for this account from the token relations map.&lt;br/&gt; The token relations are connected by including the &#34;next&#34; and &#34;previous&#34; TokenID in each TokenRelation message. The &#34;head&#34; item in that list is found by looking up the TokenRelation with this Account&#39;s account_id and this head_token_id. Each subsequent item in the list is found via similar lookup with both an AccountID and a TokenID. |
| head_nft_id | [NftID](#proto-NftID) |  | A NftID at the head of the linked list for this account from the unique tokens map.&lt;br/&gt; The unique token relations are connected by including the &#34;next&#34; and &#34;previous&#34; NftID in each Nft message. The &#34;head&#34; item in that list is found by looking up the Nft with ID matching this head_nft_id. Each subsequent item in the list is found via similar lookup with the next or previous NftID. |
| head_nft_serial_number | [int64](#int64) |  | A serial number in the NftID at the head of the linked list for this account from unique tokens map. &lt;p&gt; This MUST match the `serial_number` field of `head_nft_id`. |
| number_owned_nfts | [int64](#int64) |  | A number of non-fungible tokens (NTFs) owned by the account. |
| max_auto_associations | [int32](#int32) |  | A maximum for the number of tokens that can be automatically associated with this account. &lt;p&gt; If this is less than or equal to `used_auto_associations` (or 0), then this account MUST manually associate with a token before transacting in that token.&lt;br/&gt; This value may also be `-1` to indicate no limit.&lt;br/&gt; This value MUST NOT be less than `-1`. |
| used_auto_associations | [int32](#int32) |  | A count of used auto-association slots. &lt;p&gt; If this is greater than, or equal to, the current value of `max_auto_associations`, then this account MUST manually associate with a new token before transacting in that token. |
| number_associations | [int32](#int32) |  | A count of tokens associated with this account. &lt;p&gt; This value determines a portion of the renewal fee for this account. |
| smart_contract | [bool](#bool) |  | A boolean indicating that this account is owned by a smart contract. |
| number_positive_balances | [int32](#int32) |  | A count of tokens with a positive balance associated with this account. &lt;p&gt; If the account has a positive balance in any token, it SHALL NOT be deleted. |
| ethereum_nonce | [int64](#int64) |  | A nonce of this account for Ethereum interoperability. |
| stake_at_start_of_last_rewarded_period | [int64](#int64) |  | An amount of HBAR staked by this account at the start of the last reward period. |
| auto_renew_account_id | [AccountID](#proto-AccountID) |  | An account identifier for automatic renewal.&lt;br/&gt; This is the identifier of another account, in the same shard and realm as this account, that has signed a transaction allowing the network to use its balance, if needed, to automatically extend this account&#39;s expiration time during automatic renewal processing. &lt;p&gt; If this is set, and this account lack sufficient HBAR balance to pay renewal fees when due, then the network SHALL deduct the necessary fees from the designated auto renew account, if that account has sufficient balance. |
| auto_renew_seconds | [int64](#int64) |  | A count of the number of seconds to extend this account&#39;s expiration. &lt;p&gt; The network SHALL extend the account&#39;s expiration by this number of seconds, if funds are available, upon automatic renewal.&lt;br/&gt; This SHALL NOT apply if the account is already deleted upon expiration.&lt;br/&gt; If this is not provided in an allowed range on account creation, the transaction SHALL fail with INVALID_AUTO_RENEWAL_PERIOD. The default values for the minimum period and maximum period are currently 30 days and 90 days, respectively. |
| contract_kv_pairs_number | [int32](#int32) |  | A count of smart contract key-value pairs.&lt;br/&gt; If this account is a smart-contract, this is the number of key-value pairs stored on the contract. &lt;p&gt; If this account is not a smart contract, this field SHALL NOT be used.&lt;br/&gt; This value SHALL determine a portion of the storage rental fees for the contract. |
| crypto_allowances | [AccountCryptoAllowance](#proto-AccountCryptoAllowance) | repeated | A list of crypto (HBAR) allowances approved by this account. &lt;p&gt; If this is not empty, each allowance SHALL permit a specified &#34;spender&#34; account to spend this account&#39;s HBAR balance, up to a designated limit.&lt;br/&gt; This field SHALL permit spending only HBAR balance, not other tokens the account may hold. Allowances for other tokens SHALL be listed in the `token_allowances` field or the `approve_for_all_nft_allowances` field. |
| approve_for_all_nft_allowances | [AccountApprovalForAllAllowance](#proto-AccountApprovalForAllAllowance) | repeated | A list of non-fungible token (NFT) allowances approved by this account. &lt;p&gt; If this is not empty, each allowance permits a specified &#34;spender&#34; account to transfer _all_ of this account&#39;s non-fungible tokens from a particular collection.&lt;br/&gt; Allowances for a specific serial number MUST be directly associated with that specific non-fungible token, rather than the holding account. |
| token_allowances | [AccountFungibleTokenAllowance](#proto-AccountFungibleTokenAllowance) | repeated | A list of fungible token allowances approved by this account. &lt;p&gt; If this is not empty, each allowance permits a specified &#34;spender&#34; to spend this account&#39;s fungible tokens, of the designated type, up to a designated limit. |
| number_treasury_titles | [uint32](#uint32) |  | A count of tokens for which this account is the treasury account. &lt;p&gt; Each native token is initially created with all tokens held by its treasury, and the owner of that account (which may be a smart contract) determines how those tokens are distributed. |
| expired_and_pending_removal | [bool](#bool) |  | A flag indicating that the account is expired and pending removal. &lt;p&gt; When the network checks for entity expiration, it SHALL set this flag if the account expiration time has past and the account has no HBAR sufficient to pay current renewal fees.&lt;br/&gt; If the account has an auto-renew account set with an HBAR balance that could pay for an auto-renewal, then this flag SHALL NOT be set. This ensures the account is not encumbered during the time between expiration and when the auto-renewal processing renews the account. |
| first_contract_storage_key | [bytes](#bytes) |  | A contract storage key.&lt;br/&gt; This is the first key in the doubly-linked list of this contract&#39;s storage mappings. &lt;p&gt; This value SHALL be empty if the account is not a contract or the contract has no storage mappings. |
| head_pending_airdrop_id | [PendingAirdropId](#proto-PendingAirdropId) |  | A pending airdrop ID.&lt;br/&gt; This is the head of the linked list for this account from the account airdrops map.&lt;br/&gt; &lt;p&gt; The account airdrops SHALL be connected by including the &#34;next&#34; and &#34;previous&#34; `PendingAirdropID` in each `AccountAirdrop` message.&lt;br/&gt; This value SHALL NOT be empty if this account is &#34;sender&#34; for any pending airdrop, and SHALL be empty otherwise. |
| number_pending_airdrops | [uint64](#uint64) |  | A number of pending airdrops. &lt;p&gt; This count SHALL be used to calculate rent _without_ walking the linked list of pending airdrops associated to this account via the `head_pending_airdrop_id` field.&lt;br/&gt; This value MUST be updated for every airdrop, clam, or cancel transaction that designates this account as a receiver.&lt;br/&gt; This number MUST always match the count of entries in the &#34;list&#34; identified by `head_pending_airdrop_id`. |






<a name="proto-AccountApprovalForAllAllowance"></a>

### AccountApprovalForAllAllowance
Permission granted by one account (the &#34;funding&#34; account) to another account
(the &#34;spender&#34; account) that allows the spender to transfer all serial
numbers of a specific non-fungible token (NFT) collection owned by the
funding account.&lt;br/&gt;
This is a broad permission, as it does not matter how many NFTs of the
specified collection the funding account owns, the spender MAY dispose of
any or all of them with this allowance.&lt;br/&gt;
Each token type (typically a collection of NFTs) SHALL require a separate
allowance.&lt;br/&gt;
Allowances for a specific serial number MUST be directly associated with
that specific non-fungible token, rather than the holding account.

An allowance SHALL NOT transfer any tokens directly, it only permits
transactions signed only by the spender account to transfer any non-fungible
tokens of the specified type owned by the funding account.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| token_id | [TokenID](#proto-TokenID) |  | The identifier for the token associated with this allowance. &lt;p&gt; This token MUST be a non-fungible/unique token. |
| spender_id | [AccountID](#proto-AccountID) |  | The identifier for the spending account associated with this allowance. &lt;p&gt; This account SHALL be permitted to sign transactions to spend tokens of the associated token type from the funding/allowing account. |






<a name="proto-AccountCryptoAllowance"></a>

### AccountCryptoAllowance
Permission granted by one account (the &#34;funding&#34; account) to another account
(the &#34;spender&#34; account) that allows the spender to spend a specified amount
of HBAR owned by the funding account.

An allowance SHALL NOT transfer any HBAR directly, it only permits
transactions signed only by the spender account to transfer HBAR, up to the
amount specified, from the funding account.

Once the specified amount is spent, the allowance SHALL be consumed and a
new allowance SHALL be required before that spending account may spend
additional HBAR from the funding account.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| spender_id | [AccountID](#proto-AccountID) |  | The identifier for the spending account associated with this allowance. &lt;p&gt; This account SHALL be permitted to sign transactions to spend HBAR from the funding/allowing account.&lt;br/&gt; This permission SHALL be limited to no more than the specified `amount`. |
| amount | [int64](#int64) |  | The maximum amount that the spender account may transfer within the scope of this allowance. &lt;p&gt; This allowance SHALL be consumed if any combination of transfers authorized via this allowance meet this value in total.&lt;br/&gt; This value MUST be specified in tinybar (i.e. 10&lt;sup&gt;-8&lt;/sup&gt; HBAR). |






<a name="proto-AccountFungibleTokenAllowance"></a>

### AccountFungibleTokenAllowance
Permission granted by one account (the &#34;funding&#34; account) to another account
(the &#34;spender&#34; account) that allows the spender to spend a specified amount
of a specific non-HBAR fungible token from the balance owned by the funding
account.

An allowance SHALL NOT transfer any tokens directly, it only permits
transactions signed only by the spender account to transfer tokens of the
specified type, up to the amount specified, from the funding account.

Once the specified amount is spent, the allowance SHALL be consumed and a
new allowance SHALL be required before that spending account may spend
additional tokens from the funding account.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| token_id | [TokenID](#proto-TokenID) |  | The identifier for the token associated with this allowance. &lt;p&gt; This token MUST be a fungible/common token. |
| spender_id | [AccountID](#proto-AccountID) |  | The identifier for the spending account associated with this allowance. &lt;p&gt; This account SHALL be permitted to sign transactions to spend tokens of the associated token type from the funding/allowing account.&lt;br/&gt; This permission SHALL be limited to no more than the specified `amount`. |
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

