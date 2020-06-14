# DataPack Specification

Inspired by [MessagePack](https://msgpack.org).

format name | first byte (in binary) | first byte (in hex) | first byte (in decimal)
:----------- | :---------------------- | :------------------- | :-
positive fixint | 00xx xxxx | 0x00–0x3f | 0–63
nil | 0100 0000 | 0x40 | 64
collection end | 0100 0001 | 0x41 | 65
false | 0100 0010 | 0x42 | 66
true | 0100 0011 | 0x43 | 67
uint 8 | 0100 0100 | 0x44 | 68
uint 16 | 0100 0101 | 0x45 | 69
uint 32 | 0100 0110 | 0x46 | 70
uint 64 | 0100 0111 | 0x47 | 71
int 8 | 0100 1000 | 0x48 | 72
int 16 | 0100 1001 | 0x49 | 73
int 32 | 0100 1010 | 0x4a | 74
int 64 | 0100 1011 | 0x4b | 75
float 32 | 0100 1100 | 0x4c | 76
float 64 | 0100 1101 | 0x4d | 77
bin 8 | 0100 1110 | 0x4e | 78
bin 16 | 0100 1111 | 0x4f | 79
bin 32 | 0101 0000 | 0x50 | 80
string 8 | 0101 0001 | 0x51 | 81
string 16 | 0101 0010 | 0x52 | 82
string 32 | 0101 0011 | 0x53 | 83
namespace 8 | 0101 0100 | 0x54 | 84
namespace 16 | 0101 0101 | 0x55 | 85
namespace 32 | 0101 0110 | 0x56 | 86
class name | 0101 0111 | 0x57 | 87
sequence | 0101 1000 | 0x58 | 88
dictionary | 0101 1001 | 0x59 | 89
object | 0101 1010 | 0x5a | 90
*unused* | 0101 1011–0101 1111 | 0x5b–0x5f | 91–95
fixbin | 011x xxxx | 0x60–0x7f | 96–127
fixstring | 100x xxxx | 0x80–0x9f | 128–159
fixnamespace | 101x xxxx | 0xa0–0xbf | 160–191
negative fixint | 11xx xxxx | 0xc0–0xff | 192–255 (−64–−1)

## Value Formats
### Primitives
#### Nil
This is a unit type. It is bottom for the specification. It can be used in place of a property name of an object.

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
#### Qualified name
Qualified names are use for class names and Object property names. A qualified name consists of an optional namespace name and a required local name. The local name is represented as either a string or a binary data block.

#### Class name
A class name is indicated by the class name byte followed by a qualified name.

#### Sequence
Sequence of any values; numbers, booleans, nil, strings, binary data, sequences, dictionaries, and/or objects. Sequences support an optional class name, which if specified must appear immediately after the sequence byte.

#### Dictionary
A dictionary is a series Key/value pairs. Keys and values can be any value pack type: number, boolean, nil, string, binary data, sequence, dictionary, or object.

#### Object
An object is a series of zero or more qualified name/value pairs. Nil may be used in place of a qualified name. Objects support an optional class name, which if specified must appear immediately after the object byte.

0101 0110 [0101 0100 [namespace] binary] (0 or more properties) 0100 0001

#### Namespace
Fixnamespace, namespace 8, 16, and 32. UTF-8 encoding.

#### Collection End
Marks the end of the current sequence, object, or dictionary.  Required for every collection.
