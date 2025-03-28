# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [platform/state/platform_state.proto](#platform_state_platform_state-proto)
    - [ConsensusSnapshot](#com-hedera-hapi-platform-state-ConsensusSnapshot)
    - [MinimumJudgeInfo](#com-hedera-hapi-platform-state-MinimumJudgeInfo)
    - [NodeId](#com-hedera-hapi-platform-state-NodeId)
    - [PlatformState](#com-hedera-hapi-platform-state-PlatformState)
  
- [Scalar Value Types](#scalar-value-types)



<a name="platform_state_platform_state-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## platform/state/platform_state.proto
# PlatformState
Messages that hold platform state in the network state.

The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in [RFC2119](https://www.ietf.org/rfc/rfc2119)
and clarified in [RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="com-hedera-hapi-platform-state-ConsensusSnapshot"></a>

### ConsensusSnapshot
A consensus snapshot.&lt;br/&gt;
This is a snapshot of the consensus state for a particular round.

This message SHALL record consensus data necessary for restart
and reconnect.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| round | [uint64](#uint64) |  | A consensus round.&lt;br/&gt; The round number of this snapshot. |
| judge_hashes | [bytes](#bytes) | repeated | A list of SHA-384 hash values.&lt;br/&gt; The hashes of all judges for this round. &lt;p&gt; This list SHALL be ordered by creator ID.&lt;br/&gt; This list MUST be deterministically ordered. |
| minimum_judge_info_list | [MinimumJudgeInfo](#com-hedera-hapi-platform-state-MinimumJudgeInfo) | repeated | A list of minimum judge information entries.&lt;br/&gt; These are &#34;minimum ancient&#34; entries for non-ancient rounds. |
| next_consensus_number | [uint64](#uint64) |  | A single consensus number.&lt;br/&gt; The consensus order of the next event to reach consensus. |
| consensus_timestamp | [proto.Timestamp](#proto-Timestamp) |  | A &#34;consensus&#34; timestamp.&lt;br/&gt; The consensus timestamp of this snapshot. &lt;p&gt; Depending on the context this timestamp may have different meanings: &lt;ul&gt; &lt;li&gt;if there are transactions, the timestamp is equal to the timestamp of the last transaction&lt;/li&gt; &lt;li&gt;if there are no transactions, the timestamp is equal to the timestamp of the last event&lt;/li&gt; &lt;li&gt;if there are no events, the timestamp is equal to the timestamp of the previous round plus a small constant&lt;/li&gt; &lt;/ul&gt; &lt;p&gt; This SHALL be a consensus value and MAY NOT correspond to an actual &#34;wall clock&#34; timestamp.&lt;br/&gt; Consensus Timestamps SHALL always increase. |






<a name="com-hedera-hapi-platform-state-MinimumJudgeInfo"></a>

### MinimumJudgeInfo
Records the minimum ancient indicator for all judges in a particular round.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| round | [uint64](#uint64) |  | A consensus round.&lt;br/&gt; The round this judge information applies to. |
| minimum_judge_ancient_threshold | [uint64](#uint64) |  | This is a minimum ancient threshold for all judges for a given round. The value should be interpreted as a generation if the birth round migration is not yet completed, and a birth round thereafter. &lt;p&gt; This SHALL reflect the relevant minimum threshold, whether generation-based or birth-round-based. |






<a name="com-hedera-hapi-platform-state-NodeId"></a>

### NodeId
A consensus node identifier.&lt;br/&gt;
This value uniquely identifies a single consensus node within the network.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| id | [uint64](#uint64) |  | A numeric identifier. |






<a name="com-hedera-hapi-platform-state-PlatformState"></a>

### PlatformState
The current state of platform consensus.&lt;br/&gt;
This message stores the current consensus data for the platform
in network state.

The platform state SHALL represent the latest round&#39;s consensus.&lt;br/&gt;
This data SHALL be used to ensure consistency and provide critical data for
restart and reconnect.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| creation_software_version | [proto.SemanticVersion](#proto-SemanticVersion) |  | A version describing the current version of application software. &lt;p&gt; This SHALL be the software version that created this state. |
| rounds_non_ancient | [uint32](#uint32) |  | A number of non-ancient rounds. &lt;p&gt; This SHALL be the count of rounds considered non-ancient. |
| consensus_snapshot | [ConsensusSnapshot](#com-hedera-hapi-platform-state-ConsensusSnapshot) |  | A snapshot of the consensus state at the end of the round. &lt;p&gt; This SHALL be used for restart/reconnect. |
| freeze_time | [proto.Timestamp](#proto-Timestamp) |  | A timestamp for the next scheduled time when a freeze will start. &lt;p&gt; If a freeze is not scheduled, this SHALL NOT be set.&lt;br/&gt; If a freeze is currently scheduled, this MUST be set, and MUST match the timestamp requested for that freeze. |
| last_frozen_time | [proto.Timestamp](#proto-Timestamp) |  | A timestamp for the last time a freeze was performed.&lt;br/&gt; If not set, there has never been a freeze. |
| legacy_running_event_hash | [bytes](#bytes) |  | **Deprecated.** A running event hash.&lt;br/&gt; This is computed by the consensus event stream. &lt;p&gt; This will be _removed_ and the field number reserved once the consensus event stream is retired. |
| lowest_judge_generation_before_birth_round_mode | [uint64](#uint64) |  | **Deprecated.** A consensus generation.&lt;br/&gt; The lowest judge generation before birth round mode was enabled. &lt;p&gt; This SHALL be `MAX_UNSIGNED` if birth round mode has not yet been enabled. |
| last_round_before_birth_round_mode | [uint64](#uint64) |  | **Deprecated.** A consensus round.&lt;br/&gt; The last round before the birth round mode was enabled. Will be removed after the birth round migration. &lt;p&gt; This SHALL be `MAX_UNSIGNED` if birth round mode has not yet been enabled. |
| first_version_in_birth_round_mode | [proto.SemanticVersion](#proto-SemanticVersion) |  | **Deprecated.** A consensus node semantic version.&lt;br/&gt; The software version that enabled birth round mode. &lt;p&gt; This SHALL be unset if birth round migration has not yet happened.&lt;br/&gt; If birth round migration is complete, this SHALL be the _first_ software version that enabled birth round mode. |





 

 

 

 



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

