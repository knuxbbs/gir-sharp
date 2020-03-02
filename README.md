# gir-sharp

## Build Status

| Branch | Status |
|--------|--------|
| Master |[![Build Status](https://travis-ci.org/mono/gir-sharp.svg?branch=master)](https://travis-ci.org/mono/gir-sharp)|

## Generation implementations

- [x] Alias
- [ ] Array
- [x] Bitfield
- [ ] Callback
- [ ] Class - Stubbed out
- [ ] Constant
- [ ] Constructor - Stubbed out
- [x] Documentation - Done via `IndentWriter.WriteDocumentation`, needs a bit of rework (`IPreGeneratable` and do it automatically in `IGeneratable` extensions)
- [x] Enumeration
- [ ] Field
- [ ] Function
- [ ] Implements
- [x] Include
- [ ] InstanceParameter
- [ ] Interface - Stubbed out
- [x] Member - Done, but needs a bit more rework to hex formatting smarter and not hacky
- [ ] Method
- [x] Namespace
- [x] Package
- [ ] Parameter
- [ ] Prerequisite
- [ ] Property
- [ ] Record
- [x] Repository
- [ ] ReturnValue
- [x] Scope
- [ ] Signal
- [x] TransferOwnership
- [x] Type
- [ ] Union
- [x] Varargs
- [ ] VirtualMethod

## Generator Architecture

### Main Architecture

- Serialization Model
- Generation
- Marshalling
- Handle capabilities by interface (`IGeneratable` - generation, `ISymbol` - typesystem information)
- A way to override GIR data (if needed)
- Given a GIR file, process all the included GIR files Repository.Includes data

### Generation

- `IGeneratable` defines the capability to generate code
- Types which conform to IGeneratable will handle generation of the type and its members

### Typesystem

- `ISymbol` defines the capability of being used as a type and marshalling semantics
- `SymbolTable` maps the C type name to an `ISymbol` to know marshalling semantics
- Port over primitives (`SimpleGen`, `LP(U)Gen`) and improve naming `SimpleGen` → `PrimitiveGen`, `LPGen` → `LongGen`? (no idea about this one)

### Marshalling

- Default Marshalling (Pass by value) to / from `IPassByValue`, `IMarshalFromValue` / `IFromValue`?
- ByRef Marshalling (Pass by reference) to / from `IPassByReference`, `IMarshalFromReference` / `IFromReference`?
- Caller allocates case -> default allocation

### Better naming for marshalling

- `CallByName` (this one is really bad, this method used to prepare a csharp variable so it can be passed to the native side, i.e. casting a enum to an int) Maybe each parameter should have something like string PrepareToPassToNative() which converts the variable so it can be passed to native and returns the variable name to use when passing it
- `CSType` → `CSharpType`?

### Move logic from symbol table to the types

- `IsBlittable` (true for primitives by default, records need to check if they contain only primitives)

### Better name mangling?

- There are a lot of duplicate names in gir between fields and properties, sometimes also signals or callbacks → Name clashes
- Name clashes used to historically be solved via metadata files.

### Unsafe / safe code

- Provide a switch to use unsafe code for optimization?

## Chat
[![Join the chat at https://gitter.im/mono/gtk-sharp](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/mono/gtk-sharp?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)
