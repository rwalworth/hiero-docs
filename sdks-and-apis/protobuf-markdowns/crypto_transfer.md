# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/crypto_transfer.proto](#services_crypto_transfer-proto)
    - [CryptoTransferTransactionBody](#proto-CryptoTransferTransactionBody)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_crypto_transfer-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/crypto_transfer.proto
# Crypto Transfer
Transaction to transfer HBAR between accounts, or between accounts and
smart contracts.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in
[RFC2119](https://www.ietf.org/rfc/rfc2119) and clarified in
[RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="proto-CryptoTransferTransactionBody"></a>

### CryptoTransferTransactionBody
Transfer HBAR and/or other tokens among two or more accounts and/or smart
contracts.

Transfers of HBAR or fungible/common tokens in this transaction are
structured as a &#34;double-entry&#34; transfer list which debits one or more
accounts, and separately credits one or more accounts. Each such transfer
list may specify up to 10 individual credits or debits.&lt;br/&gt;
Transfers of non-fungible/unique tokens in this transaction are
structured as a &#34;single-entry&#34; transfer list, which both debits one account
and credits another account in a single entry.

At least one transfer MUST be present, this MAY be an HBAR transfer in
`transfers`, or MAY be a token transfer in `tokenTransfers`.&lt;br/&gt;
Either `transfers` or `tokenTransfers` MAY be unset, provided the other
is set and not empty.&lt;br/&gt;
If any one account with a debit in any transfer list holds insufficient
balance to complete the transfer, the entire transaction SHALL fail, and
all transfers SHALL NOT be completed.&lt;br/&gt;
If any one account that is _sending_ an individual non-fungible/unique (NFT)
token does not currently hold that unique NFT, the entire transaction SHALL
FAIL, and all transfers SHALL NOT be completed.
The transaction fee SHALL be charged for a transaction that fails due to
insufficient balance or not holding the NFT to be transferred.&lt;br/&gt;
Each account with any debit amounts in any transfer list MUST sign this
transaction.&lt;br/&gt;
Each account with any credit amounts in any transfer list that also has the
`receiverSigRequired` flag set MUST sign this transaction.

### Block Stream Effects
All debits and credits completed by this transaction SHALL be included in
the transaction result transfer list.&lt;br/&gt;
Multiple fungible/common debits from one account, or credits to one account,
MAY be consolidated to a single debit or credit entry in the
transaction result.&lt;br/&gt;
Multiple non-fungible/unique transfers SHALL NOT be consolidated in the
transaction result.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| transfers | [TransferList](#proto-TransferList) |  | A list of HBAR transfers. &lt;p&gt; Each transfer in this list MUST be denominated in tinybar. |
| tokenTransfers | [TokenTransferList](#proto-TokenTransferList) | repeated | One or more lists of token transfers. &lt;p&gt; This list MUST NOT contain more than 10 entries.&lt;br/&gt; If custom fees must be charged, the fee SHALL be assessed against the effective &#34;payer&#34; for this transaction.&lt;br/&gt; If the effective &#34;payer&#34; for this transaction lacks sufficient balance to pay custom fees assessed, the entire transaction SHALL fail with a response code `INSUFFICIENT_PAYER_BALANCE_FOR_CUSTOM_FEE`. |





 

 

 

 



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

