---
name: dotnet-developer
description: Create and modify NYBOSS .NET server projects using the repository's project structure, conventions, and .NET 8 tooling.
---

# NYBOSS .NET Developer

You are working on the NYBOSS .NET server repository.

Follow the NYBOSS project creation rules in this skill and its reference files.

## Current Scope

The current automation phase focuses ONLY on creating .NET projects.

During this phase, create:

- Project directories
- .csproj files
- Required basic project-to-project references
- Projects in the existing NYBOSS solution

Do NOT generate application-level contents such as:

- Controllers
- Models
- Interfaces
- Service implementation classes
- Repository implementation classes
- Business logic
- Message handlers
- Dependency injection registrations
- API endpoints

unless the user explicitly requests them.

Detailed project contents will be handled in a later phase.

---

# Target Framework

NYBOSS server projects currently target .NET 8.

All newly created projects must target:

net8.0

Do NOT allow the installed/latest .NET SDK to automatically select another target framework such as net10.0.

For every generated project, explicitly use:

--framework net8.0

when the selected dotnet template supports the framework option.

Examples:

dotnet new webapi --framework net8.0
dotnet new classlib --framework net8.0

The generated .csproj must contain:

<TargetFramework>net8.0</TargetFramework>

unless the user explicitly requests another target framework.

---

# Project Creation

When the user asks to create a new project, determine the required project composition using the following rules.

## 1. Simple / Default Project

If the user asks for:

- a simple project
- a basic project
- a normal project
- a standard project
- a regular project
- a project without specifying its structure

use the default NYBOSS Business project composition:

- API
- Common
- Repository
- Services

Example:

"Create a simple Business project called Customer"

creates:

Customer.API
Customer.Common
Customer.Repository
Customer.Services

under:

Apps/Business/Customer/

---

# 2. Explicit Project Composition

If the user explicitly specifies project components, create only those components.

Example:

"Create Customer with Common, Repository and Services"

creates:

Customer.Common
Customer.Repository
Customer.Services

Do NOT automatically add:

Customer.API

or any other project component.

Another example:

"Create Customer with API and Services"

creates:

Customer.API
Customer.Services

Only the explicitly requested components are created.

---

# 3. Similar to an Existing Project

If the user says:

"Create Customer similar to ManualForecast"

inspect the existing ManualForecast project structure and determine its project composition.

Use the existing project only as a structural/reference pattern.

For example, if the reference project contains:

ManualForecast.API
ManualForecast.Common
ManualForecast.MessageProcessing
ManualForecast.MessageProcessingHost
ManualForecast.Repository
ManualForecast.Services

then a new Customer project should contain:

Customer.API
Customer.Common
Customer.MessageProcessing
Customer.MessageProcessingHost
Customer.Repository
Customer.Services

The new project name must replace the reference project's name everywhere appropriate.

Do NOT copy the old project name.

Do NOT copy the legacy naming convention.

Do NOT blindly copy project references.

Do NOT blindly copy package references.

Do NOT blindly copy business-specific configuration.

---

# 4. Project Naming Convention

All newly created projects must follow:

<ProjectName>.<Component>

Examples:

Customer.API
Customer.Common
Customer.Repository
Customer.Services
Customer.MessageProcessing
Customer.MessageProcessingHost

Do NOT use the legacy:

BNPP.NYBOSS.<ProjectName>.<Component>

format for newly created projects.

Existing projects may use names such as:

BNPP.NYBOSS.ManualForecast.API

or:

BNPP.NYBOSS.Instrument.API

These existing names are NOT the naming convention for newly generated projects.

Existing projects are used only to understand structure and project purpose.

---

# 5. Project-to-Project References

When creating multiple components for a new project, references must point to the NEW project's components.

For example, if the new project is:

Customer

and the generated components are:

Customer.API
Customer.Common
Customer.Repository
Customer.Services

then references must be between these Customer projects.

Do NOT reference:

ManualForecast.Services
ManualForecast.Common
ManualForecast.Repository

or any other existing project's equivalent component.

## Default Basic References

For the default project composition:

Customer.API
Customer.Common
Customer.Repository
Customer.Services

use the following basic dependency direction:

Customer.API
    -> Customer.Services

Customer.Services
    -> Customer.Common
    -> Customer.Repository

Customer.Repository
    -> Customer.Common

Customer.Common
    -> no generated project dependency

The same rule applies to other project names.

For example, for TradeSettlement:

TradeSettlement.API
    -> TradeSettlement.Services

TradeSettlement.Services
    -> TradeSettlement.Common
    -> TradeSettlement.Repository

TradeSettlement.Repository
    -> TradeSettlement.Common

References must always use the newly generated project's name.

---

# 6. References for Explicit Components

When the user explicitly selects components, add only references that are applicable to the components that were actually created.

For example, if the user requests:

Customer.Common
Customer.Repository
Customer.Services

then:

Customer.Repository
    -> Customer.Common

Customer.Services
    -> Customer.Common
    -> Customer.Repository

Do NOT create Customer.API or references to Customer.API.

If a dependency cannot be established confidently from the selected components, do not invent a reference.

---

# 7. External NYBOSS Project References

Do NOT automatically add references to unrelated existing NYBOSS projects.

For example, do NOT automatically add:

Infrastructure projects
Core projects
Gateway projects
DateService projects
MessageEngine projects
Messaging projects
Other Business projects
Other Core projects

unless the user explicitly requests the reference or the user explicitly asks to create a project pattern that requires it.

Example user request:

"Add a reference to BNPP.NYBOSS.DateService.Services to Customer.Services."

In that case, modify:

Customer.Services.csproj

to add the requested external project reference.

External project-reference automation is separate from default project creation.

---

# 8. Solution

All newly created projects must be added to the existing NYBOSS solution:

BNPP.NYBOSS.NextGen.Server.sln

Do NOT create a new solution for the application.

Use:

dotnet sln <solution> add <project>

to add each generated project.

---

# 9. .NET CLI

Use the .NET CLI for project creation and solution operations.

Use:

dotnet new webapi --framework net8.0

for API projects.

Use:

dotnet new classlib --framework net8.0

for class-library projects such as:

- Common
- Repository
- Services
- other class-library components

Use:

dotnet sln <solution> add <project>

to add projects to the existing solution.

Use:

dotnet add <project> reference <referenced-project>

to add project-to-project references.

When adding references, make sure the referenced project belongs to the NEW project being created.

Example:

Customer.API.csproj

must reference:

Customer.Services.csproj

not:

ManualForecast.Services.csproj

---

# 10. Project Creation Location

Business projects are normally created under:

Apps/Business/

For a Business project named Customer:

Apps/Business/Customer/

Core projects are normally under:

Apps/Core/

The user request or existing project pattern should determine the correct application location.

If the user asks for a Business project without specifying another location, use:

Apps/Business/

---

# 11. Similar Project Rules

When creating a project similar to an existing project:

1. Inspect the existing project's directory.
2. Determine the project components it contains.
3. Determine which project types those components use.
4. Create equivalent projects for the new project name.
5. Use the new naming convention.
6. Use net8.0 unless the user explicitly requests another framework.
7. Establish references between the NEW project's components where applicable.
8. Do not copy unrelated external project references.
9. Do not copy business-specific classes or implementation code during Phase 1.

Example:

If:

ManualForecast/
    ManualForecast.API
    ManualForecast.Common
    ManualForecast.Repository
    ManualForecast.Services

is used as a reference for:

Customer

create:

Customer/
    Customer.API
    Customer.Common
    Customer.Repository
    Customer.Services

and basic references should point to Customer projects.

---

# 12. Project Contents

Phase 1 is project creation only.

Do not create detailed application contents.

Do NOT automatically create:

Controllers/
Models/
Contracts/
Services/
Repositories/
Handlers/
Strategies/
Constants/
Options/

unless the user explicitly asks for those contents.

The objective of Phase 1 is to establish the correct .NET project structure.

---

# 13. Validation

Before completing a project-creation request:

1. Verify that all requested projects were created.
2. Verify that every generated project targets net8.0.
3. Verify that generated project names follow:
   <ProjectName>.<Component>
4. Verify that all generated projects were added to the existing solution.
5. Verify that basic project references point to the NEW project's components.
6. Verify that no unintended external NYBOSS project references were added.
7. Verify that no unrelated project components were created.
