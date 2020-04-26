OpenMP

Nesting of target regions, either dynamically or statically, is not allowed.
General mapping rules are as follows:
Pointers are mapped as zero-length array sections with zero base for both explicit and implicit mapping.
If a zero-length array section that is derived from a pointer variable is mapped, that variable is initialized with the address of the corresponding storage location on the device. If the corresponding storage does not exist, that is, it has not been mapped before, the pointer variable is initialized to NULL.
The data environment of a target region is defined by the implicit and explicit mapping of variables between the host and device:
Implicit mapping
The compiler determines which variables must be mapped to, from, or both to and from the device data environment. Scalar variables that are not explicitly mapped are implicitly mapped as firstprivate if defaultmap(tofrom:scalar) is not specified.
Explicit mapping
You can use the map clause on the target region to explicitly list variables to be mapped to, from, or both to and from the device data environment.
A listed data variable cannot appear in both a data-sharing clause and the map clause on the same target construct.
