# NYBOSS Project Structure

The NYBOSS server repository contains multiple applications and project types.

Project composition can vary depending on application purpose.

This document defines the project and directory structure used when generating new projects.

It does not define application implementation.

---

# Business Applications

Business applications are normally located under:

Apps/Business/

The default Business project contains:

<ProjectName>.API
<ProjectName>.Common
<ProjectName>.Repository
<ProjectName>.Services

The project naming convention for new projects is:

<ProjectName>.<Component>

---

# Default API Structure

The default API project is:

<ProjectName>.API/

Its structure is:

<ProjectName>.API/
├── Controllers/
├── Properties/
├── appsettings.json
├── appsettings.Development.json
├── appsettings.Local.json
├── build-info.json
├── <ProjectName>.API.csproj
└── Program.cs

The Properties directory should contain the applicable launch settings file.

These entries define the expected structure.

They do not define the implementation or configuration values.

Do not copy file contents from another application during structure generation.

---

# Default Common Structure

The default Common project is:

<ProjectName>.Common/

Its structure is:

<ProjectName>.Common/
├── Constants/
├── Contracts/
│   ├── Repository/
│   └── Services/
├── Models/
├── Options/
├── GlobalUsings.cs
└── <ProjectName>.Common.csproj

The folders define the expected structural organization.

Business-specific models, constants, contracts, and options are not created unless requested.

---

# Default Repository Structure

The default Repository project is:

<ProjectName>.Repository/

Its base structure is:

<ProjectName>.Repository/
├── RegisterServices.cs
└── <ProjectName>.Repository.csproj

Additional repository folders or files may be created when explicitly requested or when required by a similar-project structure.

Do not copy repository implementation from another application.

---

# Default Services Structure

The default Services project is:

<ProjectName>.Services/

Its base structure is:

<ProjectName>.Services/
├── RegisterServices.cs
└── <ProjectName>.Services.csproj

Additional service folders or files may be created when explicitly requested or when required by a similar-project structure.

Do not copy service implementation from another application.

---

# Similar Project Structure

When the user asks for a project similar to an existing project, use the existing project as a STRUCTURAL REFERENCE ONLY.

Determine:

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

# File Structure

When a similar project contains a file, reproduce the file's:

- name
- location
- extension

Do not reproduce its contents during structure generation.

For example:

Reference:

<ExistingComponent>/
├── Controllers/
│   └── ExampleController.cs
├── Services/
│   └── ExampleService.cs
└── appsettings.json

Generated:

<ProjectName>.<Component>/
├── Controllers/
│   └── ExampleController.cs
├── Services/
│   └── ExampleService.cs
└── appsettings.json

The generated files are structural placeholders.

Their implementation will be handled separately.

---

# Project Naming

New projects use:

<ProjectName>.<Component>

Examples:

<ProjectName>.API
<ProjectName>.Common
<ProjectName>.Repository
<ProjectName>.Services

Do not add legacy organizational prefixes to newly generated project names.

Use the requested project name consistently in:

- project directory names
- project names
- `.csproj` file names
- AssemblyName
- RootNamespace
- project-specific file names

---

# Target Framework

New projects target:

net8.0

unless the user explicitly requests another framework.

All generated project files should use the requested target framework explicitly.

---

# Default Project References

The default Business composition uses:

<ProjectName>.API
    -> <ProjectName>.Services

<ProjectName>.Services
    -> <ProjectName>.Common
    -> <ProjectName>.Repository

<ProjectName>.Repository
    -> <ProjectName>.Common

References must point to the components of the same newly generated project.

Do not add unrelated external project references automatically.

---

# Solution

The existing solution is:

BNPP.NYBOSS.NextGen.Server.sln

All newly created projects belong to this solution.

Do not create a separate solution.

---

# Structure-Only Boundary

This document describes structure only.

During project creation:

- create projects
- create directories
- create folders
- create files
- create `.csproj` files
- configure project references
- add projects to the solution

Do not implement application functionality.

Do not copy existing file contents.

Do not copy business logic.

Do not copy configuration.

Do not copy secrets.

File implementation is handled separately.
