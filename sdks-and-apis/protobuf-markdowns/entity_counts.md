# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/state/entity/entity_counts.proto](#services_state_entity_entity_counts-proto)
    - [EntityCounts](#com-hedera-hapi-node-state-entity-EntityCounts)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_state_entity_entity_counts-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/state/entity/entity_counts.proto



<a name="com-hedera-hapi-node-state-entity-EntityCounts"></a>

### EntityCounts
Representation of a Hedera Entity Service entity counts in the network Merkle tree.

This message is used to store the counts of various entities in the network.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| num_accounts | [uint64](#uint64) |  | The number of accounts in the network. |
| num_aliases | [uint64](#uint64) |  | The number of aliases in the network. |
| num_tokens | [uint64](#uint64) |  | The number of tokens in the network. |
| num_token_relations | [uint64](#uint64) |  | The number of token relationships in the network. |
| num_nfts | [uint64](#uint64) |  | The number of NFTs in the network. |
| num_airdrops | [uint64](#uint64) |  | The number of airdrops in the network. |
| num_staking_infos | [uint64](#uint64) |  | The number of staking infos in the network. |
| num_topics | [uint64](#uint64) |  | The number of topics in the network. |
| num_files | [uint64](#uint64) |  | The number of files in the network. |
| num_nodes | [uint64](#uint64) |  | The number of nodes in the network. |
| num_schedules | [uint64](#uint64) |  | The number of schedules in the network. |
| num_contract_storage_slots | [uint64](#uint64) |  | The number of contract storage slots in the network. |
| num_contract_bytecodes | [uint64](#uint64) |  | The number of contract bytecodes in the network. |





 

 

 

 



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

