# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [streams/record_stream_file.proto](#streams_record_stream_file-proto)
    - [RecordStreamFile](#proto-RecordStreamFile)
    - [RecordStreamItem](#proto-RecordStreamItem)
    - [SidecarMetadata](#proto-SidecarMetadata)
  
    - [SidecarType](#proto-SidecarType)
  
- [Scalar Value Types](#scalar-value-types)



<a name="streams_record_stream_file-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## streams/record_stream_file.proto



<a name="proto-RecordStreamFile"></a>

### RecordStreamFile
RecordStreamFile is used to serialize all RecordStreamItems that are part of the
same period into record stream files.
This structure represents a block in Hedera (HIP-415).


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| hapi_proto_version | [SemanticVersion](#proto-SemanticVersion) |  | Version of HAPI that was used to serialize the file. |
| start_object_running_hash | [HashObject](#proto-HashObject) |  | Running Hash of all RecordStreamItems before writing this file. |
| record_stream_items | [RecordStreamItem](#proto-RecordStreamItem) | repeated | List of all the record stream items from that period. |
| end_object_running_hash | [HashObject](#proto-HashObject) |  | Running Hash of all RecordStreamItems before closing this file. |
| block_number | [int64](#int64) |  | The block number associated with this period. |
| sidecars | [SidecarMetadata](#proto-SidecarMetadata) | repeated | List of the hashes of all the sidecar record files created for the same period. Allows multiple sidecar files to be linked to this RecordStreamFile. |






<a name="proto-RecordStreamItem"></a>

### RecordStreamItem
A RecordStreamItem consists of a Transaction and a TransactionRecord,
which are already defined protobuf messages.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| transaction | [Transaction](#proto-Transaction) |  |  |
| record | [TransactionRecord](#proto-TransactionRecord) |  |  |






<a name="proto-SidecarMetadata"></a>

### SidecarMetadata
Information about a single sidecar file.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| hash | [HashObject](#proto-HashObject) |  | The hash of the entire file. |
| id | [int32](#int32) |  | The id of the sidecar record file |
| types | [SidecarType](#proto-SidecarType) | repeated | The types of sidecar records that will be included in the file. |





 


<a name="proto-SidecarType"></a>

### SidecarType
The type of sidecar records contained in the sidecar record file

| Name | Number | Description |
| ---- | ------ | ----------- |
| SIDECAR_TYPE_UNKNOWN | 0 |  |
| CONTRACT_STATE_CHANGE | 1 |  |
| CONTRACT_ACTION | 2 |  |
| CONTRACT_BYTECODE | 3 |  |


 

 

 



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

