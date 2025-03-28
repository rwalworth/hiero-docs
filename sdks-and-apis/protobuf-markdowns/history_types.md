# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/state/history/history_types.proto](#services_state_history_history_types-proto)
    - [ConstructionNodeId](#com-hedera-hapi-node-state-history-ConstructionNodeId)
    - [History](#com-hedera-hapi-node-state-history-History)
    - [HistoryProof](#com-hedera-hapi-node-state-history-HistoryProof)
    - [HistoryProofConstruction](#com-hedera-hapi-node-state-history-HistoryProofConstruction)
    - [HistoryProofVote](#com-hedera-hapi-node-state-history-HistoryProofVote)
    - [HistorySignature](#com-hedera-hapi-node-state-history-HistorySignature)
    - [ProofKey](#com-hedera-hapi-node-state-history-ProofKey)
    - [ProofKeySet](#com-hedera-hapi-node-state-history-ProofKeySet)
    - [RecordedHistorySignature](#com-hedera-hapi-node-state-history-RecordedHistorySignature)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_state_history_history_types-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/state/history/history_types.proto



<a name="com-hedera-hapi-node-state-history-ConstructionNodeId"></a>

### ConstructionNodeId
A construction-scoped node id.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| construction_id | [uint64](#uint64) |  | The unique id of a history proof construction. |
| node_id | [uint64](#uint64) |  | The unique id of a node. |






<a name="com-hedera-hapi-node-state-history-History"></a>

### History
A piece of new history in the form of an address book hash and
associated metadata.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| address_book_hash | [bytes](#bytes) |  | The address book hash of the new history. |
| metadata | [bytes](#bytes) |  | The metadata associated to the address book. |






<a name="com-hedera-hapi-node-state-history-HistoryProof"></a>

### HistoryProof
A proof that some address book history belongs to the ledger id&#39;s
chain of trust.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| source_address_book_hash | [bytes](#bytes) |  | The hash of the source address book. |
| target_proof_keys | [ProofKey](#com-hedera-hapi-node-state-history-ProofKey) | repeated | The proof keys for the target address book, needed to keep constructing proofs after adopting the target address book&#39;s roster at a handoff. |
| target_history | [History](#com-hedera-hapi-node-state-history-History) |  | The target history of the proof. |
| proof | [bytes](#bytes) |  | The proof of chain of trust from the ledger id. |






<a name="com-hedera-hapi-node-state-history-HistoryProofConstruction"></a>

### HistoryProofConstruction
Summary of the status of constructing a metadata proof, necessary to
ensure deterministic construction ending in a roster with sufficient
weight to enact its own constructions.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| construction_id | [uint64](#uint64) |  | The construction id. |
| source_roster_hash | [bytes](#bytes) |  | The hash of the roster whose weights are used to determine when certain thresholds are during construction. |
| source_proof | [HistoryProof](#com-hedera-hapi-node-state-history-HistoryProof) |  | If set, the proof that the address book of the source roster belongs to the the ledger id&#39;s chain of trust; if not set, the source roster&#39;s address book must *be* the ledger id. |
| target_roster_hash | [bytes](#bytes) |  | The hash of the roster whose weights are used to assess progress toward obtaining proof keys for parties that hold at least a strong minority of the stake in that roster. |
| grace_period_end_time | [proto.Timestamp](#proto-Timestamp) |  | If the network is still gathering proof keys for this construction, the next time at which nodes should stop waiting for tardy proof keys and assembly the history to be proven as soon as it has the associated metadata and proof keys for nodes with &gt;2/3 weight in the target roster. |
| assembly_start_time | [proto.Timestamp](#proto-Timestamp) |  | If the network has gathered enough proof keys to assemble the history for this construction, the cutoff time at which those keys must have been adopted to be included in the final history. |
| target_proof | [HistoryProof](#com-hedera-hapi-node-state-history-HistoryProof) |  | When this construction is complete, the recursive proof that the target roster&#39;s address book and associated metadata belong to the ledger id&#39;s chain of trust. |
| failure_reason | [string](#string) |  | If set, the reason the construction failed. |






<a name="com-hedera-hapi-node-state-history-HistoryProofVote"></a>

### HistoryProofVote
A node&#39;s vote for a particular history proof; either by explicitly
giving the proof, or by identifying a node that already voted for it.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| proof | [HistoryProof](#com-hedera-hapi-node-state-history-HistoryProof) |  | The history proof the submitting node is voting for. |
| congruent_node_id | [uint64](#uint64) |  | The id of another node that already voted for the exact proof the submitting node is voting for. |






<a name="com-hedera-hapi-node-state-history-HistorySignature"></a>

### HistorySignature
A node&#39;s signature blessing some new history.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| history | [History](#com-hedera-hapi-node-state-history-History) |  | The new history the node is signing. |
| signature | [bytes](#bytes) |  | The node&#39;s signature on the canonical serialization of the new history. |






<a name="com-hedera-hapi-node-state-history-ProofKey"></a>

### ProofKey
A record of the proof key a node had in a particular address
book. Necessary to keep at each point history so that nodes
can verify the correct key was used to sign in transitions
starting from the current address book; no matter how keys
have been rotated from the time the address book was created.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| node_id | [uint64](#uint64) |  | The node id. |
| key | [bytes](#bytes) |  | The key. |






<a name="com-hedera-hapi-node-state-history-ProofKeySet"></a>

### ProofKeySet
A set of proof keys for a node; that is, the key the node is
currently using and the key it wants to use in assembling the
next address book in the ledger id&#39;s chain of trust.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| adoption_time | [proto.Timestamp](#proto-Timestamp) |  | The consensus time when the network adopted the active proof key in this set. An adoption time that is sufficiently tardy relative to the latest assembly start time may result in the node&#39;s key being omitted from the address book. |
| key | [bytes](#bytes) |  | The proof key the node is using. |
| next_key | [bytes](#bytes) |  | If set, the proof key the node wants to start using in the address book. |






<a name="com-hedera-hapi-node-state-history-RecordedHistorySignature"></a>

### RecordedHistorySignature
A signature on some new history recorded at a certain time.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| signing_time | [proto.Timestamp](#proto-Timestamp) |  | The time at which the signature was recorded. |
| history_signature | [HistorySignature](#com-hedera-hapi-node-state-history-HistorySignature) |  | The signature on some new history. |





 

 

 

 



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

