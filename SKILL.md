---
name: dotnet-developer
description: Create and modify NYBOSS .NET server projects using the repository's project structure, conventions, and .NET 8 tooling.
---

# NYBOSS .NET Developer

Use this skill when the user asks to create or modify a NYBOSS .NET server project.

Read the relevant reference files before performing the requested operation.

## Project Creation

There are three supported project creation modes:

1. Default project
2. Explicit project composition
3. Project similar to an existing project

The selected creation mode determines the project composition and internal structure.

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

under:

Apps/Business/<ProjectName>/

Use the standard internal structure defined in:

references/project-creation.md

and:

references/project-structure.md

---

## 2. Explicit Project Composition

If the user explicitly specifies the project components, create only those components.

Example:

"Create a project with Common, Repository and Services"

creates:

<ProjectName>.Common
<ProjectName>.Repository
<ProjectName>.Services

Do not add API or any other component that was not requested.

Create the standard folders and files applicable to each selected component.

---

## 3. Similar to an Existing Project

If the user asks for a project similar to an existing project:

1. Locate the existing project.
2. Inspect its complete project-level structure.
3. Inspect the folders inside each project.
4. Inspect the files inside those folders.
5. Determine the project types and internal structure.
6. Create equivalent projects for the new project name.
7. Recreate the applicable folders and files.
8. Replace the old project name with the new project name where applicable.
9. Apply the new project naming convention.
10. Recreate applicable internal project references using the new project names.
11. Do not copy unrelated external project references.
12. Do not copy unrelated project-specific dependencies.
13. Do not copy business-specific implementation when it cannot be generalized safely.

The purpose of a similar-project request is to reproduce the structure and applicable project setup of the reference project for the new project.

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

Use the requested project name consistently in:

- directory names
- project names
- .csproj file names
- AssemblyName
- RootNamespace
- generated project-specific file names

unless a specific project structure requires otherwise.

---

# Target Framework

All newly created NYBOSS projects must target:

net8.0

Use the .NET 8 framework explicitly.

Do not allow the latest installed SDK/framework to be selected automatically.

Every generated project must contain:

<TargetFramework>net8.0</TargetFramework>

unless the user explicitly requests another framework.

---

# Project Creation Tools

Use the .NET CLI to create .NET projects.

Use the appropriate `dotnet new` template for the project type.

Use:

dotnet sln <solution> add <project>

to add projects to the existing solution.

Use:

dotnet add <project> reference <referenced-project>

for project-to-project references.

Use OpenCode's shell capabilities to execute the required commands.

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

# Internal Project Structure

After creating the .NET projects, create the standard folders and files required by the selected project type.

Read:

references/project-creation.md

for the standard project structure.

Do not create business-specific classes unless the user requests them.

---

# Project References

When multiple components are created for the same new project, references must be between those newly created components.

For example:

<ProjectName>.API
    -> <ProjectName>.Services

<ProjectName>.Services
    -> <ProjectName>.Common
    -> <ProjectName>.Repository

<ProjectName>.Repository
    -> <ProjectName>.Common

The references must use the new project name.

Do not reference equivalent projects belonging to another application.

Do not add unrelated external NYBOSS project references automatically.

---

# External Project References

Do not automatically add references to existing external NYBOSS projects.

If the user explicitly asks for an external project reference, add it only to the requested project.

External project-reference operations are separate from normal project creation.

---

# Solution

Use the existing:

BNPP.NYBOSS.NextGen.Server.sln

Do not create a new solution.

Every newly generated project must be added to the existing solution.

---

# Project Contents

For a default or explicitly requested project:

1. Create the .NET project.
2. Create the required folders.
3. Create the standard files.
4. Create the applicable project configuration.
5. Create basic internal project references.
6. Add the project to the existing solution.

Do not invent business-specific implementation.

For a similar-project request, inspect and reproduce the applicable folder/file structure of the reference project.

---

# Similar Project File Handling

When copying a structure from an existing project:

- Recreate applicable folders.
- Recreate applicable structural files.
- Rename project-specific files using the new project name.
- Update project-specific namespaces and references where applicable.
- Do not retain references to the original application.
- Do not retain the original application's project name in generated project names.
- Do not copy unrelated business implementation.

If a file is clearly business-specific and cannot be safely generalized, do not copy its business logic into the new project unless the user explicitly requests it.

---

# Validation

Before completing a project creation request, verify:

1. The requested project exists.
2. All required project components exist.
3. Required folders exist.
4. Required files exist.
5. Project names follow the new naming convention.
6. All generated projects target net8.0.
7. All generated projects are added to the existing solution.
8. Basic references point to the newly generated project's components.
9. No unintended external project references were added.
10. The generated solution and project files are valid.
