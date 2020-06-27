# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [datalogy-dlp.proto](#datalogy-dlp.proto)
    - [ByteContentItem](#v1.ByteContentItem)
    - [CharacterMaskConfig](#v1.CharacterMaskConfig)
    - [ContentItem](#v1.ContentItem)
    - [CreateProtectConfigRequest](#v1.CreateProtectConfigRequest)
    - [CryptoDeterministicConfig](#v1.CryptoDeterministicConfig)
    - [CryptoFPEConfig](#v1.CryptoFPEConfig)
    - [CryptoHashConfig](#v1.CryptoHashConfig)
    - [CryptoKey](#v1.CryptoKey)
    - [DlpProtectRequest](#v1.DlpProtectRequest)
    - [DlpProtectResponse](#v1.DlpProtectResponse)
    - [DlpTransformReply](#v1.DlpTransformReply)
    - [DlpTransformRequest](#v1.DlpTransformRequest)
    - [FieldId](#v1.FieldId)
    - [FieldTransformation](#v1.FieldTransformation)
    - [GetProtectConfigRequest](#v1.GetProtectConfigRequest)
    - [HelloReply](#v1.HelloReply)
    - [HelloRequest](#v1.HelloRequest)
    - [InfoType](#v1.InfoType)
    - [InfoTypeTransformation](#v1.InfoTypeTransformation)
    - [InfoTypeTransformations](#v1.InfoTypeTransformations)
    - [ListProtectConfigsRequest](#v1.ListProtectConfigsRequest)
    - [ListProtectConfigsResponse](#v1.ListProtectConfigsResponse)
    - [PrimitiveTransformation](#v1.PrimitiveTransformation)
    - [ProtectConfig](#v1.ProtectConfig)
    - [RecordTransformations](#v1.RecordTransformations)
    - [Row](#v1.Row)
    - [Table](#v1.Table)
    - [TransformationSummary](#v1.TransformationSummary)
    - [Value](#v1.Value)
  
    - [ByteContentItem.BytesType](#v1.ByteContentItem.BytesType)
  
    - [DatalogyDlp](#v1.DatalogyDlp)
  
- [Scalar Value Types](#scalar-value-types)



<a name="datalogy-dlp.proto"></a>
<p align="right"><a href="#top">Top</a></p>

## datalogy-dlp.proto



<a name="v1.ByteContentItem"></a>

### ByteContentItem
ByteContentItem to inspect or protect.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| type | [ByteContentItem.BytesType](#v1.ByteContentItem.BytesType) |  | The type of data stored in the bytes string. Default will be TEXT_UTF8. |
| data | [bytes](#bytes) |  | Content data |






<a name="v1.CharacterMaskConfig"></a>

### CharacterMaskConfig
CharacterMaskConfig is a type of PrimitiveTransformation
Partially mask a string by replacing a given number of characters with a fixed character.
Masking can start from the beginning or end of the string.
This can be used on data of any type (numbers, longs, and so on) and when de-identifying
structured data we&#39;ll attempt to preserve the original data&#39;s type. (This allows you to
take a long like 123 and modify it to a string like **3.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| masking_char | [string](#string) |  | Character to use to mask the sensitive values—for example, * for an alphabetic string such as a name, or 0 for a numeric string such as ZIP code or credit card number. This string must have a length of 1. If not supplied, this value defaults to * for strings, and 0 for digits. |
| num_chars_to_mask | [int32](#int32) |  | Number of characters to mask. If not set, all matching chars will be masked. Skipped characters do not count towards this tally. |






<a name="v1.ContentItem"></a>

### ContentItem
ContentItem to inspect. Can be either value, Table, or Bytes.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| value | [string](#string) |  | string value to inspect or protect |
| table | [Table](#v1.Table) |  | DLP Table structure in the form of headers[] and rows[] |
| byte_item | [ByteContentItem](#v1.ByteContentItem) |  | byte[] can represent TEXT_UTF8, AVRO, or ARROW_TABLE |






<a name="v1.CreateProtectConfigRequest"></a>

### CreateProtectConfigRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| protect_config | [ProtectConfig](#v1.ProtectConfig) |  |  |
| name | [string](#string) |  |  |






<a name="v1.CryptoDeterministicConfig"></a>

### CryptoDeterministicConfig
CryptoDeterministicConfig is Pseudonymization method that generates deterministic
encryption for the given input. Outputs a base64 encoded representation of the
encrypted output.
Uses AES-SIV based on the RFC https://tools.ietf.org/html/rfc5297.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| crypto_key | [CryptoKey](#v1.CryptoKey) |  |  |






<a name="v1.CryptoFPEConfig"></a>

### CryptoFPEConfig
CryptoFPEConfig Replaces an identifier with a surrogate using Format Preserving Encryption (FPE)
with the FFX mode of operation; however when used in the ReidentifyContent API method, it serves
the opposite function by reversing the surrogate back into the original identifier.
The identifier must be encoded as ASCII. For a given crypto key and context, the same identifier
will be replaced with the same surrogate. Identifiers must be at least two characters long.
In the case that the identifier is the empty string, it will be skipped.
See https://cloud.google.com/dlp/docs/pseudonymization to learn more.

Note: We recommend using CryptoDeterministicConfig for all use cases which do not require preserving
the input alphabet space and size, plus warrant referential integrity.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| crypto_key | [CryptoKey](#v1.CryptoKey) |  |  |






<a name="v1.CryptoHashConfig"></a>

### CryptoHashConfig
CryptoHashConfig is a type of PrimitiveTransformation
Pseudonymization method that generates surrogates via cryptographic hashing.
Uses SHA-256. The key size must be either 32 or 64 bytes. Outputs a base64
encoded representation of the hashed output (for example, L7k0BHmF1ha5U3NfGykjro4xWi1MPVQPjhMAZbSV9mM=).
Currently, only string and integer values can be hashed.
See https://cloud.google.com/dlp/docs/pseudonymization to learn more.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| crypto_key | [CryptoKey](#v1.CryptoKey) |  |  |






<a name="v1.CryptoKey"></a>

### CryptoKey
CryptoKey is a data encryption key (DEK) (as opposed to a key encryption key (KEK) stored by KMS).
When using KMS to wrap/unwrap DEKs, be sure to set an appropriate IAM policy on the
KMS CryptoKey (KEK) to ensure an attacker cannot unwrap the data crypto key.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| name | [string](#string) |  |  |






<a name="v1.DlpProtectRequest"></a>

### DlpProtectRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| protect_config | [ProtectConfig](#v1.ProtectConfig) |  |  |
| item | [ContentItem](#v1.ContentItem) |  |  |






<a name="v1.DlpProtectResponse"></a>

### DlpProtectResponse



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| item | [ContentItem](#v1.ContentItem) |  |  |
| transformation_summaries | [TransformationSummary](#v1.TransformationSummary) | repeated |  |






<a name="v1.DlpTransformReply"></a>

### DlpTransformReply



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| item | [ContentItem](#v1.ContentItem) |  |  |
| error_msg | [string](#string) |  |  |






<a name="v1.DlpTransformRequest"></a>

### DlpTransformRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| item | [ContentItem](#v1.ContentItem) |  |  |






<a name="v1.FieldId"></a>

### FieldId
Identifier of a data field or column


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| name | [string](#string) |  | name describing the field |






<a name="v1.FieldTransformation"></a>

### FieldTransformation



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| fields | [FieldId](#v1.FieldId) | repeated |  |
| primitive_transformation | [PrimitiveTransformation](#v1.PrimitiveTransformation) |  |  |
| info_type_transformations | [InfoTypeTransformation](#v1.InfoTypeTransformation) |  |  |






<a name="v1.GetProtectConfigRequest"></a>

### GetProtectConfigRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| name | [string](#string) |  |  |






<a name="v1.HelloReply"></a>

### HelloReply



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| message | [string](#string) |  |  |






<a name="v1.HelloRequest"></a>

### HelloRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| name | [string](#string) |  |  |






<a name="v1.InfoType"></a>

### InfoType



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| name | [string](#string) |  |  |






<a name="v1.InfoTypeTransformation"></a>

### InfoTypeTransformation



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| info_types | [InfoType](#v1.InfoType) | repeated |  |
| primitive_transformation | [PrimitiveTransformation](#v1.PrimitiveTransformation) |  |  |






<a name="v1.InfoTypeTransformations"></a>

### InfoTypeTransformations



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| transformations | [InfoTypeTransformation](#v1.InfoTypeTransformation) | repeated |  |






<a name="v1.ListProtectConfigsRequest"></a>

### ListProtectConfigsRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| page_token | [string](#string) |  |  |
| page_size | [int32](#int32) |  |  |
| order_by | [string](#string) |  |  |






<a name="v1.ListProtectConfigsResponse"></a>

### ListProtectConfigsResponse



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| protect_configs | [ProtectConfig](#v1.ProtectConfig) | repeated |  |
| next_page_token | [string](#string) |  |  |






<a name="v1.PrimitiveTransformation"></a>

### PrimitiveTransformation
PrimitiveTransformation represents an action that Datalogy DLP service need to perform on ContentItem


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| character_mask_config | [CharacterMaskConfig](#v1.CharacterMaskConfig) |  |  |
| crypto_hash_config | [CryptoHashConfig](#v1.CryptoHashConfig) |  |  |
| crypto_deterministic_config | [CryptoDeterministicConfig](#v1.CryptoDeterministicConfig) |  |  |
| crypto_fpe_config | [CryptoFPEConfig](#v1.CryptoFPEConfig) |  |  |






<a name="v1.ProtectConfig"></a>

### ProtectConfig
ProtectConfig represents a template for transforming ContentItem. In general terms,
it is a map of fieldIds to Transformation types
Example:
protect_config = {
           &#34;name&#34;: &#34;projects/&lt;project_id&gt;/deidentifyTemplates/&lt;template_id&gt;&#34;,
           &#34;displayname&#34;: &#34;Config to DeIdentify Sample Dataset&#34;,
           &#34;description&#34;: &#34;De-identifies Card Number, Card PIN, Card Holder&#39;s Name, SSN, Age, Job Title, Additional Details and Online UserId Fields&#34;,
           &#34;createTime&#34;: &#34;2019-12-01T19:21:07.306279Z&#34;,
           &#34;updateTime&#34;: &#34;2019-12-01T19:21:07.306279Z&#34;,
           &#34;record_transformations&#34;: {
                   &#34;field_transformations&#34;: [
                       {
                           &#34;fields&#34;: [
                               {
                                   &#34;name&#34;: &#34;SSN&#34;
                               }
                           ],
                           &#34;primitive_transformation&#34;: {
                               &#34;character_mask_config&#34;: {
                                   &#34;masking_char&#34;: &#34;*&#34;,
                                   &#34;num_chars_to_mask&#34;: 4
                               }
                           }
                       },
                       {
                           &#34;fields&#34;: [
                               {
                                   &#34;name&#34;: &#34;email&#34;
                               }
                           ],
                           &#34;primitive_transformation&#34;: {
                               &#34;crypto_hash_config&#34;: {
                                   &#34;crypto_key&#34;: {
                                       &#34;name&#34;: &#34;vault/kms-wrapped-key&#34;
                                   }

                               }
                           }
                       }
                   ]
           }
       }
Note:
ProtectConfig library is maintained by Datalogy DLP service internally


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| name | [string](#string) |  |  |
| displayname | [string](#string) |  |  |
| description | [string](#string) |  |  |
| createTime | [string](#string) |  |  |
| updateTime | [string](#string) |  |  |
| info_type_transformations | [InfoTypeTransformations](#v1.InfoTypeTransformations) |  |  |
| record_transformations | [RecordTransformations](#v1.RecordTransformations) |  |  |






<a name="v1.RecordTransformations"></a>

### RecordTransformations
RecordTransformations is A type of transformation that is applied over a table.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| field_transformations | [FieldTransformation](#v1.FieldTransformation) | repeated |  |






<a name="v1.Row"></a>

### Row
Row representation of a row in the Table with array of Values


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| values | [Value](#v1.Value) | repeated | array of Values |






<a name="v1.Table"></a>

### Table
Table structure in the form of headers[] and rows[]


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| headers | [FieldId](#v1.FieldId) | repeated | headers of the table |
| rows | [Row](#v1.Row) | repeated | rows of the table |






<a name="v1.TransformationSummary"></a>

### TransformationSummary



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| details | [string](#string) |  |  |






<a name="v1.Value"></a>

### Value
Set of primitive values supported by the system.
Note that for the purposes of inspection or transformation,
the number of bytes considered to comprise a &#39;Value&#39; is based on
its representation as a UTF-8 encoded string.
For example, if &#39;integer_value&#39; is set to 123456789, the number of
bytes would be counted as 9, even though an int64 only holds up to 8 bytes of data.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| integer_value | [int64](#int64) |  |  |
| float_value | [double](#double) |  |  |
| string_value | [string](#string) |  |  |
| bool_value | [bool](#bool) |  |  |
| timestamp_value | [google.protobuf.Timestamp](#google.protobuf.Timestamp) |  |  |





 


<a name="v1.ByteContentItem.BytesType"></a>

### ByteContentItem.BytesType


| Name | Number | Description |
| ---- | ------ | ----------- |
| TEXT_UTF8 | 0 |  |
| AVRO | 1 |  |
| ARROW_TABLE | 2 |  |


 

 


<a name="v1.DatalogyDlp"></a>

### DatalogyDlp
Datalogy-DLP Service is a Data Loss Protection module within Datalogy. It is used
de-identify sensitive data by applying Transformations available within the service to
Strings, Tables, AVRO, APACHE ARROW tables

| Method Name | Request Type | Response Type | Description |
| ----------- | ------------ | ------------- | ------------|
| TransformSensitiveData | [DlpTransformRequest](#v1.DlpTransformRequest) | [DlpTransformReply](#v1.DlpTransformReply) |  |
| SayHello | [HelloRequest](#v1.HelloRequest) | [HelloReply](#v1.HelloReply) |  |
| ProtectContent | [DlpProtectRequest](#v1.DlpProtectRequest) | [DlpProtectResponse](#v1.DlpProtectResponse) | ProtectContent de-identifies content of ContentItem based on ProtectConfig template |
| CreateProtectConfig | [CreateProtectConfigRequest](#v1.CreateProtectConfigRequest) | [ProtectConfig](#v1.ProtectConfig) | CreateProtectConfig |
| GetProtectConfig | [GetProtectConfigRequest](#v1.GetProtectConfigRequest) | [ProtectConfig](#v1.ProtectConfig) |  |
| ListProtectConfigs | [ListProtectConfigsRequest](#v1.ListProtectConfigsRequest) | [ListProtectConfigsResponse](#v1.ListProtectConfigsResponse) |  |

 



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

