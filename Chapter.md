# Application Binary Interface

The other chapters of the MPI standard specify an Application Programminng Interface (API)
that defines (amongst other things)
a set of opaque handle types and named constants without specifying their memory
layout or values, respectively.
This allows implementations to choose these according different types of requirement.
However, this flexibility means that different implementations are incompatible
from the perspective of compiled applications, because the Application Binary Interface (ABI)
is not specified.

This chapter defines an Application Binary Interface (ABI) for MPI, meaning that it specifies
the memory layouts of all opaque handle types, the values of integer constants and other aspects
of MPI needed by use cases that require a defined ABI.
This standardized ABI for MPI exists in parallel with existing implementation ABIs, in order to
preserve backwards-compatibility of existing MPI implementations.

A standard ABI for MPI serves many purposes, including support for third-party languages
that intend to call MPI directly from the ABI, rather than via the API using C code, for example.

## The MPI ABI header file and shared library

The ABI should be implemented using a header named `mpi.h`, to ensure that users
can adopt the ABI without code changes.  If another `mpi.h` that defines
a different ABI already exists, implementations should document the locations
and behavors of these headers and associated object files.

For example, an implementation may have its own ABI defined in `path/include/mpi.h`
and `path/lib/libmpi.so`, in which case, `path/abi/include/mpi.h` and
`path/abi/lib/libmpi.so` indicate to the user that these files are associated with
the MPI standard ABI.

## The Status Object

The `MPI_Status` object is a 256-bit struct containing 8 integers: the three
public member fields described in Section 3.2.5 ("Return Status") and
5 private member fields that are reserved for implementations and must never be
directly accessed or set by applications.

## Opaque Handles

Handles for MPI objects are defined to be incomplete struct pointers,
which allows for C compilers to do type-checking, while also satisfying
the existing requirements, such as equality comparison.

Rationale: _Integer handles do not provide type safety, while `struct`
or `union` handles fail to satisfy the existing API requirements._

For example, the following handle type definitions are part of the MPI ABI:
```c
typedef struct mpi_abi_comm * MPI_Comm;
typedef struct mpi_abi_request * MPI_Request;
...
```

## Handle constants

Every handle type has at least one, and often many, predefined constants of that type.
For example, `MPI_Request` has `MPI_REQUEST_NULL`, `MPI_Comm` has `MPI_COMM_WORLD`, etc.
The MPI ABI defines handle constants to be compile-time constants, which are specified
as integers.

Rationale: _Link-time constants, while convenient, are not strictly portable._

All predefined handle constants correspond to integer representations that are not
valid addresses.  Implementations must ensure that the handles they create for the user
are never in the range reserved for predefined handle constants.
The MPI ABI reservers addresses corresponding to the integers 1 to 4095 for predefined
constants.

Advice to implementers: _Many operating systems support a "zero page" that corresponds
to the above address range, in which case, implementations will not need to do any
runtime checking to ensure the above requirement is satisfied._

## Integer Constants

Integer constants fall into groups, where all groups must have unique
values.  To enforce unique values, groups of integer constants are defined in
anonymous unions.

In cases where integer constants are combined using bitwise logical
expressions, there are additional requirements, specified elsewhere
(e.g. Section 14.2 under `MPI_File_open`).

A different constraint
exists in cases like `MPI_ANY_SOURCE`, which must be negative, because
any non-negative integer may be a valid rank.

All of the constants are specified in the Appendix.

## Integer Types

MPI defines three special types of integers in C,
`MPI_Aint`, `MPI_Offset` and `MPI_Count`.
These types depend on the underlying platform ABI.
For example, `MPI_Aint` must be able to hold both the absolute value
of an address and the signed difference of two addresses.
The MPI ABI requires that this integer type be a signed
integer that is the same size as an address.

Advice to implementers: _The use of C standard types
designed to satisfy the above properties are strongly encouraged.
For example, _`intptr_t`_ is an integer that is guarenteed to be able
to store an address._

`MPI_Offset` pertains to file I/O and depends on the underlying filesystem.
In some cases, filesystems support offsets that are larger than the
maximum difference of two locations in memory.
If `MPI_Offset` is defined in terms of the filesystem offset type itself,
it is possible to have two MPI ABIs on the same platform, depending only
on the filesystem used.  This is underdesireable and implementations
are strongly encouraged to define `MPI_Offset` to not change the size
of this value based on the filesystem.  If this occurs, implementations
must document it clearly.

Advice to implementers: _On platforms wtih 64-bit
addresses, _`MPI_Offset`_ should be a 64-bit integer.  If the underlying filesystem
is larger, individual _`MPI_Files`_ will be limited to 2^63 bytes._

`MPI_Count` is required to hold values of `MPI_Aint` and `MPI_Offset`.
The restrictions and recommendations above carry forward here.

## Calling Conventions and Binary Representations

The MPI ABI is specified via C language constructs and
the actual binary properties of an implementation must behave as if
the source code is compiled by the system C compiler on the platform.
This ensures that MPI implementations will interoperate with other
software installed on the system that is build with the system C compiler,
or a C compiler that is ABI compatible with it.

Users must ensure that the compiler they use to compile their application
is ABI-compatible with the system C compiler.

## Fortran

If we standardize an ABI based on MPI F08, discuss it here.


