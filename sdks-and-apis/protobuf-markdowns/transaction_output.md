# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [block/stream/output/transaction_output.proto](#block_stream_output_transaction_output-proto)
    - [TransactionOutput](#com-hedera-hapi-block-stream-output-TransactionOutput)
  
- [Scalar Value Types](#scalar-value-types)



<a name="block_stream_output_transaction_output-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## block/stream/output/transaction_output.proto
# Transaction Output
Messages that describe the output of a transaction; data reported
in the block stream that is not in the transaction body and is
not reported in state changes.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in
[RFC2119](https://www.ietf.org/rfc/rfc2119) and clarified in
[RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="com-hedera-hapi-block-stream-output-TransactionOutput"></a>

### TransactionOutput
Output from a transaction.

The values in transaction outputs SHALL be data that is neither present
in the original transaction nor present in state changes.

&gt; Note
&gt;&gt; Only a few transactions produce output that is not in the transaction
&gt;&gt; and also not reflected in state changes. All other transaction types
&gt;&gt; are _currently_ not included here. We have, however, allocated names
&gt;&gt; for those transaction types to preserve consistency if we
&gt;&gt; add them later.

&lt;!--
Reserved definitions:
import &#34;stream/smart_contract_service.proto&#34;;
   UpdateContractOutput contract_update;
   DeleteContractOutput contract_delete;
   SystemDeleteContractOutput
   SystemUnDeleteContractOutput
   CreateTopicOutput create_topic;
   UpdateTopicOutput update_topic;

import &#34;stream/file_service.proto&#34;;
   CreateFileOutput file_create;
   AppendFileOutput file_append;
   UpdateFileOutput file_update;
   DeleteFileOutput file_delete;
   SystemDeleteOutput system_delete;
   SystemUndeleteOutput system_undelete;

import &#34;stream/crypto_service.proto&#34;;
   CryptoTransferOutput crypto_transfer;
   UpdateNodeStakeOutput update_node_stake;
   ApproveAllowanceOutput approve_allowance;
   DeleteAllowanceOutput delete_allowance;
   UpdateAccountOutput update_account;
   DeleteAccountOutput delete_account;

import &#34;stream/token_service.proto&#34;;
   CreateTokenOutput create_token;
   DeleteTokenOutput delete_token;
   FreezeTokenAccountOutput freeze_token_account;
   UnfreezeTokenAccountOutput unfreeze_token_account;
   GrantTokenKycOutput grant_token_account_kyc;
   RevokeTokenKycOutput revoke_token_account_kyc;
   UpdateTokenOutput update_token;
   UpdateTokenNftsOutput update_token_nfts;
   MintTokenOutput mint_token;
   BurnTokenOutput burn_token;
   WipeTokenAccountOutput wipe_token_account;
   AssociateTokenOutput associate_token;
   DissociateTokenOutput dissociate_token;
   UpdateTokenFeeScheduleOutput update_token_fee_schedule;
   PauseTokenOutput pause_token;
   UnpauseTokenOutput unpause_token;
   TokenAirdropOutput token_airdrop;

import &#34;stream/consensus_service.proto&#34;;
   DeleteTopicOutput delete_topic;
   SubmitMessageOutput submit_message;

import &#34;stream/schedule_service.proto&#34;;
   DeleteScheduleOutput delete_schedule;

import &#34;stream/network_service.proto&#34;;
   FreezeOutput freeze_network;
--&gt;


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| util_prng | [UtilPrngOutput](#com-hedera-hapi-block-stream-output-UtilPrngOutput) |  | Output from a utilPrng transaction to request a deterministic pseudo-random number. |
| contract_call | [CallContractOutput](#com-hedera-hapi-block-stream-output-CallContractOutput) |  | Output from a contract call transaction. |
| ethereum_call | [EthereumOutput](#com-hedera-hapi-block-stream-output-EthereumOutput) |  | Output from an ethereum call transaction. |
| contract_create | [CreateContractOutput](#com-hedera-hapi-block-stream-output-CreateContractOutput) |  | Output from a contract create transaction. |
| create_schedule | [CreateScheduleOutput](#com-hedera-hapi-block-stream-output-CreateScheduleOutput) |  | Output from a schedule create transaction that executed immediately on creation. |
| sign_schedule | [SignScheduleOutput](#com-hedera-hapi-block-stream-output-SignScheduleOutput) |  | Output from a schedule sign transaction that resulted in executing the scheduled transaction. |
| account_create | [CreateAccountOutput](#com-hedera-hapi-block-stream-output-CreateAccountOutput) |  | Output from a transaction that includes the creation of a new CryptoService Account. |





 

 

 

 



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

