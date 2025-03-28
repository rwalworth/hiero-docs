# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [block/stream/output/token_service.proto](#block_stream_output_token_service-proto)
    - [AssociateTokenOutput](#com-hedera-hapi-block-stream-output-AssociateTokenOutput)
    - [BurnTokenOutput](#com-hedera-hapi-block-stream-output-BurnTokenOutput)
    - [CreateTokenOutput](#com-hedera-hapi-block-stream-output-CreateTokenOutput)
    - [DeleteTokenOutput](#com-hedera-hapi-block-stream-output-DeleteTokenOutput)
    - [DissociateTokenOutput](#com-hedera-hapi-block-stream-output-DissociateTokenOutput)
    - [FreezeTokenAccountOutput](#com-hedera-hapi-block-stream-output-FreezeTokenAccountOutput)
    - [GrantTokenKycOutput](#com-hedera-hapi-block-stream-output-GrantTokenKycOutput)
    - [MintTokenOutput](#com-hedera-hapi-block-stream-output-MintTokenOutput)
    - [PauseTokenOutput](#com-hedera-hapi-block-stream-output-PauseTokenOutput)
    - [RevokeTokenKycOutput](#com-hedera-hapi-block-stream-output-RevokeTokenKycOutput)
    - [TokenAirdropOutput](#com-hedera-hapi-block-stream-output-TokenAirdropOutput)
    - [UnfreezeTokenAccountOutput](#com-hedera-hapi-block-stream-output-UnfreezeTokenAccountOutput)
    - [UnpauseTokenOutput](#com-hedera-hapi-block-stream-output-UnpauseTokenOutput)
    - [UpdateTokenFeeScheduleOutput](#com-hedera-hapi-block-stream-output-UpdateTokenFeeScheduleOutput)
    - [UpdateTokenNftsOutput](#com-hedera-hapi-block-stream-output-UpdateTokenNftsOutput)
    - [UpdateTokenOutput](#com-hedera-hapi-block-stream-output-UpdateTokenOutput)
    - [WipeTokenAccountOutput](#com-hedera-hapi-block-stream-output-WipeTokenAccountOutput)
  
- [Scalar Value Types](#scalar-value-types)



<a name="block_stream_output_token_service-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## block/stream/output/token_service.proto
# Token Service
Block stream messages that report the results of transactions
handled by the `Token` service.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in
[RFC2119](https://www.ietf.org/rfc/rfc2119) and clarified in
[RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="com-hedera-hapi-block-stream-output-AssociateTokenOutput"></a>

### AssociateTokenOutput
Block Stream data for an `associateTokens` transaction.

This message SHALL NOT duplicate information already contained in
the original transaction.






<a name="com-hedera-hapi-block-stream-output-BurnTokenOutput"></a>

### BurnTokenOutput
Block Stream data for a `burnToken` transaction.

This message SHALL NOT duplicate information already contained in
the original transaction.






<a name="com-hedera-hapi-block-stream-output-CreateTokenOutput"></a>

### CreateTokenOutput
Block Stream data for a `createToken` transaction.

This message SHALL NOT duplicate information already contained in
the original transaction.






<a name="com-hedera-hapi-block-stream-output-DeleteTokenOutput"></a>

### DeleteTokenOutput
Block Stream data for a `deleteToken` transaction.

This message SHALL NOT duplicate information already contained in
the original transaction.






<a name="com-hedera-hapi-block-stream-output-DissociateTokenOutput"></a>

### DissociateTokenOutput
Block Stream data for a `dissociateTokens` transaction.

This message SHALL NOT duplicate information already contained in
the original transaction.






<a name="com-hedera-hapi-block-stream-output-FreezeTokenAccountOutput"></a>

### FreezeTokenAccountOutput
Block Stream data for a `freezeTokenAccount` transaction.

This message SHALL NOT duplicate information already contained in
the original transaction.






<a name="com-hedera-hapi-block-stream-output-GrantTokenKycOutput"></a>

### GrantTokenKycOutput
Block Stream data for a `grantKycToTokenAccount` transaction.

This message SHALL NOT duplicate information already contained in
the original transaction.






<a name="com-hedera-hapi-block-stream-output-MintTokenOutput"></a>

### MintTokenOutput
Block Stream data for a `mintToken` transaction.

This message SHALL NOT duplicate information already contained in
the original transaction.






<a name="com-hedera-hapi-block-stream-output-PauseTokenOutput"></a>

### PauseTokenOutput
Block Stream data for a `pauseToken` transaction.

This message SHALL NOT duplicate information already contained in
the original transaction.






<a name="com-hedera-hapi-block-stream-output-RevokeTokenKycOutput"></a>

### RevokeTokenKycOutput
Block Stream data for a `revokeKycFromTokenAccount` transaction.

This message SHALL NOT duplicate information already contained in
the original transaction.






<a name="com-hedera-hapi-block-stream-output-TokenAirdropOutput"></a>

### TokenAirdropOutput
Block Stream data for a `tokenAirdrop` transaction.

This message SHALL NOT duplicate information already contained in
the original transaction.






<a name="com-hedera-hapi-block-stream-output-UnfreezeTokenAccountOutput"></a>

### UnfreezeTokenAccountOutput
Block Stream data for an `unfreezeTokenAccount` transaction.

This message SHALL NOT duplicate information already contained in
the original transaction.






<a name="com-hedera-hapi-block-stream-output-UnpauseTokenOutput"></a>

### UnpauseTokenOutput
Block Stream data for an `unpauseToken` transaction.

This message SHALL NOT duplicate information already contained in
the original transaction.






<a name="com-hedera-hapi-block-stream-output-UpdateTokenFeeScheduleOutput"></a>

### UpdateTokenFeeScheduleOutput
Block Stream data for an `updateTokenFeeSchedule` transaction.

This message SHALL NOT duplicate information already contained in
the original transaction.






<a name="com-hedera-hapi-block-stream-output-UpdateTokenNftsOutput"></a>

### UpdateTokenNftsOutput
Block Stream data for an `updateNfts` transaction.

This message SHALL NOT duplicate information already contained in
the original transaction.






<a name="com-hedera-hapi-block-stream-output-UpdateTokenOutput"></a>

### UpdateTokenOutput
Block Stream data for an `updateToken` transaction.

This message SHALL NOT duplicate information already contained in
the original transaction.






<a name="com-hedera-hapi-block-stream-output-WipeTokenAccountOutput"></a>

### WipeTokenAccountOutput
Block Stream data for a `wipeTokenAccount` transaction.

This message SHALL NOT duplicate information already contained in
the original transaction.





 

 

 

 



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

