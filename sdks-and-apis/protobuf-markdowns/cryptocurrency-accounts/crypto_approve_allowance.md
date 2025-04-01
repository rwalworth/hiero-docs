# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/crypto_approve_allowance.proto](#services_crypto_approve_allowance-proto)
    - [CryptoAllowance](#proto-CryptoAllowance)
    - [CryptoApproveAllowanceTransactionBody](#proto-CryptoApproveAllowanceTransactionBody)
    - [NftAllowance](#proto-NftAllowance)
    - [TokenAllowance](#proto-TokenAllowance)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_crypto_approve_allowance-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/crypto_approve_allowance.proto
# Approve Allowance
This transaction body provides a mechanism to add &#34;allowance&#34; entries
for an account. These allowances enable one account to spend or transfer
token balances (for fungible/common tokens), individual tokens (for
non-fungible/unique tokens), or all non-fungible tokens owned by the
account, now or in the future (if `approved_for_all` is set).

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in
[RFC2119](https://www.ietf.org/rfc/rfc2119) and clarified in
[RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="proto-CryptoAllowance"></a>

### CryptoAllowance
An approved allowance of hbar transfers.
This message specifies one allowance for a single, unique, combination
of owner, spender, and amount.

If `owner` is not set, the effective `owner` SHALL be the `payer` for the
enclosing transaction.&lt;br/&gt;
The `spender` MUST be specified and MUST be a valid account.&lt;br/&gt;
The `amount` MUST be a whole number, and SHOULD be greater than `0` unless
this allowance is intended to _remove_ a previously approved allowance.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| owner | [AccountID](#proto-AccountID) |  | An owner account identifier.&lt;br/&gt; This is the account identifier of the account granting an allowance for the `spender` to transfer tokens held by this account. |
| spender | [AccountID](#proto-AccountID) |  | A spender account identifier.&lt;br/&gt; This is the account identifier of the account permitted to transfer tokens held by the `owner`. |
| amount | [int64](#int64) |  | An amount of tinybar (10&lt;sup&gt;-8&lt;/sup&gt; HBAR).&lt;br/&gt; This is the amount of HBAR held by the `owner` that the `spender` is permitted to transfer. &lt;p&gt; This value MUST be a whole number.&lt;br/&gt; This value MUST be greater than 0 to create a new allowance.&lt;br/&gt; This value MAY be exactly `0` to _remove_ an existing allowance.&lt;br/&gt; |






<a name="proto-CryptoApproveAllowanceTransactionBody"></a>

### CryptoApproveAllowanceTransactionBody
Create (&#34;Approve&#34;) allowances for one account to transfer tokens owned
by a different account.&lt;br/&gt;
An allowance permits a &#34;spender&#34; account to independently transfer tokens
owned by a separate &#34;owner&#34; account. Each such allowance permits spending
any amount, up to a specified limit, for fungible/common tokens; a single
specified non-fungible/unique token, or all non-fungible/unique tokens
of a particular token type held by the &#34;owner&#34; account.

If the &#34;owner&#34; account is not specified for any allowance in this
transaction (the `owner` field is not set), the `payer` account for this
transaction SHALL be owner for that allowance.&lt;br/&gt;
Each `owner` account specified in any allowance approved in this
transaction MUST sign this transaction.&lt;br/&gt;
If the `amount` field for any fungible/common allowance in this
transaction is `0`, then that allowance SHOULD match an existing,
previously approved, allowance which SHALL be removed.&lt;br/&gt;
There are three lists in this message. Each list MAY be empty, but
_at least one_ list MUST contain _at least one_ entry.

Example for the `payer` rule.&lt;br/&gt;
 - Given an account `0.0.X` that pays for this transaction, and owner
   is not specified in an allowance of `200` HBAR to spender account
   `0.0.Y`. At consensus the spender account `0.0.Y` will have a new
   allowance to spend `200` HBAR from the balance of account `0.0.X`.

### Block Stream Effects
None


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| cryptoAllowances | [CryptoAllowance](#proto-CryptoAllowance) | repeated | List of hbar allowances approved by the account owner. &lt;p&gt; This list MAY be empty, provided at least one other list is not empty. |
| nftAllowances | [NftAllowance](#proto-NftAllowance) | repeated | List of non-fungible token allowances approved by the account owner. &lt;p&gt; This list MAY be empty, provided at least one other list is not empty. |
| tokenAllowances | [TokenAllowance](#proto-TokenAllowance) | repeated | List of fungible token allowances approved by the account owner. &lt;p&gt; This list MAY be empty, provided at least one other list is not empty. |






<a name="proto-NftAllowance"></a>

### NftAllowance
An approved allowance of non-fungible tokens.&lt;br/&gt;
This type of allowance may permit transfers for one or more individual
unique tokens, or may permit transfers for all unique tokens of the
specified type.

If `owner` is not set, the effective `owner` SHALL be the `payer` for the
enclosing transaction.&lt;br/&gt;
The `spender` MUST be specified and MUST be a valid account.&lt;br/&gt;
If `approve_for_all` is set, then `serial_numbers` SHOULD be empty
and SHALL be ignored.
If `approve_for_all` is unset, then `serial_numbers` MUST NOT be empty.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| tokenId | [TokenID](#proto-TokenID) |  | A token identifier.&lt;br/&gt; This identifies the type of token the `spender` is permitted to transfer from the `owner`. &lt;p&gt; The identified token type MUST be a non-fungible/unique token. |
| owner | [AccountID](#proto-AccountID) |  | An owner account identifier.&lt;br/&gt; This is the account identifier of the account granting an allowance for the `spender` to transfer tokens held by this account. |
| spender | [AccountID](#proto-AccountID) |  | A spender account identifier.&lt;br/&gt; This is the account identifier of the account permitted to transfer tokens held by the `owner`. |
| serial_numbers | [int64](#int64) | repeated | A list of token serial numbers.&lt;br/&gt; The list of serial numbers that the spender is permitted to transfer. &lt;p&gt; The `owner` MUST currently hold each token identified in this list. |
| approved_for_all | [google.protobuf.BoolValue](#google-protobuf-BoolValue) |  | A flag indicating this allowance applies to all tokens of the specified (non-fungible/unique) type. &lt;p&gt; If true, the `spender` SHALL be permitted to transfer any or all of the `owner`&#39;s tokens of the specified token type. This SHALL apply not only to currently owned tokens, but to all such tokens acquired in the future, unless the allowance is `delete`d. |
| delegating_spender | [AccountID](#proto-AccountID) |  | A spender-owner account identifier.&lt;br/&gt; This account identifier identifies a `spender` for whom an existing `approved_for_all` allowance was previously created. This enables an account with such broad access to grant allowances to transfer individual tokens from the original owner without involving that original owner. &lt;p&gt; If this is set, the account identified MUST sign this transaction, but the `owner` account MAY NOT sign this transaction.&lt;br/&gt; If this is set, there MUST exist an active `approved_for_all` allowance from the `owner` for the `delegating_spender` to transfer all tokens of the type identified by the `tokenId` field.&lt;br/&gt; If this value is set, the `approved_for_all` flag MUST be `false`. |






<a name="proto-TokenAllowance"></a>

### TokenAllowance
An approved allowance of fungible/common token transfers.
This message specifies one allowance for a single, unique, combination
of token, owner, spender, and amount.

If `owner` is not set, the effective `owner` SHALL be the `payer` for the
enclosing transaction.&lt;br/&gt;
The `tokenId` MUST be specified and MUST be a valid
fungible/common token type.&lt;br/&gt;
The `spender` MUST be specified and MUST be a valid account.&lt;br/&gt;
The `amount` MUST be a whole number, and SHOULD be greater than `0` unless
this allowance is intended to _remove_ a previously approved allowance.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| tokenId | [TokenID](#proto-TokenID) |  | A token identifier.&lt;br/&gt; This identifies the type of token the `spender` is permitted to transfer from the `owner`. &lt;p&gt; The identified token type MUST be a fungible/common token. |
| owner | [AccountID](#proto-AccountID) |  | An owner account identifier.&lt;br/&gt; This is the account identifier of the account granting an allowance for the `spender` to transfer tokens held by this account. |
| spender | [AccountID](#proto-AccountID) |  | A spender account identifier.&lt;br/&gt; This is the account identifier of the account permitted to transfer tokens held by the `owner`. |
| amount | [int64](#int64) |  | An amount of fractional tokens (10&lt;sup&gt;-decimals&lt;/sup&gt; tokens).&lt;br/&gt; This is the amount of tokens held by the `owner` that the `spender` is permitted to transfer. &lt;p&gt; This value MUST be a whole number.&lt;br/&gt; This value MUST be greater than 0 to create a new allowance.&lt;br/&gt; This value MAY be exactly `0` to _remove_ an existing allowance.&lt;br/&gt; |





 

 

 

 



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

