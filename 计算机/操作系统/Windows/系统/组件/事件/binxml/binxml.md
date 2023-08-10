# binxml

## 目录

-   [binxml 二进制xml](#binxml-二进制xml)
    -   [参考示例](#参考示例)
    -   [二进制字节对应意义](#二进制字节对应意义)
        -   [4.2. Token types](#42-Token-types)
        -   [4.3. Value types](#43-Value-types)

# binxml 二进制xml

参考文档：[https://learn.microsoft.com/en-us/openspecs/windows\_protocols/ms-even6/e6fc7c72-b8c0-475b-aef7-25eaf1a64530](https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-even6/e6fc7c72-b8c0-475b-aef7-25eaf1a64530 "https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-even6/e6fc7c72-b8c0-475b-aef7-25eaf1a64530")

binxml整体说明：[https://learn.microsoft.com/en-us/openspecs/windows\_protocols/ms-even6/f59be2a3-315d-43b2-987f-ddb8f81f022e](https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-even6/f59be2a3-315d-43b2-987f-ddb8f81f022e "https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-even6/f59be2a3-315d-43b2-987f-ddb8f81f022e")

<https://github.com/libyal/libevtx/blob/main/documentation/Windows%20XML%20Event%20Log%20(EVTX).asciidoc#token_types>

<https://github.com/0xrawsec/golang-evtx/raw/master/doc/fileformat/%5BMS-EVEN6%5D.pdf>

## 参考示例

| Text                   | Binary                  |
| ---------------------- | ----------------------- |
| \<SomeEvent>           | 01 SomeEvent 02         |
| \<PropA> 99 \</PropA>  | 01 PropA 02 05 "99" 04  |
| \<PropB> 101 \</PropB> | 01 PropB 02 05 "101" 04 |
| \</SomeEvent>          | 04 00                   |

## 二进制字节对应意义

### 4.2. Token types

Binary XML defines multiple token types.

| Value     | Identifier                      | Description                                                                                     |
| --------- | ------------------------------- | ----------------------------------------------------------------------------------------------- |
| 0x00      | BinXmlTokenEOF                  | End of file                                                                                     |
| 0x01 0x41 | BinXmlTokenOpenStartElementTag  | Open start element tag Indicates the start of a start element, correlates to '<' in '\<Event>'  |
| 0x02      | BinXmlTokenCloseStartElementTag | Close start element tag Indicates the end of a start element, correlates to '>' in '\<Event>'   |
| 0x03      | BinXmlTokenCloseEmptyElementTag | Close empty element tag Indicates the end of a start element, correlates to '/>' in '\<Event/>' |
| 0x04      | BinXmlTokenEndElementTag        | Close end element tag Indicates the end of element, correlates to '\</Event>'                   |
| 0x05 0x45 | BinXmlTokenValue                | Value                                                                                           |
| 0x06 0x46 | BinXmlTokenAttribute            | Attribute                                                                                       |
| 0x07 0x47 | BinXmlTokenCDATASection         | CDATA section                                                                                   |
| 0x08 0x48 | BinXmlTokenCharRef              | Character entity reference                                                                      |
| 0x09 0x49 | BinXmlTokenEntityRef            | Entity reference                                                                                |
| 0x0a      | BinXmlTokenPITarget             | Processing instructions (PI) target XML processing instructions                                 |
| 0x0b      | BinXmlTokenPIData               | Processing instructions (PI) data XML processing instructions                                   |
| 0x0c      | BinXmlTokenTemplateInstance     | Template instance                                                                               |
| 0x0d      | BinXmlTokenNormalSubstitution   | Normal substitution                                                                             |
| 0x0e      | BinXmlTokenOptionalSubstitution | Optional substitution                                                                           |
| 0x0f      | BinXmlFragmentHeaderToken       | Fragment header token                                                                           |

Some of the token types can contain the has more data flag 0x40.

**TODO bitmask of 0x1f ? is this defined in winevt.h ? If so what do the other flags signify?**

### 4.3. Value types

| Value | Identifier     | Description                                                                                             |
| ----- | -------------- | ------------------------------------------------------------------------------------------------------- |
| 0x00  | NullType       | NULL or empty                                                                                           |
| 0x01  | StringType     | Unicode string Stored as UTF-16 little-endian without an end-of-string character                        |
| 0x02  | AnsiStringType | ASCII string Stored using a codepage without an end-of-string character                                 |
| 0x03  | Int8Type       | 8-bit integer signed                                                                                    |
| 0x04  | UInt8Type      | 8-bit integer unsigned                                                                                  |
| 0x05  | Int16Type      | 16-bit integer signed                                                                                   |
| 0x06  | UInt16Type     | 16-bit integer unsigned                                                                                 |
| 0x07  | Int32Type      | 32-bit integer signed                                                                                   |
| 0x08  | UInt32Type     | 32-bit integer unsigned                                                                                 |
| 0x09  | Int64Type      | 64-bit integer signed                                                                                   |
| 0x0a  | UInt64Type     | 64-bit integer unsigned                                                                                 |
| 0x0b  | Real32Type     | Floating point 32-bit (single precision)                                                                |
| 0x0c  | Real64Type     | Floating point 64-bit (double precision)                                                                |
| 0x0d  | BoolType       | Boolean**An 32-bit integer that MUST be 0x00 or 0x01 (mapping to true or false, respectively).**        |
| 0x0e  | BinaryType     | Binary data                                                                                             |
| 0x0f  | GuidType       | GUID Stored in little-endian                                                                            |
| 0x10  | SizeTType      | Size type Either 32 or 64-bits. This value type should be pair up with a HexInt32Type or HexInt64Type   |
| 0x11  | FileTimeType   | FILETIME (64-bit) Stored in little-endian                                                               |
| 0x12  | SysTimeType    | System time (128-bit) Stored in little-endian                                                           |
| 0x13  | SidType        | NT Security Identifier (SID) See `[NTSID]`                                                              |
| 0x14  | HexInt32Type   | 32-bit integer hexadecimal 32-bit (unsigned) integer that should be represented in hexadecimal notation |
| 0x15  | HexInt64Type   | 64-bit integer hexadecimal 64-bit (unsigned) integer that should be represented in hexadecimal notation |
|       |                |                                                                                                         |
| 0x20  | EvtHandle      | ​**Unknown**                                                                                            |
| 0x21  | BinXmlType     | Binary XML fragment                                                                                     |
|       |                |                                                                                                         |
| 0x23  | EvtXml         | ​**Unknown**                                                                                            |

If the MSB of the value type (0x80) is use to indicate an array type. According to `[MSDN]` binary data and binary XML fragment types are not supported. For the string types the end-of-string character is used as a separator.

| Value | Identifier | Description                                                                                                                                     |
| ----- | ---------- | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| 0x81  |            | Array of Unicode strings Individual strings are stored as UTF-16 little-endian with an end-of-string character                                  |
| 0x82  |            | Array of ASCII strings Individual strings are stored as ASCII string using a codepage with an end-of-string character                           |
| 0x83  |            | Array of 8-bit integer signed Every 1 byte is an individual value                                                                               |
| 0x84  |            | Array of 8-bit integer unsigned Every 1 byte is an individual value                                                                             |
| 0x85  |            | Array of 16-bit integer signed Every 2 bytes are an individual value in little-endian                                                           |
| 0x86  |            | Array of 16-bit integer unsigned Every 2 bytes are an individual value in little-endian                                                         |
| 0x87  |            | Array of 32-bit integer signed Every 4 bytes are an individual value in little-endian                                                           |
| 0x88  |            | Array of 32-bit integer unsigned Every 4 bytes are an individual value in little-endian                                                         |
| 0x89  |            | Array of 64-bit integer signed Every 8 bytes are an individual value in little-endian                                                           |
| 0x8a  |            | Array of 64-bit integer unsigned Every 8 bytes are an individual value in little-endian                                                         |
| 0x8b  |            | Array of Floating point 32-bit (single precision) Every 4 bytes are an individual value in little-endian                                        |
| 0x8c  |            | Array of Floating point 64-bit (double precision) Every 8 bytes are an individual value in little-endian                                        |
| 0x8d  |            | ​**Array of boolean****Every 4 bytes are an individual value in little-endian**                                                                 |
|       |            |                                                                                                                                                 |
| 0x8f  |            | Array of GUID Every 16 bytes are an individual value in little-endian                                                                           |
| 0x90  |            | Array of size type An individual value is either 32 or 64-bits. This value type should be pair up with an array of HexInt32Type or HexInt64Type |
| 0x91  |            | Array of FILETIME Every 8 bytes are an individual value in little-endian                                                                        |
| 0x92  |            | Array of system time Every 16 bytes are an individual value in little-endian                                                                    |
| 0x93  |            | ​**Array of NT Security Identifiers (SID)**                                                                                                     |
| 0x94  |            | Array of 32-bit integer hexadecimal Every 4 bytes are an individual value in little-endian                                                      |
| 0x95  |            | Array of 64-bit integer hexadecimal Every 8 bytes are an individual value in little-endian                                                      |
