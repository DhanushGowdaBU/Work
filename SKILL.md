---
name: dotnet-developer
description: Create and modify NYBOSS .NET server projects using the repository's project structure, project archetypes, dependency conventions, and .NET 8 tooling.
license: MIT
metadata:
  author: Copyright 2026 Google LLC
  version: '1.0'
---

# NYBOSS .NET Developer

Use this skill when the user asks to create or modify a NYBOSS .NET server project.

Read the relevant reference files before performing the requested operation.

Required references:

- references/project-creation.md
- references/project-structure.md
- references/project-archetypes.md

The archetype reference defines the detailed project types, SDKs, structures, and dependency patterns used by NYBOSS.

---

# Project Creation

Supported creation concepts:

1. Default project
2. Explicit project composition
3. Project similar to an existing project
4. Module creation
5. Archetype-specific project creation

Supported application areas:

- Business
- Core
- Infrastructure
- Tests

Determine from the user's request:

- application area
- project or module
- project name
- module name, if provided
- creation mode
- requested components
- requested archetype
- project type
- whether project references are required

The current phase creates:

- project directories
- project files
- folders
- structural source files
- `.csproj` files
- solution entries
- applicable project-to-project references
- applicable project configuration

This phase does not implement business functionality.

---

# Project vs Module

A project is an individual `.csproj`.

A module is a logical directory containing one or more related projects.

Examples:

Apps/Business/<ModuleName>/

Apps/Core/<ModuleName>/

Infrastructure/<ModuleName>/

A module directory is not itself a project.

If the user explicitly says "module", create the module directory and the projects belonging to that module.

Do not create an additional module when the user did not request one.

---

# Naming Convention

All newly generated projects must use the new NYBOSS naming convention.

General form:

<ProjectName>.<Component>

Examples:

<ProjectName>.API
<ProjectName>.Common
<ProjectName>.Domain
<ProjectName>.Repository
<ProjectName>.Services
<ProjectName>.ServiceHost
<ProjectName>.API.Client
<ProjectName>.SDK
<ProjectName>.MessageProcessing
<ProjectName>.ProcessorHost
<ProjectName>.Host
<ProjectName>.UnitTest

Do not create new projects using legacy organizational prefixes such as:

BNPP.NYBOSS.<ProjectName>

Legacy project names in the repository are structural references only.

Senior-provided templates may contain legacy names. Adapt those templates to the new naming convention when generating new projects.

---

# Important Naming Rule for Archetypes

The senior-provided archetype documentation may use names such as:

BNPP.NYBOSS.<Domain>.Common
BNPP.NYBOSS.<Domain>.Repository
BNPP.NYBOSS.<Domain>.Services
BNPP.NYBOSS.<Domain>.API
BNPP.NYBOSS.MessageProcessing.<Domain>
BNPP.NYBOSS.MessageProcessing.<Domain>ProcessorHost

These are legacy repository conventions.

For NEW projects, use the current NYBOSS naming convention.

For example:

<ProjectName>.Common
<ProjectName>.Repository
<ProjectName>.Services
<ProjectName>.API
<ProjectName>.MessageProcessing
<ProjectName>.ProcessorHost

Do not copy the `BNPP.NYBOSS.` prefix into a newly generated project.

---

# Application Areas

NYBOSS server projects are organized into:

- Business
- Core
- Infrastructure
- Tests

Business and Core application projects normally use the standard domain slice architecture.

Infrastructure uses heterogeneous project and module types.

Tests contain test projects corresponding to production projects.

---

# Business

Business projects are located under:

Apps/Business/

Default Business composition:

<ProjectName>.API
<ProjectName>.Common
<ProjectName>.Repository
<ProjectName>.Services

Default location:

Apps/Business/<ProjectName>/

A Business module uses the same composition unless the user explicitly requests another composition.

---

# Core

Core projects are located under:

Apps/Core/

Default Core composition:

<ProjectName>.API
<ProjectName>.Domain
<ProjectName>.Repository
<ProjectName>.Services

Default location:

Apps/Core/<ProjectName>/

Core uses Domain instead of Common.

Common and Domain are distinct project types.

Do not automatically convert:

Common -> Domain

or:

Domain -> Common.

---

# Infrastructure

Infrastructure is located under:

Infrastructure/

Infrastructure has no single universal composition.

Infrastructure may contain:

- standalone projects
- service modules
- service-host modules
- API client modules
- message-processing modules
- notification SDK modules
- transport modules
- watcher modules
- other explicitly defined structures

Use:

references/project-creation.md
references/project-structure.md
references/project-archetypes.md

to determine the requested Infrastructure structure.

Never force the Business/Core composition onto Infrastructure.

If the user explicitly specifies components, create only those components.

If the requested Infrastructure structure cannot be determined safely, do not invent architecture.

---

# Tests

Test projects are located under:

Tests/UnitTests/

The unit-test archetype is defined in:

references/project-archetypes.md

Default unit-test project naming:

<ProjectName>.UnitTest

Test projects target:

net8.0

Test projects use the test SDK and required test packages defined by the archetype.

A test project is a leaf project.

It may reference the production projects it tests.

Production projects must not reference the unit-test project.

---

# Direct Component Creation

If the user explicitly names a component project, create only that project.

Example:

Create <ProjectName>.API project

creates:

<ProjectName>.API

only.

Do not create:

<ProjectName>.Common
<ProjectName>.Repository
<ProjectName>.Services

Example:

Create <ProjectName>.Repository project

creates only:

<ProjectName>.Repository

The direct component request takes precedence over default composition.

---

# Explicit Project Composition

If the user explicitly specifies components, create only those components.

Example:

Create <ProjectName> project with API, Common and Services

creates:

<ProjectName>.API
<ProjectName>.Common
<ProjectName>.Services

Do not create Repository.

References must be created only when both sides of the required relationship exist and the architecture requires the dependency.

---

# Module Creation

Modules are supported in:

- Business
- Core
- Infrastructure

## Business Module

Default:

Apps/Business/<ModuleName>/

<ProjectName>.API
<ProjectName>.Common
<ProjectName>.Repository
<ProjectName>.Services

If no separate project name is supplied:

<ProjectName> = <ModuleName>

## Core Module

Default:

Apps/Core/<ModuleName>/

<ProjectName>.API
<ProjectName>.Domain
<ProjectName>.Repository
<ProjectName>.Services

If no separate project name is supplied:

<ProjectName> = <ModuleName>

## Infrastructure Module

Infrastructure module composition depends on the requested module type.

Use the Infrastructure templates from:

references/project-creation.md
references/project-structure.md

Do not assume Common/Repository/Services for every Infrastructure module.

---

# Similar Project Creation

When the user asks for a project similar to an existing project:

1. Locate the reference project.
2. Determine its application area.
3. Determine whether it belongs to a module.
4. Determine the project type.
5. Determine the project composition.
6. Inspect project directories.
7. Inspect folder names.
8. Inspect file names.
9. Inspect project metadata required to identify the project type.
10. Inspect applicable project-to-project relationships.
11. Recreate the applicable structure.
12. Rename project-specific names.
13. Apply the new naming convention.
14. Recreate applicable references according to the current architecture.
15. Add generated projects to the existing solution.

The reference project is structural only.

Do not copy:

- business logic
- service logic
- repository implementation
- controller implementation
- domain implementation
- handler implementation
- configuration values
- secrets
- connection strings
- application data
- business-specific JSON/XML
- client-specific settings

If a CLI-generated file exists in the new project, retain the CLI-generated content.

---

# Structural File Rules

Structural files created specifically by the generator must contain minimal valid skeleton code.

Examples:

<ProjectName>Model.cs
<ProjectName>Constants.cs
<ProjectName>Options.cs
I<ProjectName>Repository.cs
I<ProjectName>Service.cs
<ProjectName>Repository.cs
<ProjectName>Service.cs
RegisterServices.cs

Do not implement business functionality.

---

# CLI-Generated Files

When `dotnet new` creates a file, preserve its generated content.

Examples:

- Program.cs
- `.csproj`
- launchSettings.json
- appsettings.json
- appsettings.Development.json
- Worker.cs
- other template-generated files

Do not:

- blank the file
- replace the file with a placeholder
- copy implementation from another application

Modify generated files only when required by:

- target framework
- project naming
- required project configuration
- required project references
- NYBOSS-specific template cleanup

---

# API Template

API projects use:

Microsoft.NET.Sdk.Web

and the controller-based Web API template.

Use:

dotnet new webapi --framework net8.0 --use-controllers --name <ProjectName>.API

The `.http` file generated by the API template is not part of the default NYBOSS API structure.

Remove:

<ProjectName>.API.http

unless explicitly requested.

The API must not receive business implementation during project creation.

---

# Class Libraries

Common, Domain, Repository, Services and other library projects use:

Microsoft.NET.Sdk

unless the selected archetype requires another SDK.

Use:

dotnet new classlib --framework net8.0 --name <ProjectName>.<Component>

---

# Worker / Windows Service Projects

Worker/Windows Service projects use:

Microsoft.NET.Sdk.Worker

Use the Worker template where appropriate:

dotnet new worker --framework net8.0 --name <ProjectName>.ProcessorHost

or the equivalent output/name configuration required by the requested project structure.

The Worker project must remain a host/composition project.

Do not put business logic into the Worker host.

Worker-specific dependencies are defined in:

references/project-archetypes.md

---

# Workflow Executable Projects

Workflow/run-to-completion projects use:

Microsoft.NET.Sdk

with:

<OutputType>Exe</OutputType>

They are not Worker projects.

Use this archetype when the user requests:

- executable
- workflow host
- batch executable
- scheduler executable
- run-once process
- workflow EXE

Default new-project name:

<ProjectName>.Host

The executable runs to completion and exits.

Do not use `Microsoft.NET.Sdk.Worker` for workflow executables.

---

# Message Processing Projects

Message-processing logic is a separate library archetype.

Default new-project name:

<ProjectName>.MessageProcessing

It contains structural areas such as:

Handlers/
Strategies/
Services/
Models/
Options/

and:

ModuleLoader.cs

It must not contain the long-running host.

The Worker/ProcessorHost project references the MessageProcessing library.

---

# Unit Test Projects

Unit test projects use:

<ProjectName>.UnitTest

and target:

net8.0

They should use the repository's defined xUnit/Moq/FluentAssertions/coverage conventions.

Test projects may reference production projects.

Production projects must not reference test projects.

---

# Target Framework

All new projects target:

net8.0

unless the user explicitly requests another framework.

Every generated `.csproj` must contain:

<TargetFramework>net8.0</TargetFramework>

Do not allow the installed SDK version to select another target framework automatically.

---

# Shared Assembly Information

NYBOSS centralizes assembly information in:

Shared/GlobalAssemblyInfo.cs

New projects should not generate independent assembly information when the selected archetype requires the shared assembly info.

Where required by the archetype, link:

Shared/GlobalAssemblyInfo.cs

using the correct relative path.

The relative path must be calculated from the actual project location.

Do not blindly copy a path from an existing project.

---

# Dependency Architecture

Project references must follow the selected archetype.

Do not use the old simplified rule:

API -> Services -> Common/Repository

as the only dependency model.

Use the dependency rules from:

references/project-archetypes.md

---

# Business Dependency Rules

For a complete Business slice:

API:

<ProjectName>.API
    -> Infrastructure API Host
    -> <ProjectName>.Services

Services:

<ProjectName>.Services
    -> <ProjectName>.Repository

Repository:

<ProjectName>.Repository
    -> Infrastructure DataAccess
    -> <ProjectName>.Common

Common:

<ProjectName>.Common
    -> Infrastructure Common

Services may additionally reference:

- Logging
- DateService
- required proxy projects

only when the selected business functionality/archetype requires them.

Do not add optional dependencies without evidence.

---

# Core Dependency Rules

Core uses the same layered principle but must be derived from the Core project structure.

Default:

<ProjectName>.API
    -> API Host
    -> <ProjectName>.Services

<ProjectName>.Services
    -> <ProjectName>.Repository
    -> <ProjectName>.Domain

<ProjectName>.Repository
    -> DataAccess
    -> <ProjectName>.Domain

Do not automatically add unrelated Infrastructure references.

If a Core structural reference establishes a different dependency, inspect and follow that architecture.

---

# Infrastructure Dependency Rules

Infrastructure references are now part of the project-creation phase only when:

- the selected archetype explicitly defines them
- the user explicitly requests them
- a similar-project structural reference establishes them

Do not invent Infrastructure dependencies.

Examples:

Repository archetype may reference:

Infrastructure/BNPP.NYBOSS.DataAccess

API archetype may reference:

Infrastructure/BNPP.NYBOSS.API.Host

Worker host may reference:

Infrastructure/BNPP.NYBOSS.Service.Host
Infrastructure MessageEngine projects

Message-processing library may reference:

Infrastructure MessageEngine projects

Workflow EXE may reference:

Infrastructure/BNPP.NYBOSS.CustomExecutable.Host
Infrastructure/BNPP.NYBOSS.Service.Host

These are archetype-specific dependencies, not universal dependencies.

---

# External References

Do not automatically add unrelated external project references.

External references are allowed when:

1. The selected archetype requires them.
2. A similar-project structure establishes them.
3. The user explicitly requests them.

Examples include:

- WCF proxy projects
- vendor DLL references
- DateService
- Logging
- MessageEngine
- Service.Host
- API.Host
- CustomExecutable.Host

Do not add a dependency merely because it exists somewhere else in the repository.

---

# Infrastructure Legacy Names

Existing infrastructure projects may have names such as:

BNPP.NYBOSS.API.Host
BNPP.NYBOSS.DataAccess
BNPP.NYBOSS.Logging
BNPP.NYBOSS.Service.Host
BNPP.NYBOSS.CustomExecutable.Host

These are existing projects.

They may be referenced when an archetype requires them.

Do not rename or recreate them.

Do not use those legacy names for newly created projects.

---

# Solution

Use the existing solution:

BNPP.NYBOSS.NextGen.Server.sln

Do not create a new solution.

Add every generated project to the existing solution.

Use:

dotnet sln BNPP.NYBOSS.NextGen.Server.sln add <project-path>

If the repository actually uses a solution format that the installed SDK cannot parse, do not invent a replacement solution.

Build/test individual projects when necessary.

---

# Creation Workflow

Before creation:

1. Read relevant reference files.
2. Identify application area.
3. Identify project/module.
4. Identify project name.
5. Identify requested archetype.
6. Identify project composition.
7. Determine project type.
8. Determine required project references.
9. Determine structural folders/files.

During creation:

1. Create the project with the appropriate .NET 8 template.
2. Preserve valid CLI-generated content.
3. Remove explicitly unwanted generated files.
4. Create required directories.
5. Create required structural files.
6. Configure project metadata.
7. Add applicable project references.
8. Add project to the existing solution.

After creation:

1. Validate project names.
2. Validate target framework.
3. Validate SDK.
4. Validate folders.
5. Validate structural files.
6. Validate project references.
7. Validate solution membership.
8. Validate that no unwanted `.http` file remains.
9. Validate that no legacy project name was introduced.
10. Validate that no business implementation was generated.

---

# No Business Implementation

This phase creates project structure and configuration.

Do not implement:

- business logic
- service logic
- repository logic
- controller functionality
- domain logic
- message-processing logic
- worker processing
- workflow logic
- Infrastructure behavior

Only create the minimal structural skeleton required by the selected archetype.

---

# Validation

Before completing a request, verify:

1. Correct application area.
2. Correct project/module.
3. Correct project name.
4. Correct module name.
5. Correct archetype.
6. Correct project composition.
7. Correct project SDK.
8. All projects target net8.0.
9. Correct folders exist.
10. Required structural files exist.
11. CLI-generated content remains intact.
12. API `.http` file is removed unless requested.
13. No new `BNPP.NYBOSS.*` project name was introduced.
14. Business uses Common.
15. Core uses Domain.
16. Common and Domain are not treated as interchangeable.
17. Correct archetype-specific references exist.
18. No unrelated references exist.
19. Infrastructure references are not invented.
20. Worker projects use the Worker SDK.
21. Workflow executables use `Microsoft.NET.Sdk` + `OutputType=Exe`.
22. Message-processing projects do not contain host logic.
23. Unit-test projects are leaf projects.
24. All generated projects are added to the existing solution.
25. No business/application implementation was generated.
26. Final structure matches the selected archetype.
