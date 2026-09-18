# NYBOSS Project Creation

This document defines the project-creation decision rules.

Detailed project-type specifications are defined in:

references/project-archetypes.md

Detailed repository structures are defined in:

references/project-structure.md

---

# Creation Modes

Supported modes:

1. Default project
2. Explicit composition
3. Direct component project
4. Similar project
5. Module creation
6. Archetype-specific project

---

# Application Areas

Supported application areas:

Apps/Business/
Apps/Core/
Infrastructure/
Tests/UnitTests/

---

# Default Business

Request:

Create <ProjectName> project

Result:

Apps/Business/<ProjectName>/

<ProjectName>.API
<ProjectName>.Common
<ProjectName>.Repository
<ProjectName>.Services

---

# Default Core

Request:

Create <ProjectName> project

Result:

Apps/Core/<ProjectName>/

<ProjectName>.API
<ProjectName>.Domain
<ProjectName>.Repository
<ProjectName>.Services

---

# Default Infrastructure

Infrastructure has no universal default composition.

If the user says:

Create <ProjectName> project

for Infrastructure:

create one Infrastructure project unless a module/archetype is explicitly identified.

Do not invent Common/Repository/Services.

---

# Direct Component

Request:

Create <ProjectName>.API project

Result:

<ProjectName>.API

only.

Request:

Create <ProjectName>.Repository project

Result:

<ProjectName>.Repository

only.

Request:

Create <ProjectName>.Services project

Result:

<ProjectName>.Services

only.

---

# Explicit Composition

Request:

Create <ProjectName> with API, Common and Services

Create:

<ProjectName>.API
<ProjectName>.Common
<ProjectName>.Services

Do not add Repository.

References must only connect projects that exist.

---

# Module

Request:

Create <ModuleName> module

Business:

Apps/Business/<ModuleName>/

Core:

Apps/Core/<ModuleName>/

Infrastructure:

Infrastructure/<ModuleName>/

For Business and Core, use the default composition.

For Infrastructure, determine the module type from:

- explicit wording
- known template
- structural reference

---

# Archetype Selection

If the user uses terminology associated with an archetype, select that archetype.

Examples:

"web api"
-> Web API

"worker"
-> Worker / Windows Service host

"windows service"
-> Worker / Windows Service host

"message processor"
-> Message-processing logic library or Worker host depending on wording

"message processing library"
-> Message-processing logic library

"workflow executable"
-> Workflow EXE host

"console executable"
-> Workflow EXE host unless another type is explicitly requested

"unit test"
-> Unit-test project

"repository"
-> Repository library

"services"
-> Services library

"common"
-> Common library

"domain"
-> Domain library

---

# Message Processing

Message-processing consists of two separate archetypes:

1. Message-processing logic library
2. Worker / Processor host

Do not combine them into one project unless the user explicitly requests a single custom project.

---

# Worker Host

Default new project:

<ProjectName>.ProcessorHost

SDK:

Microsoft.NET.Sdk.Worker

Target:

net8.0

The Worker host references the message-processing logic project when the two are created together.

---

# Workflow EXE

Default new project:

<ProjectName>.Host

SDK:

Microsoft.NET.Sdk

OutputType:

Exe

Target:

net8.0

This is a run-once process.

Do not use the Worker SDK.

---

# Unit Tests

Default:

Tests/UnitTests/<ApplicationArea>/<ProjectName>/

<ProjectName>.UnitTest

The test project references the production projects it tests.

Production projects must not reference the test project.

---

# Project References

Project references must be selected from the archetype.

Do not use a single universal dependency graph.

Business default:

API
-> API.Host
-> Services

Services
-> Repository

Repository
-> DataAccess
-> Common

Common
-> Infrastructure Common

Core follows the equivalent Core architecture.

Infrastructure dependencies are archetype-specific.

---

# Reference Safety

Before adding a reference:

1. Confirm the referenced project exists.
2. Confirm the archetype permits the dependency.
3. Confirm the reference path.
4. Confirm the dependency does not create an architectural cycle.
5. Add the reference only once.

Do not reference:

- API from Services
- API from Repository
- Services from Repository
- host projects from lower layers
- tests from production projects

unless a structural reference explicitly establishes an exception.

---

# Naming

New projects:

<ProjectName>.<Component>

Never create new:

BNPP.NYBOSS.<ProjectName>...

Legacy projects may be referenced.

Legacy project names must not be copied into new project names.

---

# Framework

All new projects:

net8.0

---

# Solution

Existing solution:

BNPP.NYBOSS.NextGen.Server.sln

Every generated project must be added to the existing solution.

Never create another solution.

---

# CLI Templates

API:

dotnet new webapi --framework net8.0 --use-controllers --name <ProjectName>.API

Class library:

dotnet new classlib --framework net8.0 --name <ProjectName>.<Component>

Worker:

dotnet new worker --framework net8.0 --name <ProjectName>.ProcessorHost

Workflow EXE:

Use Microsoft.NET.Sdk and configure:

<OutputType>Exe</OutputType>

Unit test:

Use the test project template appropriate for the installed .NET 8 SDK and then apply the NYBOSS test configuration from project-archetypes.md.

---

# Template Cleanup

Remove:

<ProjectName>.API.http

unless explicitly requested.

Do not remove valid CLI-generated files.

---

# Similar Project

For similar-project requests:

inspect structure only.

Allowed:

- project names
- project type
- directories
- folders
- file names
- project references
- project metadata

Do not copy implementation.

Apply the current naming convention to the new project.

---

# Structure-only Boundary

Create:

- project
- module
- directories
- structural files
- project configuration
- applicable project references
- solution entries

Do not implement application functionality.
