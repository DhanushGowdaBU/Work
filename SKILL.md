---
name: dotnet-developer
description: Create and modify NYBOSS .NET 8 server modules and projects using the repository's project structure, conventions, and .NET CLI tooling.
license: MIT
metadata:
  author: Copyright 2026 Google LLC
  version: '1.0'
---

# NYBOSS .NET Developer

Use this skill when the user asks to create or modify a NYBOSS .NET server module or project.

Read the relevant reference files before performing the requested operation.

The current phase covers:

- module creation
- project creation
- project folders
- project files
- structural source files
- `.csproj` files
- project naming
- project configuration
- solution entries

The current phase does not implement application business functionality.

---

# Application Areas

NYBOSS server code is organized into three application areas:

- Business
- Core
- Infrastructure

Business projects are located under:

Apps/Business/

Core projects are located under:

Apps/Core/

Infrastructure projects and modules are located under:

Infrastructure/

Determine the application area from the user's request.

If the user explicitly specifies an application area, use it.

Examples:

- "Create a Business Customer project"
- "Create a Core Configuration project"
- "Create an Infrastructure Storage project"

If the application area is not explicitly stated, use the wording and requested project/module type to determine the most appropriate area.

Do not place a project in multiple application areas.

---

# Module and Project Concepts

A module is a logical container/folder that can contain one or more projects.

A project is an actual .NET project containing a `.csproj` file.

A module and a project are not the same thing.

For example:

Apps/Business/Customer/

is a module.

Inside it:

Customer.API/
Customer.Common/
Customer.Repository/
Customer.Services/

are projects.

---

# Module Creation

The user may explicitly request only a module.

Examples:

- "Create a Customer module"
- "Create an Order module in Business"
- "Create a Configuration module in Core"
- "Create a Storage module in Infrastructure"

When the user requests only a module:

1. Create the appropriate module directory.
2. Do not create any projects automatically.
3. Do not create `.csproj` files.
4. Do not create project-specific source files.
5. Do not create default Business/Core components.
6. Do not infer a project composition.

For example:

"Create a Customer module in Business"

creates:

Apps/Business/Customer/

and nothing else.

---

# Module Naming

New module names must use the user-requested logical module name.

Do not add legacy organizational prefixes.

For example:

Customer

Order

Configuration

Storage

are valid new module names.

Do not automatically create:

BNPP.NYBOSS.Customer
BNPP.NYBOSS.Order
BNPP.NYBOSS.Configuration

for new modules.

Existing legacy module names are reference structures only.

---

# Project Naming

All newly generated projects must use the new NYBOSS naming convention.

The base format is:

<ProjectName>.<Component>

Examples:

<ProjectName>.API
<ProjectName>.Common
<ProjectName>.Domain
<ProjectName>.Repository
<ProjectName>.Services
<ProjectName>.ServiceHost
<ProjectName>.SDK

Do not add legacy organizational prefixes.

Do not create new projects using names such as:

BNPP.NYBOSS.Customer.API
BNPP.NYBOSS.Configuration.Domain
BNPP.NYBOSS.Storage

unless the user explicitly requires that exact legacy name.

The new naming convention applies to:

- Business projects
- Core projects
- Infrastructure projects
- projects inside modules
- standalone projects
- projects created from similar-project requests

When a module is specified, the module name is represented by the directory hierarchy, not by adding an unnecessary organizational prefix to the project name.

For example:

Apps/Business/Customer/

contains:

Customer.API
Customer.Common
Customer.Repository
Customer.Services

---

# Project Name vs Module Name

The user may provide:

- only a project name
- only a module name
- both a module and project name

Interpret the request carefully.

## Project only

"Create a Customer project"

means:

Module = Customer

Projects = default projects for the selected application area.

For Business:

Apps/Business/Customer/

with:

Customer.API
Customer.Common
Customer.Repository
Customer.Services

For Core:

Apps/Core/Customer/

with:

Customer.API
Customer.Domain
Customer.Repository
Customer.Services

---

## Project with explicit component

"Create a Customer API project"

means:

Project = Customer.API

Create only:

Apps/Business/Customer/Customer.API/

Do not create:

Customer.Common
Customer.Repository
Customer.Services

unless explicitly requested.

Likewise:

"Create a Customer Repository project"

creates only:

Customer.Repository

---

## Project with explicit module

If the user says:

"Create Customer project in Order module"

create:

Apps/Business/Order/Customer/

with the default Business project composition.

The projects are:

Order/
└── Customer/
    ├── Customer.API/
    ├── Customer.Common/
    ├── Customer.Repository/
    └── Customer.Services/

The module name does not become a legacy project prefix.

---

## Explicit module and component

If the user says:

"Create Customer API project in Order module"

create only:

Apps/Business/Order/Customer/Customer.API/

Do not create other Customer projects.

---

# Project Creation Modes

There are four supported project/module creation modes:

1. Module-only creation
2. Default project creation
3. Explicit project composition
4. Similar-project creation

Determine the mode from the user's wording.

---

# 1. Module-Only Creation

If the user explicitly asks to create a module and does not request a project:

create only the module directory.

Example:

"Create Customer module"

Business:

Apps/Business/Customer/

Core:

Apps/Core/Customer/

Infrastructure:

Infrastructure/Customer/

Do not create projects automatically.

---

# 2. Default Project Creation

A default project request means the user wants the normal project composition for the selected application area.

Examples:

- "Create a Customer project"
- "Create a simple Customer project"
- "Create a basic Customer project"
- "Create a normal Customer project"
- "Create a standard Customer project"
- "Create a regular Customer project"
- "Create a default Customer project"

Do not interpret a component word such as API, Repository, Services, Domain, Common or ServiceHost as a default project request.

If the user explicitly names a component, create only that project unless multiple components are explicitly requested.

---

# Default Business Project

For a default Business project create:

<ProjectName>.API
<ProjectName>.Common
<ProjectName>.Repository
<ProjectName>.Services

under:

Apps/Business/<Module>/<ProjectName>/

If no separate module is specified, use the project name as the module:

Apps/Business/<ProjectName>/

For example:

"Create Customer project"

creates:

Apps/Business/Customer/
├── Customer.API/
├── Customer.Common/
├── Customer.Repository/
└── Customer.Services/

---

# Default Core Project

For a default Core project create:

<ProjectName>.API
<ProjectName>.Domain
<ProjectName>.Repository
<ProjectName>.Services

under:

Apps/Core/<Module>/<ProjectName>/

If no separate module is specified, use the project name as the module:

Apps/Core/<ProjectName>/

For example:

"Create Configuration project in Core"

creates:

Apps/Core/Configuration/
├── Configuration.API/
├── Configuration.Domain/
├── Configuration.Repository/
└── Configuration.Services/

---

# Infrastructure Project Creation

Infrastructure does not have one universal project composition.

Existing Infrastructure contains different types of projects and modules.

Examples include:

- API clients
- service modules
- watchers
- messaging components
- notification components
- transport components
- host projects
- common libraries
- data-access libraries
- logging libraries
- storage libraries

Do not invent a universal Infrastructure composition.

For Infrastructure, project structure is determined by:

1. Explicit user requirements, or
2. An existing Infrastructure project/module used as a structural reference.

---

# Infrastructure Explicit Project

If the user explicitly requests an Infrastructure project:

create the requested project using the .NET 8 appropriate template.

Example:

"Create Storage project in Infrastructure"

creates:

Infrastructure/Storage/

with:

Storage.csproj

Do not automatically create:

Storage.Common
Storage.Repository
Storage.Services

unless requested.

---

# Infrastructure Explicit Components

If the user specifies components:

"Create EmailWatcher with Common, Repository and Services in Infrastructure"

create:

Infrastructure/EmailWatcher/
├── EmailWatcher.Common/
├── EmailWatcher.Repository/
└── EmailWatcher.Services/

Create only the requested projects.

Do not add ServiceHost unless explicitly requested.

For example:

"Create EmailWatcher with Common, Repository, Services and ServiceHost"

creates:

Infrastructure/EmailWatcher/
├── EmailWatcher.Common/
├── EmailWatcher.Repository/
├── EmailWatcher.ServiceHost/
└── EmailWatcher.Services/

---

# Infrastructure Module Creation

If the user requests only an Infrastructure module:

"Create FileWatcher module in Infrastructure"

create:

Infrastructure/FileWatcher/

Do not create projects automatically.

---

# Infrastructure Similar Project

If the user requests an Infrastructure project similar to an existing Infrastructure project:

1. Locate the reference project.
2. Inspect its project type.
3. Inspect its directory structure.
4. Inspect its folders.
5. Inspect its subfolders.
6. Inspect its file names.
7. Inspect its project metadata needed to determine the project type.
8. Recreate the applicable structure using the new project name.
9. Apply the new naming convention.
10. Do not copy implementation.

Example:

If the existing project is:

Infrastructure/BNPP.NYBOSS.Storage/

with:

Options/
GlobalAssemblyInfo.cs
IS3FileManager.cs
RegisterServices.cs
S3FileManager.cs

and the user requests:

"Create FileStorage similar to BNPP.NYBOSS.Storage"

create:

Infrastructure/FileStorage/

with the equivalent structural files renamed where appropriate.

Do not copy the implementation from Storage.

---

# Infrastructure Similar Module

If the user requests:

"Create a new Infrastructure module similar to CurrencyService"

inspect the entire module structure.

For example:

Infrastructure/CurrencyService/

may contain:

CurrencyService.Common/
CurrencyService.Repository/
CurrencyService.Services/

Recreate the applicable structure for the new module.

Do not assume every Infrastructure module has the same composition.

---

# Similar Project Rules

Existing projects are structural references only.

Similar-project creation means:

STRUCTURE SIMILARITY

not:

CONTENT COPYING

Inspect only information needed to determine structure.

Allowed:

- project names
- project type
- project directories
- folder names
- subfolder names
- file names
- file locations
- project metadata required to determine project type
- project-to-project relationships when needed to reproduce structure

Do not copy:

- business logic
- source-code implementation
- controller implementation
- service implementation
- repository implementation
- domain implementation
- handler implementation
- model implementation
- configuration values
- connection strings
- secrets
- application data
- business-specific JSON
- business-specific XML
- unrelated dependencies
- application-specific settings

---

# Similar Project Naming

When recreating a similar project:

1. Preserve the structural component type.
2. Replace reference project-specific names with the new project name.
3. Do not preserve legacy organizational prefixes.
4. Apply the new naming convention to newly created projects.
5. Preserve the directory/module organization where applicable.

Example:

Existing:

BNPP.NYBOSS.Configuration.Domain

New:

Configuration.Domain

Existing:

BNPP.NYBOSS.Configuration.Repository

New:

Configuration.Repository

Existing:

BNPP.NYBOSS.Configuration.Services

New:

Configuration.Services

---

# Domain and Common

Common and Domain are separate project types.

Business uses:

<ProjectName>.Common

Core uses:

<ProjectName>.Domain

Do not automatically convert:

Common -> Domain

or:

Domain -> Common

When creating a similar project, preserve the reference project's component type.

---

# Target Framework

All newly generated NYBOSS projects must target:

net8.0

.NET 8 is the standard framework for the NYBOSS server repository.

Do not allow the latest installed SDK/framework to determine the target framework automatically.

Use:

--framework net8.0

when supported by the selected template.

Every generated project must target net8.0 unless the user explicitly requests another framework.

---

# Project Creation Tools

Use the .NET CLI for project creation.

Do not use npm, tsx or an Angular generator for server-side project creation.

Use the .NET CLI appropriate to the project type.

---

# API Projects

For API projects use the controller-based ASP.NET Core Web API template.

Use:

dotnet new webapi --framework net8.0 --use-controllers --name <ProjectName>.API

Remove the generated:

<ProjectName>.API.http

file unless the user explicitly requests it.

Do not remove valid CLI-generated files.

---

# Class Library Projects

For standard class-library projects use:

dotnet new classlib --framework net8.0 --name <ProjectName>.<Component>

Examples:

dotnet new classlib --framework net8.0 --name Customer.Common

dotnet new classlib --framework net8.0 --name Customer.Domain

dotnet new classlib --framework net8.0 --name Customer.Repository

dotnet new classlib --framework net8.0 --name Customer.Services

---

# Infrastructure Project Type

Infrastructure projects do not all use the same template.

Determine the appropriate .NET project template from:

- explicit user request
- reference project metadata
- existing project structure

Do not blindly use Web API or Class Library for every Infrastructure project.

If the project type cannot be determined from the request or reference structure, use the most appropriate standard .NET 8 template based on the project name and purpose.

Do not copy the reference `.csproj`.

---

# CLI-Generated File Rule

When `dotnet new` generates a file, retain its generated content.

Examples:

- Program.cs
- `.csproj`
- launchSettings.json
- appsettings.json
- appsettings.Development.json
- other valid template-generated files

Do not:

- blank the file
- replace it with a placeholder
- recreate it unnecessarily
- copy implementation from another project

Only modify CLI-generated files when required for:

- net8.0 targeting
- project naming
- required project configuration
- explicitly requested project structure

---

# Structural Files

Files created specifically to represent NYBOSS structure may contain minimal valid skeleton code.

Structural files must not contain business implementation.

Examples include:

<ProjectName>Model.cs
<ProjectName>Constants.cs
<ProjectName>Options.cs
I<ProjectName>Repository.cs
I<ProjectName>Service.cs
<ProjectName>Repository.cs
<ProjectName>Service.cs

For Infrastructure, do not assume these files are required.

Create Infrastructure structural files only when:

- explicitly requested, or
- required by a similar-project structural reference.

---

# Empty Folder Rule

A required folder should have a representative file when the development environment or source control would otherwise not represent the folder.

For default Business/Core structures, create the structural files defined in:

references/project-creation.md

and:

references/project-structure.md

For Infrastructure, do not create arbitrary placeholder files merely to populate a folder.

Use the reference project's structure when generating a similar Infrastructure project.

---

# Default Business Structure

Default Business:

Apps/Business/<Module>/<ProjectName>/

Projects:

<ProjectName>.API
<ProjectName>.Common
<ProjectName>.Repository
<ProjectName>.Services

If no module is explicitly supplied:

<Module> = <ProjectName>

---

# Default Core Structure

Default Core:

Apps/Core/<Module>/<ProjectName>/

Projects:

<ProjectName>.API
<ProjectName>.Domain
<ProjectName>.Repository
<ProjectName>.Services

If no module is explicitly supplied:

<Module> = <ProjectName>

---

# Explicit Composition

If the user explicitly specifies project components:

create only those components.

Example:

"Create Customer with API, Common and Services"

creates:

Customer.API
Customer.Common
Customer.Services

Do not create:

Customer.Repository

because it was not requested.

Likewise:

"Create Configuration with API, Domain and Services"

creates:

Configuration.API
Configuration.Domain
Configuration.Services

---

# Single Component Project Requests

If the user names a specific project component, treat it as an explicit single-project request.

Examples:

"Create Customer API project"

creates only:

Customer.API

"Create Customer Common project"

creates only:

Customer.Common

"Create Customer Domain project"

creates only:

Customer.Domain

"Create Customer Repository project"

creates only:

Customer.Repository

"Create Customer Services project"

creates only:

Customer.Services

"Create Customer ServiceHost project"

creates only:

Customer.ServiceHost

Do not create the default project composition in these cases.

---

# Project Creation with Module

If a module is explicitly provided, place the project inside that module.

Example:

"Create Customer API project in Order module"

creates:

Apps/Business/Order/Customer/Customer.API/

Example:

"Create Customer project in Order module"

creates:

Apps/Business/Order/Customer/
├── Customer.API/
├── Customer.Common/
├── Customer.Repository/
└── Customer.Services/

The module name must not be added to the project name unless explicitly requested.

---

# Module-Only vs Project Requests

Use the following interpretation:

"Create Customer module"

= module only.

"Create Customer project"

= default project composition.

"Create Customer API project"

= only Customer.API.

"Create Customer project with API and Services"

= only Customer.API and Customer.Services.

"Create Customer project in Order module"

= default Customer project composition inside Order.

"Create Customer module with Customer API project"

= create the Customer module and Customer.API project inside it.

Do not create additional projects unless requested or required by the selected default composition.

---

# Solution

Use the existing solution:

BNPP.NYBOSS.NextGen.Server.sln

Do not create a new solution.

Every newly created .NET project must be added to the existing solution.

Use:

dotnet sln BNPP.NYBOSS.NextGen.Server.sln add <project-path>

---

# Project References

Reference creation is not part of the current Infrastructure phase.

For Business/Core default projects, preserve the currently defined internal project-reference rules.

For Infrastructure creation in this phase:

- do not infer references
- do not copy references from existing projects
- do not add external references
- do not modify dependency relationships unless explicitly requested

Infrastructure project creation currently focuses on:

- project creation
- folders
- files
- project configuration
- solution registration

---

# Existing Projects

Existing projects may use legacy names such as:

BNPP.NYBOSS.<ProjectName>.<Component>

These names must not be used as the naming convention for new projects.

Existing projects are reference material only.

Example:

Existing:

BNPP.NYBOSS.Configuration.Domain

New:

Configuration.Domain

---

# Module and Project Directory Rules

Business:

Apps/Business/<Module>/<Project>/

Core:

Apps/Core/<Module>/<Project>/

Infrastructure:

Infrastructure/<Module>/<Project>/

For standalone Infrastructure projects where there is no logical module:

Infrastructure/<Project>/

If the user explicitly provides a module, use:

Infrastructure/<Module>/<Project>/

Do not create an unnecessary additional module directory.

---

# Project Structure Generation

For every requested project:

1. Determine application area.
2. Determine module if supplied.
3. Determine whether the request is module-only or project creation.
4. Determine project creation mode.
5. Determine project components.
6. Determine project type.
7. Determine target framework.
8. Create the required project using the .NET CLI.
9. Keep valid CLI-generated content.
10. Remove explicitly unwanted template files.
11. Create required folders.
12. Create required structural files.
13. Apply the new project naming convention.
14. Add the project to the existing solution.
15. Validate the generated structure.

Do not implement business functionality.

---

# Infrastructure Structure Rule

Infrastructure has no universal default project composition.

Do not assume:

Common + Repository + Services

for every Infrastructure project.

Do not assume:

API + Common + Repository + Services

for every Infrastructure project.

Do not assume:

ServiceHost + Services

for every Infrastructure project.

Determine Infrastructure structure from:

- explicit user request
- similar-project reference
- similar-module reference

---

# Current Phase Boundary

This phase creates:

- modules
- projects
- project directories
- folders
- structural source files
- `.csproj` files
- project configuration
- solution entries

This phase does not implement:

- business logic
- controller functionality
- service functionality
- repository functionality
- domain logic
- Infrastructure implementation
- integrations
- database logic
- external service logic

Structural source files must contain only minimal valid skeleton code.

CLI-generated files must retain their generated content.

---

# Validation

Before completing a request, verify:

1. The correct application area is used.
2. Module-only requests created only a module.
3. Project requests created the requested projects.
4. Default Business projects use API/Common/Repository/Services.
5. Default Core projects use API/Domain/Repository/Services.
6. Explicit component requests create only requested projects.
7. A request such as "Customer API project" creates only Customer.API.
8. Module names are represented by directory structure.
9. Project names follow the new naming convention.
10. Legacy BNPP.NYBOSS prefixes are not introduced into new project names.
11. All projects target net8.0 unless explicitly overridden.
12. CLI-generated content is preserved.
13. Unwanted API `.http` files are removed.
14. Required folders exist.
15. Required structural files exist.
16. Infrastructure does not receive an invented default composition.
17. Similar-project generation reproduces structure without copying implementation.
18. Projects are added to the existing solution.
19. No business functionality is generated.
20. The solution remains valid.
