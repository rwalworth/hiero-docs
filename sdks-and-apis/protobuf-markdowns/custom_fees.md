# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/custom_fees.proto](#services_custom_fees-proto)
    - [AssessedCustomFee](#proto-AssessedCustomFee)
    - [CustomFee](#proto-CustomFee)
    - [CustomFeeLimit](#proto-CustomFeeLimit)
    - [FeeExemptKeyList](#proto-FeeExemptKeyList)
    - [FixedCustomFee](#proto-FixedCustomFee)
    - [FixedCustomFeeList](#proto-FixedCustomFeeList)
    - [FixedFee](#proto-FixedFee)
    - [FractionalFee](#proto-FractionalFee)
    - [RoyaltyFee](#proto-RoyaltyFee)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_custom_fees-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/custom_fees.proto
# Custom Fees
Fees defined by token creators that are charged as part of each
transfer of that token type.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in
[RFC2119](https://www.ietf.org/rfc/rfc2119) and clarified in
[RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="proto-AssessedCustomFee"></a>

### AssessedCustomFee
Description of a transfer added to a `cryptoTransfer` transaction that
satisfies custom fee requirements.

It is important to note that this is not the actual transfer. The transfer
of value SHALL be merged into the original transaction to minimize the
number of actual transfers. This descriptor presents the fee assessed
separately in the record stream so that the details of the fee assessed
are not hidden in this process.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| amount | [int64](#int64) |  | An amount of tokens assessed for this custom fee. &lt;p&gt; This shall be expressed in units of 10&lt;sup&gt;-decimals&lt;/sup&gt; tokens. |
| token_id | [TokenID](#proto-TokenID) |  | The token transferred to satisfy this fee. &lt;p&gt; If the token transferred is HBAR, this field SHALL NOT be set. |
| fee_collector_account_id | [AccountID](#proto-AccountID) |  | An account that received the fee assessed. &lt;p&gt; This SHALL NOT be the sender or receiver of the original cryptoTransfer transaction. |
| effective_payer_account_id | [AccountID](#proto-AccountID) | repeated | An account that provided the tokens assessed as a fee. &lt;p&gt; This SHALL be the account that _would have_ had a higher balance absent the fee. In most cases this SHALL be the `sender`, but some _fractional_ fees reduce the amount transferred, and in those cases the `receiver` SHALL be the effective payer for the fee.&lt;br/&gt; There are currently no situations where a third party pays a custom fee. This MAY change in a future release. |






<a name="proto-CustomFee"></a>

### CustomFee
A transfer fee to assess during a CryptoTransfer.&lt;br/&gt;
This fee applies to transactions that transfer units of the token to
which the fee is attached. A custom fee may be either fixed or fractional,
and must specify a fee collector account to receive the assessed fees.

Custom fees MUST be greater than zero (0).


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| fixed_fee | [FixedFee](#proto-FixedFee) |  | A fixed fee to be charged to the `sender` for every token transfer. &lt;p&gt; This type of fee MAY be defined for any token type.&lt;br/&gt; This type of fee MAY be more consistent and reliable than other types. |
| fractional_fee | [FractionalFee](#proto-FractionalFee) |  | A fee defined as a fraction of the tokens transferred. &lt;p&gt; This type of fee MUST NOT be defined for a non-fungible/unique token type.&lt;br/&gt; This fee MAY be charged to either sender, as an increase to the amount sent, or receiver, as a reduction to the amount received. |
| royalty_fee | [RoyaltyFee](#proto-RoyaltyFee) |  | A fee charged as royalty for any transfer of a non-fungible/unique token. &lt;p&gt; This type of fee MUST NOT be defined for a fungible/common token type. |
| fee_collector_account_id | [AccountID](#proto-AccountID) |  | The account to receive the custom fee. |
| all_collectors_are_exempt | [bool](#bool) |  | Flag indicating to exempt all custom fee collector accounts for this token type from paying this custom fee when sending tokens. &lt;p&gt; The treasury account for a token, and the account identified by the `fee_collector_account_id` field of this `CustomFee` are always exempt from this custom fee to avoid redundant and unnecessary transfers. If this value is `true` then the account(s) identified in `fee_collector_account_id` for _all_ custom fee definitions for this token type SHALL also be exempt from this custom fee. This behavior is specified in HIP-573. |






<a name="proto-CustomFeeLimit"></a>

### CustomFeeLimit
A maximum custom fee that the user is willing to pay.
&lt;p&gt;
This message is used to specify the maximum custom fee that given user is
willing to pay.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| account_id | [AccountID](#proto-AccountID) |  | A payer account identifier. |
| fees | [FixedFee](#proto-FixedFee) | repeated | The maximum fees that the user is willing to pay for the message. |






<a name="proto-FeeExemptKeyList"></a>

### FeeExemptKeyList
A wrapper for fee exempt key list.&lt;br/&gt;
This wrapper exists to enable an update transaction to differentiate between
a field that is not set and an empty list of keys.
&lt;p&gt;
An _unset_ field of this type SHALL NOT modify existing values.&lt;br/&gt;
A _set_ field of this type with an empty `keys` list SHALL remove any
existing values.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| keys | [Key](#proto-Key) | repeated | A set of keys.&lt;br/&gt; The keys in this list are permitted to submit messages to the topic without paying the topic&#39;s custom fees. &lt;p&gt; If a submit transaction is signed by _any_ key included in this set, custom fees SHALL NOT be charged for that transaction. |






<a name="proto-FixedCustomFee"></a>

### FixedCustomFee
A custom fee definition for a consensus topic.
&lt;p&gt;
This fee definition is specific to an Hedera Consensus Service (HCS) topic
and SHOULD NOT be used in any other context.&lt;br/&gt;
All fields for this message are REQUIRED.&lt;br/&gt;
Only &#34;fixed&#34; fee definitions are supported because there is no basis for
a fractional fee on a consensus submit transaction.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| fixed_fee | [FixedFee](#proto-FixedFee) |  | A fixed custom fee. &lt;p&gt; The amount of HBAR or other token described by this `FixedFee` SHALL be charged to the transction payer for each message submitted to a topic that assigns this consensus custom fee. |
| fee_collector_account_id | [AccountID](#proto-AccountID) |  | A collection account identifier. &lt;p&gt; All amounts collected for this consensus custom fee SHALL be transferred to the account identified by this field. |






<a name="proto-FixedCustomFeeList"></a>

### FixedCustomFeeList
A wrapper around a consensus custom fee list.&lt;br/&gt;
This wrapper exists to enable an update transaction to differentiate between
 a field that is not set and an empty list of custom fees.
&lt;p&gt;
An _unset_ field of this type SHALL NOT modify existing values.&lt;br/&gt;
A _set_ field of this type with an empty `fees` list SHALL remove any
 existing values.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| fees | [FixedCustomFee](#proto-FixedCustomFee) | repeated | A set of custom fee definitions.&lt;br/&gt; These are fees to be assessed for each submit to a topic. |






<a name="proto-FixedFee"></a>

### FixedFee
A fixed fee to assess for each token transfer, regardless of the
amount transferred.&lt;br/&gt;
This fee type describes a fixed fee for each transfer of a token type.

The fee SHALL be charged to the `sender` for the token transfer
transaction.&lt;br/&gt;
This fee MAY be assessed in HBAR, the token type transferred, or any
other token type, as determined by the `denominating_token_id` field.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| amount | [int64](#int64) |  | The amount to assess for each transfer. &lt;p&gt; This value MUST be greater than `0`.&lt;br/&gt; This amount is expressed in units of 10&lt;sup&gt;-decimals&lt;/sup&gt; tokens. |
| denominating_token_id | [TokenID](#proto-TokenID) |  | The token type used to pay the assessed fee. &lt;p&gt; If this is unset, the fee SHALL be assessed in HBAR.&lt;br/&gt; If this is set, the fee SHALL be assessed in the token identified. This MAY be any token type. Custom fees assessed in other token types are more likely to fail, however, and it is RECOMMENDED that token creators denominate custom fees in the transferred token, HBAR, or well documented and closely related token types.&lt;br/&gt; If this value is set to `0.0.0` in the `tokenCreate` transaction, it SHALL be replaced with the `TokenID` of the newly created token. |






<a name="proto-FractionalFee"></a>

### FractionalFee
A descriptor for a fee based on a portion of the tokens transferred.

This fee option describes fees as a fraction of the amount of
fungible/common token(s) transferred.  The fee also describes a minimum
and maximum amount, both of which are OPTIONAL.

This type of fee SHALL be assessed only for fungible/common tokens.&lt;br/&gt;
This type of fee MUST NOT be defined for a non-fungible/unique
token type.&lt;br/&gt;
This fee SHALL be paid with the same type of tokens as those
transferred.&lt;br/&gt;
The fee MAY be subtracted from the transferred tokens, or MAY be assessed
to the sender in addition to the tokens actually transferred, based on
the `net_of_transfers` field.

When a single transaction sends tokens from one sender to multiple
recipients, and the `net_of_transfers` flag is false, the network
SHALL attempt to evenly assess the total fee across all recipients
proportionally. This may be inexact and, particularly when there are
large differences between recipients, MAY result in small deviations
from an ideal &#34;fair&#34; distribution.&lt;br/&gt;
If the sender lacks sufficient tokens to pay fees, or the assessment
of custom fees reduces the net amount transferred to or below zero,
the transaction MAY fail due to insufficient funds to pay all fees.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| fractional_amount | [Fraction](#proto-Fraction) |  | A Fraction of the transferred tokens to assess as a fee.&lt;br/&gt; This value MUST be less than or equal to one.&lt;br/&gt; This value MUST be greater than zero. |
| minimum_amount | [int64](#int64) |  | A minimum fee to charge, in units of 10&lt;sup&gt;-decimals&lt;/sup&gt; tokens. &lt;p&gt; This value is OPTIONAL, with a default of `0` indicating no minimum.&lt;br/&gt; If set, this value MUST be greater than zero.&lt;br/&gt; If set, all transfers SHALL pay at least this amount. |
| maximum_amount | [int64](#int64) |  | A maximum fee to charge, in units of 10&lt;sup&gt;-decimals&lt;/sup&gt; tokens. &lt;p&gt; This value is OPTIONAL, with a default of `0` indicating no maximum.&lt;br/&gt; If set, this value MUST be greater than zero.&lt;br/&gt; If set, any fee charged SHALL NOT exceed this value.&lt;br/&gt; This value SHOULD be strictly greater than `minimum_amount`. If this amount is less than or equal to `minimum_amount`, then the fee charged SHALL always be equal to this value and `fractional_amount` SHALL NOT have any effect. |
| net_of_transfers | [bool](#bool) |  | Flag requesting to assess the calculated fee against the sender, without reducing the amount transferred.&lt;br/&gt; #### Effects of this flag &lt;ol&gt; &lt;li&gt;If this value is true &lt;ul&gt; &lt;li&gt;The receiver of a transfer SHALL receive the entire amount sent.&lt;/li&gt; &lt;li&gt;The fee SHALL be charged to the sender as an additional amount, increasing the token transfer debit.&lt;/li&gt; &lt;/ul&gt; &lt;/li&gt; &lt;li&gt;If this value is false &lt;ul&gt; &lt;li&gt;The receiver of a transfer SHALL receive the amount sent _after_ deduction of the calculated fee.&lt;/li&gt; &lt;/ul&gt; &lt;/li&gt; &lt;/ol&gt; |






<a name="proto-RoyaltyFee"></a>

### RoyaltyFee
A fee to assess during a CryptoTransfer that changes ownership of a
non-fungible/unique (NFT) token.&lt;br/&gt;
This message defines the fraction of the fungible value exchanged for an
NFT that the ledger should collect as a royalty.
&#34;Fungible value&#34; includes both HBAR (ℏ) and units of fungible HTS tokens.
When the NFT sender does not receive any fungible value, the ledger will
assess the fallback fee, if present, to the new NFT owner. Royalty fees
can only be added to non-fungible/unique tokens.

#### Important Note
&gt; Users should be aware that native royalty fees are _strictly_ a
&gt; convenience feature, SHALL NOT be guaranteed, and the network SHALL NOT
&gt; enforce _inescapable_ royalties on the exchange of a unique NFT.&lt;br/&gt;
&gt; For _one_ example, if the counterparties agree to split their value
&gt; transfer and NFT exchange into separate transactions, the network cannot
&gt; possibly determine the value exchanged. Even trustless transactions,
&gt; using a smart contract or other form of escrow, can arrange such split
&gt; transactions as a single _logical_ transfer.

Counterparties that wish to _respect_ creator royalties MUST follow the
pattern the network recognizes.
&lt;div style=&#34;margin-left: 2em; margin-top: -0.8em&#34;&gt;
A single transaction MUST contain all three elements, transfer of the NFT,
debit of fungible value from the receiver, and credit of fungible value to
the sender, in order for the network to accurately assess royalty fees.
&lt;/div&gt;
&lt;div style=&#34;margin-left: 1em; margin-top: -0.8em&#34;&gt;
Two examples are presented here.
&lt;div style=&#34;margin-left: 1em&#34;&gt;
The NFT sender and receiver MUST both sign a single `cryptoTransfer` that
transfers the NFT from sender to receiver, debits the fungible value from
the receiver, and credits the sender with the fungible value the receiver
is exchanging for the NFT.&lt;br/&gt;
A marketplace using an approved spender account for an escrow transaction
MUST credit the account selling the NFT in the same `cryptoTransfer`
transaction that transfers the NFT to, and deducts fungible value from,
the buying account.
&lt;/div&gt;&lt;/div&gt;
This type of fee MAY NOT produce accurate results if multiple transfers
are executed in a single transaction. It is RECOMMENDED that each
NFT subject to royalty fees be transferred separately and without
unrelated fungible token transfers.

The network SHALL NOT consider third-party transfers, including
&#34;approved spender&#34; accounts, in collecting royalty fees. An honest
broker MUST ensure that transfer of an NFT and payment delivered to
the sender are present in the same transaction.
There is an
[open suggestion](https://github.com/hashgraph/hedera-improvement-proposal/discussions/578)
that proposes to broaden the scope of transfers from which the network
automatically collects royalties to cover related third parties. If this
interests or concerns you, please add your voice to that discussion.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| exchange_value_fraction | [Fraction](#proto-Fraction) |  | The fraction of fungible value exchanged for an NFT to collect as royalty. &lt;p&gt; This SHALL be applied once to the total fungible value transferred for the transaction.&lt;br/&gt; There SHALL NOT be any adjustment based on multiple transfers involving the NFT sender as part of a single transaction. |
| fallback_fee | [FixedFee](#proto-FixedFee) |  | A fixed fee to assess if no fungible value is known to be traded for the NFT. &lt;p&gt; If an NFT is transferred without a corresponding transfer of _fungible_ value returned in the same transaction, the network SHALL charge this fee as a fallback.&lt;br/&gt; Fallback fees MAY have unexpected effects when interacting with escrow, market transfers, and smart contracts. It is RECOMMENDED that developers carefully consider possible effects from fallback fees when designing systems that facilitate the transfer of NFTs. |





 

 

 

 



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

