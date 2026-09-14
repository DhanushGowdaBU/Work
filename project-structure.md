# NYBOSS Project Structure

The NYBOSS server repository contains:

- Business applications
- Core applications
- Infrastructure projects
- Infrastructure modules

Project composition varies by application area.

This document defines the structure used when generating new projects and modules.

It does not define application implementation.

---

# Application Areas

NYBOSS projects are organized under:

Apps/Business/

Apps/Core/

Infrastructure/

---

# Project vs Module

A project is an individual `.csproj`.

A module is a logical directory containing one or more projects.

For example:

Apps/Business/<ModuleName>/

Apps/Core/<ModuleName>/

Infrastructure/<ModuleName>/

Do not create a module directory unless the user requests a module or the selected structure explicitly requires one.

---

# Business Applications

Business applications are normally located under:

Apps/Business/

Default Business composition:

<ProjectName>.API
<ProjectName>.Common
<ProjectName>.Repository
<ProjectName>.Services

---

# Default Business Module

A Business module uses:

Apps/Business/<ModuleName>/

with:

<ModuleName>.API
<ModuleName>.Common
<ModuleName>.Repository
<ModuleName>.Services

---

# Default Business API Structure

<ProjectName>.API/

├── Controllers/
├── Properties/
│   └── launchSettings.json
├── appsettings.json
├── appsettings.Development.json
├── appsettings.Local.json
├── build-info.json
├── <ProjectName>.API.csproj
└── Program.cs

The API uses the controller-based ASP.NET Core Web API template.

The template-generated:

<ProjectName>.API.http

file is not part of the default NYBOSS structure.

If generated, remove it.

CLI-generated files must retain their generated content.

---

# Default Business Common Structure

<ProjectName>.Common/

├── Constants/
│   └── <ProjectName>Constants.cs
├── Contracts/
│   ├── Repository/
│   │   └── I<ProjectName>Repository.cs
│   └── Services/
│       └── I<ProjectName>Service.cs
├── Models/
│   └── <ProjectName>Model.cs
├── Options/
│   └── <ProjectName>Options.cs
├── GlobalUsings.cs
└── <ProjectName>.Common.csproj

These files represent the initial project skeleton.

They do not contain business implementation.

---

# Default Business Repository Structure

<ProjectName>.Repository/

├── <ProjectName>Repository.cs
├── RegisterServices.cs
└── <ProjectName>.Repository.csproj

These files are structural skeletons.

---

# Default Business Services Structure

<ProjectName>.Services/

├── <ProjectName>Service.cs
├── RegisterServices.cs
└── <ProjectName>.Services.csproj

These files are structural skeletons.

---

# Core Applications

Core applications are normally located under:

Apps/Core/

Default Core composition:

<ProjectName>.API
<ProjectName>.Domain
<ProjectName>.Repository
<ProjectName>.Services

---

# Default Core Module

A Core module uses:

Apps/Core/<ModuleName>/

with:

<ModuleName>.API
<ModuleName>.Domain
<ModuleName>.Repository
<ModuleName>.Services

---

# Default Core API Structure

<ProjectName>.API/

├── Controllers/
├── Properties/
│   └── launchSettings.json
├── appsettings.json
├── appsettings.Development.json
├── appsettings.Local.json
├── build-info.json
├── <ProjectName>.API.csproj
└── Program.cs

The API uses the controller-based ASP.NET Core Web API template.

The template-generated:

<ProjectName>.API.http

file is not part of the default NYBOSS structure.

If generated, remove it.

CLI-generated files must retain their generated content.

---

# Default Core Domain Structure

<ProjectName>.Domain/

├── Models/
│   └── <ProjectName>Model.cs
├── Contracts/
│   ├── Repository/
│   │   └── I<ProjectName>Repository.cs
│   └── Services/
│       └── I<ProjectName>Service.cs
├── Constants/
│   └── <ProjectName>Constants.cs
├── Options/
│   └── <ProjectName>Options.cs
├── GlobalUsings.cs
└── <ProjectName>.Domain.csproj

Domain is a Core domain project.

Domain is not automatically equivalent to Business Common.

Do not replace Domain with Common.

---

# Default Core Repository Structure

<ProjectName>.Repository/

├── <ProjectName>Repository.cs
├── RegisterServices.cs
└── <ProjectName>.Repository.csproj

These are structural skeleton files.

---

# Default Core Services Structure

<ProjectName>.Services/

├── <ProjectName>Service.cs
├── RegisterServices.cs
└── <ProjectName>.Services.csproj

These are structural skeleton files.

---

# Common vs Domain

Business default:

<ProjectName>.Common

Core default:

<ProjectName>.Domain

Common and Domain are separate project types.

Do not treat them as interchangeable.

When a similar project is used as a reference, preserve the reference component type unless the user explicitly requests a different composition.

---

# Infrastructure

Infrastructure is structurally different from Business and Core.

Infrastructure contains:

1. Modules containing multiple projects
2. Standalone projects

There is no single universal Infrastructure project composition.

Do not apply the Business or Core default composition to Infrastructure.

---

# Infrastructure Module Structure

Known Infrastructure module categories include:

- APIClients
- service modules
- message engine modules
- watcher modules
- notification modules
- transport modules

Each category can have a different project composition.

---

# Infrastructure Standard Service Module

Default structure for a standard Infrastructure service module:

Infrastructure/<ModuleName>/

├── <ModuleName>.Common/
├── <ModuleName>.Repository/
└── <ModuleName>.Services/

The projects are:

<ModuleName>.Common

<ModuleName>.Repository

<ModuleName>.Services

All target:

net8.0

No Infrastructure project references are automatically created in the current phase.

---

# Infrastructure Service Host Module

A service-host-style module may contain:

Infrastructure/<ModuleName>/

├── <ModuleName>.Common/
├── <ModuleName>.Repository/
├── <ModuleName>.ServiceHost/
└── <ModuleName>.Services/

The exact project type and structure of ServiceHost should be determined by the requested module type or structural reference.

Do not copy implementation.

---

# Infrastructure API Client Module

An API client module may contain:

Infrastructure/<ModuleName>/

└── <ModuleName>.API.Client/

or multiple API client projects when explicitly requested.

New projects must use the new naming convention.

Do not create new legacy names such as:

BNPP.NYBOSS.<ProjectName>

---

# Infrastructure Message Engine Module

A MessageEngine-style module can contain several different project types.

A representative structure is:

Infrastructure/<ModuleName>/

├── <ModuleName>.MessageProcessing/
├── <ModuleName>.Messaging/
├── <ModuleName>.Common/
├── <ModuleName>.Repository/
└── <ModuleName>.Services/

This is a module-specific structure, not a universal Infrastructure template.

Use it only when the user requests a MessageEngine-style module or supplies an appropriate structural reference.

---

# Infrastructure Notification SDK Module

A notification SDK-style module may contain:

Infrastructure/<ModuleName>/

├── <ModuleName>.SDK/
└── <ModuleName>.SDK.Common/

Use the new naming convention for new projects.

Do not introduce legacy organizational prefixes.

---

# Infrastructure Transport Module

A transport-style module may contain:

Infrastructure/<ModuleName>/

├── <ModuleName>.FileShare/
├── <ModuleName>.Repository/
├── <ModuleName>.Common/
└── <ModuleName>.Services/

The actual transport components may vary.

Do not automatically create components that were not requested.

---

# Infrastructure Watcher Module

A watcher-style module may contain:

Infrastructure/<ModuleName>/

├── <ModuleName>.Common/
├── <ModuleName>.Repository/
├── <ModuleName>.ServiceHost/
└── <ModuleName>.Services/

This structure is used for watcher/service-host style modules.

If the user provides a different watcher structure, follow the requested structure instead.

---

# Infrastructure Standalone Projects

Infrastructure also contains standalone projects.

Examples of categories include:

API Host

API Infrastructure

Common

Custom Executable Host

Data Access

Logging

Repository

Service Host

Storage

A standalone project should not automatically create additional projects.

For example, creating:

Infrastructure/<ProjectName>/

does not imply:

<ProjectName>.Common

<ProjectName>.Repository

<ProjectName>.Services

unless explicitly requested.

---

# Infrastructure Standalone Project Structure

A standalone Infrastructure project may contain its own folders and files.

Examples of structural folder categories observed in the repository include:

Constants/

Contracts/

DBContexts/

Handlers/

Models/

Utilities/

Options/

Attributes/

Authentication/

Controller/

Http/

Middleware/

Services/

Swagger/

Enrichers/

Extensions/

Sinks/

Validators/

Hubs/

ServiceLocator/

These are examples of existing Infrastructure structures.

They are not a universal template.

Only create them when:

- the selected Infrastructure template requires them
- the user explicitly requests them
- or a similar-project request establishes them structurally

---

# Infrastructure Structural File Rule

Infrastructure structural files are project-specific.

Examples of existing structural file categories include:

GlobalAssemblyInfo.cs

globalusing.cs

RegisterServices.cs

Startup.cs

ApplicationInfo.cs

InternalsVisibleTo.cs

ModuleLoader.cs

RepositoryBase.cs

IS3FileManager.cs

S3FileManager.cs

Do not automatically add these files to every Infrastructure project.

For new projects, create only files required by the selected structure.

Structural files created by the generator should contain minimal valid skeleton code.

Do not copy implementation from existing Infrastructure projects.

---

# Project Naming

All newly created projects use:

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

Do not add:

BNPP.NYBOSS.

or another legacy organizational prefix.

Existing projects containing legacy prefixes are structural references only.

---

# Module Naming

A module is a directory.

Example:

<ModuleName>/

Projects inside the module use:

<ModuleName>.<Component>

For example:

<ModuleName>/

├── <ModuleName>.Common/
├── <ModuleName>.Repository/
└── <ModuleName>.Services/

The module name itself is not required to be a `.csproj`.

---

# Direct Project Creation

If the user explicitly requests:

"Create <ProjectName>.API project"

create only:

<ProjectName>.API

Do not create:

<ProjectName>.Common

<ProjectName>.Domain

<ProjectName>.Repository

<ProjectName>.Services

or any other project.

---

# Explicit Composition

If the user specifies components, create only those components.

Example:

"Create <ProjectName> with API, Common and Services"

creates:

<ProjectName>.API

<ProjectName>.Common

<ProjectName>.Services

Do not create Repository.

---

# Structural File Rule

Required structural folders should not be left as empty directories when the development environment or source control would not represent them.

Create a minimal structural file where necessary.

Examples:

Constants/
    <ProjectName>Constants.cs

Models/
    <ProjectName>Model.cs

Options/
    <ProjectName>Options.cs

Contracts/Repository/
    I<ProjectName>Repository.cs

Contracts/Services/
    I<ProjectName>Service.cs

Repository:

<ProjectName>Repository.cs

Services:

<ProjectName>Service.cs

For Infrastructure, use the structural files associated with the selected Infrastructure structure.

---

# CLI-Generated File Rule

Files generated by `dotnet new` must retain their generated content.

Do not blank or replace:

- Program.cs
- `.csproj`
- launchSettings.json
- template-generated appsettings files
- other valid CLI-generated files

Modify them only when required for:

- target framework
- project naming
- project configuration
- Business/Core project references

---

# Similar Project Structure

When creating a project similar to an existing project, determine:

- application area
- module structure
- project composition
- project type
- project directories
- folder hierarchy
- file hierarchy
- file names
- file locations

Recreate the applicable structure with the new naming convention.

---

# Similar Project Rule

A similar-project request means:

STRUCTURE SIMILARITY

not:

CONTENT COPYING

Reproduce:

- project structure
- module structure
- directory structure
- folder structure
- file structure
- applicable Business/Core project relationships

Do not reproduce:

- source-code implementation
- business logic
- configuration values
- connection strings
- secrets
- application data
- application-specific settings
- unrelated dependencies

---

# Similar Project File Structure

When a similar project contains:

<Folder>/<File>

reproduce:

- file name
- location
- extension

Do not reproduce implementation.

If the .NET CLI creates a corresponding file, retain the CLI-generated content.

If the file is a structural file created specifically by the generator, create minimal valid skeleton content.

---

# Target Framework

All newly generated projects target:

net8.0

This applies to:

- Business
- Core
- Infrastructure

unless the user explicitly requests another framework.

---

# Default Business References

Default Business:

<ProjectName>.API
    -> <ProjectName>.Services

<ProjectName>.Services
    -> <ProjectName>.Common
    -> <ProjectName>.Repository

<ProjectName>.Repository
    -> <ProjectName>.Common

Only add these references when the corresponding projects exist.

---

# Default Core References

Default Core:

<ProjectName>.API
    -> <ProjectName>.Services

<ProjectName>.Services
    -> <ProjectName>.Domain
    -> <ProjectName>.Repository

<ProjectName>.Repository
    -> <ProjectName>.Domain

Only add these references when the corresponding projects exist.

---

# Infrastructure References

Infrastructure project references are intentionally not defined in this phase.

Do not:

- infer Infrastructure references
- copy Infrastructure references
- add Infrastructure project references automatically

Infrastructure reference analysis will be handled in a later phase.

---

# Solution

The existing solution is:

BNPP.NYBOSS.NextGen.Server.sln

All newly generated projects belong to this solution.

Do not create a separate solution.

Use:

dotnet sln BNPP.NYBOSS.NextGen.Server.sln add <project-path>

---

# API Template Cleanup

The API template-generated:

<ProjectName>.API.http

file is not part of the default NYBOSS structure.

Remove it after project creation unless explicitly requested.

---

# Structure-Only Boundary

This phase creates:

- projects
- modules
- folders
- structural files
- `.csproj` files
- project configuration
- Business/Core project references
- solution entries

This phase does not implement:

- business functionality
- application functionality
- controller logic
- service logic
- repository logic
- domain logic
- Infrastructure logic

Infrastructure project references are deferred to a later phase.

CLI-generated content must remain intact.
