---
name: dotnet-developer
description: Create and modify NYBOSS .NET server projects using the repository's project structure, conventions, and .NET 8 tooling.
---

# NYBOSS .NET Developer

Use this skill when the user asks to create or modify a NYBOSS .NET server project.

Read the relevant reference files before performing the requested operation.

## Project Creation

When the user asks to create a project, determine which creation mode applies.

There are three supported creation modes:

1. Default project
2. Explicit project composition
3. Project similar to an existing project

Follow the rules below.

---

## 1. Default Project

If the user asks for a:

- simple project
- basic project
- normal project
- standard project
- regular project
- project without specifying components

create the default Business project composition:

- API
- Common
- Repository
- Services

For a project named `<ProjectName>`, create:

<ProjectName>.API
<ProjectName>.Common
<ProjectName>.Repository
<ProjectName>.Services

The default location for a Business project is:

Apps/Business/<ProjectName>/

---

## 2. Explicit Project Composition

If the user explicitly specifies the project components, create only the requested components.

Example:

"Create a project with Common, Repository and Services"

creates:

<ProjectName>.Common
<ProjectName>.Repository
<ProjectName>.Services

Do not add API unless requested.

The same rule applies to any other explicitly requested combination.

---

## 3. Similar to Existing Project

If the user asks for a project similar to an existing project:

1. Locate the existing project.
2. Inspect its project-level composition.
3. Identify the project types/components it contains.
4. Use that composition for the new project.
5. Replace the existing project name with the requested new project name.
6. Apply the new project naming convention.
7. Create the required projects.
8. Create references between the newly generated projects where applicable.

Do not copy the existing project's project names.

Do not copy unrelated project references.

Do not copy project-specific business implementation.

Do not copy project-specific package dependencies unless they are explicitly required for the requested structure.

---

# Naming Convention

All newly generated project names must follow:

<ProjectName>.<Component>

Examples:

<ProjectName>.API
<ProjectName>.Common
<ProjectName>.Repository
<ProjectName>.Services

Do not add legacy organizational prefixes to newly generated project names.

The name of the new project must be used consistently in:

- Directory names
- Project names
- .csproj names
- AssemblyName
- RootNamespace

unless a specific project type requires another convention.

---

# Target Framework

All newly created NYBOSS projects must target:

net8.0

Use the .NET 8 framework explicitly when creating projects.

Do not allow the installed/latest SDK to select another target framework automatically.

Every generated project must contain:

<TargetFramework>net8.0</TargetFramework>

unless the user explicitly requests another framework.

---

# Project Creation Tools

Use the .NET CLI to create projects.

Use the appropriate `dotnet new` template for each project type.

Use:

dotnet sln <solution> add <project>

to add projects to the existing solution.

Use:

dotnet add <project> reference <referenced-project>

for project-to-project references.

OpenCode should use its shell capabilities to execute the required .NET CLI commands.

---

# Default Project Types

For the default project:

<ProjectName>.API

is an ASP.NET Core Web API project.

The following are class-library projects:

<ProjectName>.Common
<ProjectName>.Repository
<ProjectName>.Services

All projects target net8.0.

---

# Project References

When multiple components are created for the same new project, references must be between those newly created components.

For example, if the new project is `<ProjectName>`:

<ProjectName>.API
    -> <ProjectName>.Services

<ProjectName>.Services
    -> <ProjectName>.Common
    -> <ProjectName>.Repository

<ProjectName>.Repository
    -> <ProjectName>.Common

Do not reference an existing application's equivalent project.

Do not add unrelated external project references automatically.

If the user explicitly requests an external project reference, handle that request separately.

---

# Solution

Use the existing:

BNPP.NYBOSS.NextGen.Server.sln

Do not create a new solution.

Every newly generated project must be added to the existing solution.

---

# Project Contents

When creating a project, generate the complete project structure required by the selected project type.

This includes the project file and the standard files created by the selected .NET template.

For an API project, use the appropriate ASP.NET Core Web API template.

For class-library projects, use the appropriate class-library template.

Do not generate NYBOSS business-specific implementation unless the user requests it.

---

# Existing Project Structure

Existing NYBOSS projects are used as references for understanding project composition and conventions.

Do not assume that all existing projects have the same structure.

Do not treat any one existing project as the universal template.

When the user explicitly asks for a project similar to an existing project, use that project's composition as the requested pattern.

When the user asks for a simple/default project, use the default:

API
Common
Repository
Services

---

# Validation

Before completing a project creation request, verify:

1. The requested project was created in the correct location.
2. All required project components were created.
3. Project names follow the new naming convention.
4. All generated projects target net8.0.
5. All generated projects are added to the existing solution.
6. Required references between generated components are present.
7. No unintended external project references were added.
8. The generated project files are valid.
9. The solution remains valid.
