---
name: dotnet-developer
description: Create and modify NYBOSS .NET server projects using the repository's project structure, conventions, and .NET 8 tooling.
license: MIT
metadata:
  author: Copyright 2026 Google LLC
  version: '1.0'
---

# NYBOSS .NET Developer

Use this skill when the user asks to create or modify a NYBOSS .NET server project.

Read the relevant reference files before performing the requested operation.

---

# Project Creation

There are four supported creation concepts:

1. Default project
2. Explicit project composition
3. Project similar to an existing project
4. Module creation

Projects and modules may belong to:

- Business
- Core
- Infrastructure

Determine:

- application area
- project or module
- project name
- module name, if provided
- creation mode
- requested components, if provided

from the user's request.

The current project-creation phase creates only:

- project directories
- project files
- folders
- structural source files
- `.csproj` files
- solution entries
- project-to-project references where already defined by the Business/Core rules

This phase does not implement business functionality.

---

# Project vs Module

A project and a module are different concepts.

## Project

A project is an individual `.csproj` project.

For example:

<ProjectName>.API

<ProjectName>.Common

<ProjectName>.Repository

<ProjectName>.Services

If the user says:

"Create <ProjectName> project"

interpret this as a request for the default project composition for the selected application area.

If the user says:

"Create <ProjectName>.API project"

create only:

<ProjectName>.API

Do not create the other components.

---

## Module

A module is a logical container containing one or more related projects.

For example:

Apps/Business/<ModuleName>/

or:

Apps/Core/<ModuleName>/

or:

Infrastructure/<ModuleName>/

A module may contain multiple projects.

If the user explicitly says "module", create the module directory and the projects belonging to that module according to the selected application area and module type.

Do not interpret "module" as a single `.csproj` unless the requested module definition contains only one project.

---

# Project Name and Module Name

The user may:

- specify only a project name
- specify only a module name
- specify both
- specify a project component directly

Examples:

"Create <ProjectName> project"

"Create <ModuleName> module"

"Create <ProjectName> project inside <ModuleName> module"

"Create <ProjectName>.API project"

"Create <ModuleName> module with API, Common and Services"

Determine the requested structure from the wording.

If a module is explicitly provided, place the generated projects under that module.

If no module is provided, use the normal application-area location.

Do not invent an additional module directory when the user did not request one.

---

# Application Areas

NYBOSS server projects are organized into:

- Business
- Core
- Infrastructure

---

# Business

Business projects are located under:

Apps/Business/

The default Business project composition is:

<ProjectName>.API
<ProjectName>.Common
<ProjectName>.Repository
<ProjectName>.Services

A Business module uses the same default composition unless the user explicitly requests another composition.

Example:

Apps/Business/<ModuleName>/

with:

<ProjectName>.API
<ProjectName>.Common
<ProjectName>.Repository
<ProjectName>.Services

The module name and project name may be the same when the user requests a module without separately specifying a project name.

---

# Core

Core projects are located under:

Apps/Core/

The default Core project composition is:

<ProjectName>.API
<ProjectName>.Domain
<ProjectName>.Repository
<ProjectName>.Services

A Core module uses the same default composition unless the user explicitly requests another composition.

Example:

Apps/Core/<ModuleName>/

with:

<ProjectName>.API
<ProjectName>.Domain
<ProjectName>.Repository
<ProjectName>.Services

Domain and Common are different project types.

Do not replace Domain with Common for Core projects.

Do not assume that Domain and Common contain the same structure or responsibilities.

---

# Infrastructure

Infrastructure projects are located under:

Infrastructure/

Infrastructure does not have one universal project composition.

Infrastructure contains both:

1. Multi-project modules
2. Standalone infrastructure projects

Examples of Infrastructure module categories found in the repository include:

- API client modules
- service modules
- message-processing modules
- watcher modules
- notification modules
- transport modules

Examples of standalone infrastructure projects include:

- API host projects
- API infrastructure projects
- common infrastructure projects
- executable host projects
- data access projects
- logging projects
- repository projects
- service host projects
- storage projects

Do not assume that every Infrastructure module uses the same project composition.

Use the known Infrastructure module templates defined in:

references/project-creation.md

and:

references/project-structure.md

If the requested Infrastructure module matches a known module type, use that module template.

If the user explicitly specifies components, create only those components.

If the requested Infrastructure structure cannot be determined from the known module types or explicit components, do not invent an architecture.

---

# Structure and Generated Content Rules

The current project creation stage creates the project skeleton.

There are two types of generated files.

## 1. CLI-generated files

When a file is generated by a .NET CLI template, keep its generated content.

Examples include:

- Program.cs
- `.csproj`
- `launchSettings.json`
- template-generated `appsettings` files
- other files created by `dotnet new`

Do not:

- empty these files
- replace their content
- overwrite their content with placeholders
- copy content from another application

The content generated by the .NET template must remain intact.

Only make changes required by the project-generation rules, such as:

- target framework
- project references
- project naming
- required project configuration

---

## 2. NYBOSS structural files

When additional files are required to represent the NYBOSS project structure, create them with minimal, valid, project-specific skeleton content.

These files provide the initial project structure only.

Do not implement business functionality in them.

---

# Do Not Copy Existing Implementation

Do not copy implementation from existing applications during normal project creation.

Do not copy:

- business logic
- controller implementations
- service implementations
- repository implementations
- domain implementations
- handler implementations
- model implementations
- configuration values
- connection strings
- secrets
- application data
- application-specific settings

For a similar-project request, existing projects are structural references only.

---

# 1. Default Project

Determine whether the user requested a Business, Core, or Infrastructure project.

---

## Default Business Project

If the user asks:

"Create <ProjectName> project"

and the selected application area is Business, create:

<ProjectName>.API
<ProjectName>.Common
<ProjectName>.Repository
<ProjectName>.Services

under:

Apps/Business/<ProjectName>/

If the user says:

"Create <ProjectName> module"

and the selected application area is Business, create:

Apps/Business/<ProjectName>/

containing the default Business composition:

<ProjectName>.API
<ProjectName>.Common
<ProjectName>.Repository
<ProjectName>.Services

Use:

references/project-creation.md

and:

references/project-structure.md

to determine the required structure.

---

## Default Core Project

If the user asks:

"Create <ProjectName> project"

and the selected application area is Core, create:

<ProjectName>.API
<ProjectName>.Domain
<ProjectName>.Repository
<ProjectName>.Services

under:

Apps/Core/<ProjectName>/

If the user says:

"Create <ProjectName> module"

and the selected application area is Core, create:

Apps/Core/<ProjectName>/

containing:

<ProjectName>.API
<ProjectName>.Domain
<ProjectName>.Repository
<ProjectName>.Services

Use:

references/project-creation.md

and:

references/project-structure.md

to determine the required structure.

---

## Default Infrastructure Project

Infrastructure does not have one universal default project composition.

If the user says:

"Create <ProjectName> project"

for Infrastructure, treat <ProjectName> as an individual Infrastructure project unless the user identifies a known Infrastructure module type.

Create the project using the appropriate .NET 8 template and do not invent additional projects.

---

# 2. Explicit Project Composition

If the user explicitly specifies project components, create only the requested components.

Do not automatically add missing components.

---

## Business

For:

"Create <ProjectName> project with API, Common and Services"

create:

<ProjectName>.API
<ProjectName>.Common
<ProjectName>.Services

under:

Apps/Business/<ProjectName>/

Do not create Repository.

---

## Core

For:

"Create <ProjectName> project with API, Domain and Services"

create:

<ProjectName>.API
<ProjectName>.Domain
<ProjectName>.Services

under:

Apps/Core/<ProjectName>/

Do not create Repository.

---

## Infrastructure

For:

"Create <ModuleName> module with Common, Repository and Services"

create:

<ModuleName>.Common
<ModuleName>.Repository
<ModuleName>.Services

under:

Infrastructure/<ModuleName>/

Create only the explicitly requested components.

---

# Direct Component Project Creation

If the user names a component as part of the project name, create only that project.

For example:

"Create <ProjectName>.API project"

creates only:

<ProjectName>.API

Do not create:

<ProjectName>.Common
<ProjectName>.Repository
<ProjectName>.Services

or any other project.

Similarly:

"Create <ProjectName>.Repository project"

creates only:

<ProjectName>.Repository

---

# 3. Similar to an Existing Project

If the user asks for a project similar to an existing project:

1. Locate the existing project.
2. Determine whether it is Business, Core, or Infrastructure.
3. Determine whether it belongs to a module.
4. Determine its project composition.
5. Determine project types.
6. Inspect the project directory structure.
7. Inspect folder names.
8. Inspect file names.
9. Determine applicable project-to-project relationships where required.
10. Create equivalent projects under the appropriate application area.
11. Recreate the applicable folder hierarchy.
12. Recreate the applicable file hierarchy.
13. Rename project-specific names to the new project name.
14. Apply the new naming convention.
15. Recreate applicable internal project references using the new project names where the existing Business/Core rules require them.
16. Add generated projects to the existing solution.

The existing project is a STRUCTURAL REFERENCE ONLY.

---

# Module Creation

Module creation is supported in:

- Business
- Core
- Infrastructure

A module is a container for one or more projects.

---

## Business Module

Default Business module:

Apps/Business/<ModuleName>/

contains:

<ModuleName>.API
<ModuleName>.Common
<ModuleName>.Repository
<ModuleName>.Services

The user may explicitly specify another composition.

---

## Core Module

Default Core module:

Apps/Core/<ModuleName>/

contains:

<ModuleName>.API
<ModuleName>.Domain
<ModuleName>.Repository
<ModuleName>.Services

The user may explicitly specify another composition.

---

## Infrastructure Module

Infrastructure module composition depends on the module type.

Known Infrastructure module types are defined in:

references/project-creation.md

and:

references/project-structure.md

Do not force Business/Core project composition onto Infrastructure.

Do not assume every Infrastructure module contains Common, Repository and Services.

---

# Module Naming

A module directory is not itself a `.csproj` project.

For a module:

<ModuleName>/

contains one or more projects.

New project names must use the new naming convention.

Examples:

<ModuleName>.API
<ModuleName>.Common
<ModuleName>.Domain
<ModuleName>.Repository
<ModuleName>.Services

Do not add:

BNPP.NYBOSS.

or another legacy organizational prefix to newly created projects.

---

# Similar Project Inspection Rules

For a similar-project request, inspect only information required to determine structure.

Allowed information includes:

- project names
- project types
- project directories
- module directories
- folder names
- subfolder names
- file names
- file locations
- project-to-project relationships
- project metadata required to determine project type

Do not inspect source files for the purpose of copying their implementation.

Do not copy:

- source-code implementation
- class implementation
- method implementation
- business logic
- controller implementation
- service implementation
- repository implementation
- domain implementation
- handler implementation
- model implementation
- configuration values
- connection strings
- secrets
- application-specific settings
- application data
- business-specific JSON content
- business-specific XML content

---

# Similar Project File Handling

If the reference project contains:

<Folder>/<File>

create the corresponding folder and file in the new project.

Reproduce:

- file name
- file location
- file type

Do not reproduce the reference file's implementation.

If the new project is created using a .NET CLI template and the CLI generates a file with the same name, keep the CLI-generated content.

If the file is a structural file created specifically by the generator, create minimal valid project-specific skeleton content.

---

# Naming Convention

All newly generated projects must use the new NYBOSS naming convention.

The general format is:

<ProjectName>.<Component>

Examples:

<ProjectName>.API
<ProjectName>.Common
<ProjectName>.Domain
<ProjectName>.Repository
<ProjectName>.Services

The same rule applies to newly generated Infrastructure projects.

Do not add legacy organizational prefixes.

For example, do not create new projects named:

BNPP.NYBOSS.<ProjectName>.<Component>

Existing legacy names are reference structures only.

---

# Project and Module Naming

Use the requested name as the base name.

Examples:

<ProjectName>

<ProjectName>.API

<ProjectName>.Common

<ProjectName>.Repository

<ProjectName>.Services

For module-based creation:

<ModuleName>/

<ModuleName>.API

<ModuleName>.Common

<ModuleName>.Repository

<ModuleName>.Services

Do not introduce underscores, organizational prefixes, or additional naming segments unless the user explicitly requests them or the selected structure requires them.

---

# Common and Domain Rule

Common and Domain are separate concepts.

For Business projects:

<ProjectName>.Common

is the shared Common project.

For Core projects:

<ProjectName>.Domain

is the domain project.

Do not automatically convert:

Common -> Domain

or:

Domain -> Common

when creating projects.

When creating a similar project, preserve the component type of the reference project unless the user explicitly requests a different composition.

---

# Target Framework

All newly created NYBOSS projects must target:

net8.0

Use .NET 8 explicitly when creating projects.

Do not allow the latest installed SDK/framework to determine the target framework automatically.

Every generated project must contain:

<TargetFramework>net8.0</TargetFramework>

unless the user explicitly requests another framework.

This rule applies to:

- Business
- Core
- Infrastructure

---

# Project Creation Tools

Use the .NET CLI to create .NET projects.

All newly generated projects must target .NET 8.

---

## API Projects

For API projects, use the controller-based ASP.NET Core Web API template.

Use the equivalent of:

dotnet new webapi --framework net8.0 --use-controllers --name <ProjectName>.API

The API project must not retain the template-generated `.http` file.

If the template creates:

<ProjectName>.API.http

remove that file after project creation.

---

## Class Library Projects

For class-library projects use:

dotnet new classlib --framework net8.0 --name <ProjectName>.<Component>

---

## Other Project Types

If an Infrastructure project requires another .NET project type, determine the appropriate .NET 8 template from the requested project type or the structural reference.

Do not use an arbitrary project template.

---

# Solution

Use the existing solution:

BNPP.NYBOSS.NextGen.Server.sln

Do not create a new solution.

Every newly generated project must be added to the existing solution.

Use:

dotnet sln BNPP.NYBOSS.NextGen.Server.sln add <project-path>

---

# Business Project Types

For the default Business project:

<ProjectName>.API

is an ASP.NET Core Web API project using controllers.

The following are class-library projects:

<ProjectName>.Common
<ProjectName>.Repository
<ProjectName>.Services

All target:

net8.0

---

# Core Project Types

For the default Core project:

<ProjectName>.API

is an ASP.NET Core Web API project using controllers.

The following are class-library projects:

<ProjectName>.Domain
<ProjectName>.Repository
<ProjectName>.Services

All target:

net8.0

---

# Default Business Project References

When all default Business components are created:

<ProjectName>.API
    -> <ProjectName>.Services

<ProjectName>.Services
    -> <ProjectName>.Common
    -> <ProjectName>.Repository

<ProjectName>.Repository
    -> <ProjectName>.Common

References must point to components belonging to the same newly created Business project.

Do not reference equivalent components belonging to another application.

---

# Default Core Project References

When all default Core components are created:

<ProjectName>.API
    -> <ProjectName>.Services

<ProjectName>.Services
    -> <ProjectName>.Domain
    -> <ProjectName>.Repository

<ProjectName>.Repository
    -> <ProjectName>.Domain

References must point to components belonging to the same newly created Core project.

Do not reference equivalent components belonging to another application.

---

# Explicit Project References

For explicitly requested Business/Core project compositions:

- create only the requested projects
- add only applicable references
- do not reference components that were not created
- do not add unrelated external project references

References must be based on the components that actually exist.

Infrastructure project references are not part of the current Infrastructure creation phase.

Do not add Infrastructure references automatically.

---

# External Project References

Do not automatically add references to existing external NYBOSS projects.

If the user explicitly requests an external project reference, add only the requested reference.

External project references are separate from normal project creation.

---

# Project Structure Generation

For every generated project:

1. Determine the application area.
2. Determine whether the request is for a project or module.
3. Determine the requested project name and module name.
4. Determine the project composition.
5. Create the project using the appropriate .NET 8 CLI template.
6. Keep all valid content generated by the CLI template.
7. Remove unwanted template-generated files explicitly excluded by the NYBOSS structure.
8. Create required NYBOSS directories.
9. Create required NYBOSS structural files.
10. Configure the target framework.
11. Add applicable Business/Core project references.
12. Add the project to the existing solution.
13. Validate the final project structure.

For Infrastructure:

- create only the requested project/module structure
- do not invent project references
- do not add Infrastructure dependencies automatically

Do not overwrite valid CLI-generated file content.

---

# Empty Folder Rule

Required structural folders must not remain represented only as empty directories when Visual Studio or source control would not display them.

Where a required folder would otherwise be empty, create the appropriate minimal structural file defined in the project structure reference.

For Business Common, use project-specific structural files such as:

<ProjectName>Constants.cs
<ProjectName>Model.cs
I<ProjectName>Repository.cs
I<ProjectName>Service.cs
<ProjectName>Options.cs

For Core Domain, use the structural files defined in:

references/project-creation.md

and:

references/project-structure.md

For Repository and Services, create:

<ProjectName>Repository.cs
<ProjectName>Service.cs

For Infrastructure, use the structural files defined by the selected Infrastructure project/module template.

These files are structural skeletons only.

Do not implement business functionality in them.

---

# CLI-Generated File Rule

Never blank or replace content that was generated by the .NET CLI.

For example, if `dotnet new` creates:

Program.cs

keep the generated `Program.cs` content.

If `dotnet new` creates:

<ProjectName>.csproj

keep the generated project configuration and modify it only where required to:

- target net8.0
- add required Business/Core project references
- apply required project naming
- apply required project configuration

Do not replace the entire file with an empty or placeholder file.

---

# Template Cleanup

After creating an API project, remove template-generated files that are not part of the required NYBOSS structure.

The default example is:

<ProjectName>.API.http

Do not create or retain this file unless the user explicitly requests it or the structural reference requires it.

Do not remove valid files required by the NYBOSS structure.

---

# Current Phase Boundary

The current phase creates:

- projects
- project configuration
- folders
- structural source files
- Business/Core project references where already defined
- solution entries

For Infrastructure, this phase focuses only on:

- projects
- project configuration
- folders
- structural source files
- solution entries

Infrastructure project references are intentionally deferred.

The current phase does not implement:

- business functionality
- application functionality
- service logic
- repository logic
- domain logic
- controller functionality
- Infrastructure functionality

Structural source files should contain only minimal valid skeleton code.

CLI-generated files retain their original generated content.

---

# Validation

Before completing a project creation request, verify:

1. The requested project or module exists.
2. The correct application area is used.
3. The requested project components exist.
4. Required folders exist.
5. Required structural files exist.
6. CLI-generated files retain their generated content unless a specific project configuration change was required.
7. No unwanted `.http` API file remains.
8. Project names follow the new naming convention.
9. No new legacy `BNPP.NYBOSS.*` project name was introduced.
10. Business projects use Common where Common was requested.
11. Core projects use Domain where Domain was requested.
12. Common and Domain were not incorrectly treated as interchangeable.
13. All generated projects target net8.0 unless another framework was explicitly requested.
14. All generated projects are added to the existing solution.
15. Required Business/Core internal references point to the newly generated project's components.
16. No unintended external project references were added.
17. Infrastructure references were not added automatically.
18. The generated folder and file structure matches the selected creation mode.
19. Module creation creates the correct module directory and projects inside it.
20. Direct component requests create only the requested project.
21. Similar-project generation reproduces structure without copying implementation.
22. No business or application implementation was generated.
23. The solution remains valid after project creation.
