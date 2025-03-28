# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/state/addressbook/node.proto](#services_state_addressbook_node-proto)
    - [Node](#com-hedera-hapi-node-state-addressbook-Node)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_state_addressbook_node-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/state/addressbook/node.proto



<a name="com-hedera-hapi-node-state-addressbook-Node"></a>

### Node
A single address book node in the network state.

Each node in the network address book SHALL represent a single actual
consensus node that is eligible to participate in network consensus.

Address book nodes SHALL NOT be _globally_ uniquely identified. A given node
is only valid within a single realm and shard combination, so the identifier
for a network node SHALL only be unique within a single realm and shard
combination.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| node_id | [uint64](#uint64) |  | A consensus node identifier. &lt;p&gt; Node identifiers SHALL be globally unique for a given ledger. |
| account_id | [proto.AccountID](#proto-AccountID) |  | An account identifier. &lt;p&gt; This account SHALL be owned by the entity responsible for the node.&lt;br/&gt; This account SHALL be charged transaction fees for any transactions that are submitted to the network by this node and fail due diligence checks.&lt;br/&gt; This account SHALL be paid the node portion of transaction fees for transactions submitted by this node. |
| description | [string](#string) |  | A short description of the node. &lt;p&gt; This value, if set, MUST NOT exceed `transaction.maxMemoUtf8Bytes` (default 100) bytes when encoded as UTF-8. |
| gossip_endpoint | [proto.ServiceEndpoint](#proto-ServiceEndpoint) | repeated | A list of service endpoints for gossip. &lt;p&gt; These endpoints SHALL represent the published endpoints to which other consensus nodes may _gossip_ transactions.&lt;br/&gt; If the network configuration value `gossipFqdnRestricted` is set, then all endpoints in this list SHALL supply only IP address.&lt;br/&gt; If the network configuration value `gossipFqdnRestricted` is _not_ set, then endpoints in this list MAY supply either IP address or FQDN, but SHALL NOT supply both values for the same endpoint.&lt;br/&gt; This list SHALL NOT be empty.&lt;br/&gt; This list SHALL NOT contain more than `10` entries.&lt;br/&gt; The first two entries in this list SHALL be the endpoints published to all consensus nodes.&lt;br/&gt; All other entries SHALL be reserved for future use. |
| service_endpoint | [proto.ServiceEndpoint](#proto-ServiceEndpoint) | repeated | A list of service endpoints for client calls. &lt;p&gt; These endpoints SHALL represent the published endpoints to which clients may submit transactions.&lt;br/&gt; These endpoints SHALL specify a port.&lt;br/&gt; Endpoints in this list MAY supply either IP address or FQDN, but SHALL NOT supply both values for the same endpoint.&lt;br/&gt; This list SHALL NOT be empty.&lt;br/&gt; This list SHALL NOT contain more than `8` entries. |
| gossip_ca_certificate | [bytes](#bytes) |  | A certificate used to sign gossip events. &lt;p&gt; This value SHALL be a certificate of a type permitted for gossip signatures.&lt;br/&gt; This value SHALL be the DER encoding of the certificate presented.&lt;br/&gt; This field is REQUIRED and MUST NOT be empty. |
| grpc_certificate_hash | [bytes](#bytes) |  | A hash of the node gRPC certificate. &lt;p&gt; This value MAY be used to verify the certificate presented by the node during TLS negotiation for gRPC.&lt;br/&gt; This value SHALL be a SHA-384 hash.&lt;br/&gt; The TLS certificate to be hashed SHALL first be in PEM format and SHALL be encoded with UTF-8 NFKD encoding to a stream of bytes provided to the hash algorithm.&lt;br/&gt; This field is OPTIONAL. |
| weight | [uint64](#uint64) |  | **Deprecated.** A consensus weight. &lt;p&gt; Each node SHALL have a weight in consensus calculations.&lt;br/&gt; The consensus weight of a node SHALL be calculated based on the amount of HBAR staked to that node.&lt;br/&gt; Consensus SHALL be calculated based on agreement of greater than `2/3` of the total `weight` value of all nodes on the network. &lt;p&gt; This field is deprecated and SHALL NOT be used when RosterLifecycle is enabled. |
| deleted | [bool](#bool) |  | A flag indicating this node is deleted. &lt;p&gt; If this field is set, then this node SHALL NOT be included in the next update of the network address book.&lt;br/&gt; If this field is set, then this node SHALL be immutable and SHALL NOT be modified.&lt;br/&gt; If this field is set, then any `nodeUpdate` transaction to modify this node SHALL fail. |
| admin_key | [proto.Key](#proto-Key) |  | An administrative key controlled by the node operator. &lt;p&gt; This key MUST sign each transaction to update this node.&lt;br/&gt; This field MUST contain a valid `Key` value.&lt;br/&gt; This field is REQUIRED and MUST NOT be set to an empty `KeyList`. |





 

 

 

 



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

