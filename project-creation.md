# NYBOSS .NET Project Creation

## Phase 1

The current implementation creates the .NET project structure only.

The generator/OpenCode must create:

- Project directories
- .csproj files
- Basic project-to-project references
- Solution registration

Do not create application implementation contents during this phase.

---

# Target Framework

All newly created NYBOSS projects must target:

net8.0

The .NET CLI must be instructed explicitly to use:

--framework net8.0

Do not rely on the default/latest installed framework.

The generated .csproj must contain:

<TargetFramework>net8.0</TargetFramework>

unless the user explicitly requests another target framework.

---

# Default Business Project

When the user requests a simple, basic, normal, standard, or unspecified Business project, create:

<ProjectName>.API
<ProjectName>.Common
<ProjectName>.Repository
<ProjectName>.Services

Example:

Customer

creates:

Apps/Business/Customer/
├── Customer.API/
├── Customer.Common/
├── Customer.Repository/
└── Customer.Services/

---

# Project Templates

## API

Use the ASP.NET Core Web API template.

Command:

dotnet new webapi --framework net8.0 --name <ProjectName>.API

Example:

dotnet new webapi --framework net8.0 --name Customer.API

The API project must target net8.0.

---

## Common

Use a .NET class library.

Command:

dotnet new classlib --framework net8.0 --name <ProjectName>.Common

Example:

dotnet new classlib --framework net8.0 --name Customer.Common

---

## Repository

Use a .NET class library.

Command:

dotnet new classlib --framework net8.0 --name <ProjectName>.Repository

Example:

dotnet new classlib --framework net8.0 --name Customer.Repository

---

## Services

Use a .NET class library.

Command:

dotnet new classlib --framework net8.0 --name <ProjectName>.Services

Example:

dotnet new classlib --framework net8.0 --name Customer.Services

---

# Project Creation Order

For the default Business project, create:

1. Common
2. Repository
3. Services
4. API

Example:

Customer.Common
Customer.Repository
Customer.Services
Customer.API

This allows all generated projects to exist before project references are added.

---

# Basic Project References

For the default Business project:

Customer.API
    -> Customer.Services

Customer.Services
    -> Customer.Common
    -> Customer.Repository

Customer.Repository
    -> Customer.Common

Customer.Common
    -> no generated project dependency

Use:

dotnet add <project> reference <referenced-project>

Examples:

dotnet add Customer.API/Customer.API.csproj reference Customer.Services/Customer.Services.csproj

dotnet add Customer.Services/Customer.Services.csproj reference Customer.Common/Customer.Common.csproj

dotnet add Customer.Services/Customer.Services.csproj reference Customer.Repository/Customer.Repository.csproj

dotnet add Customer.Repository/Customer.Repository.csproj reference Customer.Common/Customer.Common.csproj

The referenced project MUST belong to the project currently being created.

For Customer, use Customer.* projects.

Never substitute a reference project such as ManualForecast.*.

---

# Explicit Components

If the user specifies components, create only the requested components.

Example:

"Create Customer with Common, Repository and Services"

creates:

Customer.Common
Customer.Repository
Customer.Services

Basic references:

Customer.Services
    -> Customer.Common
    -> Customer.Repository

Customer.Repository
    -> Customer.Common

Do not create Customer.API.

---

# Similar to Existing Project

If the user says:

"Create Customer similar to ManualForecast"

inspect:

Apps/Business/ManualForecast/

Determine the project composition.

If the reference composition is:

ManualForecast.API
ManualForecast.Common
ManualForecast.MessageProcessing
ManualForecast.MessageProcessingHost
ManualForecast.Repository
ManualForecast.Services

create:

Customer.API
Customer.Common
Customer.MessageProcessing
Customer.MessageProcessingHost
Customer.Repository
Customer.Services

The following must NOT be copied automatically:

- ManualForecast project names
- BNPP.NYBOSS naming prefix
- ManualForecast-specific project references
- ManualForecast-specific package references
- ManualForecast-specific business code
- ManualForecast-specific configuration

---

# TradeLifeStatus Example

If the user says:

"Create TradeSettlement similar to TradeLifeStatus"

inspect the actual TradeLifeStatus project structure.

If the project composition is:

Aft.Services
Odin.Services
TradeLifeStatus.Common
TradeLifeStatus.Repository
TradeLifeStatusInternal.Host
TradeLifeStatusInternal.Services

create equivalent project components using the new naming convention.

Do not assume that every component must use:

<ProjectName>.<Component>

if the reference contains an architectural component whose exact new name is unclear.

Determine the component role first and then apply the new naming convention consistently.

Do not copy the old TradeLifeStatus naming directly.

---

# Solution Registration

The existing solution is:

BNPP.NYBOSS.NextGen.Server.sln

Do not create a new solution.

Add every newly generated project:

dotnet sln BNPP.NYBOSS.NextGen.Server.sln add <project-path>

Example:

dotnet sln BNPP.NYBOSS.NextGen.Server.sln add Apps/Business/Customer/Customer.Common/Customer.Common.csproj

Repeat for all generated projects.

---

# External Project References

Do not add external NYBOSS project references during normal/default project creation.

For example, do not automatically add:

Infrastructure references
Core references
Gateway references
DateService references
MessageEngine references
Messaging references
Other application references

even if a reference project contains them.

The user may explicitly request them later.

Example:

"Add DateService Services reference to Customer.Services."

Then add the requested external reference to:

Customer.Services.csproj

This is a separate modification operation.

---

# Project Contents

Do not generate implementation contents during Phase 1.

Do not automatically create:

- Controllers
- Models
- Interfaces
- Repository classes
- Service classes
- Handlers
- Strategies
- Constants
- Options
- DI registration
- API endpoints

unless explicitly requested.

The purpose of Phase 1 is project creation.

---

# Validation

After creation, verify:

1. All requested projects exist.
2. All generated projects target net8.0.
3. Project names use the new naming convention.
4. Projects are added to BNPP.NYBOSS.NextGen.Server.sln.
5. Basic references point to newly generated projects.
6. No unrelated external references were added.
7. No unintended projects were created.
