# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [block/stream/block_proof.proto](#block_stream_block_proof-proto)
    - [BlockProof](#com-hedera-hapi-block-stream-BlockProof)
    - [MerkleSiblingHash](#com-hedera-hapi-block-stream-MerkleSiblingHash)
  
- [Scalar Value Types](#scalar-value-types)



<a name="block_stream_block_proof-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## block/stream/block_proof.proto
# Block Proof
A proof for the block streamed from a consensus node.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in
[RFC2119](https://www.ietf.org/rfc/rfc2119) and clarified in
[RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="com-hedera-hapi-block-stream-BlockProof"></a>

### BlockProof
A cryptographic proof for the &#34;Block Merkle Tree&#34;.

This message SHALL offer a proof for the &#34;Block Merkle Tree&#34;.
The information in the &#34;Block Merkle Tree&#34; SHALL be used to validate the
full content of the most recent block, and, with chained validation,
all prior blocks.

### Block Merkle Tree
The Block Hash of any block is a merkle root hash comprised of a 4 leaf
binary merkle tree. The 4 leaves represent
1. Previous block proof hash
1. Merkle root of transaction inputs tree
1. Merkle root of transaction outputs tree
1. Merkle rook of state tree

#### Computing the hash
The process for computing a block hash is somewhat complex, and involves
creating a &#34;virtual&#34; merkle tree to obtain the root merkle hash of
that virtual tree.&lt;br/&gt;
The merkle tree SHALL have a 4 part structure with 2 internal nodes,
structured in a strictly binary tree.
- The merkle tree root SHALL be the parent of both
  internal nodes.
   1. The first &#34;internal&#34; node SHALL be the parent of the
      two &#34;left-most&#34; nodes.
      1. The first leaf MUST be the previous block hash, and is a
         single 48-byte value.
      1. The second leaf MUST be the root of a, strictly binary, merkle tree
         composed of all &#34;input&#34; block items in the block.&lt;br/&gt;
         Input items SHALL be transactions, system transactions,
         and events.&lt;br/&gt;
         Leaf nodes in this subtree SHALL be ordered in the same order
         that the block items are encountered in the stream.
   1. The second &#34;internal&#34; node SHALL be the parent of the two
      &#34;right-most&#34; nodes.
      1. The third leaf MUST be the root of a, strictly binary, merkle tree
         composed of all &#34;output&#34; block items in the block.&lt;br/&gt;
         Output items SHALL be transaction result, transaction
         output, and state changes.&lt;br/&gt;
         Leaf nodes in this subtree SHALL be ordered in the same order that
         the block items are encountered in the stream.
      1. The fourth leaf MUST be the merkle tree root hash for network state
         at the start of the block, and is a single 48-byte value.
- The block hash SHALL be the hash calculated for the root of this merkle
  tree.
- The hash algorithm used SHALL be the algorithm specified in the
  corresponding block header.

The &#34;inputs&#34; and &#34;outputs&#34; subtrees SHALL be &#34;complete&#34; binary merkle trees,
with nodes that would otherwise be missing replaced by a &#34;null&#34; hash
leaf.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| block | [uint64](#uint64) |  | The block this proof secures.&lt;br/&gt; We provide this because a proof for a future block can be used to prove the state of the ledger at that block and the blocks before it.&lt;br/&gt; &lt;p&gt; This value SHOULD match the block number of the current block, under normal operation. |
| previous_block_root_hash | [bytes](#bytes) |  | A block root hash for the previous block. &lt;p&gt; This value MUST match the block merkle tree root hash of the previous block in the block stream.&lt;br/&gt; This value SHALL be empty for the genesis block, and SHALL NOT be empty for any other block.&lt;br/&gt; Client systems SHOULD optimistically reject any block with a `previous_block_proof_hash` that does not match the block hash of the previous block and MAY assume the block stream has encountered data loss, data corruption, or unauthorized modification. &lt;p&gt; The process for computing a block hash is somewhat complex, and involves creating a &#34;virtual&#34; merkle tree to obtain the root merkle hash of that virtual tree.&lt;br/&gt; The merkle tree SHALL have a 4 part structure with 2 internal nodes, structured in a strictly binary tree. &lt;ul&gt; &lt;li&gt;The merkle tree root SHALL be the parent of both internal nodes. &lt;ol&gt; &lt;li&gt;The first &#34;internal&#34; node SHALL be the parent of the two &#34;left-most&#34; nodes. &lt;ol&gt; &lt;li&gt;The first leaf MUST be the previous block hash, and is a single 48-byte value.&lt;/li&gt; &lt;li&gt;The second leaf MUST be the root of a, strictly binary, merkle tree composed of all &#34;input&#34; block items in the block.&lt;br/&gt; Input items SHALL be transactions, system transactions, and events.&lt;br/&gt; Leaf nodes in this subtree SHALL be ordered in the same order that the block items are encountered in the stream.&lt;/li&gt; &lt;/ol&gt; &lt;/li&gt; &lt;li&gt;The second &#34;internal&#34; node SHALL be the parent of the two &#34;right-most&#34; nodes. &lt;ol&gt; &lt;li&gt;The third leaf MUST be the root of a, strictly binary, merkle tree composed of all &#34;output&#34; block items in the block.&lt;br/&gt; Output items SHALL be transaction result, transaction output, and state changes.&lt;br/&gt; Leaf nodes in this subtree SHALL be ordered in the same order that the block items are encountered in the stream.&lt;/li&gt; &lt;li&gt;The fourth leaf MUST be the merkle tree root hash for network state at the start of the block, and is a single 48-byte value.&lt;/li&gt; &lt;/ol&gt; &lt;/li&gt; &lt;/ol&gt; &lt;/li&gt; &lt;li&gt;The block hash SHALL be the SHA-384 hash calculated for the root of this merkle tree.&lt;/li&gt; &lt;/ul&gt; |
| start_of_block_state_root_hash | [bytes](#bytes) |  | A merkle root hash of the network state.&lt;br/&gt; This is present to support validation of this block proof by clients that do not maintain a full copy of the network state. &lt;p&gt; This MUST contain a hash of the &#34;state&#34; merkle tree root at the start of the current block (which this block proof verifies).&lt;br/&gt; State processing clients SHOULD calculate the state root hash independently and SHOULD NOT rely on this value.&lt;br/&gt; State processing clients MUST validate the application of state changes for a block using the value present in the Block Proof of the _following_ block. Compliant consensus nodes MUST produce an &#34;empty&#34; block (containing only `BlockHeader` and `BlockProof` as the last block prior to a network &#34;freeze&#34; to ensure the final state hash is incorporated into the Block Stream correctly. Stateless (non-state-processing) clients MUST use this value to construct the block merkle tree. |
| block_signature | [bytes](#bytes) |  | A TSS signature for one block.&lt;br/&gt; This is a single signature representing the collection of partial signatures from nodes holding strictly greater than 2/3 of the current network &#34;weight&#34; in aggregate. The signature is produced by cryptographic &#34;aggregation&#34; of the partial signatures to produce a single signature that can be verified with the network public key, but could not be produced by fewer nodes than required to meet the threshold for network stake &#34;weight&#34;. &lt;p&gt; This message MUST make use of a threshold signature scheme like `BLS` which provides the necessary cryptographic guarantees.&lt;br/&gt; This signature SHALL use a TSS signature to provide a single signature that represents the consensus signature of consensus nodes.&lt;br/&gt; The exact subset of nodes that signed SHALL neither be known nor tracked, but it SHALL be cryptographically verifiable that the threshold was met if the signature itself can be validated with the network public key (a.k.a `LedgerID`). |
| sibling_hashes | [MerkleSiblingHash](#com-hedera-hapi-block-stream-MerkleSiblingHash) | repeated | A set of hash values along with ordering information.&lt;br/&gt; This list of hash values form the set of sibling hash values needed to correctly reconstruct the parent hash, and all hash values &#34;above&#34; that hash in the merkle tree. &lt;p&gt; A Block proof can be constructed by combining the sibling hashes for a previous block hash and sibling hashes for each entry &#34;above&#34; that node in the merkle tree of a block proof that incorporates that previous block hash. This form of block proof may be used to prove a chain of blocks when one or more older blocks is missing the original block proof that signed the block&#39;s merkle root directly. &lt;p&gt; This list MUST be ordered from the sibling of the node that contains this block&#39;s root node hash, and continues up the merkle tree to the root hash of the signed block proof. &lt;p&gt; If this block proof has a &#34;direct&#34; signature, then this list MUST be empty.&lt;br/&gt; If this list is not empty, then this block proof MUST be verified by first constructing the &#34;block&#34; merkle tree and computing the root hash of that tree, then combining that hash with the values in this list, paying attention to the first/second sibling ordering, until the root merkle hash is produced from the last pair of sibling hashes. That &#34;secondary&#34; root hash MUST then be verified using the value of `block_signature`. |






<a name="com-hedera-hapi-block-stream-MerkleSiblingHash"></a>

### MerkleSiblingHash
A hash of a &#34;sibling&#34; to an entry in a Merkle tree.

When constructing a binary merkle tree, each internal node is a hash
constructed from the hash of two &#34;descendant&#34; nodes. Those two nodes
are &#34;siblings&#34; and the order (first, second) in which the two hash values
are combined affects the parent hash.&lt;br/&gt;
This may be used to reconstruct a portion of a merkle tree starting from
a node of interest up to the root of the tree.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| is_first | [bool](#bool) |  | A flag for the position of this sibling. &lt;p&gt; If this is set then this sibling MUST be the first hash in the pair of sibling hashes of a binary merkle tree.&lt;br/&gt; If this is unset, then this sibling MUST be the second hash in the pair of sibling hashes of a binary merkle tree. |
| sibling_hash | [bytes](#bytes) |  | A byte array of a sibling hash.&lt;br/&gt; This is the hash for the sibling at this point in the merkle tree. &lt;p&gt; The algorithm for this hash SHALL match the algorithm for the block that contains this sibling.&lt;br/&gt; This SHALL contain the raw (e.g.) 384 bits (48 bytes) of the hash value. |





 

 

 

 



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

