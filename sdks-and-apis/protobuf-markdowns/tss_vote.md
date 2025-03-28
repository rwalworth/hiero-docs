# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/auxiliary/tss/tss_vote.proto](#services_auxiliary_tss_tss_vote-proto)
    - [TssVoteTransactionBody](#com-hedera-hapi-services-auxiliary-tss-TssVoteTransactionBody)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_auxiliary_tss_tss_vote-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/auxiliary/tss/tss_vote.proto
# Tss Vote Transaction

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in
[RFC2119](https://www.ietf.org/rfc/rfc2119) and clarified in
[RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="com-hedera-hapi-services-auxiliary-tss-TssVoteTransactionBody"></a>

### TssVoteTransactionBody
A transaction body to vote on the validity of Threshold Signature Scheme
(TSS) Messages for a candidate roster.

- A `TssVoteTransactionBody` MUST identify the hash of the roster containing
  the node generating this TssVote
- A `TssVoteTransactionBody` MUST identify the hash of the roster that the
  TSS messages is for
 - If the candidate roster has received enough yes votes, the candidate
   roster SHALL be adopted.
 - Switching to the candidate roster MUST not happen until enough nodes have
   voted that they have verified a threshold number of TSS messages from the
   active roster.
 - A vote consists of a bit vector of message statuses where each bit
   corresponds to the order of TssMessages as they have come through
   consensus.
 - The threshold for votes to adopt a candidate roster SHALL be at least 1/3
   of the consensus weight of the active roster to ensure that at least 1
   honest node has validated the TSS key material.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| source_roster_hash | [bytes](#bytes) |  | A hash of the roster containing the node generating this TssVote. |
| target_roster_hash | [bytes](#bytes) |  | A hash of the roster that this TssVote is for. |
| ledger_id | [bytes](#bytes) |  | An identifier (and public key) computed from the TssMessages for the target roster. |
| node_signature | [bytes](#bytes) |  | A signature produced by the node. &lt;p&gt; This signature SHALL be produced using the node RSA signing key to sign the ledger_id.&lt;br/&gt; This signature SHALL be used to establish a chain of trust in the ledger id. |
| tss_vote | [bytes](#bytes) |  | A bit vector of message statuses. &lt;p&gt; #### Example &lt;ul&gt;&lt;li&gt;The least significant bit of byte[0] SHALL be the 0th item in the sequence.&lt;/li&gt; &lt;li&gt;The most significant bit of byte[0] SHALL be the 7th item in the sequence.&lt;/li&gt; &lt;li&gt;The least significant bit of byte[1] SHALL be the 8th item in the sequence.&lt;/li&gt; &lt;li&gt;The most significant bit of byte[1] SHALL be the 15th item in the sequence.&lt;/li&gt; &lt;/ul&gt; A bit SHALL be set if the `TssMessage` for the `TssMessageTransaction` with a sequence number matching that bit index has been received, and is valid.&lt;br/&gt; A bit SHALL NOT be set if the `TssMessage` has not been received or was received but not valid. |





 

 

 

 



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

