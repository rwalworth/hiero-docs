# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/state/roster/ledger_id.proto](#services_state_roster_ledger_id-proto)
    - [LedgerId](#com-hedera-hapi-node-state-roster-LedgerId)
    - [NodeSignature](#com-hedera-hapi-node-state-roster-NodeSignature)
    - [RosterSignatures](#com-hedera-hapi-node-state-roster-RosterSignatures)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_state_roster_ledger_id-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/state/roster/ledger_id.proto
# Ledger ID
A Ledger ID is a Threshold Signature Scheme (TSS) public key that
identifies the ledger.  A Ledger ID is intended to be long-lived, but
may change under rare circumstances.  The Ledger ID for a particular
distributed ledger can be used to validate the signature of that ledger
in, for example, a block stream &#34;block proof&#34;.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in
[RFC2119](https://www.ietf.org/rfc/rfc2119) and clarified in
[RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="com-hedera-hapi-node-state-roster-LedgerId"></a>

### LedgerId
A ledger identifier.&lt;br/&gt;
This message identifies a ledger and is used to verify ledger
signatures in a Threshold Signature Scheme (TSS).

A ledger identifier SHALL be a public key defined according to the TSS
process.&lt;br/&gt;
A ledger identifier SHOULD NOT change, but MAY do so in rare
circumstances.&lt;br/&gt;
Clients SHOULD always check for the correct ledger identifier, according to
the network roster, before attempting to verify any state proof or other
ledger signature.

### Block Stream Effects
Every block in the Block Stream `BlockProof` SHALL be signed via TSS and
MUST be verified with the ledger identifier current at the _start_ of that
block.
If the ledger identifier changes, the new value MUST be used to validate
Block Proof items after the change.
A change to the ledger identifier SHALL be reported in a State Change for
the block containing that change, which SHALL be verified with the _prior_
ledger identifier.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| ledger_id | [bytes](#bytes) |  | A public key.&lt;br/&gt; This key both identifies the ledger and can be used to verify ledger signatures. &lt;p&gt; This value MUST be set.&lt;br/&gt; This value MUST NOT be empty.&lt;br/&gt; This value MUST contain a valid public key. |
| round | [uint64](#uint64) |  | A round number.&lt;br/&gt; This identifies when this ledger id becomes active.&lt;br/&gt; This value is REQUIRED. |
| ledger_signature | [bytes](#bytes) |  | A signature from the prior ledger key.&lt;br/&gt; This signature is the _previous_ ledger ID signing _this_ ledger ID.&lt;br/&gt; This value MAY be unset, if there is no prior ledger ID.&lt;br/&gt; This value SHOULD be set if a prior ledger ID exists to generate the signature. |
| roster_signatures | [RosterSignatures](#com-hedera-hapi-node-state-roster-RosterSignatures) |  | The signatures from nodes in the active roster signing the new ledger id.&lt;br/&gt; These signatures establish a chain of trust from the network to the new ledger id. &lt;p&gt; This value MUST be present when the ledger signature of a previous ledger id is absent. |






<a name="com-hedera-hapi-node-state-roster-NodeSignature"></a>

### NodeSignature
A pair of a _RSA_ signature and the node id of the node that created the
signature.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| node_id | [uint64](#uint64) |  | The node id of the node that created the _RSA_ signature. This value MUST be set.&lt;br/&gt; This value MUST NOT be empty.&lt;br/&gt; This value is REQUIRED. |
| node_signature | [bytes](#bytes) |  | The bytes of an _RSA_ signature. This value MUST be set.&lt;br/&gt; This value MUST NOT be empty.&lt;br/&gt; This value MUST contain a valid signature. |






<a name="com-hedera-hapi-node-state-roster-RosterSignatures"></a>

### RosterSignatures
A collection of signatures from nodes in a roster.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| roster_hash | [bytes](#bytes) |  | A roster hash for the roster that the node signatures are from. |
| node_signatures | [NodeSignature](#com-hedera-hapi-node-state-roster-NodeSignature) | repeated | A list of node signatures on the same message where all node ids in the NodeSignature objects are from the roster that the roster_hash represents. |





 

 

 

 



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

