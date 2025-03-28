# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/state/roster/roster_state.proto](#services_state_roster_roster_state-proto)
    - [RosterState](#com-hedera-hapi-node-state-roster-RosterState)
    - [RoundRosterPair](#com-hedera-hapi-node-state-roster-RoundRosterPair)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_state_roster_roster_state-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/state/roster/roster_state.proto



<a name="com-hedera-hapi-node-state-roster-RosterState"></a>

### RosterState
The current state of platform rosters.&lt;br/&gt;
This message stores a roster data for the platform in network state.

The roster state SHALL encapsulate the incoming candidate roster&#39;s hash,
and a list of pairs of round number and active roster hash.&lt;br/&gt;
This data SHALL be used to track round numbers and the rosters used in determining the consensus.&lt;br/&gt;


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| candidate_roster_hash | [bytes](#bytes) |  | The SHA-384 hash of a candidate roster. &lt;p&gt; This is the hash of the roster that is currently being considered for adoption.&lt;br/&gt; A Node SHALL NOT, ever, have more than one candidate roster at the same time. |
| round_roster_pairs | [RoundRosterPair](#com-hedera-hapi-node-state-roster-RoundRosterPair) | repeated | A list of round numbers and roster hashes.&lt;br/&gt; The round number indicates the round in which the corresponding roster became active &lt;p&gt; This list SHALL be ordered by round numbers in descending order. |






<a name="com-hedera-hapi-node-state-roster-RoundRosterPair"></a>

### RoundRosterPair
A pair of round number and active roster hash.
&lt;p&gt;
This message SHALL encapsulate the round number and the hash of the
active roster used for that round.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| round_number | [uint64](#uint64) |  | The round number. &lt;p&gt; This value SHALL be the round number of the consensus round in which this roster became active. |
| active_roster_hash | [bytes](#bytes) |  | The SHA-384 hash of the active roster for the given round number. &lt;p&gt; This value SHALL be the hash of the active roster used for the round. |





 

 

 

 



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

