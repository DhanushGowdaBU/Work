---
name: dotnet-developer
description: Create and modify NYBOSS .NET server projects following the repository conventions and .NET 8 standards.
---

# NYBOSS .NET Developer

You are working on the NYBOSS .NET server repository.

Follow the project creation and modification rules defined in this skill and its reference files.

## Technology

- Use .NET 8 for newly created projects.
- All newly generated projects must target `net8.0`.
- Use the .NET CLI for .NET project creation and solution management.
- Do not use npm, TypeScript, tsx, Angular tooling, or Node.js tooling for server-side project creation.

## Project Naming

All newly created projects must follow:

`<ProjectName>.<Component>`

Examples:

- `<ProjectName>.API`
- `<ProjectName>.Common`
- `<ProjectName>.Repository`
- `<ProjectName>.Services`
- `<ProjectName>.MessageProcessing`
- `<ProjectName>.MessageProcessingHost`

Do not use the legacy `BNPP.NYBOSS` prefix for newly created projects.

Existing project names are references for understanding structure only.

## Project Creation

When the user requests a new Business project, determine the project composition from the request.

### Default Project

If the user asks for a:

- simple project
- basic project
- normal project
- standard project
- regular project
- project without specifying a structure

create these components:

- API
- Common
- Repository
- Services

### Explicit Components

If the user explicitly specifies project components, create only those components.

Do not add components that were not requested unless they are required for the requested structure.

### Similar Project

If the user asks to create a project similar to an existing project:

1. Inspect the existing project.
2. Determine its project composition.
3. Determine the structure required for those project components.
4. Create equivalent components using the new project name.
5. Apply the new naming convention.
6. Do not copy legacy project names.
7. Do not copy unrelated project-specific configuration.

## Target Framework

All newly created projects must target:

`net8.0`

When using `dotnet new`, explicitly specify:

`--framework net8.0`

Do not allow the installed/latest SDK to select another framework automatically.

## Project Templates

Use the appropriate .NET template for each project type.

For an API project, use the ASP.NET Core Web API template.

For class-library based projects, use the .NET class library template.

Refer to:

`references/project-creation.md`

for the project creation commands and structure.

## Project Structure

Project creation must generate the required structural folders and files for the selected project components.

The generated structure should follow the established NYBOSS conventions.

Do not generate unnecessary folders or files.

## Project Contents

When a project component requires standard files or folders, create them as part of project creation.

For example, an API project may require:

- Controllers
- Properties
- launchSettings.json
- appsettings.json
- appsettings.Development.json
- appsettings.Local.json
- build-info.json
- Program.cs
- project file

Other project types should receive the structural files appropriate to their purpose.

Do not invent business-specific implementation code.

## Project References

When multiple components belonging to the same new project are created, references must point to the newly created components.

For example, for a project named `<ProjectName>`:

`<ProjectName>.API` should reference `<ProjectName>.Services`.

`<ProjectName>.Services` should reference `<ProjectName>.Common` and `<ProjectName>.Repository`.

`<ProjectName>.Repository` should reference `<ProjectName>.Common`.

Do not reference an existing project's equivalent component.

Do not automatically add unrelated external NYBOSS project references.

External project references should only be added when explicitly requested.

## Solution

Use the existing solution:

`BNPP.NYBOSS.NextGen.Server.sln`

All newly created projects must be added to this solution.

Do not create a separate solution for the new project.

## .NET CLI

Use the .NET CLI for project creation and solution management.

API:

`dotnet new webapi --framework net8.0 --name <ProjectName>.API`

Class library:

`dotnet new classlib --framework net8.0 --name <ProjectName>.<Component>`

Add project to solution:

`dotnet sln <solution> add <project>`

Add a project reference:

`dotnet add <project> reference <referenced-project>`

Use references only when required by the generated project structure or explicitly requested by the user.

## Application Location

Business applications are normally created under:

`Apps/Business/`

Core applications are normally created under:

`Apps/Core/`

If the user explicitly specifies another location, follow the user's request.

For an unspecified Business project, use:

`Apps/Business/<ProjectName>/`

## Validation

After creating a project, verify:

1. All requested project components were created.
2. Every generated project targets `net8.0`.
3. Project names follow `<ProjectName>.<Component>`.
4. Required structural files and folders exist.
5. All generated projects are added to the existing solution.
6. Internal references point to the newly created project's components.
7. No unintended external project references were added.
8. No unnecessary project components were created.
