# NYBOSS Project Creation

## Default Business Project

When the user requests a simple, basic, normal, standard, or unspecified Business project, create:

<ProjectName>.API
<ProjectName>.Common
<ProjectName>.Repository
<ProjectName>.Services

Create them under:

Apps/Business/<ProjectName>/

---

# Target Framework

All generated projects must target:

net8.0

Use the framework explicitly when creating the project.

---

# API Project

Create:

<ProjectName>.API

using the ASP.NET Core Web API template.

Command:

dotnet new webapi --framework net8.0 --name <ProjectName>.API

The API project must contain the standard Web API project files and the following NYBOSS structure where applicable:

<ProjectName>.API/
├── Controllers/
├── Properties/
├── appsettings.json
├── appsettings.Development.json
├── appsettings.Local.json
├── build-info.json
├── <ProjectName>.API.csproj
└── Program.cs

The Properties folder should contain the standard launch settings file when applicable.

Do not create business-specific controllers unless requested.

---

# Common Project

Create:

<ProjectName>.Common

using:

dotnet new classlib --framework net8.0 --name <ProjectName>.Common

The standard structure is:

<ProjectName>.Common/
├── Constants/
├── Contracts/
│   ├── Repository/
│   └── Services/
├── Models/
├── Options/
├── GlobalUsings.cs
└── <ProjectName>.Common.csproj

The folders are created as part of the project structure.

Do not create business-specific models, constants, contracts, or options unless requested.

---

# Repository Project

Create:

<ProjectName>.Repository

using:

dotnet new classlib --framework net8.0 --name <ProjectName>.Repository

The standard project contains:

<ProjectName>.Repository/
├── RegisterServices.cs
└── <ProjectName>.Repository.csproj

Create additional repository files only when the user requests specific repository functionality.

---

# Services Project

Create:

<ProjectName>.Services

using:

dotnet new classlib --framework net8.0 --name <ProjectName>.Services

The standard project contains:

<ProjectName>.Services/
├── RegisterServices.cs
└── <ProjectName>.Services.csproj

Create additional service files only when the user requests specific service functionality.

---

# Default Project References

For the default composition:

<ProjectName>.API
<ProjectName>.Common
<ProjectName>.Repository
<ProjectName>.Services

create:

<ProjectName>.API
    -> <ProjectName>.Services

<ProjectName>.Services
    -> <ProjectName>.Common
    -> <ProjectName>.Repository

<ProjectName>.Repository
    -> <ProjectName>.Common

Use:

dotnet add <project> reference <referenced-project>

The references must point to components belonging to the same newly created project.

---

# Explicit Project Composition

If the user specifies components, create only those components.

Example:

"Create <ProjectName> with API, Common and Services"

creates:

<ProjectName>.API
<ProjectName>.Common
<ProjectName>.Services

Create only the folders and files applicable to those components.

For references, use only references that are valid for the components that actually exist.

Do not create references to components that were not created.

---

# Similar Project

If the user requests:

"Create <ProjectName> similar to <ExistingProject>"

inspect the existing project before creating the new one.

Inspect:

- project directories
- project files
- folders
- files
- project types
- applicable project configuration
- applicable internal project relationships

Then recreate the equivalent structure using:

<ProjectName>.<Component>

The new project should contain equivalent folders and applicable files.

Rename project-specific files and namespaces to the new project name.

Do not copy unrelated external references.

Do not copy unrelated application-specific dependencies.

Do not copy business-specific implementation when it cannot be safely generalized.

---

# Solution

Use:

BNPP.NYBOSS.NextGen.Server.sln

Add every generated project using:

dotnet sln BNPP.NYBOSS.NextGen.Server.sln add <project-path>

Do not create a separate solution.

---

# Project Configuration

New project files must:

- target net8.0
- use the new project name
- use the new naming convention
- contain only required package references
- contain only applicable project references

Do not copy legacy organizational prefixes into AssemblyName or RootNamespace.

---

# Project Contents

The generated project must contain the standard structure for its project type.

Do not invent business-specific implementation.

For example, when creating:

<ProjectName>.Services

do not automatically create:

<ProjectName>Service.cs

unless the user requested a specific service.

The same rule applies to repositories, controllers, models, and other business-specific files.
