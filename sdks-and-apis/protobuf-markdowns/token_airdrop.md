# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/token_airdrop.proto](#services_token_airdrop-proto)
    - [TokenAirdropTransactionBody](#proto-TokenAirdropTransactionBody)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_token_airdrop-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/token_airdrop.proto
# Token Airdrop
Messages used to implement a transaction to &#34;airdrop&#34; tokens.&lt;br/&gt;
An &#34;airdrop&#34; is a distribution of tokens from a funding account
to one or more recipient accounts, ideally with no action required
by the recipient account(s).

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in
[RFC2119](https://www.ietf.org/rfc/rfc2119) and clarified in
[RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="proto-TokenAirdropTransactionBody"></a>

### TokenAirdropTransactionBody
Airdrop one or more tokens to one or more accounts.

### Effects
This distributes tokens from the balance of one or more sending account(s)
to the balance of one or more recipient accounts. Accounts MAY receive the
tokens in one of four ways.

 - An account already associated to the token to be distributed SHALL
   receive the airdropped tokens immediately to the recipient account
   balance.&lt;br/&gt;
   The fee for this transfer SHALL include the transfer, the airdrop fee,
   and any custom fees.
 - An account with available automatic association slots SHALL be
   automatically associated to the token, and SHALL immediately receive
   the airdropped tokens to the recipient account balance.&lt;br/&gt;
   The fee for this transfer SHALL include the transfer, the association,
   the cost to renew that association once, the airdrop fee, and
   any custom fees.
 - An account with &#34;receiver signature required&#34; set SHALL have a
   &#34;Pending Airdrop&#34; created and must claim that airdrop with a
   `claimAirdrop` transaction.&lt;br/&gt;
   The fee for this transfer SHALL include the transfer, the association,
   the cost to renew that association once, the airdrop fee, and
   any custom fees.&lt;br/&gt;
   If the pending airdrop is not claimed immediately, the `sender` SHALL
   pay the cost to renew the token association, and the cost to maintain
   the pending airdrop, until the pending airdrop is claimed or cancelled.
 - An account with no available automatic association slots SHALL have a
   &#34;Pending Airdrop&#34; created and must claim that airdrop with a
   `claimAirdrop` transaction.&lt;br/&gt;
   The fee for this transfer SHALL include the transfer, the association,
   the cost to renew that association once, the airdrop fee, and any custom
   fees.&lt;br/&gt;
   If the pending airdrop is not claimed immediately, the `sender` SHALL
   pay the cost to renew the token association, and the cost to maintain
   the pending airdrop, until the pending airdrop is claimed or cancelled.

If an airdrop would create a pending airdrop for a fungible/common token,
and a pending airdrop for the same sender, receiver, and token already
exists, the existing pending airdrop SHALL be updated to add the new
amount to the existing airdrop, rather than creating
a new pending airdrop.&lt;br/&gt;
Any airdrop that completes immediately SHALL be irreversible. Any airdrop
that results in a &#34;Pending Airdrop&#34; MAY be canceled via a `cancelAirdrop`
transaction.&lt;br/&gt;
All transfer fees (including custom fees and royalties), as well as the
rent cost for the first auto-renewal period for any automatic-association
slot occupied by the airdropped tokens, SHALL be charged to the account
paying for this transaction.&lt;br/&gt;

### Block Stream Effects
- Each successful transfer SHALL be recorded in `token_transfer_list`
  for the transaction record.
- Each successful transfer that consumes an automatic association slot
  SHALL populate the `automatic_association` field for the record.
- Each pending transfer _created_ SHALL be added to the
  `pending_airdrops` field for the record.
- Each pending transfer _updated_ SHALL be added to the
  `pending_airdrops` field for the record.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| token_transfers | [TokenTransferList](#proto-TokenTransferList) | repeated | A list of token transfers representing one or more airdrops. &lt;p&gt; The sender for each transfer MUST have sufficient balance to complete the transfers.&lt;br/&gt; All token transfers MUST successfully transfer tokens or create a pending airdrop for this transaction to succeed.&lt;br/&gt; This list MUST contain between 1 and 10 transfers, inclusive. &lt;p&gt; Note that each transfer of fungible/common tokens requires both a debit and a credit, so each _fungible_ token transfer MUST have _balanced_ entries in the TokenTransferList for that transfer. |





 

 

 

 



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

