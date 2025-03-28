# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/node_update.proto](#services_node_update-proto)
    - [NodeUpdateTransactionBody](#com-hedera-hapi-node-addressbook-NodeUpdateTransactionBody)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_node_update-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/node_update.proto



<a name="com-hedera-hapi-node-addressbook-NodeUpdateTransactionBody"></a>

### NodeUpdateTransactionBody
Transaction body to modify address book node attributes.

- This transaction SHALL enable the node operator, as identified by the
  `admin_key`, to modify operational attributes of the node.
- This transaction MUST be signed by the active `admin_key` for the node.
- If this transaction sets a new value for the `admin_key`, then both the
  current `admin_key`, and the new `admin_key` MUST sign this transaction.
- This transaction SHALL NOT change any field that is not set (is null) in
  this transaction body.
- This SHALL create a pending update to the node, but the change SHALL NOT
  be immediately applied to the active configuration.
- All pending node updates SHALL be applied to the active network
  configuration during the next `freeze` transaction with the field
  `freeze_type` set to `PREPARE_UPGRADE`.

### Block Stream Effects
None.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| node_id | [uint64](#uint64) |  | A consensus node identifier in the network state. &lt;p&gt; The node identified MUST exist in the network address book.&lt;br/&gt; The node identified MUST NOT be deleted.&lt;br/&gt; This value is REQUIRED. |
| account_id | [proto.AccountID](#proto-AccountID) |  | An account identifier. &lt;p&gt; If set, this SHALL replace the node account identifier.&lt;br/&gt; If set, this transaction MUST be signed by the active `key` for _both_ the current node account _and_ the identified new node account. |
| description | [google.protobuf.StringValue](#google-protobuf-StringValue) |  | A short description of the node. &lt;p&gt; This value, if set, MUST NOT exceed `transaction.maxMemoUtf8Bytes` (default 100) bytes when encoded as UTF-8.&lt;br/&gt; If set, this value SHALL replace the previous value. |
| gossip_endpoint | [proto.ServiceEndpoint](#proto-ServiceEndpoint) | repeated | A list of service endpoints for gossip. &lt;p&gt; If set, this list MUST meet the following requirements. &lt;hr/&gt; These endpoints SHALL represent the published endpoints to which other consensus nodes may _gossip_ transactions.&lt;br/&gt; These endpoints SHOULD NOT specify both address and DNS name.&lt;br/&gt; This list MUST NOT be empty.&lt;br/&gt; This list MUST NOT contain more than `10` entries.&lt;br/&gt; The first two entries in this list SHALL be the endpoints published to all consensus nodes.&lt;br/&gt; All other entries SHALL be reserved for future use. &lt;p&gt; Each network may have additional requirements for these endpoints. A client MUST check network-specific documentation for those details.&lt;br/&gt; &lt;blockquote&gt;Example&lt;blockquote&gt; Hedera Mainnet _requires_ that address be specified, and does not permit DNS name (FQDN) to be specified. &lt;/blockquote&gt; &lt;blockquote&gt; Solo, however, _requires_ DNS name (FQDN) but also permits address. &lt;/blockquote&gt;&lt;/blockquote&gt; &lt;p&gt; If set, the new list SHALL replace the existing list. |
| service_endpoint | [proto.ServiceEndpoint](#proto-ServiceEndpoint) | repeated | A list of service endpoints for gRPC calls. &lt;p&gt; If set, this list MUST meet the following requirements. &lt;hr/&gt; These endpoints SHALL represent the published endpoints to which clients may submit transactions.&lt;br/&gt; These endpoints SHOULD specify address and port.&lt;br/&gt; These endpoints MAY specify a DNS name.&lt;br/&gt; These endpoints SHOULD NOT specify both address and DNS name.&lt;br/&gt; This list MUST NOT be empty.&lt;br/&gt; This list MUST NOT contain more than `8` entries. &lt;p&gt; Each network may have additional requirements for these endpoints. A client MUST check network-specific documentation for those details. &lt;p&gt; If set, the new list SHALL replace the existing list. |
| gossip_ca_certificate | [google.protobuf.BytesValue](#google-protobuf-BytesValue) |  | A certificate used to sign gossip events. &lt;p&gt; This value MUST be a certificate of a type permitted for gossip signatures.&lt;br/&gt; This value MUST be the DER encoding of the certificate presented. &lt;p&gt; If set, the new value SHALL replace the existing bytes value. |
| grpc_certificate_hash | [google.protobuf.BytesValue](#google-protobuf-BytesValue) |  | A hash of the node gRPC TLS certificate. &lt;p&gt; This value MAY be used to verify the certificate presented by the node during TLS negotiation for gRPC.&lt;br/&gt; This value MUST be a SHA-384 hash.&lt;br/&gt; The TLS certificate to be hashed MUST first be in PEM format and MUST be encoded with UTF-8 NFKD encoding to a stream of bytes provided to the hash algorithm.&lt;br/&gt; &lt;p&gt; If set, the new value SHALL replace the existing hash value. |
| admin_key | [proto.Key](#proto-Key) |  | An administrative key controlled by the node operator. &lt;p&gt; This field is OPTIONAL.&lt;br/&gt; If set, this key MUST sign this transaction.&lt;br/&gt; If set, this key MUST sign each subsequent transaction to update this node.&lt;br/&gt; If set, this field MUST contain a valid `Key` value.&lt;br/&gt; If set, this field MUST NOT be set to an empty `KeyList`. |





 

 

 

 



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

