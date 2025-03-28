# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/node_create.proto](#services_node_create-proto)
    - [NodeCreateTransactionBody](#com-hedera-hapi-node-addressbook-NodeCreateTransactionBody)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_node_create-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/node_create.proto



<a name="com-hedera-hapi-node-addressbook-NodeCreateTransactionBody"></a>

### NodeCreateTransactionBody
A transaction body to add a new consensus node to the network address book.

This transaction body SHALL be considered a &#34;privileged transaction&#34;.

This message supports a transaction to create a new node in the network
address book. The transaction, once complete, enables a new consensus node
to join the network, and requires governing council authorization.

- A `NodeCreateTransactionBody` MUST be signed by the `Key` assigned to the
  `admin_key` field and one of those keys: treasure account (2) key,
  systemAdmin(50) key, or addressBookAdmin(55) key.
- The newly created node information SHALL be added to the network address
  book information in the network state.
- The new entry SHALL be created in &#34;state&#34; but SHALL NOT participate in
  network consensus and SHALL NOT be present in network &#34;configuration&#34;
  until the next &#34;upgrade&#34; transaction (as noted below).
- All new address book entries SHALL be added to the active network
  configuration during the next `freeze` transaction with the field
  `freeze_type` set to `PREPARE_UPGRADE`.

### Block Stream Effects
Upon completion the newly assigned `node_id` SHALL be recorded in
the transaction receipt.&lt;br/&gt;
This value SHALL be the next available node identifier.&lt;br/&gt;
Node identifiers SHALL NOT be reused.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| account_id | [proto.AccountID](#proto-AccountID) |  | A Node account identifier. &lt;p&gt; This account identifier MUST be in the &#34;account number&#34; form.&lt;br/&gt; This account identifier MUST NOT use the alias field.&lt;br/&gt; If the identified account does not exist, this transaction SHALL fail.&lt;br/&gt; Multiple nodes MAY share the same node account.&lt;br/&gt; This field is REQUIRED. |
| description | [string](#string) |  | A short description of the node. &lt;p&gt; This value, if set, MUST NOT exceed `transaction.maxMemoUtf8Bytes` (default 100) bytes when encoded as UTF-8.&lt;br/&gt; This field is OPTIONAL. |
| gossip_endpoint | [proto.ServiceEndpoint](#proto-ServiceEndpoint) | repeated | A list of service endpoints for gossip. &lt;p&gt; These endpoints SHALL represent the published endpoints to which other consensus nodes may _gossip_ transactions.&lt;br/&gt; These endpoints MUST specify a port.&lt;br/&gt; This list MUST NOT be empty.&lt;br/&gt; This list MUST NOT contain more than `10` entries.&lt;br/&gt; The first two entries in this list SHALL be the endpoints published to all consensus nodes.&lt;br/&gt; All other entries SHALL be reserved for future use. &lt;p&gt; Each network may have additional requirements for these endpoints. A client MUST check network-specific documentation for those details.&lt;br/&gt; If the network configuration value `gossipFqdnRestricted` is set, then all endpoints in this list MUST supply only IP address.&lt;br/&gt; If the network configuration value `gossipFqdnRestricted` is _not_ set, then endpoints in this list MAY supply either IP address or FQDN, but MUST NOT supply both values for the same endpoint. |
| service_endpoint | [proto.ServiceEndpoint](#proto-ServiceEndpoint) | repeated | A list of service endpoints for gRPC calls. &lt;p&gt; These endpoints SHALL represent the published gRPC endpoints to which clients may submit transactions.&lt;br/&gt; These endpoints MUST specify a port.&lt;br/&gt; Endpoints in this list MAY supply either IP address or FQDN, but MUST NOT supply both values for the same endpoint.&lt;br/&gt; This list MUST NOT be empty.&lt;br/&gt; This list MUST NOT contain more than `8` entries. |
| gossip_ca_certificate | [bytes](#bytes) |  | A certificate used to sign gossip events. &lt;p&gt; This value MUST be a certificate of a type permitted for gossip signatures.&lt;br/&gt; This value MUST be the DER encoding of the certificate presented.&lt;br/&gt; This field is REQUIRED and MUST NOT be empty. |
| grpc_certificate_hash | [bytes](#bytes) |  | A hash of the node gRPC TLS certificate. &lt;p&gt; This value MAY be used to verify the certificate presented by the node during TLS negotiation for gRPC.&lt;br/&gt; This value MUST be a SHA-384 hash.&lt;br/&gt; The TLS certificate to be hashed MUST first be in PEM format and MUST be encoded with UTF-8 NFKD encoding to a stream of bytes provided to the hash algorithm.&lt;br/&gt; This field is OPTIONAL. |
| admin_key | [proto.Key](#proto-Key) |  | An administrative key controlled by the node operator. &lt;p&gt; This key MUST sign this transaction.&lt;br/&gt; This key MUST sign each transaction to update this node.&lt;br/&gt; This field MUST contain a valid `Key` value.&lt;br/&gt; This field is REQUIRED and MUST NOT be set to an empty `KeyList`. |





 

 

 

 



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

