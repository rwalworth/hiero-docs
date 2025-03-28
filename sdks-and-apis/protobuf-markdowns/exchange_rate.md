# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [services/exchange_rate.proto](#services_exchange_rate-proto)
    - [ExchangeRate](#proto-ExchangeRate)
    - [ExchangeRateSet](#proto-ExchangeRateSet)
  
- [Scalar Value Types](#scalar-value-types)



<a name="services_exchange_rate-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## services/exchange_rate.proto
# Exchange Rates
Exchange rates that define ratios between HBAR and USD.

Fees are denominated in USD, but paid in HBAR, so accurate exchange
rates are important and the exchange rates kept in state are updated
frequently.&lt;br/&gt;
Exchange rates are also reported in every receipt for fee transparency.

### Keywords
The key words &#34;MUST&#34;, &#34;MUST NOT&#34;, &#34;REQUIRED&#34;, &#34;SHALL&#34;, &#34;SHALL NOT&#34;,
&#34;SHOULD&#34;, &#34;SHOULD NOT&#34;, &#34;RECOMMENDED&#34;, &#34;MAY&#34;, and &#34;OPTIONAL&#34; in this
document are to be interpreted as described in
[RFC2119](https://www.ietf.org/rfc/rfc2119) and clarified in
[RFC8174](https://www.ietf.org/rfc/rfc8174).


<a name="proto-ExchangeRate"></a>

### ExchangeRate
An exchange rate as a ratio of USD cents per HBAR.

This ratio SHALL be used to convert tinycent (`10&lt;sup&gt;-8&lt;/sup&gt;` USD cent)
to tinybar for fees and other purposes.&lt;br/&gt;
When applying an `ExchangeRate`, implementations SHOULD ensure input values
are `tinycent` and/or `tinybar` before applying the exchange ratio.&lt;br/&gt;
Exchange results MAY be converted to USD or HBAR via division if whole
unit values are required.

The ratio described here SHALL be assigned such that a value in `tinybar`
may be obtained with the following equation.
```
  amountInTinybar = (amountInTinycent * hbarEquiv) / centEquiv
```


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| hbarEquiv | [int32](#int32) |  | Denominator for a ratio of USD cents per HBAR. |
| centEquiv | [int32](#int32) |  | Numerator for a ratio of USD cents per HBAR. |
| expirationTime | [TimestampSeconds](#proto-TimestampSeconds) |  | Expiration time stamp for this exchange rate. |






<a name="proto-ExchangeRateSet"></a>

### ExchangeRateSet
A set of two exchange rates.&lt;br/&gt;
The exchange rate for the network is stored and reported as a set of
two rates; current and next. This structure supports the network cleanly
switching between exchange rates when necessary. This also provides clear
notice to clients when the exchange rate will change and the exchange
rate that will be applied for the next time period.

The difference in rate between `currentRate` and `nextRate` MUST NOT exceed
the configured maximum percentage change. This limit SHALL be a
network configuration value.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| currentRate | [ExchangeRate](#proto-ExchangeRate) |  | A current exchange rate. &lt;p&gt; When present in a receipt, this SHALL be the exchange rate used to compute the fees for that transaction. |
| nextRate | [ExchangeRate](#proto-ExchangeRate) |  | A future exchange rate. &lt;p&gt; This exchange rate SHALL be applied after the current exchange rate expires. |





 

 

 

 



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

