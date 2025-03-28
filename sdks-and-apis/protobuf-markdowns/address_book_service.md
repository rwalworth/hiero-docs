# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/address_book_service.proto](#services_address_book_service-proto)
    - [AddressBookService](#proto-AddressBookService)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_address_book_service-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/address_book_service.proto
# Address Book Service API
GRPC service definitions for the Hedera Address Book Service (HABS).

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in
[RFC2119](https://www.ietf.org/rfc/rfc2119) and clarified in
[RFC8174](https://www.ietf.org/rfc/rfc8174).

 

 

 


<a name="proto-AddressBookService"></a>

### AddressBookService
The Address Book service provides the ability for Hedera network node
administrators to add, update, and remove consensus nodes. This addition,
update, or removal of a consensus node requires governing council approval,
but each node operator may update their own operational attributes without
additional approval, reducing overhead for routine operations.

Most operations are `privileged operations` and require governing council
approval.

### For a node creation transaction.
- The node operator SHALL create a `createNode` transaction.
   - The node operator MUST sign this transaction with the `Key`
     set as the `admin_key` for the new `Node`.
   - The node operator SHALL deliver the signed transaction to the Hedera
     council representative.
   - The Hedera council representative SHALL arrange for council members to
     review and sign the transaction.
   - Once sufficient council members have signed the transaction, the
     Hedera council representative SHALL submit the transaction to the
     network.
- Upon receipt of a valid and signed node creation transaction the network
  software SHALL
   - Validate the threshold signature for the Hedera governing council
   - Validate the signature of the `Key` provided as the new `admin_key`
     for the `Node`.
   - Create the new node in state, this new node SHALL NOT be active in the
     network at this time.
   - When executing the next `freeze` transaction with `freeze_type` set to
     `PREPARE_UPGRADE`, update network configuration and bring the
     new node to an active status within the network. The node to be added
     SHALL be active in the network following this upgrade.

### For a node deletion transaction.
- The node operator or Hedera council representative SHALL create a
  `deleteNode` transaction.
   - If the node operator creates the transaction
      - The node operator MUST sign this transaction with the `Key`
        set as the `admin_key` for the existing `Node`.
      - The node operator SHALL deliver the signed transaction to the Hedera
        council representative.
   - The Hedera council representative SHALL arrange for council members to
     review and sign the transaction.
   - Once sufficient council members have signed the transaction, the
     Hedera council representative SHALL submit the transaction to the
     network.
- Upon receipt of a valid and signed node deletion transaction the network
  software SHALL
   - Validate the signature for the Hedera governing council
   - Remove the existing node from network state. The node SHALL still
     be active in the network at this time.
   - When executing the next `freeze` transaction with `freeze_type` set to
     `PREPARE_UPGRADE`, update network configuration and remove the
     node to be deleted from the network. The node to be deleted SHALL NOT
     be active in the network following this upgrade.

### For a node update transaction.
- The node operator SHALL create an `updateNode` transaction.
   - The node operator MUST sign this transaction with the active `key`
     assigned as the `admin_key`.
   - The node operator SHALL submit the transaction to the
     network.  Hedera council approval SHALL NOT be sought for this
     transaction
- Upon receipt of a valid and signed node update transaction the network
  software SHALL
   - If the transaction modifies the value of the &#34;node account&#34;,
      - Validate the signature of the active `key` for the account
        assigned as the _current_ &#34;node account&#34;.
      - Validate the signature of the active `key` for the account to be
        assigned as the _new_ &#34;node account&#34;.
   - Modify the node information held in network state with the changes
     requested in the update transaction. The node changes SHALL NOT be
     applied to network configuration, and SHALL NOT affect network
     operation at this time.
   - When executing the next `freeze` transaction with `freeze_type` set to
     `PREPARE_UPGRADE`, update network configuration according to the
     modified information in network state. The requested changes SHALL
     affect network operation following this upgrade.

| Method Name | Request Type | Response Type | Description |
| ----------- | ------------ | ------------- | ------------|
| createNode | [Transaction](#proto-Transaction) | [TransactionResponse](#proto-TransactionResponse) | A transaction to create a new consensus node in the network address book. &lt;p&gt; This transaction, once complete, SHALL add a new consensus node to the network state.&lt;br/&gt; The new consensus node SHALL remain in state, but SHALL NOT participate in network consensus until the network updates the network configuration. &lt;p&gt; Hedera governing council authorization is REQUIRED for this transaction. |
| deleteNode | [Transaction](#proto-Transaction) | [TransactionResponse](#proto-TransactionResponse) | A transaction to remove a consensus node from the network address book. &lt;p&gt; This transaction, once complete, SHALL remove the identified consensus node from the network state. &lt;p&gt; Hedera governing council authorization is REQUIRED for this transaction. |
| updateNode | [Transaction](#proto-Transaction) | [TransactionResponse](#proto-TransactionResponse) | A transaction to update an existing consensus node from the network address book. &lt;p&gt; This transaction, once complete, SHALL modify the identified consensus node state as requested. &lt;p&gt; This transaction is authorized by the node operator |

 



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

