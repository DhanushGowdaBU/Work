# NYBOSS Project Structure

The NYBOSS server repository contains multiple applications and project types.

Project composition can vary depending on application purpose.

## Business Applications

Business applications are normally located under:

Apps/Business/

The default Business project contains:

<ProjectName>.API
<ProjectName>.Common
<ProjectName>.Repository
<ProjectName>.Services

---

## Default API Structure

<ProjectName>.API/
├── Controllers/
├── Properties/
├── appsettings.json
├── appsettings.Development.json
├── appsettings.Local.json
├── build-info.json
├── <ProjectName>.API.csproj
└── Program.cs

---

## Default Common Structure

<ProjectName>.Common/
├── Constants/
├── Contracts/
│   ├── Repository/
│   └── Services/
├── Models/
├── Options/
├── GlobalUsings.cs
└── <ProjectName>.Common.csproj

---

## Default Repository Structure

<ProjectName>.Repository/
├── RegisterServices.cs
└── <ProjectName>.Repository.csproj

Additional repository implementation files are created when requested.

---

## Default Services Structure

<ProjectName>.Services/
├── RegisterServices.cs
└── <ProjectName>.Services.csproj

Additional service implementation files are created when requested.

---

## Project Naming

New projects use:

<ProjectName>.<Component>

Examples:

<ProjectName>.API
<ProjectName>.Common
<ProjectName>.Repository
<ProjectName>.Services

Do not add legacy organizational prefixes to newly generated projects.

---

## Target Framework

New projects target:

net8.0

unless the user explicitly requests another framework.

---

## Existing Project Pattern

When a user asks for a project similar to an existing project, inspect the existing project structure.

The generated project should reproduce:

- applicable project components
- applicable folders
- applicable structural files
- applicable project configuration

The new project name must be applied consistently.

Do not blindly copy:

- legacy project names
- unrelated external references
- unrelated packages
- business-specific implementation
- application-specific configuration

---

## Solution

The existing solution is:

BNPP.NYBOSS.NextGen.Server.sln

All newly created projects belong to this solution.

Do not create a separate solution.
