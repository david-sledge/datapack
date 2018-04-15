# DataPack Specification

Inspired by [MessagePack](https://msgpack.org).

format name | first byte (in binary) | first byte (in hex) | first byte (in decimal)
----------- | ---------------------- | ------------------- | -
positive fixint | 00xx xxxx | 0x00–0x3f | 0–63
nil | 0100 0000 | 0x40 | 64
collection end | 0100 0001 | 0x41 | 65
false | 0100 0010 | 0x42 | 66
true | 0100 0011 | 0x43 | 67
int 8 | 0100 0100 | 0x44 | 68
int 16 | 0100 0101 | 0x45 | 69
int 32 | 0100 0110 | 0x46 | 70
int 64 | 0100 0111 | 0x47 | 71
float 32 | 0100 1000 | 0x48 | 72
float 64 | 0100 1001 | 0x49 | 73
bin 8 | 0100 1010 | 0x4a | 74
bin 16 | 0100 1011 | 0x4b | 75
bin 32 | 0100 1100 | 0x4c | 76
string 8 | 0100 1101 | 0x4d | 77
string 16 | 0100 1110 | 0x4e | 78
string 32 | 0100 1111 | 0x4f | 79
namespace 8 | 0101 0000 | 0x50 | 80
namespace 16 | 0101 0001 | 0x51 | 81
namespace 32 | 0101 0010 | 0x52 | 82
class name | 0101 0011 | 0x53 | 83
sequence | 0101 0100 | 0x54 | 84
assortment | 0101 0101 | 0x55 | 85
object | 0101 0110 | 0x56 | 86
no key/value | 0101 0111 | 0x57 | 87
*unused* | 0101 1000–0101 1111 | 0x58–0x5f | 88–95
fixbin | 011x xxxx | 0x60–0x7f | 96–127
fixstring | 100x xxxx | 0x80–0x9f | 128–159
fixnamespace | 101x xxxx | 0xa0–0xbf | 160–191
negative fixint | 11xx xxxx | 0xc0–0xff | 192–255 (−64–−1)

## Value Formats
### Primitives
#### Nil
This is a unit type.  It is bottom for the specification.
#### Boolean
Binary value of true or false.
#### Numbers
##### Integers
Signed integers of up to 8 bytes.
##### Floating Points
Single and double precision.
### Bin
This represents raw binary data.  Fixbin, bin 8, 16, and 32.  This data type can also be used to represent property and class local names (see below).
### String
This represents a UTF-8 string.  Fixstring, string 8, 16, and 32.  This data type can also be used to represent property and class local names (see below).
### Collections
#### Sequence
Sequence of any values; numbers, booleans, nil, sequences, assortments, binary data, and/or objects.  Sequences support an optional class name.
#### Assortment
##### Assortment Element
Key, value, or key/value pair.  Assortments can be used to represent maps, sequences, sets, or a combination thereof.  Keys and values can be any value pack type: number, boolean, nil, binary data, sequence, assortment, or objects.
#### Object
Objects support an optional class name.  A class name consists of an optional namespace and a required local name.  The object format byte must be followed by either a class name format byte, or a series of zero or more properties (qualified names, values, or qualified name/value pairs).  An object is terminated by a collection end format byte.  If a class name byte is supplied, it must be followed by either a UTF-8 string (represented by a binary data block) or a namespace and then a UTF-8 string.  Following the string must be a series of zero or more properties.

0101 0110 [0101 0100 [namespace] binary] (0 or more properties) 0100 0001
##### Property
Each property consists of a qualified name, a value, or a qualified name/value pair.  A qualified name is an optional namespace and a UTF-8 string (binary data block) which represents the local name.
#### Class Name
The class name byte is followed by a qualified name.
#### Qualified Name
A qualified name consists of an optional namespace name and a required local name.  The local name is represented by binary data block and itself represents a UTF-8 string.
#### Namespace
Fixnamespace, namespace 8, 16, and 32.  UTF-8
#### Collection End
Marks the end of the current sequence, object, or assortment.  Required for every collection.
#### No Key/Value Format
*TBD*
