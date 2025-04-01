# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/state/token/token_relation.proto](#services_state_token_token_relation-proto)
    - [TokenRelation](#proto-TokenRelation)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_state_token_token_relation-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/state/token/token_relation.proto
# Token Relationship.
This is a connection between one Account, one _fungible_ Token, and
associated balance within the Hedera network.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in [RFC2119](https://www.ietf.org/rfc/rfc2119)
and clarified in [RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="proto-TokenRelation"></a>

### TokenRelation
An Hedera Token Service token relationship.

A token relationship connects an Account with a Token and is necessary for
that Account to transact in that Token. TokenRelationship defines a
connection between one account and one token type.

A TokenRelation SHALL be identified by the combination of token_id and
account_id.&lt;br/&gt;
A TokenRelation SHALL contain, for the referenced token,&lt;br/&gt;
The account&#39;s current balance, whether the account has KYC granted,
and whether the assets are frozen.

TokenRelation entries SHALL be connected via a &#34;virtual linked list&#34; with the
next TokenID and previous TokenID stored in the TokenRelation.
These TokenIDs MUST be combined with the AccountID to find the next or
previous relationship in the list.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| token_id | [TokenID](#proto-TokenID) |  | A token identifier. &lt;p&gt; This SHALL identify the token involved in this association. |
| account_id | [AccountID](#proto-AccountID) |  | An account identifier. &lt;p&gt; This SHALL identify the account involved in this association. |
| balance | [int64](#int64) |  | The fungible token balance of this token relationship. &lt;p&gt; This MUST be a whole number. |
| frozen | [bool](#bool) |  | A flag indicating that this token relationship is frozen. &lt;p&gt; When a token relationship is frozen the associated account SHALL NOT be permitted to transfer to or from the associated balance. &lt;p&gt; This flag is associated with the Token value `freeze_key`, and any transaction to set this flag MUST be signed by that key. If the Token does not have a `freeze_key` set, then this flag SHALL NOT be set true for relationships between accounts and that token. |
| kyc_granted | [bool](#bool) |  | A flag indicating that this token relationship has been granted KYC status. &lt;p&gt; If the token flag `accounts_kyc_granted_by_default` is set true, then this flag SHALL be set true for all accounts subsequently associated to that token. Otherwise this flag SHALL NOT be set until a transaction is submitted, and signed with the Token `kyc_key` to set the flag true.&lt;br/&gt; If the Token does not have a `kyc_key` set and the token flag `accounts_kyc_granted_by_default` is not set true, then this value MUST be false for all accounts subsequently associated to that token. &lt;p&gt; Typically a transaction to set this value to true is considered equivalent to asserting that the &#34;Know Your Customer&#34; (KYC) requirements have been met for this combination of account and token and the relevant records are available as required. |
| automatic_association | [bool](#bool) |  | A flag indicating that this token relationship was created using automatic association. &lt;p&gt; If this is true then there MUST NOT exist a customer-signed transaction associating this account and token combination and the account `used_auto_associations` SHALL be incremented when this relationship is created. |
| previous_token | [TokenID](#proto-TokenID) |  | The Token ID of the previous entry in the associated Account&#39;s &#34;virtual double-linked list&#34; of token relationships. &lt;p&gt; This must be combined with the value of `account_id` to identify the actual `TokenRelation` referenced. |
| next_token | [TokenID](#proto-TokenID) |  | The Token ID of the next entry in the associated Account&#39;s &#34;virtual double-linked list&#34; of token relationships. &lt;p&gt; This must be combined with the value of `account_id` to identify the actual `TokenRelation` referenced. |





 

 

 

 



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

