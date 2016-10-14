### 0.4.3 (unreleased)

Features:

 * Inline assembly: support both `sucide` and `selfdestruct` opcodes
   (note: `suicide` is deprecated)
 * Include `keccak256()` as an alias to `sha3()`

Bugfixes:
 * Disallow unknown options in `solc`
 * Proper type checking for bound functions.
 * Inline assembly: support the `address` opcode
 * Inline assembly: fix parsing of assignment after a label.

### 0.4.2 (2016-09-17)

Bugfixes:

 * Code Generator: Fix library functions being called from payable functions.
 * Type Checker: Fixed a crash about invalid array types.
 * Code Generator: Fixed a call gas bug that became visible after
   version 0.4.0 for calls where the output is larger than the input.

### 0.4.1 (2016-09-09)

 * Build System: Fixes to allow library compilation.

### 0.4.0 (2016-09-08)

This release deliberately breaks backwards compatibility mostly to
enforce some safety features. The most important change is that you have
to explicitly specify if functions can receive ether via the ``payable``
modifier. Furthermore, more situations cause exceptions to be thrown.

Minimal changes to be made for upgrade:
 - Add ``payable`` to all functions that want to receive Ether
   (including the constructor and the fallback function).
 - Change ``_`` to ``_;`` in modifiers.
 - Add version pragma to each file: ``pragma solidity ^0.4.0;``

Breaking Changes:

 * Source files have to specify the compiler version they are
   compatible with using e.g. ``pragma solidity ^0.4.0;`` or
   ``pragma solidity >=0.4.0 <0.4.8;``
 * Functions that want to receive Ether have to specify the
   new ``payable`` modifier (otherwise they throw).
 * Contracts that want to receive Ether with a plain "send"
   have to implement a fallback function with the ``payable``
   modifier. Contracts now throw if no payable fallback
   function is defined and no function matches the signature.
 * Failing contract creation through "new" throws.
 * Division / modulus by zero throws.
 * Function call throws if target contract does not have code
 * Modifiers are required to contain ``_`` (use ``if (false) _`` as a workaround if needed).
 * Modifiers: return does not skip part in modifier after ``_``.
 * Placeholder statement `_` in modifier now requires explicit `;`.
 * ``ecrecover`` now returns zero if the input is malformed (it previously returned garbage).
 * The ``constant`` keyword cannot be used for constructors or the fallback function.
 * Removed ``--interface`` (Solidity interface) output option
 * JSON AST: General cleanup, renamed many nodes to match their C++ names.
 * JSON output: ``srcmap-runtime`` renamed to ``srcmapRuntime``.
 * Moved (and reworked) standard library contracts from inside the compiler to github.com/ethereum/solidity/std
   (``import "std";`` or ``import owned;`` do not work anymore).
 * Confusing and undocumented keyword ``after`` was removed.
 * New reserved words: ``abstract``, ``hex``, ``interface``, ``payable``, ``pure``, ``static``, ``view``.

Features:

 * Hexadecimal string literals: ``hex"ab1248fe"``
 * Internal: Inline assembly usable by the code generator.
 * Commandline interface: Using ``-`` as filename allows reading from stdin.
 * Interface JSON: Fallback function is now part of the ABI.
 * Interface: Version string now *semver* compatible.
 * Code generator: Do not provide "new account gas" if we know the called account exists.

Bugfixes:

 * JSON AST: Nodes were added at wrong parent
 * Why3 translator: Crash fix for exponentiation
 * Commandline Interface: linking libraries with underscores in their name.
 * Type Checker: Fallback function cannot return data anymore.
 * Code Generator: Fix crash when ``sha3()`` was used on unsupported types.
 * Code Generator: Manually set gas stipend for ``.send(0)``.

Lots of changes to the documentation mainly by voluntary external contributors.

### 0.3.6 (2016-08-10)

Features:

 * Formal verification: Take external effects on a contract into account.
 * Type Checker: Warning about unused return value of low-level calls and send.
 * Output: Source location and node id as part of AST output
 * Output: Source location mappings for bytecode
 * Output: Formal verification as part of json compiler output.

Bugfixes:

 * Commandline Interface: Do not crash if input is taken from stdin.
 * Scanner: Correctly support unicode escape codes in strings.
 * JSON output: Fix error about relative / absolute source file names.
 * JSON output: Fix error about invalid utf8 strings.
 * Code Generator: Dynamic allocation of empty array caused infinite loop.
 * Code Generator: Correctly calculate gas requirements for memcpy precompile.
 * Optimizer: Clear known state if two code paths are joined.

### 0.3.5 (2016-06-10)

Features:

 * Context-dependent path remappings (different modules can use the same library in different versions)

Bugfixes:

 * Type Checking: Dynamic return types were removed when fetching data from external calls, now they are replaced by an "unusable" type.
 * Type Checking: Overrides by constructors were considered making a function non-abstract.

### 0.3.4 (2016-05-31)

No change outside documentation.

### 0.3.3 (2016-05-27)

 * Allow internal library functions to be called (by "inlining")
 * Fractional/rational constants (only usable with fixed point types, which are still in progress)
 * Inline assembly has access to internal functions (as jump labels)
 * Running `solc` without arguments on a terminal will print help.
 * Bugfix: Remove some non-determinism in code generation.
 * Bugfix: Corrected usage of not / bnot / iszero in inline assembly
 * Bugfix: Correctly clean bytesNN types before comparison

### 0.3.2 (2016-04-18)

 * Bugfix: Inline assembly parser: `byte` opcode was unusable
 * Bugfix: Error reporting: tokens for variably-sized types were not converted to string properly
 * Bugfix: Dynamic arrays of structs were not deleted correctly.
 * Bugfix: Static arrays in constructor parameter list were not decoded correctly.

### 0.3.1 (2016-03-31)

 * Inline assembly
 * Bugfix: Code generation: array access with narrow types did not clean higher order bits
 * Bugfix: Error reporting: error reporting with unknown source location caused a crash

### 0.3.0 (2016-03-11)

BREAKING CHANGES:

 * Added new keywords `assembly`, `foreign`, `fixed`, `ufixed`, `fixedNxM`, `ufixedNxM` (for various values of M and N), `timestamp`
 * Number constant division does not round to integer, but to a fixed point type (e.g. `1 / 2 != 1`, but `1 / 2 == 0.5`).
 * Library calls now default to use DELEGATECALL (e.g. called library functions see the same value as the calling function for `msg.value` and `msg.sender`).
 * `<address>.delegatecall` as a low-level calling interface

Bugfixes:
 * Fixed a bug in the optimizer that resulted in comparisons being wrong.


### 0.2.2 (2016-02-17)

 * Index access for types `bytes1`, ..., `bytes32` (only read access for now).
 * Bugfix: Type checker crash for wrong number of base constructor parameters.

### 0.2.1 (2016-01-30)

 * Inline arrays, i.e. `var y = [1,x,f()];` if there is a common type for `1`, `x` and `f()`. Note that the result is always a fixed-length memory array and conversion to dynamic-length memory arrays is not yet possible.
 * Import similar to ECMAScript6 import (`import "abc.sol" as d` and `import {x, y} from "abc.sol"`).
 * Commandline compiler solc automatically resolves missing imports and allows for "include directories".
 * Conditional: `x ? y : z`
 * Bugfix: Fixed several bugs where the optimizer generated invalid code.
 * Bugfix: Enums and structs were not accessible to other contracts.
 * Bugfix: Fixed segfault connected to function paramater types, appeared during gas estimation.
 * Bugfix: Type checker crash for wrong number of base constructor parameters.
 * Bugfix: Allow function overloads with different array types.
 * Bugfix: Allow assignments of type `(x) = 7`.
 * Bugfix: Type `uint176` was not available.
 * Bugfix: Fixed crash during type checking concerning constructor calls.
 * Bugfix: Fixed crash during code generation concerning invalid accessors for struct types.
 * Bugfix: Fixed crash during code generating concerning computing a hash of a struct type.

### 0.2.0 (2015-12-02)

 * **Breaking Change**: `new ContractName.value(10)()` has to be written as `(new ContractName).value(10)()`
 * Added `selfdestruct` as an alias for `suicide`.
 * Allocation of memory arrays using `new`.
 * Binding library functions to types via `using x for y`
 * `addmod` and `mulmod` (modular addition and modular multiplication with arbitrary intermediate precision)
 * Bugfix: Constructor arguments of fixed array type were not read correctly.
 * Bugfix: Memory allocation of structs containing arrays or strings.
 * Bugfix: Data location for explicit memory parameters in libraries was set to storage.

### 0.1.7 (2015-11-17)

 * Improved error messages for unexpected tokens.
 * Proof-of-concept transcompilation to why3 for formal verification of contracts.
 * Bugfix: Arrays (also strings) as indexed parameters of events.
 * Bugfix: Writing to elements of `bytes` or `string` overwrite others.
 * Bugfix: "Successor block not found" on Windows.
 * Bugfix: Using string literals in tuples.
 * Bugfix: Cope with invalid commit hash in version for libraries.
 * Bugfix: Some test framework fixes on windows.

### 0.1.6 (2015-10-16)

 * `.push()` for dynamic storage arrays.
 * Tuple expressions (`(1,2,3)` or `return (1,2,3);`)
 * Declaration and assignment of multiple variables (`var (x,y,) = (1,2,3,4,5);` or `var (x,y) = f();`)
 * Destructuring assignment (`(x,y,) = (1,2,3)`)
 * Bugfix: Internal error about usage of library function with invalid types.
 * Bugfix: Correctly parse `Library.structType a` at statement level.
 * Bugfix: Correctly report source locations of parenthesized expressions (as part of "tuple" story).

### 0.1.5 (2015-10-07)

 * Breaking change in storage encoding: Encode short byte arrays and strings together with their length in storage.
 * Report warnings
 * Allow storage reference types for public library functions.
 * Access to types declared in other contracts and libraries via `.`.
 * Version stamp at beginning of runtime bytecode of libraries.
 * Bugfix: Problem with initialized string state variables and dynamic data in constructor.
 * Bugfix: Resolve dependencies concerning `new` automatically.
 * Bugfix: Allow four indexed arguments for anonymous events.
 * Bugfix: Detect too large integer constants in functions that accept arbitrary parameters.

### 0.1.4 (2015-09-30)

 * Bugfix: Returning fixed-size arrays.
 * Bugfix: combined-json output of solc.
 * Bugfix: Accessing fixed-size array return values.
 * Bugfix: Disallow assignment from literal strings to storage pointers.
 * Refactoring: Move type checking into its own module.

### 0.1.3 (2015-09-25)

 * `throw` statement.
 * Libraries that contain functions which are called via CALLCODE.
 * Linker stage for compiler to insert other contract's addresses (used for libraries).
 * Compiler option to output runtime part of contracts.
 * Compile-time out of bounds check for access to fixed-size arrays by integer constants.
 * Version string includes libevmasm/libethereum's version (contains the optimizer).
 * Bugfix: Accessors for constant public state variables.
 * Bugfix: Propagate exceptions in clone contracts.
 * Bugfix: Empty single-line comments are now treated properly.
 * Bugfix: Properly check the number of indexed arguments for events.
 * Bugfix: Strings in struct constructors.

### 0.1.2 (2015-08-20)

 * Improved commandline interface.
 * Explicit conversion between `bytes` and `string`.
 * Bugfix: Value transfer used in clone contracts.
 * Bugfix: Problem with strings as mapping keys.
 * Bugfix: Prevent usage of some operators.

### 0.1.1 (2015-08-04)

 * Strings can be used as mapping keys.
 * Clone contracts.
 * Mapping members are skipped for structs in memory.
 * Use only a single stack slot for storage references.
 * Improved error message for wrong argument count. (#2456)
 * Bugfix: Fix comparison between `bytesXX` types. (#2087)
 * Bugfix: Do not allow floats for integer literals. (#2078)
 * Bugfix: Some problem with many local variables. (#2478)
 * Bugfix: Correctly initialise `string` and `bytes` state variables.
 * Bugfix: Correctly compute gas requirements for callcode.

### 0.1.0 (2015-07-10)
