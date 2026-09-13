---
name: dotnet-developer
description: Create and modify NYBOSS .NET server projects using the repository's project structure, conventions, and .NET 8 tooling.
---

# NYBOSS .NET Developer

Use this skill when the user asks to create or modify a NYBOSS .NET server project.

Read the relevant reference files before performing the requested operation.

# Project Creation

There are three supported project creation modes:

1. Default project
2. Explicit project composition
3. Project similar to an existing project

All three modes currently generate the project structure only.

Project structure generation means:

- projects
- directories
- file names
- .csproj files required by project creation
- solution entries
- project-to-project references

Do not generate or copy business implementation content at this stage.

---

# 1. Default Project

If the user asks for a:

- simple project
- basic project
- normal project
- standard project
- regular project
- project without specifying components

create:

<ProjectName>.API
<ProjectName>.Common
<ProjectName>.Repository
<ProjectName>.Services

under:

Apps/Business/<ProjectName>/

Use the standard structure defined in:

references/project-creation.md

and:

references/project-structure.md

Create the required folders and file names.

Do not create business-specific implementation.

---

# 2. Explicit Project Composition

If the user explicitly specifies the project components, create only those components.

For example:

"Create a project with Common, Repository and Services"

creates:

<ProjectName>.Common
<ProjectName>.Repository
<ProjectName>.Services

Do not add API or any other component that was not requested.

Create only the folders and file names applicable to the selected components.

---

# 3. Similar to an Existing Project

If the user asks for a project similar to an existing project:

1. Locate the existing project.
2. Inspect its project-level directory structure.
3. Inspect the directory names inside each project.
4. Inspect the file names inside those directories.
5. Determine the project composition.
6. Recreate the equivalent project composition for the new project.
7. Recreate the applicable folder structure.
8. Recreate the applicable file names.
9. Rename project-specific names to the new project name.
10. Apply the new project naming convention.
11. Create applicable project-to-project references using the new project names.

## Important

For a similar-project request, inspect the existing project only for its:

- project names
- folder names
- file names
- project relationships
- project types

Do NOT inspect existing source files to reproduce their contents.

Do NOT copy:

- source-code implementation
- business logic
- method implementations
- class implementations
- configuration values
- connection strings
- secrets
- business-specific JSON content
- business-specific XML content
- business-specific settings
- existing application data

At this stage, the goal is to reproduce the STRUCTURE, not the CONTENT.

If an existing project contains:

<folder>/<file>

create the corresponding:

<new-project-folder>/<file>

but do not copy the contents of the original file.

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

Use OpenCode shell capabilities to execute the required commands.

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

Default references:

<ProjectName>.API
    -> <ProjectName>.Services

<ProjectName>.Services
    -> <ProjectName>.Common
    -> <ProjectName>.Repository

<ProjectName>.Repository
    -> <ProjectName>.Common

Do not reference equivalent projects belonging to another application.

Do not add unrelated external NYBOSS project references automatically.

---

# External Project References

Do not automatically add references to existing external NYBOSS projects.

If the user explicitly asks for an external project reference, add it only to the requested project.

---

# Solution

Use the existing:

BNPP.NYBOSS.NextGen.Server.sln

Do not create a new solution.

Every newly generated project must be added to the existing solution.

---

# File and Folder Generation

The current project-generation stage is STRUCTURE ONLY.

For every generated project:

1. Create the project.
2. Create the required folders.
3. Create the required file names.
4. Create the required .csproj.
5. Add required project references.
6. Add the project to the existing solution.

Do not populate business-specific file contents.

Do not copy implementation from existing projects.

Do not analyze existing source files for content.

When a file must exist structurally, create the file with minimal/empty content appropriate for the current stage.

---

# Similar Project Structure Generation

When creating a project similar to an existing project, use the existing project as a STRUCTURAL REFERENCE ONLY.

Inspect:

- project directories
- folder names
- file names
- project types
- project relationships

Do not inspect the implementation inside source files unless required later by the user.

The generated structure should preserve the applicable:

- project composition
- directory hierarchy
- file hierarchy
- project relationships

while replacing the original project name with the requested new project name.

---

# Current Phase Boundary

Do not implement application functionality during project creation.

Do not:

- implement controllers
- implement services
- implement repositories
- implement models
- implement business logic
- copy existing implementation
- copy configuration values
- copy application-specific settings

The purpose of this stage is to create the correct project skeleton.

File contents and implementation will be handled in a later request.

---

# Validation

Before completing a project creation request, verify:

1. The requested project exists.
2. All requested project components exist.
3. Required folders exist.
4. Required file names exist.
5. Project names follow the new naming convention.
6. All generated projects target net8.0.
7. All generated projects are added to the existing solution.
8. Basic references point to the newly generated project's components.
9. No unintended external project references were added.
10. No existing source-code implementation was copied into the generated project.
11. The generated structure matches the requested creation mode.
