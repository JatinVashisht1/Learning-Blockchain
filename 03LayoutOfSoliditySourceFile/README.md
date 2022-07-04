# Layout of a Solidity Source File
- Understanding the different components of a solidity source file.
## Before we start
- This blog is inspired from official solidity documentation.
- Read the official docs by clicking [here](https://docs.soliditylang.org/en/v0.8.15/layout-of-source-files.html)
- A solidity source file can contain anything from a contract definition, function, variable, etc.
- In this blog we will try to breakdown different parts of solidity source file.

## SPDX License Identifier 
- Writing source code might touch some legal problems due to the copyright issues.
- It is adviced to use machine readable `SPDX license identifiers`.
- Therefore it is adviced that every solidity file should start with a SPDX identifier.
- The way of writing SPDX identifier is as follows
```
// SPDX-License-Identifier: MIT
```
- Although compiler will identify this comment at any place in the file, it is adviced to write it at the `top` of the source file.

## Pragmas
- `pragma` keyword is used to enable certain compiler features or checks.
- A pragma directive is local to a source file, so if you want to make a set of features enable globally you have to add the pragma directive in all of the project files.
- If you import another file pragma of that file is not automatically applied to the importing file.

### Version Pragma
- A solidity source file should be annotated witha a version pragma so that it can reject compilations with future compiler versions.
- It is because in future some breaking changes could have been introduced that can prevent the program from doing for what it was intended to do.
- Solidity use [semantic versioning](https://semver.org/), the releases that introduce breaking changes are `0.x.0` and `x.0.0`.
- The `syntax` of writing a version pragma is as follows
```
pragma solidity ^0.5.2
```
- Let us understand what does it means
- This statement will tell the compiler and reader for which version the program was written for.
- This is making it clear that this program will not work for compiler version before 0.5.2 and will not work as intended for version 0.6.0
- The condition that this code will not work for compiler version 0.6.0 and later is added by `^` before the versioning
- So the thing this pragma will do is to issue an error while trying to run the code in compiler version other than it is intended to.

### ABI Coder pragma
- By using `pragma abicoder v1` and `pragma abicoder v2` you can select between two implementationf of ABI encoder and decoder.
- After `Solidity 0.8.0` v2 of ABI encoder and decoder will be used by default but there is still option to use v1 if you want.
- With version v2 it is possible to use it to use structs, nested and dynamic variables to be passed to functions, etc.
- V2 of the encoder is the superset of v1 so everything supported in v1 is valid in v2 also.

### Experimental Pragma
- The experimental pragma is used to enable features of compiler or language that are yet experimental and are not enabled by default.
- Since these features are experimental it is NOT recommended to use these in production code.
- As of July 2022, `SMTChecker` is experimental feature, you can enable it by using `pragma experimental SMTChecker`. This will show some additional safety warnings which are obtained by querying an SMT solver.
- If you are beginner there is not need to read about SMTChecker as of now.

## Importing other Source Files
- Importing files in solidity is inspired from ES6 but there is no concept of `default export`.
- You can import a file in global context of importing file by using following syntax.
```
import "filename"
```
- This will import all the global symbols from "filename" into the current global scope of the file.
- This format is not recommended to use because it can unpredictably pollutes the namespace.
- If you add a new top level items inside "filename", they will automatically appear in all files that are importing this file in the above manner.
- It is better to import specific symbols explicitly
- The following example is creating a new symbol whose members are all global symbols of "filename"
```
import * as symbolName from "filename";
// the above variant can also be written as
import "filename" as symbolName
```
- you can rename symbols while importing symbols from other file.
```
import {symbol as alias, symbol2} from "filename"
```
- This is also helpful when we may have naming collisions between symbols.

### Import Paths
- I am skipping this part as of now, because it might be overwhelming for beginners.
- What I am planning to do is to write another blog dedicating to this topic and explaining it in detail.
- you can read about it in breif [here](https://docs.soliditylang.org/en/v0.8.15/layout-of-source-files.html) if you still want to read about it.

## Comments
- Single-line (//) and multi-line (/*...*/) both are possible.
```
// this is a single-line comment
/*
this is a
multiline comment
*/
``` 
- There is another type of comment called `NatSpec` comment.
- They are written with tripple slash (///) or a double astrisk block (/** ... **/) and they should be used directly above declarations or statements.
- They are special form of comments to provide rich documentation for functions, return variables and more.
- These are used to provide messages which will be seen by end-user interacting with contract.
- They will be shown when the user is asked to confirm a transaction or when an error is displayed.
- You can read more about them [here](https://docs.soliditylang.org/en/v0.8.15/natspec-format.html#natspec)