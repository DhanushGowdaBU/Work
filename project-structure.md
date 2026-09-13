# NYBOSS Project Structure

The NYBOSS server repository contains multiple applications and project types.

Project composition can vary depending on application purpose.

This document defines the project and directory structure used when generating new projects.

It does not define business implementation.

---

# Application Areas

NYBOSS projects can be organized under:

Apps/Business/

and:

Apps/Core/

The structure depends on the application area.

---

# Business Applications

Business applications are normally located under:

Apps/Business/

The default Business project contains:

<ProjectName>.API
<ProjectName>.Common
<ProjectName>.Repository
<ProjectName>.Services

---

# Default Business API Structure

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

The API uses the controller-based ASP.NET Core Web API template.

The template-generated:

<ProjectName>.API.http

file is not part of the default NYBOSS structure.

If generated, remove it.

CLI-generated files must retain their generated content.

---

# Default Business Common Structure

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

The structural files ensure that the required directories are represented in the project.

These files are skeleton files only.

Do not add business-specific implementation.

---

# Default Business Repository Structure

<ProjectName>.Repository/
├── <ProjectName>Repository.cs
├── RegisterServices.cs
└── <ProjectName>.Repository.csproj

The files are structural skeletons.

Do not add business-specific repository implementation.

---

# Default Business Services Structure

<ProjectName>.Services/
├── <ProjectName>Service.cs
├── RegisterServices.cs
└── <ProjectName>.Services.csproj

The files are structural skeletons.

Do not add business-specific service implementation.

---

# Core Applications

Core applications are normally located under:

Apps/Core/

The default Core project contains:

<ProjectName>.API
<ProjectName>.Domain
<ProjectName>.Repository
<ProjectName>.Services

---

# Default Core API Structure

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

The API uses the controller-based ASP.NET Core Web API template.

The template-generated:

<ProjectName>.API.http

file is not part of the default NYBOSS structure.

If generated, remove it.

CLI-generated files must retain their generated content.

---

# Default Core Domain Structure

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

The Domain project represents the domain layer of the Core application.

Domain is not automatically equivalent to Common.

Do not replace Domain with Common when creating a Core project.

The structural files are skeleton files only.

Do not add business-specific implementation.

---

# Default Core Repository Structure

<ProjectName>.Repository/
├── <ProjectName>Repository.cs
├── RegisterServices.cs
└── <ProjectName>.Repository.csproj

The files are structural skeletons.

Do not add business-specific repository implementation.

---

# Default Core Services Structure

<ProjectName>.Services/
├── <ProjectName>Service.cs
├── RegisterServices.cs
└── <ProjectName>.Services.csproj

The files are structural skeletons.

Do not add business-specific service implementation.

---

# Common vs Domain

Common and Domain are separate project types.

Business default:

<ProjectName>.Common

Core default:

<ProjectName>.Domain

Common and Domain must not be treated as interchangeable.

When a similar project is used as a reference, preserve the component type of the reference project.

---

# Structural File Rule

Folders required by the project structure should not be left without a representative file when that would make the folder invisible in the development environment or source control.

Create a minimal structural file appropriate to the folder.

Business Common examples:

Constants/
    <ProjectName>Constants.cs

Models/
    <ProjectName>Model.cs

Options/
    <ProjectName>Options.cs

Contracts/Repository/
    I<ProjectName>Repository.cs

Contracts/Services/
    I<ProjectName>Service.cs

Core Domain examples:

Models/
    <ProjectName>Model.cs

Constants/
    <ProjectName>Constants.cs

Options/
    <ProjectName>Options.cs

Contracts/Repository/
    I<ProjectName>Repository.cs

Contracts/Services/
    I<ProjectName>Service.cs

Repository:

<ProjectName>Repository.cs

Services:

<ProjectName>Service.cs

These files establish the project skeleton.

They do not implement business functionality.

---

# CLI-Generated File Rule

Files generated by `dotnet new` must retain their generated content.

Do not blank or replace:

- Program.cs
- `.csproj`
- launchSettings.json
- template-generated appsettings files
- other valid template-generated files

Modify them only when required for:

- target framework
- project references
- project naming
- other explicit project-generation requirements

---

# Similar Project Structure

When the user asks for a project similar to an existing project, use the existing project as a STRUCTURAL REFERENCE ONLY.

Determine:

- application area
- project composition
- project types
- project directories
- folder hierarchy
- file hierarchy
- file names
- applicable project relationships

Recreate the applicable structure using the new project name.

---

# Similar Project Rule

A similar-project request means:

STRUCTURE SIMILARITY

not:

CONTENT COPYING

The generated project should reproduce the applicable:

- project structure
- directory structure
- folder structure
- file structure
- project relationships

The generated project should not reproduce:

- source-code implementation
- business logic
- configuration values
- connection strings
- secrets
- application data
- application-specific settings
- unrelated dependencies

---

# Similar Project File Structure

When a similar project contains a file, reproduce:

- name
- location
- extension

Do not reproduce the reference file's implementation.

If the new project is created using a .NET CLI template and the CLI creates a corresponding file, retain the CLI-generated content.

If the file is not generated by the CLI and is required only to represent the structural hierarchy, create a minimal valid structural file.

---

# Project Naming

New projects use:

<ProjectName>.<Component>

Examples:

<ProjectName>.API
<ProjectName>.Common
<ProjectName>.Domain
<ProjectName>.Repository
<ProjectName>.Services

Do not add legacy organizational prefixes to newly generated projects.

Existing projects with legacy prefixes are structural references only.

Use the requested project name consistently in:

- project directory names
- project names
- `.csproj` file names
- AssemblyName
- RootNamespace
- project-specific structural file names

---

# Target Framework

New projects target:

net8.0

unless the user explicitly requests another framework.

All generated project files should use the requested target framework explicitly.

---

# Default Business Project References

The default Business composition uses:

<ProjectName>.API
    -> <ProjectName>.Services

<ProjectName>.Services
    -> <ProjectName>.Common
    -> <ProjectName>.Repository

<ProjectName>.Repository
    -> <ProjectName>.Common

References must point to the components of the same newly generated Business project.

Do not add unrelated external project references automatically.

---

# Default Core Project References

The default Core composition uses:

<ProjectName>.API
    -> <ProjectName>.Services

<ProjectName>.Services
    -> <ProjectName>.Domain
    -> <ProjectName>.Repository

<ProjectName>.Repository
    -> <ProjectName>.Domain

References must point to the components of the same newly generated Core project.

Do not add unrelated external project references automatically.

---

# Solution

The existing solution is:

BNPP.NYBOSS.NextGen.Server.sln

All newly created projects belong to this solution.

Do not create a separate solution.

---

# Unwanted API Template File

The following file is not part of the default NYBOSS API structure:

<ProjectName>.API.http

If generated by the .NET Web API template, remove it.

Do not recreate it unless the user explicitly requests it.

---

# Structure-Only Boundary

This document describes structure and initial project skeletons.

During project creation:

- create projects
- create directories
- create folders
- create structural files
- create `.csproj` files
- preserve CLI-generated content
- configure project references
- add projects to the solution

Do not implement application functionality.

Do not copy business logic.

Do not copy implementation from another application.

CLI-generated content must remain intact.

File implementation beyond the initial skeleton is handled separately.
