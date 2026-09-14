# NYBOSS Project Creation

This document defines how NYBOSS .NET projects and modules are created.

The current stage creates:

- projects
- modules
- folders
- structural files
- project configuration
- Business/Core project references
- solution entries

Business and application implementation is not part of this stage.

Infrastructure references are intentionally deferred.

---

# Application Areas

NYBOSS server projects are organized under:

Apps/Business/

Apps/Core/

Infrastructure/

The structure depends on the selected application area.

---

# Project vs Module

A project is an individual `.csproj`.

A module is a logical directory containing one or more projects.

Examples:

Apps/Business/<ModuleName>/

Apps/Core/<ModuleName>/

Infrastructure/<ModuleName>/

If the user says:

"Create <ProjectName> project"

create the default composition for that application area.

If the user says:

"Create <ProjectName>.API project"

create only:

<ProjectName>.API

If the user says:

"Create <ModuleName> module"

create the module and its default project composition.

If the user specifies both a module and project name, place the project inside the requested module.

---

# Target Framework

All newly generated NYBOSS projects target:

net8.0

Use .NET 8 explicitly.

Do not allow the installed SDK version to determine the target framework automatically.

---

# Business

## Default Business Project

Create:

<ProjectName>.API
<ProjectName>.Common
<ProjectName>.Repository
<ProjectName>.Services

under:

Apps/Business/<ProjectName>/

---

## Default Business Module

When the user requests:

"Create <ModuleName> module"

for Business, create:

Apps/Business/<ModuleName>/

containing:

<ModuleName>.API
<ModuleName>.Common
<ModuleName>.Repository
<ModuleName>.Services

---

## Business API

Create:

<ProjectName>.API

using:

dotnet new webapi --framework net8.0 --use-controllers --name <ProjectName>.API

The API uses the controller-based ASP.NET Core Web API template.

The API structure is:

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

The actual .NET template may generate additional files.

Remove:

<ProjectName>.API.http

unless explicitly requested.

Keep all valid CLI-generated content.

Do not create business-specific controllers.

---

## Business Common

Create:

<ProjectName>.Common

using:

dotnet new classlib --framework net8.0 --name <ProjectName>.Common

Structure:

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

These are structural skeleton files only.

---

## Business Repository

Create:

<ProjectName>.Repository

using:

dotnet new classlib --framework net8.0 --name <ProjectName>.Repository

Structure:

<ProjectName>.Repository/
├── <ProjectName>Repository.cs
├── RegisterServices.cs
└── <ProjectName>.Repository.csproj

These are structural skeleton files only.

---

## Business Services

Create:

<ProjectName>.Services

using:

dotnet new classlib --framework net8.0 --name <ProjectName>.Services

Structure:

<ProjectName>.Services/
├── <ProjectName>Service.cs
├── RegisterServices.cs
└── <ProjectName>.Services.csproj

These are structural skeleton files only.

---

# Default Business References

When all default Business projects exist:

<ProjectName>.API
    -> <ProjectName>.Services

<ProjectName>.Services
    -> <ProjectName>.Common
    -> <ProjectName>.Repository

<ProjectName>.Repository
    -> <ProjectName>.Common

Use:

dotnet add <project> reference <referenced-project>

Only create references to projects that actually exist.

---

# Core

## Default Core Project

Create:

<ProjectName>.API
<ProjectName>.Domain
<ProjectName>.Repository
<ProjectName>.Services

under:

Apps/Core/<ProjectName>/

---

## Default Core Module

When the user requests:

"Create <ModuleName> module"

for Core, create:

Apps/Core/<ModuleName>/

containing:

<ModuleName>.API
<ModuleName>.Domain
<ModuleName>.Repository
<ModuleName>.Services

---

## Core API

Create:

<ProjectName>.API

using:

dotnet new webapi --framework net8.0 --use-controllers --name <ProjectName>.API

Structure:

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

Remove:

<ProjectName>.API.http

unless explicitly requested.

Keep all valid CLI-generated content.

---

## Core Domain

Create:

<ProjectName>.Domain

using:

dotnet new classlib --framework net8.0 --name <ProjectName>.Domain

Default structure:

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

Domain is not automatically equivalent to Common.

---

## Core Repository

Create:

<ProjectName>.Repository

using:

dotnet new classlib --framework net8.0 --name <ProjectName>.Repository

Structure:

<ProjectName>.Repository/
├── <ProjectName>Repository.cs
├── RegisterServices.cs
└── <ProjectName>.Repository.csproj

---

## Core Services

Create:

<ProjectName>.Services

using:

dotnet new classlib --framework net8.0 --name <ProjectName>.Services

Structure:

<ProjectName>.Services/
├── <ProjectName>Service.cs
├── RegisterServices.cs
└── <ProjectName>.Services.csproj

---

# Default Core References

When all default Core projects exist:

<ProjectName>.API
    -> <ProjectName>.Services

<ProjectName>.Services
    -> <ProjectName>.Domain
    -> <ProjectName>.Repository

<ProjectName>.Repository
    -> <ProjectName>.Domain

Use:

dotnet add <project> reference <referenced-project>

Only create references to projects that actually exist.

---

# Infrastructure

Infrastructure does not have one universal project structure.

Infrastructure contains different types of modules and standalone projects.

The creation rules below define the known module structures.

---

# Infrastructure Module Types

Known Infrastructure module structures include:

1. Standard service module
2. Service host module
3. API client module
4. Message engine module
5. Notification SDK module
6. Transport module
7. Watcher module

The user can explicitly specify components instead of using one of these templates.

---

# Infrastructure Standard Service Module

Use this structure for an Infrastructure module that follows the standard service pattern:

Infrastructure/<ModuleName>/

containing:

<ModuleName>.Common
<ModuleName>.Repository
<ModuleName>.Services

Structure:

<ModuleName>/
├── <ModuleName>.Common/
├── <ModuleName>.Repository/
└── <ModuleName>.Services/

All projects target:

net8.0

Create each project using:

dotnet new classlib --framework net8.0 --name <ModuleName>.Common

dotnet new classlib --framework net8.0 --name <ModuleName>.Repository

dotnet new classlib --framework net8.0 --name <ModuleName>.Services

Do not add Infrastructure project references during this phase.

---

# Infrastructure Service Host Module

Use this structure when the Infrastructure module explicitly represents a service host:

Infrastructure/<ModuleName>/

containing:

<ModuleName>.Common
<ModuleName>.Repository
<ModuleName>.Services
<ModuleName>.ServiceHost

Structure:

<ModuleName>/
├── <ModuleName>.Common/
├── <ModuleName>.Repository/
├── <ModuleName>.ServiceHost/
└── <ModuleName>.Services/

The exact project type of ServiceHost should be determined from the requested structure or structural reference.

All new projects target:

net8.0

Do not copy implementation from an existing ServiceHost.

Do not add Infrastructure project references during this phase.

---

# Infrastructure API Client Module

An API client module may contain an API client project rather than the standard Common/Repository/Services composition.

Example structure:

Infrastructure/APIClients/

containing:

<ModuleName>.API.Client

Use the new naming convention.

Do not reproduce a legacy project name such as:

BNPP.NYBOSS.<ProjectName>

for a newly created project.

Create the project as an appropriate .NET 8 class library unless the user explicitly requests another project type.

---

# Infrastructure Message Engine Module

The repository contains MessageEngine-related projects with a non-uniform structure.

A MessageEngine-style module may contain:

<ModuleName>.MessageProcessing
<ModuleName>.Messaging
<ModuleName>.Common
<ModuleName>.Repository
<ModuleName>.Services

The exact module structure should be selected only when the user explicitly requests this module type or provides a corresponding structural reference.

Do not assume MessageEngine has the standard service-module structure.

All newly generated projects target:

net8.0

Do not add Infrastructure project references during this phase.

---

# Infrastructure Notification SDK Module

A notification SDK-style module may contain:

<ModuleName>.SDK
<ModuleName>.SDK.Common

Example structure:

Infrastructure/<ModuleName>/

├── <ModuleName>.SDK/
└── <ModuleName>.SDK.Common/

Use the new naming convention.

Do not introduce legacy organizational prefixes.

All projects target:

net8.0

---

# Infrastructure Transport Module

A transport-style module may contain:

<ModuleName>.FileShare
<ModuleName>.Repository
<ModuleName>.Common
<ModuleName>.Services

Example:

Infrastructure/<ModuleName>/

├── <ModuleName>.FileShare/
├── <ModuleName>.Repository/
├── <ModuleName>.Common/
└── <ModuleName>.Services/

The exact component names may be explicitly supplied by the user.

Do not invent additional transport projects.

All new projects target:

net8.0

---

# Infrastructure Watcher Module

A watcher-style module may contain:

<ModuleName>.Common
<ModuleName>.Repository
<ModuleName>.ServiceHost
<ModuleName>.Services

Example:

Infrastructure/<ModuleName>/

├── <ModuleName>.Common/
├── <ModuleName>.Repository/
├── <ModuleName>.ServiceHost/
└── <ModuleName>.Services/

Use this structure only when the requested Infrastructure module is a watcher/service-host style module.

Do not assume every watcher uses exactly this structure if the user provides another reference.

---

# Infrastructure Standalone Projects

Infrastructure also contains projects that are not modules.

Examples of structural categories include:

- API Host
- API Infrastructure
- Common
- Custom Executable Host
- Data Access
- Logging
- Repository
- Service Host
- Storage

When creating a standalone Infrastructure project:

Infrastructure/<ProjectName>/

or, if the project is directly under Infrastructure:

Infrastructure/<ProjectName>/

create only the requested project.

Do not automatically create Common, Repository, Services, or another project.

If the user asks for a project similar to an existing Infrastructure project, reproduce its structure only.

---

# Explicit Infrastructure Composition

If the user says:

"Create <ModuleName> module with Common, Repository and Services"

create:

Infrastructure/<ModuleName>/

├── <ModuleName>.Common/
├── <ModuleName>.Repository/
└── <ModuleName>.Services/

Do not create API.

Do not create ServiceHost.

Do not create other projects.

If the user says:

"Create <ProjectName>.API project"

create only:

<ProjectName>.API

---

# Infrastructure Naming

All newly generated Infrastructure projects use the new naming convention.

Preferred:

<ProjectName>.<Component>

Examples:

<ProjectName>.Common
<ProjectName>.Repository
<ProjectName>.Services
<ProjectName>.API
<ProjectName>.ServiceHost
<ProjectName>.API.Client
<ProjectName>.SDK

Do not create new projects using legacy names such as:

BNPP.NYBOSS.<ProjectName>

Legacy projects are structural references only.

---

# Infrastructure Structural Files

Infrastructure structural files must be created only when required by the selected Infrastructure project/module structure.

Do not assume that every Infrastructure project needs:

- Models
- Contracts
- Constants
- Options
- RegisterServices
- GlobalUsings
- ServiceHost
- Repository

The structure must come from:

- the selected known module template
- explicit user requirements
- a structural reference project

Structural files contain minimal valid skeleton code only.

Do not implement Infrastructure functionality.

---

# Explicit Project Creation

If the user specifies components, create only those components.

Examples:

"Create <ProjectName> with API and Services"

creates:

<ProjectName>.API
<ProjectName>.Services

"Create <ProjectName> with Common and Repository"

creates:

<ProjectName>.Common
<ProjectName>.Repository

Do not add unspecified components.

---

# Similar Project

For:

"Create <ProjectName> similar to <ExistingProject>"

inspect:

- project composition
- project type
- folder structure
- file structure
- file names
- project directories
- module structure

Recreate the structure with the new project name.

Do not copy implementation.

---

# Similar Project File Rule

If a reference project contains:

<Folder>/<File>

create:

<NewProject>/<Folder>/<File>

using the corresponding new project naming where the file is project-specific.

Reproduce:

- file name
- location
- extension

Do not reproduce implementation.

If `dotnet new` generates a corresponding file, preserve the CLI-generated content.

---

# Structural File Content

Files created specifically to represent project structure should contain minimal valid skeleton code.

Examples:

<ProjectName>Model.cs

<ProjectName>Constants.cs

<ProjectName>Options.cs

I<ProjectName>Repository.cs

I<ProjectName>Service.cs

<ProjectName>Repository.cs

<ProjectName>Service.cs

Do not implement application functionality.

---

# CLI-Generated File Content

When `dotnet new` creates a file:

- preserve its generated content
- do not empty it
- do not replace it with a placeholder
- do not copy content from another project

Modify generated files only when required for:

- net8.0
- project naming
- required project configuration
- Business/Core references where applicable

---

# API Template Cleanup

The API template may generate:

<ProjectName>.API.http

This file is not part of the default NYBOSS API structure.

Remove it after API project creation unless explicitly requested.

---

# Solution

Use:

BNPP.NYBOSS.NextGen.Server.sln

Add every generated project using:

dotnet sln BNPP.NYBOSS.NextGen.Server.sln add <project-path>

Do not create a separate solution.

---

# Structure-Only Completion

Project/module creation is complete when:

- requested project/module exists
- correct application area is used
- requested projects exist
- required folders exist
- required structural files exist
- CLI-generated files retain their generated content
- unwanted API `.http` files are removed
- project names follow the new naming convention
- projects target net8.0
- generated projects are added to the existing solution
- Business/Core references are created where their predefined rules require them
- Infrastructure references are not added automatically
- no business or application implementation has been generated
