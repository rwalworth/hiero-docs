# Account

This is a single account within the Hedera network. An Account is the\
primary entity representing ownership of assets tracked on the ledger.\
Account Allowances are also described here, and these represent permission\
granted to an account to transfer assets owned by a different account which\
granted the "allowance". Allowances specify the assets and amounts which may\
be transferred.

A single Account in the Hedera distributed ledger.

* Each Account SHALL have a unique three-part identifier, a Key, and one\
  or more token balances.&#x20;
* Each Account SHALL have an alias, which has multiple forms, and MAY be set automatically.\<br/>\
  Several additional items SHALL be associated with the Account to enable\
  full functionality.
* Assets SHALL be represented as linked-lists with only the "head" item\
  referenced directly in the Account, and the remaining items SHALL be\
  accessible via the token relation or unique tokens maps.
* Accounts, as most items in the network, SHALL have an expiration time,\
  recorded as seconds since the epoch, and MUST be "renewed" for a small fee\
  at expiration. This helps to reduce the amount of inactive accounts retained\
  in state.
* Another account MAY be designated to pay any renewal fees and automatically\
  renew an account for (by default) 30-90 days at a time as a means to\
  optionally ensure important accounts remain active.
* Accounts MAY participate in securing the network by "staking" the account\
  balances to a particular network node, and receive a portion of network\
  fees as a reward. An account MAY optionally decline these rewards but still\
  stake its balances.
* An account MAY optionally require that inbound transfer transactions be\
  signed by that account as receiver\
  (in addition to the sender's signature).
* As with all network entities, Account ID SHALL be represented as\
  shard.realm.X.\<br/>\
  Alias and contractId SHALL be additional identifiers used to connect accounts\
  to transactions before the account is fully enabled,\
  or in EVM contracts.\<br/>

***

## **Alias**

There is considerable complexity with `alias` (aka `evm_address`) for\
Accounts. Much of this comes from the existence of a "hidden" alias for\
almost all accounts, and the reuse of the alias field for both EVM reference\
and "automatic" account creation.

For the purposes of this specification, we will use the following terms for\
clarity.

* `key_alias` is the account public key as a protobuf serialized message\
  and used for auto-creation and subsequent lookup. This is only valid if\
  the account key is a\
  single `primitive` key, either ED25519 or ECDSA\_SECP256K1.
* `evm_address` exists for every account and is one of
  * `contract_address`, which is the 20 byte EVM contract address per\
    EIP-1014
  * `evm_key_address`, which is the keccak-256 hash of a ECDSA\_SECP256K1`primitive` key.
    * This is for accounts lazy-created from EVM public keys, when the\
      corresponding ECDSA\_SECP256K1 public key is presented in a\
      transaction signed by the private key for that public key, the\
      account is created that key assigned, and the protobuf-serialized\
      form is set as the account alias.
  * `long_zero`, is a synthetic 20 byte address inferred for "normally"\
    created accounts. It is constructed from the "standard" AccountID as\
    follows.
    * 4 byte big-endian shard number
    * 8 byte big-endian realm number
    * 8 byte big-endian entity number

The `alias` field in the `Account` message SHALL contain one of four values\
for any given account.

* The `key_alias`, if the account was created by transferring HBAR to the\
  account referenced by `key_alias`.
* The `evm_key_address` if the account was created from an EVM public key
* The `contract_address` if the account belongs to an EVM contract
* Not-Set/null/Bytes.EMPTY (collectively `null`) if the account was\
  created normally

If the `alias` field of an `Account` is any form of `null`, then the account\
MAY be referenced by `alias` in an `AccountID` by using the `long_zero`\
address for the account. This "hidden default" alias SHALL NOT be stored,\
but is synthesized by the node software as needed, and may be synthesized by\
an EVM contract or client software as well.

An AccountID in a transaction MAY reference an `Account` with`shard`.`realm`.`alias`.\<br/>\
If the account `alias` field is set for an Account, that value SHALL be the\
account alias.\<br/>\
If the account `alias` field is not set for an Account, the `long_zero`\
alias SHALL be the account alias.

<table><thead><tr><th width="225.4052734375">Field</th><th width="109.4423828125">Type</th><th width="40">Label</th><th>Description</th></tr></thead><tbody><tr><td>account_id</td><td><a href="account.md#proto-AccountID">AccountID</a></td><td></td><td>The unique ID of this account. &#x3C;p> An account ID, when assigned to this field, SHALL be of the form <code>shard.realm.number</code>.&#x3C;br/> Transactions MAY reference the account by alias, but the account itself MUST always have a purely numeric identifier. This numeric ID is the value used to reference the account in query responses, transaction receipts, transaction records, and the block stream.</td></tr><tr><td>alias</td><td><a href="account.md#bytes">bytes</a></td><td></td><td>An account EVM alias. &#x3C;p> This is a value used in some contexts to reference an account when the numeric account identifier is not available.&#x3C;br/> This field, when set to a non-default value, is immutable and SHALL NOT be changed.</td></tr><tr><td>key</td><td><a href="account.md#proto-Key">Key</a></td><td></td><td>The key to be used to sign transactions from this account, if any. &#x3C;p> This key SHALL NOT be set for hollow accounts until the account is finalized.&#x3C;br/> This key SHALL be set on all other accounts, except for certain immutable accounts (0.0.800 and 0.0.801) necessary for network function and otherwise secured by the governing council.</td></tr><tr><td>expiration_second</td><td><a href="account.md#int64">int64</a></td><td></td><td>The current expiration time of this account, in seconds since the epoch. &#x3C;p> For this purpose, <code>epoch</code> SHALL be the UNIX epoch with 0 at <code>1970-01-01T00:00:00.000Z</code>.&#x3C;br/> This account SHALL be due standard renewal fees when the network consensus time exceeds this time.&#x3C;br/> If rent and expiration are enabled for the network, and automatic renewal is enabled for this account, renewal fees SHALL be charged after this time, and, if charged, the expiration time SHALL be extended for another renewal period.&#x3C;br/> This account MAY be expired and removed from state at any point after this time if not renewed.&#x3C;br/> An account holder MAY extend this time by submitting an account update transaction to modify expiration time, subject to the current maximum expiration time for the network.</td></tr><tr><td>tinybar_balance</td><td><a href="account.md#int64">int64</a></td><td></td><td>The HBAR balance of this account, in tinybar (10^-8 HBAR). This value is a signed integer for efficiency, but MUST always be a whole number.</td></tr><tr><td>memo</td><td><a href="account.md#string">string</a></td><td></td><td>A short description of this account. &#x3C;p> This value, if set, MUST NOT exceed <code>transaction.maxMemoUtf8Bytes</code> (default 100) bytes when encoded as UTF-8.</td></tr><tr><td>deleted</td><td><a href="account.md#bool">bool</a></td><td></td><td>A boolean indicating that this account is deleted.</td></tr><tr><td>staked_to_me</td><td><a href="account.md#int64">int64</a></td><td></td><td>The amount of HBAR staked to this account by others.</td></tr><tr><td>stake_period_start</td><td><a href="account.md#int64">int64</a></td><td></td><td>If this account stakes to another account, this value SHALL be set to the time when the current period for staking and reward calculations began.</td></tr><tr><td>staked_account_id</td><td><a href="account.md#proto-AccountID">AccountID</a></td><td></td><td>An identifier for the account to which this account is staking its balances as a proxy. &#x3C;p> If this account is not currently staking its balances, then this field, if set, SHALL be the sentinel value of <code>0.0.0</code>.</td></tr><tr><td>staked_node_id</td><td><a href="account.md#int64">int64</a></td><td></td><td>An identifier for the node this account is staked to. &#x3C;p> If this account is not currently staking its balances, then this field, if set, SHALL be the sentinel value of <code>-1</code>. Wallet software SHOULD surface staking issues to users and provide a simple mechanism to update staking to a new node ID in the event the prior staked node ID ceases to be valid. &#x3C;p> &#x3C;blockquote>Note: node IDs do fluctuate as node operators change. The Account owner MUST submit a new transaction to change this value if the current node ID changes or ceases to operate as a node. An account with an invalid <code>staked_node_id</code> SHALL NOT participate in staking until the <code>staked_node_id</code> is updated to a valid node ID. &#x3C;/blockquote></td></tr><tr><td>decline_reward</td><td><a href="account.md#bool">bool</a></td><td></td><td>A boolean indicating that this account has chosen to decline rewards for staking its balances. &#x3C;p> This account MAY still stake its balances, but SHALL NOT receive reward payments for doing so.</td></tr><tr><td>receiver_sig_required</td><td><a href="account.md#bool">bool</a></td><td></td><td>A boolean indicating that the account requires a receiver signature for inbound token transfer transactions. &#x3C;p> If this value is <code>true</code> then a transaction to transfer tokens to this account SHALL NOT succeed unless this account has signed the transfer transaction.</td></tr><tr><td>head_token_id</td><td><a href="account.md#proto-TokenID">TokenID</a></td><td></td><td>A token ID at the head of the linked list for this account from the token relations map.&#x3C;br/> The token relations are connected by including the "next" and "previous" TokenID in each TokenRelation message. The "head" item in that list is found by looking up the TokenRelation with this Account's account_id and this head_token_id. Each subsequent item in the list is found via similar lookup with both an AccountID and a TokenID.</td></tr><tr><td>head_nft_id</td><td><a href="account.md#proto-NftID">NftID</a></td><td></td><td>A NftID at the head of the linked list for this account from the unique tokens map.&#x3C;br/> The unique token relations are connected by including the "next" and "previous" NftID in each Nft message. The "head" item in that list is found by looking up the Nft with ID matching this head_nft_id. Each subsequent item in the list is found via similar lookup with the next or previous NftID.</td></tr><tr><td>head_nft_serial_number</td><td><a href="account.md#int64">int64</a></td><td></td><td>A serial number in the NftID at the head of the linked list for this account from unique tokens map. &#x3C;p> This MUST match the <code>serial_number</code> field of <code>head_nft_id</code>.</td></tr><tr><td>number_owned_nfts</td><td><a href="account.md#int64">int64</a></td><td></td><td>A number of non-fungible tokens (NTFs) owned by the account.</td></tr><tr><td>max_auto_associations</td><td><a href="account.md#int32">int32</a></td><td></td><td>A maximum for the number of tokens that can be automatically associated with this account. &#x3C;p> If this is less than or equal to <code>used_auto_associations</code> (or 0), then this account MUST manually associate with a token before transacting in that token.&#x3C;br/> This value may also be <code>-1</code> to indicate no limit.&#x3C;br/> This value MUST NOT be less than <code>-1</code>.</td></tr><tr><td>used_auto_associations</td><td><a href="account.md#int32">int32</a></td><td></td><td>A count of used auto-association slots. &#x3C;p> If this is greater than, or equal to, the current value of <code>max_auto_associations</code>, then this account MUST manually associate with a new token before transacting in that token.</td></tr><tr><td>number_associations</td><td><a href="account.md#int32">int32</a></td><td></td><td>A count of tokens associated with this account. &#x3C;p> This value determines a portion of the renewal fee for this account.</td></tr><tr><td>smart_contract</td><td><a href="account.md#bool">bool</a></td><td></td><td>A boolean indicating that this account is owned by a smart contract.</td></tr><tr><td>number_positive_balances</td><td><a href="account.md#int32">int32</a></td><td></td><td>A count of tokens with a positive balance associated with this account. &#x3C;p> If the account has a positive balance in any token, it SHALL NOT be deleted.</td></tr><tr><td>ethereum_nonce</td><td><a href="account.md#int64">int64</a></td><td></td><td>A nonce of this account for Ethereum interoperability.</td></tr><tr><td>stake_at_start_of_last_rewarded_period</td><td><a href="account.md#int64">int64</a></td><td></td><td>An amount of HBAR staked by this account at the start of the last reward period.</td></tr><tr><td>auto_renew_account_id</td><td><a href="account.md#proto-AccountID">AccountID</a></td><td></td><td>An account identifier for automatic renewal.&#x3C;br/> This is the identifier of another account, in the same shard and realm as this account, that has signed a transaction allowing the network to use its balance, if needed, to automatically extend this account's expiration time during automatic renewal processing. &#x3C;p> If this is set, and this account lack sufficient HBAR balance to pay renewal fees when due, then the network SHALL deduct the necessary fees from the designated auto renew account, if that account has sufficient balance.</td></tr><tr><td>auto_renew_seconds</td><td><a href="account.md#int64">int64</a></td><td></td><td>A count of the number of seconds to extend this account's expiration. &#x3C;p> The network SHALL extend the account's expiration by this number of seconds, if funds are available, upon automatic renewal.&#x3C;br/> This SHALL NOT apply if the account is already deleted upon expiration.&#x3C;br/> If this is not provided in an allowed range on account creation, the transaction SHALL fail with INVALID_AUTO_RENEWAL_PERIOD. The default values for the minimum period and maximum period are currently 30 days and 90 days, respectively.</td></tr><tr><td>contract_kv_pairs_number</td><td><a href="account.md#int32">int32</a></td><td></td><td>A count of smart contract key-value pairs.&#x3C;br/> If this account is a smart-contract, this is the number of key-value pairs stored on the contract. &#x3C;p> If this account is not a smart contract, this field SHALL NOT be used.&#x3C;br/> This value SHALL determine a portion of the storage rental fees for the contract.</td></tr><tr><td>crypto_allowances</td><td><a href="account.md#proto-AccountCryptoAllowance">AccountCryptoAllowance</a></td><td>repeated</td><td>A list of crypto (HBAR) allowances approved by this account. &#x3C;p> If this is not empty, each allowance SHALL permit a specified "spender" account to spend this account's HBAR balance, up to a designated limit.&#x3C;br/> This field SHALL permit spending only HBAR balance, not other tokens the account may hold. Allowances for other tokens SHALL be listed in the <code>token_allowances</code> field or the <code>approve_for_all_nft_allowances</code> field.</td></tr><tr><td>approve_for_all_nft_allowances</td><td><a href="account.md#proto-AccountApprovalForAllAllowance">AccountApprovalForAllAllowance</a></td><td>repeated</td><td>A list of non-fungible token (NFT) allowances approved by this account. &#x3C;p> If this is not empty, each allowance permits a specified "spender" account to transfer <em>all</em> of this account's non-fungible tokens from a particular collection.&#x3C;br/> Allowances for a specific serial number MUST be directly associated with that specific non-fungible token, rather than the holding account.</td></tr><tr><td>token_allowances</td><td><a href="account.md#proto-AccountFungibleTokenAllowance">AccountFungibleTokenAllowance</a></td><td>repeated</td><td>A list of fungible token allowances approved by this account. &#x3C;p> If this is not empty, each allowance permits a specified "spender" to spend this account's fungible tokens, of the designated type, up to a designated limit.</td></tr><tr><td>number_treasury_titles</td><td><a href="account.md#uint32">uint32</a></td><td></td><td>A count of tokens for which this account is the treasury account. &#x3C;p> Each native token is initially created with all tokens held by its treasury, and the owner of that account (which may be a smart contract) determines how those tokens are distributed.</td></tr><tr><td>expired_and_pending_removal</td><td><a href="account.md#bool">bool</a></td><td></td><td>A flag indicating that the account is expired and pending removal. &#x3C;p> When the network checks for entity expiration, it SHALL set this flag if the account expiration time has past and the account has no HBAR sufficient to pay current renewal fees.&#x3C;br/> If the account has an auto-renew account set with an HBAR balance that could pay for an auto-renewal, then this flag SHALL NOT be set. This ensures the account is not encumbered during the time between expiration and when the auto-renewal processing renews the account.</td></tr><tr><td>first_contract_storage_key</td><td><a href="account.md#bytes">bytes</a></td><td></td><td>A contract storage key.&#x3C;br/> This is the first key in the doubly-linked list of this contract's storage mappings. &#x3C;p> This value SHALL be empty if the account is not a contract or the contract has no storage mappings.</td></tr><tr><td>head_pending_airdrop_id</td><td><a href="account.md#proto-PendingAirdropId">PendingAirdropId</a></td><td></td><td>A pending airdrop ID.&#x3C;br/> This is the head of the linked list for this account from the account airdrops map.&#x3C;br/> &#x3C;p> The account airdrops SHALL be connected by including the "next" and "previous" <code>PendingAirdropID</code> in each <code>AccountAirdrop</code> message.&#x3C;br/> This value SHALL NOT be empty if this account is "sender" for any pending airdrop, and SHALL be empty otherwise.</td></tr><tr><td>number_pending_airdrops</td><td><a href="account.md#uint64">uint64</a></td><td></td><td>A number of pending airdrops. &#x3C;p> This count SHALL be used to calculate rent <em>without</em> walking the linked list of pending airdrops associated to this account via the <code>head_pending_airdrop_id</code> field.&#x3C;br/> This value MUST be updated for every airdrop, clam, or cancel transaction that designates this account as a receiver.&#x3C;br/> This number MUST always match the count of entries in the "list" identified by <code>head_pending_airdrop_id</code>.</td></tr></tbody></table>

#### AccountApprovalForAllAllowance

Permission granted by one account (the "funding" account) to another account\
(the "spender" account) that allows the spender to transfer all serial\
numbers of a specific non-fungible token (NFT) collection owned by the\
funding account.\<br/>\
This is a broad permission, as it does not matter how many NFTs of the\
specified collection the funding account owns, the spender MAY dispose of\
any or all of them with this allowance.\<br/>\
Each token type (typically a collection of NFTs) SHALL require a separate\
allowance.\<br/>\
Allowances for a specific serial number MUST be directly associated with\
that specific non-fungible token, rather than the holding account.

An allowance SHALL NOT transfer any tokens directly, it only permits\
transactions signed only by the spender account to transfer any non-fungible\
tokens of the specified type owned by the funding account.

| Field       | Type                                    | Label | Description                                                                                                                                                                                                       |
| ----------- | --------------------------------------- | ----- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| token\_id   | [TokenID](account.md#proto-TokenID)     |       | The identifier for the token associated with this allowance. \<p> This token MUST be a non-fungible/unique token.                                                                                                 |
| spender\_id | [AccountID](account.md#proto-AccountID) |       | The identifier for the spending account associated with this allowance. \<p> This account SHALL be permitted to sign transactions to spend tokens of the associated token type from the funding/allowing account. |

#### AccountCryptoAllowance

Permission granted by one account (the "funding" account) to another account\
(the "spender" account) that allows the spender to spend a specified amount\
of HBAR owned by the funding account.

An allowance SHALL NOT transfer any HBAR directly, it only permits\
transactions signed only by the spender account to transfer HBAR, up to the\
amount specified, from the funding account.

Once the specified amount is spent, the allowance SHALL be consumed and a\
new allowance SHALL be required before that spending account may spend\
additional HBAR from the funding account.

| Field       | Type                                    | Label | Description                                                                                                                                                                                                                                                                                             |
| ----------- | --------------------------------------- | ----- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| spender\_id | [AccountID](account.md#proto-AccountID) |       | The identifier for the spending account associated with this allowance. \<p> This account SHALL be permitted to sign transactions to spend HBAR from the funding/allowing account.\<br/> This permission SHALL be limited to no more than the specified `amount`.                                       |
| amount      | [int64](account.md#int64)               |       | The maximum amount that the spender account may transfer within the scope of this allowance. \<p> This allowance SHALL be consumed if any combination of transfers authorized via this allowance meet this value in total.\<br/> This value MUST be specified in tinybar (i.e. 10\<sup>-8\</sup> HBAR). |

#### AccountFungibleTokenAllowance

Permission granted by one account (the "funding" account) to another account\
(the "spender" account) that allows the spender to spend a specified amount\
of a specific non-HBAR fungible token from the balance owned by the funding\
account.

An allowance SHALL NOT transfer any tokens directly, it only permits\
transactions signed only by the spender account to transfer tokens of the\
specified type, up to the amount specified, from the funding account.

Once the specified amount is spent, the allowance SHALL be consumed and a\
new allowance SHALL be required before that spending account may spend\
additional tokens from the funding account.

| Field       | Type                                    | Label | Description                                                                                                                                                                                                                                                                                                                                             |
| ----------- | --------------------------------------- | ----- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| token\_id   | [TokenID](account.md#proto-TokenID)     |       | The identifier for the token associated with this allowance. \<p> This token MUST be a fungible/common token.                                                                                                                                                                                                                                           |
| spender\_id | [AccountID](account.md#proto-AccountID) |       | The identifier for the spending account associated with this allowance. \<p> This account SHALL be permitted to sign transactions to spend tokens of the associated token type from the funding/allowing account.\<br/> This permission SHALL be limited to no more than the specified `amount`.                                                        |
| amount      | [int64](account.md#int64)               |       | The maximum amount that the spender account may transfer within the scope of this allowance. \<p> This allowance SHALL be consumed if any combination of transfers authorized via this allowance meet this value in total.\<br/> This value MUST be specified in the smallest units of the relevant token (i.e. 10\<sup>-decimals\</sup> whole tokens). |

### Scalar Value Types

| .proto Type | Notes                                                                                                                                           | C++    | Java       | Python      | Go      | C#         | PHP            | Ruby                           |
| ----------- | ----------------------------------------------------------------------------------------------------------------------------------------------- | ------ | ---------- | ----------- | ------- | ---------- | -------------- | ------------------------------ |
| double      |                                                                                                                                                 | double | double     | float       | float64 | double     | float          | Float                          |
| float       |                                                                                                                                                 | float  | float      | float       | float32 | float      | float          | Float                          |
| int32       | Uses variable-length encoding. Inefficient for encoding negative numbers – if your field is likely to have negative values, use sint32 instead. | int32  | int        | int         | int32   | int        | integer        | Bignum or Fixnum (as required) |
| int64       | Uses variable-length encoding. Inefficient for encoding negative numbers – if your field is likely to have negative values, use sint64 instead. | int64  | long       | int/long    | int64   | long       | integer/string | Bignum                         |
| uint32      | Uses variable-length encoding.                                                                                                                  | uint32 | int        | int/long    | uint32  | uint       | integer        | Bignum or Fixnum (as required) |
| uint64      | Uses variable-length encoding.                                                                                                                  | uint64 | long       | int/long    | uint64  | ulong      | integer/string | Bignum or Fixnum (as required) |
| sint32      | Uses variable-length encoding. Signed int value. These more efficiently encode negative numbers than regular int32s.                            | int32  | int        | int         | int32   | int        | integer        | Bignum or Fixnum (as required) |
| sint64      | Uses variable-length encoding. Signed int value. These more efficiently encode negative numbers than regular int64s.                            | int64  | long       | int/long    | int64   | long       | integer/string | Bignum                         |
| fixed32     | Always four bytes. More efficient than uint32 if values are often greater than 2^28.                                                            | uint32 | int        | int         | uint32  | uint       | integer        | Bignum or Fixnum (as required) |
| fixed64     | Always eight bytes. More efficient than uint64 if values are often greater than 2^56.                                                           | uint64 | long       | int/long    | uint64  | ulong      | integer/string | Bignum                         |
| sfixed32    | Always four bytes.                                                                                                                              | int32  | int        | int         | int32   | int        | integer        | Bignum or Fixnum (as required) |
| sfixed64    | Always eight bytes.                                                                                                                             | int64  | long       | int/long    | int64   | long       | integer/string | Bignum                         |
| bool        |                                                                                                                                                 | bool   | boolean    | boolean     | bool    | bool       | boolean        | TrueClass/FalseClass           |
| string      | A string must always contain UTF-8 encoded or 7-bit ASCII text.                                                                                 | string | String     | str/unicode | string  | string     | string         | String (UTF-8)                 |
| bytes       | May contain any arbitrary sequence of bytes.                                                                                                    | string | ByteString | str         | \[]byte | ByteString | string         | String (ASCII-8BIT)            |
