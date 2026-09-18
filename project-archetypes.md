# NYBOSS Project Archetypes

This document defines the supported NYBOSS project types.

Each archetype defines:

- purpose
- SDK
- output type
- structure
- target framework
- project references
- registration conventions
- creation rules

Legacy names in the senior-provided templates are reference names only.

New projects must use the current naming convention.

---

# 01 Common / Contracts Library

## Identity

New project:

<ProjectName>.Common

SDK:

Microsoft.NET.Sdk

Output:

library

Target:

net8.0

## Purpose

Shared domain vocabulary and contracts.

Contains:

- DTOs
- commands
- entities
- response models
- repository interfaces
- service interfaces
- enums
- constants
- validators

## Structure

<ProjectName>.Common/

Constants/
    <ProjectName>Constants.cs

Contracts/
    Repository/
        I<ProjectName>Repository.cs

    Services/
        I<ProjectName>Service.cs

Enums/

Models/
    <ProjectName>Model.cs

Options/
    <ProjectName>Options.cs

Validators/

GlobalUsings.cs

<ProjectName>.Common.csproj

## Must not

Common must not:

- access databases
- call HTTP
- access files
- access Kafka
- contain business logic
- reference Services
- reference Repository
- reference API
- reference hosts

## References

Common may reference:

- Infrastructure Common
- explicitly required external contract/proxy projects
- explicitly required vendor DLLs

Only when the model actually requires the external type.

---

# 02 Repository

## Identity

New project:

<ProjectName>.Repository

SDK:

Microsoft.NET.Sdk

Output:

library

Target:

net8.0

## Purpose

Data-access layer.

Repositories:

- implement Common repository contracts
- create database command requests
- execute stored procedures
- map results
- use repository infrastructure

## Structure

<ProjectName>.Repository/

<ProjectName>Repository.cs
RegisterServices.cs

<ProjectName>.Repository.csproj

Additional repository classes may be added for explicit features.

## Dependencies

Repository may reference:

- Infrastructure DataAccess
- <ProjectName>.Common

It must not reference:

- Services
- API
- hosts

## Registration

Register repositories through:

RegisterServices.cs

Typical lifetime:

AddTransient

DbContext infrastructure may use AddSingleton where that matches existing NYBOSS practice.

---

# 03 Services

## Identity

New project:

<ProjectName>.Services

SDK:

Microsoft.NET.Sdk

Output:

library

Target:

net8.0

## Purpose

Business logic layer.

Responsibilities:

- validation orchestration
- business rules
- mapping
- repository orchestration
- result classification
- external service/proxy orchestration

## Structure

<ProjectName>.Services/

<ProjectName>Service.cs
RegisterServices.cs
globalusing.cs

<ProjectName>.Services.csproj

## Dependencies

Services may reference:

- <ProjectName>.Repository
- Common through the repository dependency where appropriate
- Infrastructure Logging
- DateService
- required proxy projects

Do not add optional dependencies without evidence.

## Must not

Services must not:

- execute SQL directly
- construct DbCommandRequest
- return HTTP ActionResult
- reference API
- reference hosts

## Registration

Register through:

RegisterServices.cs

Typical lifetime:

AddTransient

---

# 04 Web API

## Identity

New project:

<ProjectName>.API

SDK:

Microsoft.NET.Sdk.Web

Output:

web application

Target:

net8.0

## Creation

Use:

dotnet new webapi --framework net8.0 --use-controllers --name <ProjectName>.API

## Structure

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

## Dependencies

API may reference:

- Infrastructure API Host
- <ProjectName>.Services
- required proxy projects

## API responsibility

API should:

1. bind request
2. map request to command
3. call service
4. translate service result to HTTP response

Controllers remain thin.

## Must not

API must not contain:

- repository logic
- business logic
- database logic
- host infrastructure implementation

## Cleanup

Remove:

<ProjectName>.API.http

unless explicitly requested.

---

# 05 Message Processing Logic Library

## Identity

New project:

<ProjectName>.MessageProcessing

SDK:

Microsoft.NET.Sdk

Output:

library

Target:

net8.0

## Structure

<ProjectName>.MessageProcessing/

Handlers/
Strategies/
Services/
Models/
Options/

ModuleLoader.cs

<ProjectName>.MessageProcessing.csproj

## Purpose

Contains message-driven processing logic.

Handlers process messages.

Strategies contain decision logic.

Services contain processing/bootstrap logic.

Options contain configuration records.

ModuleLoader provides DI registration extensions.

## Dependencies

May reference:

- MessageEngine infrastructure
- MessageProcessing infrastructure
- <ProjectName>.Services
- <ProjectName>.Repository
- <ProjectName>.Common

only when required.

## Must not

Must not contain:

- Program.Main
- Windows Service hosting
- Kafka consumer plumbing owned by MessageEngine

The host owns process startup.

---

# 06 Worker / Windows Service Host

## Identity

New project:

<ProjectName>.ProcessorHost

SDK:

Microsoft.NET.Sdk.Worker

Output:

long-running worker/service

Target:

net8.0

## Creation

Use:

dotnet new worker --framework net8.0 --name <ProjectName>.ProcessorHost

## Structure

<ProjectName>.ProcessorHost/

Program.cs
appsettings.json
build-info.json

<ProjectName>.ProcessorHost.csproj

The Worker template may generate additional files.

Keep valid generated files.

## Dependencies

May reference:

- Infrastructure Service Host
- MessageEngine Repository
- MessageEngine Services
- MessageProcessing Core
- Messaging Consumer
- <ProjectName>.MessageProcessing

## Purpose

Thin composition root.

The host:

- configures the application
- registers dependencies
- configures MessageEngine
- configures consumer services
- starts the long-running process

Do not put domain business logic into the host.

---

# 07 Workflow EXE Host

## Identity

New project:

<ProjectName>.Host

SDK:

Microsoft.NET.Sdk

Output:

Exe

Target:

net8.0

Project setting:

<OutputType>Exe</OutputType>

## Purpose

Run-to-completion workflow/scheduler executable.

This differs from the Worker host.

Worker:

long-running

Workflow EXE:

runs once and exits

## Structure

<ProjectName>.Host/

Program.cs
appsettings.json
appsettings.Local.json
build-info.json

<ProjectName>.Host.csproj

## Dependencies

May reference:

- Infrastructure CustomExecutable Host
- Infrastructure Service Host
- <ProjectName>.Services
- <ProjectName>.Repository
- DateService
- other explicitly required shared infrastructure

## Must not

Do not use:

Microsoft.NET.Sdk.Worker

Do not place business logic directly into Program.cs.

Program is a composition bootstrap.

---

# 08 Unit Test

## Identity

New project:

<ProjectName>.UnitTest

SDK:

Microsoft.NET.Sdk

Output:

test assembly

Target:

net8.0

## Required properties

<IsPackable>false</IsPackable>
<IsTestProject>true</IsTestProject>
<SonarQubeExclude>true</SonarQubeExclude>

Enable:

ImplicitUsings
Nullable

## Packages

Use the repository-approved versions from the senior-provided template.

Expected packages include:

- coverlet.collector
- Microsoft.NET.Test.Sdk
- ReportGenerator
- xunit
- xunit.runner.visualstudio
- Moq
- FluentAssertions

Do not upgrade versions arbitrarily.

## Structure

Tests/UnitTests/<Area>/<ProjectName>/

GlobalUsings.cs

Controllers/
Services/
Repositories/
Models/
Validators/
MessageProcessing/

Only create folders relevant to the tests.

## Dependencies

Test projects reference production projects.

Production projects never reference tests.

## Testing rules

Mock:

- databases
- repositories
- HTTP
- Kafka
- files
- external services

Use synthetic test data.

Do not use real client identifiers or PII.

---

# Dependency Direction

The general architecture is:

Common
   ↑
Repository
   ↑
Services
   ↑
API

with infrastructure dependencies entering the appropriate layer.

For message processing:

Common
   ↑
Repository
   ↑
Services
   ↑
MessageProcessing
   ↑
ProcessorHost

Infrastructure MessageEngine supports the message-processing stack.

For workflow:

Common
   ↑
Repository
   ↑
Services
   ↑
Workflow Host

---

# Important Dependency Rule

Do not interpret the above as permission to add every possible dependency.

A dependency must be added only if:

1. the archetype requires it,
2. the selected project actually uses that layer,
3. the referenced project exists,
4. adding it does not create a cycle.

---

# Shared Infrastructure

Known existing Infrastructure projects may include:

BNPP.NYBOSS.Common
BNPP.NYBOSS.DataAccess
BNPP.NYBOSS.Logging
BNPP.NYBOSS.API.Host
BNPP.NYBOSS.Service.Host
BNPP.NYBOSS.CustomExecutable.Host

These existing projects may retain their legacy names.

When new projects reference them, use the actual existing `.csproj` path.

Do not recreate them.

Do not rename them as part of new project creation.

---

# Project Reference Rules

## Common

Common:

-> Infrastructure Common only when required

Common must not reference upper application layers.

## Repository

Repository:

-> DataAccess
-> <ProjectName>.Common

## Services

Services:

-> <ProjectName>.Repository

and additional required infrastructure such as:

-> Logging
-> DateService
-> required proxies

## API

API:

-> API Host
-> <ProjectName>.Services

## Message Processing

MessageProcessing:

-> MessageEngine infrastructure
-> domain Services/Repository/Common when required

## ProcessorHost

ProcessorHost:

-> Service Host
-> MessageEngine
-> <ProjectName>.MessageProcessing

## Workflow Host

Host:

-> CustomExecutable.Host
-> Service Host
-> domain Services/Repository
-> required shared infrastructure

## Unit Test

UnitTest:

-> production projects under test

---

# Reference Validation

Before adding a reference:

- confirm source project exists
- confirm target project exists
- confirm relative path
- confirm dependency is allowed
- confirm no cycle
- confirm reference is necessary

Do not add references simply because a similar project contains them if the new project does not require the dependency.

---

# Project Configuration

All new projects:

<TargetFramework>net8.0</TargetFramework>

New projects should normally use:

<ImplicitUsings>enable</ImplicitUsings>
<Nullable>enable</Nullable>

unless the selected archetype explicitly requires otherwise.

Where the repository uses:

Shared/GlobalAssemblyInfo.cs

link it instead of generating duplicate assembly information when required by the archetype.

---

# Similar Project Rule

Tier 3 reference exemplars are structural references.

Use them to determine:

- folder structure
- file names
- project configuration
- dependency relationships
- registration patterns

Do not copy:

- implementation
- business logic
- configuration values
- secrets
- connection strings
- application data

---

# Template Precedence

When there is a conflict:

1. User's explicit request
2. Current NYBOSS naming convention
3. Selected archetype
4. Current project structure
5. Existing structural reference
6. Legacy project conventions

Legacy naming must never override the current new-project naming convention.

---

# Phase Boundary

This phase now includes:

- project creation
- module creation
- project structure
- structural files
- project configuration
- correct SDK selection
- correct output type
- Business references
- Core references
- archetype-defined Infrastructure references
- MessageProcessing projects
- Worker/Windows Service projects
- Workflow EXE projects
- Unit-test projects
- solution entries

This phase still does not implement:

- business functionality
- service functionality
- repository functionality
- controller functionality
- message processing functionality
- worker processing functionality
- workflow functionality
- Infrastructure functionality

Only project scaffolding and architecture are created.
