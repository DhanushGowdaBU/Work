# NYBOSS Project Structure

The NYBOSS server repository contains multiple applications and project types.

Project composition can vary depending on application purpose.

This document defines the expected project and directory structure.

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

---

# Default API Structure

<ProjectName>.API/
├── Controllers/
├── Properties/
├── appsettings.json
├── appsettings.Development.json
├── appsettings.Local.json
├── build-info.json
├── <ProjectName>.API.csproj
└── Program.cs

These entries describe the expected structure.

Do not copy the contents of these files from another application during project creation.

---

# Default Common Structure

<ProjectName>.Common/
├── Constants/
├── Contracts/
│   ├── Repository/
│   └── Services/
├── Models/
├── Options/
├── GlobalUsings.cs
└── <ProjectName>.Common.csproj

Create the folders and files as structural elements.

Do not populate business-specific content during project creation.

---

# Default Repository Structure

<ProjectName>.Repository/
├── RegisterServices.cs
└── <ProjectName>.Repository.csproj

Additional repository files are created only when required by the user's request or by a similar-project structure.

Do not copy repository implementations from another application.

---

# Default Services Structure

<ProjectName>.Services/
├── RegisterServices.cs
└── <ProjectName>.Services.csproj

Additional service files are created only when required by the user's request or by a similar-project structure.

Do not copy service implementations from another application.

---

# Similar Project Structure

When the user asks for a project similar to an existing project, inspect the existing project structure.

Use it to determine:

- project composition
- folder hierarchy
- file hierarchy
- project relationships

Recreate the structure for the new project.

Do not copy file contents.

Do not copy implementation.

Do not copy business logic.

Do not copy configuration values.

Do not copy secrets.

Do not copy application-specific settings.

The existing project is a structural reference only.

---

# Project Naming

New projects use:

<ProjectName>.<Component>

Examples:

<ProjectName>.API
<ProjectName>.Common
<ProjectName>.Repository
<ProjectName>.Services

Do not add legacy organizational prefixes to newly generated projects.

---

# Target Framework

New projects target:

net8.0

unless the user explicitly requests another framework.

---

# Solution

The existing solution is:

BNPP.NYBOSS.NextGen.Server.sln

All newly created projects belong to this solution.

Do not create a separate solution.
