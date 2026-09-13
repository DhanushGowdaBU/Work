---
name: dotnet-developer
description: Create and modify NYBOSS .NET server projects using the repository's project structure, conventions, and .NET 8 tooling.
license: MIT
metadata:
  author: Copyright 2026 Google LLC
  version: '1.0'
---

# NYBOSS .NET Developer

Use this skill when the user asks to create or modify a NYBOSS .NET server project.

Read the relevant reference files before performing the requested operation.

---

# Project Creation

There are three supported project creation modes:

1. Default project
2. Explicit project composition
3. Project similar to an existing project

The project creation mode must be determined from the user's request.

All three creation modes currently generate the project structure only.

Project structure means:

- project directories
- project files
- folders
- file names
- `.csproj` files
- project-to-project references
- solution entries

Do not generate application implementation during project creation.

---

# Structure-Only Rule

The current project creation stage is STRUCTURE ONLY.

The purpose of this stage is to create the correct project skeleton.

Do not:

- implement business logic
- implement controllers
- implement services
- implement repositories
- implement models
- implement handlers
- implement application functionality
- copy source-code implementation
- copy business logic
- copy configuration values
- copy application-specific settings
- copy secrets
- copy connection strings

When a file needs to be created, create the file itself.

Do not populate the file by copying the contents of an existing application.

File implementation and content generation will be handled separately.

---

# 1. Default Project

If the user asks for a:

- simple project
- basic project
- normal project
- standard project
- regular project
- default project
- Business project without specifying components

create the default Business project composition:

<ProjectName>.API
<ProjectName>.Common
<ProjectName>.Repository
<ProjectName>.Services

under:

Apps/Business/<ProjectName>/

Use:

references/project-creation.md

and:

references/project-structure.md

to determine the required structure.

Create the required projects, folders, and file names.

Do not create business-specific implementation.

---

# 2. Explicit Project Composition

If the user explicitly specifies the project components, create only the requested components.

For example:

"Create a project with Common, Repository and Services"

creates:

<ProjectName>.Common
<ProjectName>.Repository
<ProjectName>.Services

Do not automatically add:

<ProjectName>.API

or any other component that was not requested.

Create the folders and file names applicable to the selected components.

Only create project references that are valid for the components that actually exist.

---

# 3. Similar to an Existing Project

If the user asks for a project similar to an existing project:

1. Locate the existing project.
2. Inspect its project-level structure.
3. Determine the project components.
4. Determine the project types.
5. Inspect directory and folder names.
6. Inspect file names.
7. Determine applicable project-to-project relationships.
8. Create equivalent projects for the new project.
9. Recreate the applicable folder hierarchy.
10. Recreate the applicable file hierarchy.
11. Rename project-specific names to the new project name.
12. Apply the new project naming convention.
13. Recreate applicable internal project references using the new project names.
14. Add the generated projects to the existing solution.

The existing project is a STRUCTURAL REFERENCE ONLY.

---

# Similar Project Inspection Rules

When creating a project similar to an existing project, inspect only information required to determine its structure.

Allowed information includes:

- project names
- project types
- project directories
- folder names
- subfolder names
- file names
- project-to-project relationships
- project configuration required to determine the project type

Do not inspect existing files for the purpose of copying their contents.

Do not copy or reproduce:

- source-code implementation
- class implementation
- method implementation
- business logic
- controller implementation
- service implementation
- repository implementation
- handler implementation
- model implementation
- configuration values
- connection strings
- secrets
- application-specific settings
- application data
- business-specific JSON content
- business-specific XML content

If a reference project contains:

<Folder>/<File>

create the corresponding folder and file in the new project.

The existence and name of the file may be reproduced.

The contents of the file must not be reproduced during this stage.

---

# File Creation Boundary

Creating a file means creating the file structure.

It does not mean copying the contents of a corresponding file from another project.

For example, if the reference project contains:

Controllers/SomeController.cs

create the corresponding controller file in the new project.

Do not copy the controller implementation.

If the reference project contains:

appsettings.json

create the required file.

Do not copy the reference application's configuration values.

If the reference project contains:

RegisterServices.cs

create the required file.

Do not copy the reference implementation.

If a file is required only because it exists in the structural reference, create the file as an empty or minimal structural placeholder.

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
- `.csproj` file names
- AssemblyName
- RootNamespace
- generated project-specific file names

unless a specific project structure requires otherwise.

---

# Target Framework

All newly created NYBOSS projects must target:

net8.0

Use .NET 8 explicitly when creating projects.

Do not allow the latest installed SDK/framework to determine the target framework automatically.

Every generated project must contain:

<TargetFramework>net8.0</TargetFramework>

unless the user explicitly requests another framework.

---

# Project Creation Tools

Use the .NET CLI to create .NET projects.

Use the appropriate `dotnet new` template for the project type.

For API projects, use the ASP.NET Core Web API template and configure it to match the required project structure.

For class-library projects, use the class library template.

Use:

dotnet sln <solution> add <project>

to add projects to the existing solution.

Use:

dotnet add <project> reference <referenced-project>

to add project-to-project references.

Use OpenCode shell capabilities to execute the required commands.

---

# Default Project Types

For the default Business project:

<ProjectName>.API

is an ASP.NET Core Web API project.

The following are class-library projects:

<ProjectName>.Common
<ProjectName>.Repository
<ProjectName>.Services

All generated projects target:

net8.0

---

# Default Project References

When all default components are created, create these references:

<ProjectName>.API
    -> <ProjectName>.Services

<ProjectName>.Services
    -> <ProjectName>.Common
    -> <ProjectName>.Repository

<ProjectName>.Repository
    -> <ProjectName>.Common

References must point to components belonging to the same newly created project.

Do not reference equivalent components belonging to another application.

---

# Explicit Project References

For explicitly requested project compositions:

- create only the requested projects
- add only applicable references
- do not reference components that were not created
- do not add unrelated external project references

References must be based on the components that actually exist in the new project.

---

# External Project References

Do not automatically add references to existing external NYBOSS projects.

If the user explicitly requests an external project reference, add only the requested reference.

External project references are separate from normal project creation.

---

# Solution

Use the existing solution:

BNPP.NYBOSS.NextGen.Server.sln

Do not create a new solution.

Every newly generated project must be added to the existing solution.

---

# Project Structure Generation

For every generated project:

1. Create the project.
2. Create the required directories.
3. Create the required folders.
4. Create the required file names.
5. Create the `.csproj`.
6. Configure the target framework.
7. Add applicable project references.
8. Add the project to the existing solution.

Do not populate application-specific implementation.

---

# Similar Project Structure Generation

For a similar-project request, reproduce the applicable structural hierarchy of the reference project.

Reproduce:

- project composition
- project types
- folder hierarchy
- file hierarchy
- applicable project relationships

Apply the new project name to project-specific names.

Do not reproduce:

- source-code content
- business logic
- configuration content
- application data
- secrets
- connection strings
- unrelated dependencies

The result should be structurally similar to the reference project but independently named and structurally prepared for later implementation.

---

# Current Phase Boundary

Project creation currently ends after the project skeleton has been generated.

Do not implement application functionality as part of project creation.

File contents and implementation are intentionally deferred to a later stage.

If the user asks for file implementation separately, that is a separate operation and should be handled according to the user's specific requirements.

---

# Validation

Before completing a project creation request, verify:

1. The requested project exists.
2. The requested project components exist.
3. Required folders exist.
4. Required file names exist.
5. Project names follow the new naming convention.
6. All generated projects target net8.0 unless another framework was explicitly requested.
7. All generated projects are added to the existing solution.
8. Required internal references point to the newly generated project's components.
9. No unintended external project references were added.
10. The generated folder and file structure matches the selected creation mode.
11. Similar-project generation reproduced structure without copying file contents.
12. No business implementation was generated as part of the structure-only operation.
