# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/state/hints/hints_types.proto](#services_state_hints_hints_types-proto)
    - [CRSState](#com-hedera-hapi-node-state-hints-CRSState)
    - [HintsConstruction](#com-hedera-hapi-node-state-hints-HintsConstruction)
    - [HintsKeySet](#com-hedera-hapi-node-state-hints-HintsKeySet)
    - [HintsPartyId](#com-hedera-hapi-node-state-hints-HintsPartyId)
    - [HintsScheme](#com-hedera-hapi-node-state-hints-HintsScheme)
    - [NodePartyId](#com-hedera-hapi-node-state-hints-NodePartyId)
    - [PreprocessedKeys](#com-hedera-hapi-node-state-hints-PreprocessedKeys)
    - [PreprocessingVote](#com-hedera-hapi-node-state-hints-PreprocessingVote)
    - [PreprocessingVoteId](#com-hedera-hapi-node-state-hints-PreprocessingVoteId)
  
    - [CRSStage](#com-hedera-hapi-node-state-hints-CRSStage)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_state_hints_hints_types-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/state/hints/hints_types.proto



<a name="com-hedera-hapi-node-state-hints-CRSState"></a>

### CRSState
The state of a CRS construction.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| crs | [bytes](#bytes) |  | The bytes of the CRS. Based on the CRSStage, this may be the initial CRS or the final CRS. |
| stage | [CRSStage](#com-hedera-hapi-node-state-hints-CRSStage) |  | The stage of the CRS construction. |
| next_contributing_node_id | [google.protobuf.UInt64Value](#google-protobuf-UInt64Value) |  | The id of the next node that should contribute to the CRS. This is used to ensure that all nodes contribute to the CRS in a round-robin fashion. If this is null, then all nodes in the network have contributed to the CRS. |
| contribution_end_time | [proto.Timestamp](#proto-Timestamp) |  | The time at which the network should stop waiting for the node&#39;s contributions and move on to the next node in the round-robin fashion. |






<a name="com-hedera-hapi-node-state-hints-HintsConstruction"></a>

### HintsConstruction
A summary of progress in constructing a hinTS scheme.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| construction_id | [uint64](#uint64) |  | The id of the construction. |
| source_roster_hash | [bytes](#bytes) |  | The hash of the roster whose weights are used to determine when the &gt;=1/3 weight signing threshold is reached. |
| target_roster_hash | [bytes](#bytes) |  | The hash of the roster whose weights are used to determine when the &gt;2/3 weight availability threshold is reached. |
| grace_period_end_time | [proto.Timestamp](#proto-Timestamp) |  | If the network is still gathering hinTS keys for this construction, the time at which honest nodes should stop waiting for tardy publications and begin preprocessing as soon as there are valid hinTS keys for nodes with &gt;2/3 weight in the target roster. |
| preprocessing_start_time | [proto.Timestamp](#proto-Timestamp) |  | If the network has gathered enough hinTS keys for this construction to begin preprocessing, the cutoff time by which keys must have been adopted to be included as input to the preprocessing algorithm. |
| hints_scheme | [HintsScheme](#com-hedera-hapi-node-state-hints-HintsScheme) |  | If set, the completed hinTS scheme. |






<a name="com-hedera-hapi-node-state-hints-HintsKeySet"></a>

### HintsKeySet
A set of hinTS keys submitted by a node.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| node_id | [uint64](#uint64) |  | The id of the node submitting these keys. |
| adoption_time | [proto.Timestamp](#proto-Timestamp) |  | The consensus time at which the network adopted the active hinTS key in this set. |
| key | [bytes](#bytes) |  | The party&#39;s active hinTS key. |
| next_key | [bytes](#bytes) |  | If set, the new hinTS key the node wants to use when the next construction begins. |






<a name="com-hedera-hapi-node-state-hints-HintsPartyId"></a>

### HintsPartyId
The id of a party in a hinTS scheme with a certain
number of parties.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| party_id | [uint32](#uint32) |  | The party id, in the range [0, num_parties). |
| num_parties | [uint32](#uint32) |  | The number of parties in the hinTS scheme. |






<a name="com-hedera-hapi-node-state-hints-HintsScheme"></a>

### HintsScheme
The information constituting the hinTS scheme Hiero TSS.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| preprocessed_keys | [PreprocessedKeys](#com-hedera-hapi-node-state-hints-PreprocessedKeys) |  | The aggregation and verification keys for the scheme. |
| node_party_ids | [NodePartyId](#com-hedera-hapi-node-state-hints-NodePartyId) | repeated | The final party ids assigned to each node in the target roster. |






<a name="com-hedera-hapi-node-state-hints-NodePartyId"></a>

### NodePartyId
A node&#39;s hinTS party id.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| node_id | [uint64](#uint64) |  | The node id. |
| party_id | [uint32](#uint32) |  | The party id. |






<a name="com-hedera-hapi-node-state-hints-PreprocessedKeys"></a>

### PreprocessedKeys
The output of the hinTS preprocessing algorithm; that is, a
linear-size aggregation key and a succinct verification key.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| aggregation_key | [bytes](#bytes) |  | The aggregation key for the hinTS scheme |
| verification_key | [bytes](#bytes) |  | The succinct verification key for the hinTS scheme. |






<a name="com-hedera-hapi-node-state-hints-PreprocessingVote"></a>

### PreprocessingVote
A node&#39;s vote for the consensus output of a hinTS preprocessing
algorithm.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| preprocessed_keys | [PreprocessedKeys](#com-hedera-hapi-node-state-hints-PreprocessedKeys) |  | The preprocessed keys this node is voting for. |
| congruent_node_id | [uint64](#uint64) |  | The id of any node that already voted for the exact keys that this node wanted to vote for. |






<a name="com-hedera-hapi-node-state-hints-PreprocessingVoteId"></a>

### PreprocessingVoteId
The id for a node&#39;s vote for the output of the
preprocessing output of a hinTS construction.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| construction_id | [uint64](#uint64) |  | The construction this vote is connected to. |
| node_id | [uint64](#uint64) |  | The id of the node submitting the vote. |





 


<a name="com-hedera-hapi-node-state-hints-CRSStage"></a>

### CRSStage
The stage of a CRS construction.

| Name | Number | Description |
| ---- | ------ | ----------- |
| GATHERING_CONTRIBUTIONS | 0 | The network is gathering contributions to the CRS from all nodes. |
| WAITING_FOR_ADOPTING_FINAL_CRS | 1 | The network is waiting for some grace period to allow the verification future to be completed after the last node has contributed to the CRS. |
| COMPLETED | 2 | The network has completed the CRS construction and is set in the CrsState. |


 

 

 



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

