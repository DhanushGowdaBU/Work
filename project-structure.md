# NYBOSS Server Project Structure

The NYBOSS server repository contains multiple applications with different project compositions.

There is no single mandatory project structure for every application.

Existing applications must therefore be treated as structural references.

---

# Repository Structure

The main server repository contains areas such as:

Apps/
Gateway/
Infrastructure/
Libraries/
Proxy/
Shared/
Tests/

Business applications are generally located under:

Apps/Business/

Core applications are generally located under:

Apps/Core/

The existing solution is:

BNPP.NYBOSS.NextGen.Server.sln

---

# Default New Business Project

When a user asks for a simple/basic/normal Business project without specifying a structure, use:

<ProjectName>.API
<ProjectName>.Common
<ProjectName>.Repository
<ProjectName>.Services

Example:

Customer/
├── Customer.API/
├── Customer.Common/
├── Customer.Repository/
└── Customer.Services/

All projects target:

net8.0

---

# ManualForecast Example

ManualForecast contains:

ManualForecast.API
ManualForecast.Common
ManualForecast.MessageProcessing
ManualForecast.MessageProcessingHost
ManualForecast.Repository
ManualForecast.Services

This demonstrates a multi-project Business application.

It is a structural reference only.

For a new project called Customer, the equivalent structure would use:

Customer.API
Customer.Common
Customer.MessageProcessing
Customer.MessageProcessingHost
Customer.Repository
Customer.Services

Do not copy the existing project's legacy naming convention.

---

# TradeLifeStatus Example

TradeLifeStatus contains:

Aft.Services
Odin.Services
TradeLifeStatus.Common
TradeLifeStatus.Repository
TradeLifeStatusInternal.Host
TradeLifeStatusInternal.Services

This demonstrates that NYBOSS applications can have different project compositions.

The generator must not assume that every application contains:

API
Common
Repository
Services

Those four components are the DEFAULT for a new simple/basic/normal Business project.

Existing project patterns may contain additional or different components.

---

# Existing Project Naming

Existing NYBOSS projects may use names such as:

BNPP.NYBOSS.Instrument.API
BNPP.NYBOSS.ManualForecast.API
BNPP.NYBOSS.ManualForecast.Services

This is an existing/legacy naming convention.

New projects must use:

<ProjectName>.<Component>

Examples:

Customer.API
Customer.Common
Customer.Repository
Customer.Services

Do not add:

BNPP.NYBOSS.

to newly generated project names.

Existing projects are references for structure and purpose only.

---

# Existing Project Files

Existing NYBOSS .csproj files may contain:

- TargetFramework
- PackageReference
- ProjectReference
- AssemblyName
- RootNamespace
- GenerateAssemblyInfo
- GlobalAssemblyInfo
- NYBOSS-specific build configuration
- Project-specific dependencies

Do not copy all of these settings automatically into a new project.

Determine which settings are generic and required for the new project.

The current default framework is:

net8.0

---

# Project References

Existing applications may contain many references to:

- Infrastructure
- Core
- Gateway
- DateService
- MessageEngine
- Messaging
- Other NYBOSS applications

These references must NOT be copied automatically when creating a new application.

For a newly created application, basic references should point to the newly created application's own projects.

Example:

Customer.API
    -> Customer.Services

Customer.Services
    -> Customer.Common
    -> Customer.Repository

Customer.Repository
    -> Customer.Common

Do not make:

Customer.API
    -> ManualForecast.Services

or:

Customer.Services
    -> ManualForecast.Common

The new application must reference its own generated components.

---

# External References

External NYBOSS references are added only when:

1. The user explicitly requests them, or
2. A specifically requested project pattern requires a known external dependency.

Example:

"Add BNPP.NYBOSS.DateService.Services reference to Customer.Services."

This should modify Customer.Services.csproj.

It should not be automatically included merely because ManualForecast.Services.csproj or ManualForecast.API.csproj contains a similar reference.

---

# Phase 1

The current automation phase focuses only on:

- Creating project directories
- Creating .csproj files
- Selecting the correct .NET project type
- Targeting net8.0
- Creating basic internal project references
- Adding projects to the existing solution

Detailed project contents will be implemented later.

Do not automatically generate:

Controllers
Models
Contracts
Interfaces
Service implementations
Repository implementations
Handlers
Strategies
Constants
Options
Dependency injection registration
Business logic
API endpoints
