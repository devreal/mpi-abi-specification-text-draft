# Huffman Code

We reserve the range 0..1024 for predefined handles.
This requires 10 bits.
Using the following Huffman code allows us to define
unique values for every predefine handle, which is
useful for debugging purposes.
```
0123456789 < bit
0 is not datatype
 0 is op or reserved
  000 reserved
     00000 reserved (invalid)
     .....
  001 is op
     00 arithmetic
       000 SUM
       001 MIN
       010 MAX
       011 PROD
       ... reserved
     01 bit ops
       000 BAND
       001 BOR
       010 BXOR
       ... reserved
     10 logical ops
       000 LAND
       001 LOR
       010 LXOR
       ... reserved
     11 other ops
       0 M**LOC
        00 MINLOC
        01 MAXLOC
        .. reserved
       1 other
        0 RMA ops
         0 REPLACE
         1 NO_OP
        1 other
         0 MPI_OP_NULL
         1 reserved
  ... reserved 
     .... reserved
 1 other
  000000 comm
        00 MPI_COMM_NULL
        01 MPI_COMM_WORLD
        10 MPI_COMM_SELF
        11 reserved
  000001 group
        00 MPI_GROUP_NULL
        01 MPI_GROUP_EMPTY
        .. reserved
  000010 win
        00 MPI_WIN_NULL
        .. reserved
  000011 file
        00 MPI_FILE_NULL
        .. reserved
  000100 session
        00 MPI_SESSION_NULL
        .. reserved
  000101 message
        00 MPI_MESSAGE_NULL
        01 MPI_MESSAGE_NO_PROC
        .. reserved
  000110 errhandler
        00 MPI_ERRHANDLER_NULL
        01 MPI_ERRORS_ARE_FATAL
        10 MPI_ERRORS_RETURN
        11 MPI_ERRORS_ABORT
  000111 reserved
        .. reserved
  001000 request
        00 MPI_REQUEST_NULL
        .. reserved
  ...... reserved
1 is datatype -- See table below
```

| Datatype Flag | SizeField | ID     |                       |
|---------------|------|--------|-----------------------|
| 1             | 000  | 000000 | int8_t                |
| 1             | 000  | 000001 | uint8_t               |
| 1             | 000  | 000010 | F integer*1           |
| 1             | 000  | 000011 | C float8              |
| 1             | 000  | 000100 | F real*1              |
| 1             | 000  | 000101 | F logical*1           |
| 1             | 000  | 000110 | C bool                |
| 1             | 000  | 000111 | signed char           |
| 1             | 000  | 001000 | unsigned char         |
| 1             | 000  | 001001 | C++ bool              |
| 1             | 000  | 001010 |                       |
| 1             | 000  | 001011 |                       |
| 1             | 000  | 001100 |                       |
| 1             | 000  | 001101 |                       |
| 1             | 000  | 001110 |                       |
| 1             | 000  | 001111 |                       |
| 1             | 001  | 000000 | int16_t               |
| 1             | 001  | 000001 | uint16_t              |
| 1             | 001  | 000010 | F integer*2           |
| 1             | 001  | 000011 | C real16              |
| 1             | 001  | 000100 | F real*2              |
| 1             | 001  | 000101 | F logical*2           |
| 1             | 001  | 000110 | C complex8            |
| 1             | 001  | 000111 | C++ complex8          |
| 1             | 001  | 001000 | F complex*1           |
| 1             | 001  | 001001 |                       |
| 1             | 001  | 001010 |                       |
| 1             | 001  | 001011 |                       |
| 1             | 001  | 001100 |                       |
| 1             | 001  | 001101 |                       |
| 1             | 001  | 001110 |                       |
| 1             | 001  | 001111 |                       |
| 1             | 010  | 000000 | int32_t               |
| 1             | 010  | 000001 | uint32_t              |
| 1             | 010  | 000010 | F integer*4           |
| 1             | 010  | 000011 | C real32              |
| 1             | 010  | 000100 | F real*4              |
| 1             | 010  | 000101 | F logical*4           |
| 1             | 010  | 000110 | C complex half        |
| 1             | 010  | 000111 | C++ complex half      |
| 1             | 010  | 001000 | F complex*2           |
| 1             | 010  | 001001 |                       |
| 1             | 010  | 001010 |                       |
| 1             | 010  | 001011 |                       |
| 1             | 010  | 001100 |                       |
| 1             | 010  | 001101 |                       |
| 1             | 010  | 001110 |                       |
| 1             | 010  | 001111 |                       |
| 1             | 011  | 000000 | int64_t               |
| 1             | 011  | 000001 | uint64_t              |
| 1             | 011  | 000010 | F integer*8           |
| 1             | 011  | 000011 | C real64              |
| 1             | 011  | 000100 | F real*8              |
| 1             | 011  | 000101 | F logical*8           |
| 1             | 011  | 000110 | C complex float       |
| 1             | 011  | 000111 | C++ complex float     |
| 1             | 011  | 001000 | F complex*4           |
| 1             | 011  | 001001 |                       |
| 1             | 011  | 001010 |                       |
| 1             | 011  | 001011 |                       |
| 1             | 011  | 001100 |                       |
| 1             | 011  | 001101 |                       |
| 1             | 011  | 001110 |                       |
| 1             | 011  | 001111 |                       |
| 1             | 100  | 000000 | int128_t              |
| 1             | 100  | 000001 | uint128_t             |
| 1             | 100  | 000010 | F integer*16          |
| 1             | 100  | 000011 | F real*16             |
| 1             | 100  | 000100 | F logical*16          |
| 1             | 100  | 000101 | C complex double      |
| 1             | 100  | 000110 | C++ complex double    |
| 1             | 100  | 000111 | F complex*8           |
| 1             | 100  | 001000 |                       |
| 1             | 100  | 001001 |                       |
| 1             | 100  | 001010 |                       |
| 1             | 100  | 001011 |                       |
| 1             | 100  | 001100 |                       |
| 1             | 100  | 001101 |                       |
| 1             | 100  | 001110 |                       |
| 1             | 100  | 001111 |                       |
| 1             | 101  | 000000 | F complex*32          |
| 1             | 101  | ...... | reserved              |
| 1             | 110  | ...... | reserved              |
| 1             | 111  | 000000 | MPI_PACKED            |
| 1             | 111  | 000001 | MPI_DATATYPE_NULL     |
| 1             | 111  | 000010 | MPI_FLOAT_INT         |
| 1             | 111  | 000011 | MPI_DOUBLE_INT        |
| 1             | 111  | 000100 | MPI_LONG_INT          |
| 1             | 111  | 000101 | MPI_2INT              |
| 1             | 111  | 000110 | MPI_SHORT_INT         |
| 1             | 111  | 000111 | MPI_LONG_DOUBLE_INT   |
| 1             | 111  | 001000 | MPI_2REAL             |
| 1             | 111  | 001001 | MPI_2DOUBLE_PRECISION |
| 1             | 111  | 001010 | MPI_2INTEGER          |
| 1             | 111  | 001011 | short                 |
| 1             | 111  | 001100 | int                   |
| 1             | 111  | 001101 | long                  |
| 1             | 111  | 001110 | unsigned short        |
| 1             | 111  | 001111 | unsigned int          |
| 1             | 111  | 010000 | unsigned long         |
| 1             | 111  | 010001 | unsigned long long    |
| 1             | 111  | 010010 | F integer             |
| 1             | 111  | 010011 | MPI_AINT              |
| 1             | 111  | 010100 | MPI_COUNT             |
| 1             | 111  | 010101 | MPI_OFFSET            |
| 1             | 111  | 010110 | F real                |
| 1             | 111  | 010111 | F complex             |
| 1             | 111  | 011000 | F logical             |

Empty fields are reserved. The SizeField is the log2 of the size (except for `111`). The lookup table entry of the type can found as (16\*SizeField)+ID, except for types with SizeField `111` where currently a factor of (16\*5)+ID is used. I deliberately used `111`  for the platform-dependent types but that could be changed if we don't feel like we need the sizes `101` and `110` reserved. 

Error-checking can be done with some clever bit masks and range queries. I tried to group integer, floating point, complex, and logical, types to make such queries easier. I don't really care about optimizing that though. 

We have 71 entries (if I counted right) that could easily be put into a lookup table. Given the compact representation with 5 size groups, we could even put the datatype objects into a lookup table of a few dozen KB to save one indirection. There is ample space for future extensions in each of these groups.
