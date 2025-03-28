# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/state/roster/roster.proto](#services_state_roster_roster-proto)
    - [Roster](#com-hedera-hapi-node-state-roster-Roster)
    - [RosterEntry](#com-hedera-hapi-node-state-roster-RosterEntry)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_state_roster_roster-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/state/roster/roster.proto



<a name="com-hedera-hapi-node-state-roster-Roster"></a>

### Roster
A single roster in the network state.
&lt;p&gt;
The roster SHALL be a list of `RosterEntry` objects.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| roster_entries | [RosterEntry](#com-hedera-hapi-node-state-roster-RosterEntry) | repeated | List of roster entries, one per consensus node. &lt;p&gt; This list SHALL contain roster entries in natural order of ascending node ids. This list SHALL NOT be empty.&lt;br/&gt; |






<a name="com-hedera-hapi-node-state-roster-RosterEntry"></a>

### RosterEntry
A single roster entry in the network state.

Each roster entry SHALL encapsulate the elements required
to manage node participation in the Threshold Signature Scheme (TSS).&lt;br/&gt;
All fields are REQUIRED.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| node_id | [uint64](#uint64) |  | A consensus node identifier. &lt;p&gt; Node identifiers SHALL be unique _within_ a ledger, and MUST NOT be repeated _between_ shards and realms. |
| weight | [uint64](#uint64) |  | A consensus weight. &lt;p&gt; Each node SHALL have a weight of zero or more in consensus calculations.&lt;br/&gt; The sum of the weights of all nodes in the roster SHALL form the total weight of the system, and each node&#39;s individual weight SHALL be proportional to that sum.&lt;br/&gt; |
| gossip_ca_certificate | [bytes](#bytes) |  | An RSA public certificate used for signing gossip events. &lt;p&gt; This value SHALL be a certificate of a type permitted for gossip signatures.&lt;br/&gt; This value SHALL be the DER encoding of the certificate presented.&lt;br/&gt; This field is REQUIRED and MUST NOT be empty. |
| gossip_endpoint | [proto.ServiceEndpoint](#proto-ServiceEndpoint) | repeated | A list of service endpoints for gossip. &lt;p&gt; These endpoints SHALL represent the published endpoints to which other consensus nodes may _gossip_ transactions.&lt;br/&gt; If the network configuration value `gossipFqdnRestricted` is set, then all endpoints in this list SHALL supply only IP address.&lt;br/&gt; If the network configuration value `gossipFqdnRestricted` is _not_ set, then endpoints in this list MAY supply either IP address or FQDN, but SHALL NOT supply both values for the same endpoint.&lt;br/&gt; This list SHALL NOT be empty.&lt;br/&gt; |





 

 

 

 



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

