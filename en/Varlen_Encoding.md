# Varlen Encoding

**Varlen Encoding** (short for "variable length") is a way of storing a QWord so that small values take as few bytes as possible. 

## Contents

  * 1 Inspiration
  * 2 Format
  * 3 Suitability
  * 4 Signed Varlen
    * 4.1 Format



## Inspiration

The inspiration and design of Varlen encoding came from two sources: 

  * UTF-8 encoding
  * The [CompactIndex type](<https://wiki.beyondunreal.com/Legacy:Package_File_Format/Data_Details#Index.2FCompactIndex_values>) used in old versions of the Unreal Engine.



Some data fields are statistically more likely to contain small values, such as those used for sequential indices and string lengths, but whose upper limit may be the size of a LongWord or a QWord. If many hundreds of these values are stored, space can be quickly wasted when most of the bytes that make up the full integer value are zero, and this can be problematic on media with limited capacity. The design of Varlen Encoding is an attempt to reduce this wastage with minimal performance impact. 

## Format

A Varlen-encoded integer (henceforth just called a **Varlen**) takes between 1 and 9 bytes to store. The first byte, known as a **Lead Byte** , encodes a byte count and the most significant bits of the integer, stored in big-endian order. The bit pattern of the Lead Byte dictates the number of **Data Bytes** that follow, which contain the rest of the bits for the stored integer: 

Lead Byte  | Value Range  | Description   
---|---|---  
`**0#######**` | `$00..$7F` | No data bytes; **#'** s encode a 7-bit value between $00 to $7F (0 to 127)   
`**10######**` | `$0080..$407F` | 1 data byte follows; **#'** s encode a 14-bit value offset by $80 (add $80 to the value to obtain the actual integer)   
`**110#####**` | `$004080..$20407F` | 2 data bytes follow; **#'** s encode a 21-bit value offset by $4080   
`**1110####**` | `$00204080..$1020407F` | 3 data bytes follow; **#'** s encode a 28-bit value offset by $204080   
`**11110###**` | `$00 10204080..$08 1020407F` | 4 data bytes follow; **#'** s encode a 35-bit value offset by $10204080   
`**111110##**` | `$0008 10204080..$0408 1020407F` | 5 data bytes follow; **#'** s encode a 42-bit value offset by $08 10204080   
`**1111110#**` | `$000408 10204080..$020408 1020407F` | 6 data bytes follow; **#'** s encode a 49-bit value offset by $0408 10204080   
`**11111110**` | `$00020408 10204080..$01020408 1020407F` | 7 data bytes follow; **#'** s encode a 56-bit value offset by $020408 10204080   
`**11111111**` | `$01020408 10204080..$FFFFFFFF FFFFFFFF` | 8 data bytes follow; **#'** s encode a 64-bit value offset by $01020408 10204080   
  
It is permissible to remove the offsets without sacrificing the full QWord domain. The use of offsets remove multiple encodings for the same value (e.g. **00000000** and **10000000 00000000** for zero) and slightly increase the chance that an integer is stored in fewer bytes (e.g. 16,384, or $4000, would otherwise take 3 bytes to store without the offset (as **11000000 00000000 00000000**), whereas with the offset, it only takes 2 bytes (as **10111111 10000000**)). 

## Suitability

As stated earlier, Varlen Encoding is best suited for situations where the field is more likely to contain a small value over a large one. As a result, not all LongWord and QWord values should be stored as Varlens because an extra byte is required to store the largest of values. For example, the following field types are good candidates for Varlen Encoding: 

  * Index values
  * String and buffer lengths
  * Counts
  * Offsets (as long as they are strictly positive)



Conversely, the following would make for poor candidates: 

  * 32-bit pointers
  * Hashes
  * Signed integers (since negative numbers map onto the largest positive integers)



## Signed Varlen

The regular Varlen can only encode unsigned integers, and while two's complement permits negative numbers to map over the top half of an unsigned integer domain, these values take up the most bytes under Varlen Encoding and so are likely to increase storage requirements if negative numbers close to zero are frequent. Therefore, for signed integers, a different encoding system is required. 

### Format

As with the regular Varlen, a Signed Varlen contains a **Lead Byte** that dictates the number of **Data Bytes** that follows, but the Lead Byte now also contains a sign bit (unless the Lead Byte is `**11111110**` or `**11111111**`, in which case it is present on the first Data Byte instead). 

Positive Values  Lead Byte  | Value Range  | Description   
---|---|---  
`**00######**` | `$00..$3F` | No data bytes; **#'** s encode a 6-bit value between $00 to $3F (0 to 63)   
`**100#####**` | `$0040..$203F` | 1 data byte follows; **#'** s encode a 13-bit value offset by $40 (add $40 to the value to obtain the actual integer)   
`**1100####**` | `$002040..$10203F` | 2 data bytes follow; **#'** s encode a 20-bit value offset by $2040   
`**11100###**` | `$00102040..$0810203F` | 3 data bytes follow; **#'** s encode a 27-bit value offset by $102040   
`**111100##**` | `$00 08102040..$04 0801023F` | 4 data bytes follow; **#'** s encode a 34-bit value offset by $08102040   
`**1111100#**` | `$0004 08102040..$0204 0810203F` | 5 data bytes follow; **#'** s encode a 41-bit value offset by $0408 10204000   
`**11111100**` | `$000204 08102040..$010204 0810203F` | 6 data bytes follow; **#'** s encode a 48-bit value offset by $020408 10204000   
`**11111110 0#######**` | `$010204 08102040..$810204 0810203F` | 7 data bytes follow; **#'** s encode a 55-bit value offset by $01020408 10204000  
Most significant bit of the first Data Byte must be **0**.   
  
For negative values, the data bits are all inverted. This allows quick and easy combination of individual bytes to the result via the **xor** operation. 

Negative Values  Lead Byte  | Value Range  | Description   
---|---|---  
`**01######**` | `$FFFFFFFF FFFFFFC0..$FFFFFFFF FFFFFFFF` | No data bytes; **#'** s encode an inverted 6-bit value between -64 to -1   
`**101#####**` | `$FFFFFFFF FFFFDFC0..$FFFFFFFF FFFFFFBF` | 1 data byte follows; **#'** s encode an inverted 13-bit value offset by -$40 (**subtract** $40 to the value to obtain the actual integer)   
`**1101####**` | `$FFFFFFFF FFEFDFC0..$FFFFFFFF FFFFDFBF` | 2 data bytes follow; **#'** s encode an inverted 20-bit value offset by -$2040   
`**11101###**` | `$FFFFFFFF F7EFDFC0..$FFFFFFFF FFEFDFBF` | 3 data bytes follow; **#'** s encode an inverted 27-bit value offset by -$102040   
`**111101##**` | `$FFFFFFFB F7EFDFC0..$FFFFFFFF F7EFDFBF` | 4 data bytes follow; **#'** s encode an inverted 34-bit value offset by -$08102040   
`**1111101#**` | `$FFFFFDFB F7EFDFC0..$FFFFFFFB F7EFDFBF` | 5 data bytes follow; **#'** s encode an inverted 41-bit value offset by -$0408 10204000   
`**11111101**` | `$FFFEFDFB F7EFDFC0..$FFFFFDFB F7EFDFBF` | 6 data bytes follow; **#'** s encode an inverted 48-bit value offset by -$020408 10204000   
`**11111110 1#######**` | `$FF7EFDFB F7EFDFC0..$FFFEFDFB F7EFDFBF` | 7 data bytes follow; **#'** s encode an inverted 55-bit value offset by -$01020408 10204000  
Most significant bit of the first Data Byte must be **1**.   
  
Finally, if the Lead Byte is `**11111111**`, then 8 Data Bytes follow and they directly encode an Int64 in big-endian order with no offset or inversion. This is an edge case and is faster than having to compute different offsets that depend purely on the sign bit.

---

_Source: [https://wiki.freepascal.org/Varlen_Encoding](https://web.archive.org/web/20231130201909/https://wiki.freepascal.org/Varlen_Encoding)_
