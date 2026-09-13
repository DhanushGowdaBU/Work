# NYBOSS Project Creation

This document defines how the NYBOSS .NET project structure is created.

Project creation at the current stage is STRUCTURE ONLY.

The goal is to create:

- projects
- folders
- files
- project configuration required for project creation
- project references
- solution entries

File implementation is not part of this stage.

---

# Target Framework

All newly generated projects must target:

net8.0

Use the framework explicitly when creating projects.

Do not allow the latest installed SDK/framework to determine the target framework automatically.

---

# Default Business Project

When the user requests a:

- simple project
- basic project
- normal project
- standard project
- regular project
- default project
- Business project without specifying components

create:

<ProjectName>.API
<ProjectName>.Common
<ProjectName>.Repository
<ProjectName>.Services

under:

Apps/Business/<ProjectName>/

---

# API Project

Create:

<ProjectName>.API

using the ASP.NET Core Web API template.

Use .NET 8 explicitly.

The API project should have the following structure:

<ProjectName>.API/
├── Controllers/
├── Properties/
├── appsettings.json
├── appsettings.Development.json
├── appsettings.Local.json
├── build-info.json
├── <ProjectName>.API.csproj
└── Program.cs

The Properties directory should contain the standard launch settings file when applicable.

Do not create business-specific controllers.

Do not copy controller implementations from another application.

Do not copy application configuration values from another application.

The files are created as structural files at this stage.

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

Create the folders and required structural files.

Do not create business-specific:

- models
- constants
- contracts
- options

unless explicitly requested.

Do not copy implementations from an existing application.

---

# Repository Project

Create:

<ProjectName>.Repository

using:

dotnet new classlib --framework net8.0 --name <ProjectName>.Repository

The standard structure is:

<ProjectName>.Repository/
├── RegisterServices.cs
└── <ProjectName>.Repository.csproj

Create additional repository files only when:

- explicitly requested by the user, or
- required by the structure of a reference project in a similar-project request.

When a file is created because of a reference project, reproduce the file name and location only.

Do not copy its contents.

---

# Services Project

Create:

<ProjectName>.Services

using:

dotnet new classlib --framework net8.0 --name <ProjectName>.Services

The standard structure is:

<ProjectName>.Services/
├── RegisterServices.cs
└── <ProjectName>.Services.csproj

Create additional service files only when:

- explicitly requested by the user, or
- required by the structure of a reference project in a similar-project request.

When a file is created because of a reference project, reproduce the file name and location only.

Do not copy its contents.

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

Do not reference equivalent projects from another application.

---

# Explicit Project Composition

If the user specifies components, create only those components.

For example:

"Create <ProjectName> with API, Common and Services"

creates:

<ProjectName>.API
<ProjectName>.Common
<ProjectName>.Services

Do not create Repository because it was not requested.

Create only the folders and files applicable to the selected components.

Only add project references that are valid for the selected components.

Do not add references to components that do not exist.

---

# Similar Project

If the user requests:

"Create <ProjectName> similar to <ExistingProject>"

the existing project is used as a STRUCTURAL REFERENCE ONLY.

Inspect the reference project to determine:

- project composition
- project types
- project directories
- folder names
- subfolder names
- file names
- file locations
- applicable project-to-project relationships

Use this information to create the new project structure.

---

# Similar Project Structure Process

For a similar-project request:

1. Locate the reference project.
2. Determine its project components.
3. Determine the type of each component.
4. Determine the directory hierarchy.
5. Determine the file hierarchy.
6. Create equivalent new projects.
7. Apply the new naming convention.
8. Recreate the folder hierarchy.
9. Recreate the file hierarchy.
10. Recreate applicable internal project references.
11. Add all generated projects to the existing solution.

---

# Similar Project File Rule

If the reference project contains:

<Folder>/<File>

create:

<ProjectName>.<Component>/<Folder>/<File>

where applicable.

The file name and location may be reproduced.

The contents must not be copied.

For example:

Reference:

Controllers/ExampleController.cs

Generated:

Controllers/ExampleController.cs

The generated file must not contain the implementation of the reference controller.

The same rule applies to:

- `.cs`
- `.json`
- `.xml`
- `.config`
- `.csproj`
- `.props`
- `.targets`
- `.resx`
- `.md`
- and other files

unless specific content generation is explicitly requested as a separate operation.

---

# Structure-Only File Content

At this stage, structural files should contain only the minimum content required for the generated project to remain valid.

For files that do not require content for project creation, create them as empty or minimal placeholders.

Do not populate them with copied application-specific content.

Do not attempt to make business functionality work during this stage.

---

# Project Configuration

Generated `.csproj` files must:

- use the new project name
- target net8.0
- contain applicable project references
- contain only required configuration for the generated project

Do not blindly copy an existing application's `.csproj`.

Do not copy unrelated package references.

Do not copy unrelated project references.

Do not copy application-specific configuration.

---

# Solution

Use:

BNPP.NYBOSS.NextGen.Server.sln

Add every generated project using:

dotnet sln BNPP.NYBOSS.NextGen.Server.sln add <project-path>

Do not create a separate solution.

---

# Structure-Only Completion

Project creation is complete when:

- requested projects exist
- requested folders exist
- required files exist
- project names are correct
- project references are correct
- projects target net8.0
- projects are added to the existing solution

Do not continue into implementation automatically.

File implementation will be handled in a later stage.
