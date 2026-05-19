# Document Index Page

Public-facing index for MachineEnglish repository documents, user-visible source files, and public entities.

- Generated: 2026-05-19
- Columns: `Name`, `Document Type`, `Folder`, `File Type`, `Description`.

## Table of Contents

- [.github](#github)
- [Architecture Decisions](#architecture-decisions)
- [CSharpToME](#csharptome)
- [Grammar](#grammar)
- [Handoff Briefs](#handoff-briefs)
- [Language Specs](#language-specs)
- [Major Decisions](#major-decisions)
- [ME](#me)
- [ME Core Entry Points](#me-core-entry-points)
- [ME Core Enums](#me-core-enums)
- [ME Core Extensions](#me-core-extensions)
- [ME Core Interfaces](#me-core-interfaces)
- [ME Core Models](#me-core-models)
- [ME Standard Library](#me-standard-library)
- [ME.Language](#melanguage)
- [ME.LSP](#melsp)
- [ME.Platforms](#meplatforms)
- [ME.Shared](#meshared)
- [ME.Tools](#metools)
- [ME.VSCode](#mevscode)
- [Ready-Reckoner](#ready-reckoner)
- [Repository Root](#repository-root)
- [Roadmaps](#roadmaps)
- [Semantic Rules](#semantic-rules)
- [Summary](#summary)


## .github

| Name | Document Type | Folder | File Type | Description |
| --- | --- | --- | --- | --- |
| main.yml | Configuration / Automation | .github/workflows | yml file | yml file for .github. |

[Back to TOC](#table-of-contents)

## Architecture Decisions

| Name | Document Type | Folder | File Type | Description |
| --- | --- | --- | --- | --- |
| ADR-001-Backend-Test-Refactor.md | Documentation | ME.Architecture/ADRs | Architecture Decision Record | Records an accepted architectural decision for the MachineEnglish project. |
| ADR-002-Remove-ANTLR.md | Documentation | ME.Architecture/ADRs | Architecture Decision Record | Records an accepted architectural decision for the MachineEnglish project. |
| ADR-003-CSharp-As-Source-Of-Truth.md | Documentation | ME.Architecture/ADRs | Architecture Decision Record | Records an accepted architectural decision for the MachineEnglish project. |
| ADR-004-Word-T-Interned-Identity.md | Documentation | ME.Architecture/ADRs | Architecture Decision Record | Records an accepted architectural decision for the MachineEnglish project. |
| ADR-005-LLVM-Backend.md | Documentation | ME.Architecture/ADRs | Architecture Decision Record | Records an accepted architectural decision for the MachineEnglish project. |
| ADR-006-Remove-Ubiquity-Native-LLVM-Facade.md | Documentation | ME.Architecture/ADRs | Architecture Decision Record | Records an accepted architectural decision for the MachineEnglish project. |
| ADR-007-ME-Platforms-Cross-Platform-Abstraction.md | Documentation | ME.Architecture/ADRs | Architecture Decision Record | Records an accepted architectural decision for the MachineEnglish project. |
| ADR-008-Generics-Monomorphization.md | Documentation | ME.Architecture/ADRs | Architecture Decision Record | Records an accepted architectural decision for the MachineEnglish project. |
| ADR-009-Bracketed-Generic-Syntax.md | Documentation | ME.Architecture/ADRs | Architecture Decision Record | Records an accepted architectural decision for the MachineEnglish project. |
| ADR-010-Else-Otherwise-Unification.md | Documentation | ME.Architecture/ADRs | Architecture Decision Record | Records an accepted architectural decision for the MachineEnglish project. |
| ADR-011-Capability-Conflict-Resolution.md | Documentation | ME.Architecture/ADRs | Architecture Decision Record | Records an accepted architectural decision for the MachineEnglish project. |
| ADR-012-me-runtime-C-Stdlib.md | Documentation | ME.Architecture/ADRs | Architecture Decision Record | Records an accepted architectural decision for the MachineEnglish project. |
| ADR-013-LSP-Language-Server.md | Documentation | ME.Architecture/ADRs | Architecture Decision Record | Records an accepted architectural decision for the MachineEnglish project. |
| ADR-014-Syntax-Highlighting.md | Documentation | ME.Architecture/ADRs | Architecture Decision Record | Records an accepted architectural decision for the MachineEnglish project. |
| ADR-015-No-InternalsVisibleTo-For-Tests.md | Documentation | ME.Architecture/ADRs | Architecture Decision Record | Records an accepted architectural decision for the MachineEnglish project. |
| ADR-016-Strict-Testing-Policy.md | Documentation | ME.Architecture/ADRs | Architecture Decision Record | Records an accepted architectural decision for the MachineEnglish project. |
| ADR-017-CSharpToME-Migration-Policy.md | Documentation | ME.Architecture/ADRs | Architecture Decision Record | Records an accepted architectural decision for the MachineEnglish project. |

[Back to TOC](#table-of-contents)

## CSharpToME

| Name | Document Type | Folder | File Type | Description |
| --- | --- | --- | --- | --- |
| MIGRATION-POLICY.md | Documentation | ME.Architecture/CSharpToME | Markdown document | Migration policy for C# to MachineEnglish. |
| README.md | Documentation | ME.Architecture/CSharpToME | README | README for CSharpToME migration programme. |
| REVERSE-FEASIBILITY.md | Documentation | ME.Architecture/CSharpToME | Markdown document | Reverse feasibility analysis for CSharpToME. |

[Back to TOC](#table-of-contents)

## Grammar

| Name | Document Type | Folder | File Type | Description |
| --- | --- | --- | --- | --- |
| MELexer.g4 | Source File | ME/Content/Grammar | ANTLR grammar | ANTLR grammar for the ME lexer. |
| MEParser.g4 | Source File | ME/Content/Grammar | ANTLR grammar | ANTLR grammar for the ME parser. |

[Back to TOC](#table-of-contents)

## Handoff Briefs

| Name | Document Type | Folder | File Type | Description |
| --- | --- | --- | --- | --- |
| BRIEF-001-Signature-Help.md | Documentation | ME.Architecture/Handoff | Markdown document | Captures implementation handoff context for coordinated development. |
| BRIEF-002-Find-References.md | Documentation | ME.Architecture/Handoff | Markdown document | Captures implementation handoff context for coordinated development. |
| BRIEF-003-Rename.md | Documentation | ME.Architecture/Handoff | Markdown document | Captures implementation handoff context for coordinated development. |
| README.md | Documentation | ME.Architecture/Handoff | README | Captures implementation handoff context for coordinated development. |

[Back to TOC](#table-of-contents)

## Language Specs

| Name | Document Type | Folder | File Type | Description |
| --- | --- | --- | --- | --- |
| Spc-Callables.md | Documentation | ME/Content/Specs | Language Specification | Canonical language specification for callables. |
| Spc-CompilerConstants.md | Documentation | ME/Content/Specs | Language Specification | Canonical language specification for compiler constants. |
| Spc-ControlFlow.md | Documentation | ME/Content/Specs | Language Specification | Canonical language specification for control flow. |
| Spc-Entities.md | Documentation | ME/Content/Specs | Language Specification | Canonical language specification for entities, records, specs. |
| Spc-Expressions.md | Documentation | ME/Content/Specs | Language Specification | Canonical language specification for expressions. |
| Spc-Frontend.md | Documentation | ME/Content/Specs | Language Specification | Canonical language specification for frontend design. |
| Spc-IaC.md | Documentation | ME/Content/Specs | Language Specification | Canonical language specification for IaC design. |
| Spc-Modifiers.md | Documentation | ME/Content/Specs | Language Specification | Canonical language specification for modifiers. |
| Spc-Project_ModuleSystem.md | Documentation | ME/Content/Specs | Language Specification | Canonical language specification for the project/module system. |
| Spc-Properties.md | Documentation | ME/Content/Specs | Language Specification | Canonical language specification for properties. |
| Spc-TypeCast.md | Documentation | ME/Content/Specs | Language Specification | Canonical language specification for type casts. |
| Spc-TypesAndValues.md | Documentation | ME/Content/Specs | Language Specification | Canonical language specification for types and values. |
| Spc-WarningsAndErrors.md | Documentation | ME/Content/Specs | Language Specification | Canonical language specification for warnings and errors. |

[Back to TOC](#table-of-contents)

## Major Decisions

| Name | Document Type | Folder | File Type | Description |
| --- | --- | --- | --- | --- |
| MD-001-Spec-Default-Implementations.md | Documentation | ME.Architecture/Major-Decisions | Markdown document | Specs are pure contracts; the SOLIDE manifesto. |
| MD-002-CSharp-Authored-Language-Definitions.md | Documentation | ME.Architecture/Major-Decisions | Markdown document | C# authored language definitions as a major project direction. |

[Back to TOC](#table-of-contents)

## ME

| Name | Document Type | Folder | File Type | Description |
| --- | --- | --- | --- | --- |
| MELexer.tsv | Repository File | ME/Content/InputFiles | tsv file | Lexer token definitions in TSV format. |
| MEParser.tsv | Repository File | ME/Content/InputFiles | tsv file | Parser rule definitions in TSV format. |
| Sem-Callables.tsv | Repository File | ME/Content/InputFiles | tsv file | Semantic rules for callables. |
| Sem-CompilerConstants.tsv | Repository File | ME/Content/InputFiles | tsv file | Semantic rules for compiler constants. |
| Sem-ControlFlow.tsv | Repository File | ME/Content/InputFiles | tsv file | Semantic rules for control flow. |
| Sem-Core.tsv | Repository File | ME/Content/InputFiles | tsv file | Semantic rules for core. |
| Sem-Entities.tsv | Repository File | ME/Content/InputFiles | tsv file | Semantic rules for entities. |
| Sem-Expressions.tsv | Repository File | ME/Content/InputFiles | tsv file | Semantic rules for expressions. |
| Sem-Frontend.tsv | Repository File | ME/Content/InputFiles | tsv file | Semantic rules for frontend. |
| Sem-IaC.tsv | Repository File | ME/Content/InputFiles | tsv file | Semantic rules for IaC. |
| Sem-Modifiers.tsv | Repository File | ME/Content/InputFiles | tsv file | Semantic rules for modifiers. |
| Sem-Project_ModuleSystem.tsv | Repository File | ME/Content/InputFiles | tsv file | Semantic rules for project/module system. |
| Sem-Properties.tsv | Repository File | ME/Content/InputFiles | tsv file | Semantic rules for properties. |
| Sem-TypeCast.tsv | Repository File | ME/Content/InputFiles | tsv file | Semantic rules for type cast. |
| Sem-TypesAndValues.tsv | Repository File | ME/Content/InputFiles | tsv file | Semantic rules for types and values. |
| Sem-WarningsAndErrors.tsv | Repository File | ME/Content/InputFiles | tsv file | Semantic rules for warnings and errors. |
| TsvColumnAttribute | Entity | ME/Attributes | C# class | Declares the class `TsvColumnAttribute`. |

[Back to TOC](#table-of-contents)

## ME Core Entry Points

| Name | Document Type | Folder | File Type | Description |
| --- | --- | --- | --- | --- |
| Backend | Entity | ME/Main | C# class | Backend emitter that transforms a successful compilation result into backend-specific output forms. |
| Factory | Entity | ME/Main | C# class | Creates public ME model objects and manages registered backend emitters. |

[Back to TOC](#table-of-contents)

## ME Core Enums

| Name | Document Type | Folder | File Type | Description |
| --- | --- | --- | --- | --- |
| DiagnosticSeverity | Entity | ME/Enums | C# enum | Indicates the severity level of a compiler diagnostic. |
| IRInstructionKind | Entity | ME/Enums | C# enum | Identifies the kind of an IR instruction. Used by backends to pattern-switch on without needing access to concrete instruction types. |
| IROpCode | Entity | ME/Enums | C# enum | Identifies the operation performed by an IR instruction. Opcodes are grouped by type family and operation kind. |
| IRTypeDeclKind | Entity | ME/Enums | C# enum | Discriminates the kind of a type declaration in the IR. |
| IRTypeKind | Entity | ME/Enums | C# enum | Identifies the kind of an IR type. Used by backends to map IR types to their platform-specific equivalents. |
| NameConversions | Entity | ME/Enums | C# enum | Specifies how a ME identifier name should be transformed when emitted by a target backend. |
| OutputType | Entity | ME/Enums | C# enum | Specifies the type of output produced by a backend or compilation target. |

[Back to TOC](#table-of-contents)

## ME Core Extensions

| Name | Document Type | Folder | File Type | Description |
| --- | --- | --- | --- | --- |
| BackendExtension | Entity | ME/Extensions | C# class | Declares the class `BackendExtension`. |
| DiagnosticExtension | Entity | ME/Extensions | C# class | Provides convenience methods for adding and printing compiler diagnostics. |
| TsvExtensions | Entity | ME/Extensions | C# class | Declares the class `TsvExtensions`. |

[Back to TOC](#table-of-contents)

## ME Core Interfaces

| Name | Document Type | Folder | File Type | Description |
| --- | --- | --- | --- | --- |
| IAnnotation | Entity | ME/Interfaces | C# interface | A declarative annotation attached to a callable (e.g. @builtin_method). |
| IAwaitInstruction | Entity | ME/Interfaces | C# interface | Represents an await instruction - suspends the current async callable until the async call completes. |
| IBackend | Entity | ME/Interfaces | C# interface | Defines an emitter that is associated with a target platform and compiled callables. |
| IBackendFactory | Entity | ME/Interfaces | C# interface | Declares the interface `IBackendFactory`. |
| IBackendOptions | Entity | ME/Interfaces | C# interface | Declares the interface `IBackendOptions`. |
| IBinaryInstruction | Entity | ME/Interfaces | C# interface | Represents a binary operation instruction. |
| IBinaryLocator | Entity | ME/Interfaces | C# interface | Resolves platform-specific binary paths and executable name conventions. |
| IBlock | Entity | ME/Interfaces | C# interface | Represents a basic block within a callable visible to backends. |
| IBranchInstruction | Entity | ME/Interfaces | C# interface | Represents a conditional branch instruction. |
| ICallable | Entity | ME/Interfaces | C# interface | Represents a compiled callable (function or action) visible to backends. |
| ICallInstruction | Entity | ME/Interfaces | C# interface | Represents a callable invocation instruction. |
| ICastInstruction | Entity | ME/Interfaces | C# interface | Represents a type cast instruction. |
| ICompiler | Entity | ME/Interfaces | C# interface | Defines the contract for compiling ME source code into an IR and filling registered backends. |
| IConstInstruction | Entity | ME/Interfaces | C# interface | Represents a constant value instruction. |
| IConversionInstruction | Entity | ME/Interfaces | C# interface | Represents a conversion instruction. |
| IDeclInstruction | Entity | ME/Interfaces | C# interface | Represents a local declaration instruction. |
| IDiagnostic | Entity | ME/Interfaces | C# interface | Represents a single diagnostic message produced during compilation. |
| IDiagnosticBag | Entity | ME/Interfaces | C# interface | A mutable collection of messages accumulated during a single compilation pass. |
| IEmit | Entity | ME/Interfaces | C# interface | Declares the interface `IEmit`. |
| IEvalResult | Entity | ME/Interfaces | C# interface | Represents the outcome of evaluating a ME expression or program through an evaluation-capable backend. |
| IEvaluator | Entity | ME/Interfaces | C# interface | Extended backend contract for backends that support expression evaluation at runtime. |
| IFailInstruction | Entity | ME/Interfaces | C# interface | Represents a fail-channel instruction. |
| IFieldDecl | Entity | ME/Interfaces | C# interface | Represents a single field declaration inside an entity, record, or error type. |
| IGetFieldInstruction | Entity | ME/Interfaces | C# interface | Represents a field read instruction. |
| IGetIndexInstruction | Entity | ME/Interfaces | C# interface | Represents an indexed read instruction. |
| IGuardNonZeroInstruction | Entity | ME/Interfaces | C# interface | Represents a non-zero guard instruction. |
| IInstruction | Entity | ME/Interfaces | C# interface | Represents a single IR instruction visible to backends. |
| IIntrinsicInstruction | Entity | ME/Interfaces | C# interface | Represents an intrinsic invocation instruction. |
| IIsOkInstruction | Entity | ME/Interfaces | C# interface | Represents an instruction that tests value validity. |
| IIsTypeInstruction | Entity | ME/Interfaces | C# interface | Represents a type-test instruction. |
| IJumpInstruction | Entity | ME/Interfaces | C# interface | Represents an unconditional jump instruction. |
| ILoadInstruction | Entity | ME/Interfaces | C# interface | Represents a load instruction. |
| IModule | Entity | ME/Interfaces | C# interface | Defines an emitter that owns disposable backend module resources. |
| INameValue | Entity | ME/Interfaces | C# interface | Represents a named value pair used by instructions and backend metadata. |
| INewInstruction | Entity | ME/Interfaces | C# interface | Represents an allocation or construction instruction. |
| INopInstruction | Entity | ME/Interfaces | C# interface | Represents a no-operation instruction. |
| IOpCodeInstruction | Entity | ME/Interfaces | C# interface | Represents an instruction with an opcode. |
| IOptions | Entity | ME/Interfaces | C# interface | Declares the interface `IOptions`. |
| IOSPlatform | Entity | ME/Interfaces | C# interface | Represents the host operating-system platform and its associated tooling. |
| IParameter | Entity | ME/Interfaces | C# interface | Represents a parameter of a compiled callable visible to backends. |
| IPhiInstruction | Entity | ME/Interfaces | C# interface | Represents an SSA phi instruction. |
| IResult | Entity | ME/Interfaces | C# interface | Represents a typed operation result produced by the compiler or a backend. |
| IResultInstruction | Entity | ME/Interfaces | C# interface | Represents an instruction that produces a result value. |
| IReturnInstruction | Entity | ME/Interfaces | C# interface | Represents a return instruction. |
| ISetFieldInstruction | Entity | ME/Interfaces | C# interface | Represents a field write instruction. |
| ISetIndexInstruction | Entity | ME/Interfaces | C# interface | Represents an indexed write instruction. |
| IShell | Entity | ME/Interfaces | C# interface | Abstracts shell command execution across platforms. |
| IStoreInstruction | Entity | ME/Interfaces | C# interface | Represents a store instruction. |
| ISyncInstruction | Entity | ME/Interfaces | C# interface | Represents a sync instruction - blocks the current sync callable until the async call completes. |
| IThrowInstruction | Entity | ME/Interfaces | C# interface | Represents a throw-channel instruction. |
| IToolchain | Entity | ME/Interfaces | C# interface | Abstracts the native toolchain used to link LLVM object files into executables. |
| ITsvRow | Entity | ME/Interfaces | C# interface | Declares the interface `ITsvRow`. |
| ITypeDecl | Entity | ME/Interfaces | C# interface | Represents a type declaration (entity, record, choice, error) visible to backends. |
| IUnaryInstruction | Entity | ME/Interfaces | C# interface | Represents a unary operation instruction. |
| IWord | Entity | ME/Interfaces | C# interface | Declares the interface `IWord`. |
| ShellResult | Entity | ME/Interfaces | C# record struct | Result of a shell command execution. |

[Back to TOC](#table-of-contents)

## ME Core Models

| Name | Document Type | Folder | File Type | Description |
| --- | --- | --- | --- | --- |
| OsPlatform | Entity | ME/Models | C# record | A strongly-typed, interned identifier for an operating-system platform. |
| Platform | Entity | ME/Models | C# record | A strongly-typed, interned identifier for a code-generation target platform. |
| Word | Entity | ME/Models | C# record | Declares the record `Word`. |

[Back to TOC](#table-of-contents)

## ME Standard Library

| Name | Document Type | Folder | File Type | Description |
| --- | --- | --- | --- | --- |
| Base64 | Entity | me_stdlib/ME.Encoding | ME entity | Declares the ME entity `Base64`. |
| Date | Entity | me_stdlib/ME.Temporal | ME entity | Declares the ME entity `Date`. |
| Hash | Entity | me_stdlib/ME.Crypto | ME entity | Declares the ME entity `Hash`. |
| Hex | Entity | me_stdlib/ME.Encoding | ME entity | Declares the ME entity `Hex`. |
| Hmac | Entity | me_stdlib/ME.Crypto | ME entity | Declares the ME entity `Hmac`. |
| JsonText | Entity | me_stdlib/ME.Encoding | ME entity | Declares the ME entity `JsonText`. |
| List | Entity | me_stdlib/ME.Collections | ME entity | Declares the ME entity `List`. |
| Map | Entity | me_stdlib/ME.Collections | ME entity | Declares the ME entity `Map`. |
| Math | Entity | me_stdlib/ME.Math | ME entity | Declares the ME entity `Math`. |
| Moment | Entity | me_stdlib/ME.Temporal | ME entity | Declares the ME entity `Moment`. |
| Optional | Entity | me_stdlib/ME.Optional | ME entity | Declares the ME entity `Optional`. |
| Set | Entity | me_stdlib/ME.Collections | ME entity | Declares the ME entity `Set`. |
| Text | Entity | me_stdlib/ME.Text | ME entity | Declares the ME entity `Text`. |
| Time | Entity | me_stdlib/ME.Temporal | ME entity | Declares the ME entity `Time`. |
| Url | Entity | me_stdlib/ME.Encoding | ME entity | Declares the ME entity `Url`. |

[Back to TOC](#table-of-contents)

## ME.Language

| Name | Document Type | Folder | File Type | Description |
| --- | --- | --- | --- | --- |
| CallablesDocument | Source File | ME.Language/Semantic | C# source | Semantic document definition for callables. |
| CompilerConstantsDocument | Source File | ME.Language/Semantic | C# source | Semantic document definition for compiler constants. |
| ControlFlowDocument | Source File | ME.Language/Semantic | C# source | Semantic document definition for control flow. |
| CoreDocument | Source File | ME.Language/Semantic | C# source | Semantic document definition for core. |
| EntitiesDocument | Source File | ME.Language/Semantic | C# source | Semantic document definition for entities. |
| ExpressionsDocument | Source File | ME.Language/Semantic | C# source | Semantic document definition for expressions. |
| FrontendDocument | Source File | ME.Language/Semantic | C# source | Semantic document definition for frontend. |
| IaCDocument | Source File | ME.Language/Semantic | C# source | Semantic document definition for IaC. |
| LanguageDefinitions | Entity | ME.Language/Main | C# class | Public entry point for ME.Language generation. |
| ModifiersDocument | Source File | ME.Language/Semantic | C# source | Semantic document definition for modifiers. |
| PropertiesDocument | Source File | ME.Language/Semantic | C# source | Semantic document definition for properties. |
| TypeCastDocument | Source File | ME.Language/Semantic | C# source | Semantic document definition for type cast. |
| TypesAndValuesDocument | Source File | ME.Language/Semantic | C# source | Semantic document definition for types and values. |
| WarningsAndErrorsDocument | Source File | ME.Language/Semantic | C# source | Semantic document definition for warnings and errors. |

[Back to TOC](#table-of-contents)

## ME.LSP

| Name | Document Type | Folder | File Type | Description |
| --- | --- | --- | --- | --- |
| CompletionHandler | Source File | ME.LSP | C# source | Handles LSP completion requests. |
| DefinitionHandler | Source File | ME.LSP | C# source | Handles LSP go-to-definition requests. |
| DiagnosticsHandler | Source File | ME.LSP | C# source | Handles LSP diagnostics publishing. |
| DocumentStore | Source File | ME.LSP | C# source | Tracks open document state. |
| HoverHandler | Source File | ME.LSP | C# source | Handles LSP hover requests. |
| LspPaths | Source File | ME.LSP | C# source | LSP path utilities. |
| LspServer | Source File | ME.LSP | C# source | Main LSP server entry point. |
| LspText | Source File | ME.LSP | C# source | LSP text utilities. |
| PrepareRenameHandler | Source File | ME.LSP | C# source | Handles LSP prepareRename requests. |
| ReferencesHandler | Source File | ME.LSP | C# source | Handles LSP find-references requests. |
| RenameHandler | Source File | ME.LSP | C# source | Handles LSP rename requests. |
| SignatureHelpHandler | Source File | ME.LSP | C# source | Handles LSP signature help requests. |
| TextDocumentSyncHandler | Source File | ME.LSP | C# source | Handles LSP document sync. |
| WorkspaceScanner | Source File | ME.LSP | C# source | Scans workspace for ME files. |

[Back to TOC](#table-of-contents)

## ME.Platforms

| Name | Document Type | Folder | File Type | Description |
| --- | --- | --- | --- | --- |
| AndroidPlatform | Source File | ME.Platforms/Impl | C# source | Android platform implementation. |
| AndroidToolchain | Source File | ME.Platforms/Toolchain | C# source | Android NDK toolchain implementation. |
| ClangToolchain | Source File | ME.Platforms/Toolchain | C# source | Clang toolchain implementation. |
| LinuxPlatform | Source File | ME.Platforms/Impl | C# source | Linux platform implementation. |
| MacOsPlatform | Source File | ME.Platforms/Impl | C# source | macOS platform implementation. |
| PlatformFactory | Entity | ME.Platforms | C# class | The sole public entry point for platform resolution. |
| WindowsBinaryLocator | Source File | ME.Platforms/Locator | C# source | Windows binary locator implementation. |
| WindowsPlatform | Source File | ME.Platforms/Impl | C# source | Windows platform implementation. |
| WindowsToolchain | Source File | ME.Platforms/Toolchain | C# source | Windows (MSVC) toolchain implementation. |

[Back to TOC](#table-of-contents)

## ME.Shared

| Name | Document Type | Folder | File Type | Description |
| --- | --- | --- | --- | --- |
| ME.Shared.projitems | Repository File | ME.Shared | projitems file | Shared project items file. |
| ME.Shared.shproj | Repository File | ME.Shared | shproj file | Shared project definition. |

[Back to TOC](#table-of-contents)

## ME.Tools

| Name | Document Type | Folder | File Type | Description |
| --- | --- | --- | --- | --- |
| AntlrGenerator | Source File | ME.Tools/Infrastructure | C# source | ANTLR grammar generation command. |
| ArgParser | Source File | ME.Tools/Infrastructure | C# source | CLI argument parser. |
| CommandArgs | Entity | ME.Tools/Infrastructure | C# record | Declares the record `CommandArgs`. |
| CompileCommand | Source File | ME.Tools/Infrastructure | C# source | Compile command implementation. |
| Constants | Source File | ME.Tools/Constants | C# source | Tool constants. |
| LexerBootstrapper | Source File | ME.Tools/Infrastructure | C# source | Lexer bootstrap tool. |
| LexerDumper | Source File | ME.Tools/Infrastructure | C# source | Lexer dump tool. |
| LspCommand | Source File | ME.Tools/Infrastructure | C# source | LSP server launch command. |
| McpSetup | Source File | ME.Tools/Infrastructure | C# source | MCP server setup command. |
| McpStartCommand | Source File | ME.Tools/Infrastructure | C# source | MCP server start command. |
| ParserBootstrapper | Source File | ME.Tools/Infrastructure | C# source | Parser bootstrap tool. |
| ParserDumper | Source File | ME.Tools/Infrastructure | C# source | Parser dump tool. |
| RoslynCommand | Source File | ME.Tools/Infrastructure | C# source | Roslyn code intelligence CLI facade. |
| SemanticBootstrapper | Source File | ME.Tools/Infrastructure | C# source | Semantic bootstrap tool. |
| SemanticDumper | Source File | ME.Tools/Infrastructure | C# source | Semantic dump tool. |
| ToolTaskDispatcher | Source File | ME.Tools/Infrastructure | C# source | Dispatches tool tasks. |
| VsCodeCommand | Source File | ME.Tools/Infrastructure | C# source | VS Code packaging and install command. |

[Back to TOC](#table-of-contents)

## ME.VSCode

| Name | Document Type | Folder | File Type | Description |
| --- | --- | --- | --- | --- |
| cs-file.svg | Asset | ME.VSCode/me-file-icons/icons | SVG asset | C# file icon for VS Code extension. |
| fileicons.json | Configuration / Automation | ME.VSCode/me-file-icons | JSON configuration | File icon theme configuration. |
| language-configuration.json | Configuration / Automation | ME.VSCode/me-file-icons | JSON configuration | ME language configuration for VS Code. |
| me.tmLanguage.json | Configuration / Automation | ME.VSCode/me-file-icons/syntaxes | JSON configuration | TextMate grammar for ME syntax highlighting. |
| me-file.svg | Asset | ME.VSCode/me-file-icons/icons | SVG asset | ME file icon. |
| me-folder.svg | Asset | ME.VSCode/me-file-icons/icons | SVG asset | ME folder icon. |
| package.json | Configuration / Automation | ME.VSCode/me-file-icons | JSON configuration | VS Code extension manifest. |

[Back to TOC](#table-of-contents)

## Ready-Reckoner

| Name | Document Type | Folder | File Type | Description |
| --- | --- | --- | --- | --- |
| C-ME-Conversion-Guide.md | Documentation | Ready-Reckoner | Markdown document | Practical guide for C# to MachineEnglish syntax conversion. |

[Back to TOC](#table-of-contents)

## Repository Root

| Name | Document Type | Folder | File Type | Description |
| --- | --- | --- | --- | --- |
| README.md | Documentation | root | README | Primary repository overview and navigation entry point. |
| ME.slnx | Build Document | root | .NET solution file | .NET solution file. |

[Back to TOC](#table-of-contents)

## Roadmaps

| Name | Document Type | Folder | File Type | Description |
| --- | --- | --- | --- | --- |
| RDMP-Ambitions.md | Documentation | ME.Architecture/Roadmap | Roadmap Document | Why ME exists and what the stack is trying to become. |
| RDMP-Bootstrapping.md | Documentation | ME.Architecture/Roadmap | Roadmap Document | Bootstrapping and parser/compiler self-hosting. |
| RDMP-Compiler.md | Documentation | ME.Architecture/Roadmap | Roadmap Document | Compiler roadmap. |
| RDMP-CSharpToME.md | Documentation | ME.Architecture/Roadmap | Roadmap Document | C# to ME migration programme. |
| RDMP-EcosystemInterop.md | Documentation | ME.Architecture/Roadmap | Roadmap Document | Wrapper generation and ecosystem interop. |
| RDMP-GWS.md | Documentation | ME.Architecture/Roadmap | Roadmap Document | Graphics and Windowing System. |
| RDMP-GWS-CS-TO-ME-MIGRATION.md | Documentation | ME.Architecture/Roadmap | Roadmap Document | GWS C# to ME migration. |
| RDMP-Interpreter.md | Documentation | ME.Architecture/Roadmap | Roadmap Document | Tree-walk interpreter. |
| RDMP-LanguageDebates.md | Documentation | ME.Architecture/Roadmap | Roadmap Document | Open language design debates. |
| RDMP-MigrationTools.md | Documentation | ME.Architecture/Roadmap | Roadmap Document | Migration tooling. |
| RDMP-NativeBehavior.md | Documentation | ME.Architecture/Roadmap | Roadmap Document | Native behavior and runtime. |
| RDMP-NextFeatures.md | Documentation | ME.Architecture/Roadmap | Roadmap Document | Next language features. |
| RDMP-Scaffolding.md | Documentation | ME.Architecture/Roadmap | Roadmap Document | Project scaffolding. |
| RDMP-SelfHostingJourney.md | Documentation | ME.Architecture/Roadmap | Roadmap Document | Self-hosting journey. |
| RDMP-Wrappers.md | Documentation | ME.Architecture/Roadmap | Roadmap Document | Standard library wrappers. |

[Back to TOC](#table-of-contents)

## Semantic Rules

| Name | Document Type | Folder | File Type | Description |
| --- | --- | --- | --- | --- |
| Sem-Callables.md | Documentation | ME/Content/Semantics | Semantic Rule Document | Semantic rules and constraints for callables. |
| Sem-CompilerConstants.md | Documentation | ME/Content/Semantics | Semantic Rule Document | Semantic rules and constraints for compiler constants. |
| Sem-ControlFlow.md | Documentation | ME/Content/Semantics | Semantic Rule Document | Semantic rules and constraints for control flow. |
| Sem-Core.md | Documentation | ME/Content/Semantics | Semantic Rule Document | Semantic rules and constraints for core. |
| Sem-Entities.md | Documentation | ME/Content/Semantics | Semantic Rule Document | Semantic rules and constraints for entities. |
| Sem-Expressions.md | Documentation | ME/Content/Semantics | Semantic Rule Document | Semantic rules and constraints for expressions. |
| Sem-Frontend.md | Documentation | ME/Content/Semantics | Semantic Rule Document | Semantic rules and constraints for frontend. |
| Sem-IaC.md | Documentation | ME/Content/Semantics | Semantic Rule Document | Semantic rules and constraints for IaC. |
| Sem-Modifiers.md | Documentation | ME/Content/Semantics | Semantic Rule Document | Semantic rules and constraints for modifiers. |
| Sem-Project_ModuleSystem.md | Documentation | ME/Content/Semantics | Semantic Rule Document | Semantic rules and constraints for project/module system. |
| Sem-Properties.md | Documentation | ME/Content/Semantics | Semantic Rule Document | Semantic rules and constraints for properties. |
| Sem-TypeCast.md | Documentation | ME/Content/Semantics | Semantic Rule Document | Semantic rules and constraints for type cast. |
| Sem-TypesAndValues.md | Documentation | ME/Content/Semantics | Semantic Rule Document | Semantic rules and constraints for types and values. |
| Sem-WarningsAndErrors.md | Documentation | ME/Content/Semantics | Semantic Rule Document | Semantic rules and constraints for warnings and errors. |

[Back to TOC](#table-of-contents)

## Summary

| Metric | Count |
| --- | ---: |
| Indexed sections | 24 |
| Architecture Decision Records | 17 |
| Language Specs | 13 |
| Semantic Rule Documents | 14 |
| Roadmap Documents | 15 |
| Standard Library Entities | 15 |
