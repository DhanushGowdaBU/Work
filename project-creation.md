# NYBOSS Project Creation

## Default Project

When the user requests a simple, basic, normal, standard, or unspecified Business project, create:

<ProjectName>.API
<ProjectName>.Common
<ProjectName>.Repository
<ProjectName>.Services

Create them under:

Apps/Business/<ProjectName>/

---

## Explicit Project Composition

When the user specifies components, create only those components.

For example:

"Create <ProjectName> with Common, Repository and Services"

creates:

<ProjectName>.Common
<ProjectName>.Repository
<ProjectName>.Services

Do not add components that were not requested.

---

## Similar Project

When the user requests a project similar to an existing project:

1. Inspect the existing project's project-level structure.
2. Identify its components.
3. Create equivalent components for the requested project name.
4. Use the new naming convention.
5. Configure references between the newly created components where applicable.

Only the structure and applicable project configuration should be reused.

---

## Project Naming

Use:

<ProjectName>.<Component>

Examples:

<ProjectName>.API
<ProjectName>.Common
<ProjectName>.Repository
<ProjectName>.Services

The same project name must be used consistently throughout the generated project.

---

## Target Framework

Use:

net8.0

API:

dotnet new webapi --framework net8.0

Class library:

dotnet new classlib --framework net8.0

Do not use the latest installed framework automatically.

---

## API Project

Create:

<ProjectName>.API

using the ASP.NET Core Web API template.

The project must target:

net8.0

The generated API project should contain the standard files produced by the .NET 8 Web API template.

---

## Common Project

Create:

<ProjectName>.Common

as a .NET class-library project targeting net8.0.

---

## Repository Project

Create:

<ProjectName>.Repository

as a .NET class-library project targeting net8.0.

---

## Services Project

Create:

<ProjectName>.Services

as a .NET class-library project targeting net8.0.

---

## Default References

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

<ProjectName>.Services
    -> <ProjectName>.Repository

<ProjectName>.Repository
    -> <ProjectName>.Common

Only the newly created project's components should be referenced.

---

## Creating References

Use:

dotnet add <project> reference <referenced-project>

Example pattern:

dotnet add <ProjectName>.API/<ProjectName>.API.csproj reference <ProjectName>.Services/<ProjectName>.Services.csproj

The actual path must use the generated project name.

Do not reference another existing application's project.

---

## Solution

Use the existing solution:

BNPP.NYBOSS.NextGen.Server.sln

Add each generated project using:

dotnet sln BNPP.NYBOSS.NextGen.Server.sln add <project-path>

Do not create a new solution.

---

## Project Directory

For a Business project:

Apps/Business/<ProjectName>/

Example structure:

Apps/Business/<ProjectName>/
├── <ProjectName>.API/
├── <ProjectName>.Common/
├── <ProjectName>.Repository/
└── <ProjectName>.Services/

Each component directory contains its corresponding .csproj.

---

## Generated Project Files

The generated projects must be valid .NET 8 projects.

The project files must use the new project name.

Do not copy legacy assembly or namespace names from an existing project.

Do not copy unrelated package references.

Do not copy unrelated project references.

---

## Project Contents

Create the standard project files produced by the selected .NET template.

Do not generate application-specific implementation code unless requested.

The project creation request should produce a usable .NET solution structure.
