# NYBOSS Project Structure

This document defines the physical project and folder structures.

Archetype-specific SDK and dependency rules are defined in:

references/project-archetypes.md

---

# Application Areas

Apps/Business/
Apps/Core/
Infrastructure/
Tests/UnitTests/

---

# Business

Apps/Business/<ProjectName>/

<ProjectName>.API/
<ProjectName>.Common/
<ProjectName>.Repository/
<ProjectName>.Services/

---

# Business Common

<ProjectName>.Common/

Constants/
    <ProjectName>Constants.cs

Contracts/
    Repository/
        I<ProjectName>Repository.cs

    Services/
        I<ProjectName>Service.cs

Models/
    <ProjectName>Model.cs

Options/
    <ProjectName>Options.cs

GlobalUsings.cs

<ProjectName>.Common.csproj

Common contains shared contracts, models, enums, constants and validation structures.

Do not place business implementation here.

---

# Business Repository

<ProjectName>.Repository/

<ProjectName>Repository.cs
RegisterServices.cs
<ProjectName>.Repository.csproj

Additional repository classes may be added when explicitly required by the feature.

---

# Business Services

<ProjectName>.Services/

<ProjectName>Service.cs
RegisterServices.cs
<ProjectName>.Services.csproj

Additional service classes may be added when explicitly required by the feature.

---

# Business API

<ProjectName>.API/

Controllers/

Properties/
    launchSettings.json

appsettings.json
appsettings.Development.json
appsettings.Local.json
build-info.json
Program.cs

<ProjectName>.API.csproj

The CLI may generate additional files.

Preserve valid CLI-generated files except explicitly excluded files such as the `.http` file.

---

# Core

Apps/Core/<ProjectName>/

<ProjectName>.API/
<ProjectName>.Domain/
<ProjectName>.Repository/
<ProjectName>.Services/

---

# Core Domain

<ProjectName>.Domain/

Models/
    <ProjectName>Model.cs

Contracts/
    Repository/
        I<ProjectName>Repository.cs

    Services/
        I<ProjectName>Service.cs

Constants/
    <ProjectName>Constants.cs

Options/
    <ProjectName>Options.cs

GlobalUsings.cs

<ProjectName>.Domain.csproj

Domain and Common are different project types.

---

# Core Repository

<ProjectName>.Repository/

<ProjectName>Repository.cs
RegisterServices.cs
<ProjectName>.Repository.csproj

---

# Core Services

<ProjectName>.Services/

<ProjectName>Service.cs
RegisterServices.cs
<ProjectName>.Services.csproj

---

# Infrastructure

Infrastructure/

Infrastructure does not have one universal structure.

Known structures are defined below.

---

# Infrastructure Standard Service Module

Infrastructure/<ModuleName>/

<ModuleName>.Common/
<ModuleName>.Repository/
<ModuleName>.Services/

---

# Infrastructure Service Host Module

Infrastructure/<ModuleName>/

<ModuleName>.Common/
<ModuleName>.Repository/
<ModuleName>.Services/
<ModuleName>.ServiceHost/

---

# Infrastructure API Client

Infrastructure/<ModuleName>/

<ModuleName>.API.Client/

Additional API clients may be created when explicitly requested.

---

# Infrastructure Message Processing

Infrastructure/<ModuleName>/

<ModuleName>.MessageProcessing/
<ModuleName>.Messaging/
<ModuleName>.Common/
<ModuleName>.Repository/
<ModuleName>.Services/

Only use this structure when the user explicitly requests a MessageEngine-style module or a structural reference establishes it.

---

# Infrastructure Notification SDK

Infrastructure/<ModuleName>/

<ModuleName>.SDK/
<ModuleName>.SDK.Common/

---

# Infrastructure Transport

Infrastructure/<ModuleName>/

<ModuleName>.FileShare/
<ModuleName>.Repository/
<ModuleName>.Common/
<ModuleName>.Services/

Only create components explicitly required by the selected structure.

---

# Infrastructure Watcher

Infrastructure/<ModuleName>/

<ModuleName>.Common/
<ModuleName>.Repository/
<ModuleName>.ServiceHost/
<ModuleName>.Services/

---

# Standalone Infrastructure

A standalone Infrastructure project is one project.

Do not automatically create additional projects.

Examples:

Infrastructure/<ProjectName>/

or another explicitly requested Infrastructure location.

The project structure must come from:

- selected archetype
- explicit user request
- similar-project reference

---

# Message Processing Logic

A new message-processing logic project:

<ProjectName>.MessageProcessing/

Handlers/
Strategies/
Services/
Models/
Options/

ModuleLoader.cs

<ProjectName>.MessageProcessing.csproj

This is a library.

It is not the host.

---

# Worker / Processor Host

A new Worker host:

<ProjectName>.ProcessorHost/

Program.cs
appsettings.json
build-info.json

<ProjectName>.ProcessorHost.csproj

The actual Worker template may generate additional files.

Do not remove valid generated files unless the selected NYBOSS structure explicitly excludes them.

---

# Workflow EXE Host

A workflow executable:

<ProjectName>.Host/

Program.cs
appsettings.json
appsettings.Local.json
build-info.json

<ProjectName>.Host.csproj

The project uses:

Microsoft.NET.Sdk

with:

<OutputType>Exe</OutputType>

It is run-to-completion.

---

# Unit Test

Tests/UnitTests/<Area>/<ProjectName>/

<ProjectName>.UnitTest.csproj

GlobalUsings.cs

Controllers/
Services/
Repositories/
Models/
Validators/
MessageProcessing/

Only create test folders that are required by the projects/features under test.

---

# Naming

New projects:

<ProjectName>.<Component>

Examples:

<ProjectName>.API
<ProjectName>.Common
<ProjectName>.Domain
<ProjectName>.Repository
<ProjectName>.Services
<ProjectName>.MessageProcessing
<ProjectName>.ProcessorHost
<ProjectName>.Host
<ProjectName>.UnitTest
<ProjectName>.ServiceHost
<ProjectName>.API.Client
<ProjectName>.SDK

Never introduce:

BNPP.NYBOSS.<ProjectName>

for a new project.

---

# Structural Files

Structural files must be minimal valid skeletons.

Do not copy implementation from reference projects.

Examples:

<ProjectName>Constants.cs
<ProjectName>Model.cs
<ProjectName>Options.cs
I<ProjectName>Repository.cs
I<ProjectName>Service.cs
<ProjectName>Repository.cs
<ProjectName>Service.cs
RegisterServices.cs
ModuleLoader.cs

---

# CLI Generated Files

Keep generated content from:

dotnet new

including:

Program.cs
.csproj
launchSettings.json
appsettings.json
Worker.cs
other valid template-generated files

Modify only where required by the selected NYBOSS architecture.

---

# API Cleanup

Remove:

<ProjectName>.API.http

unless explicitly requested.

---

# Framework

All new projects target:

net8.0

---

# Solution

All projects belong to:

BNPP.NYBOSS.NextGen.Server.sln

Do not create a second solution.
