---
name: dotnet-developer
description: Create and modify NYBOSS .NET server projects following repository conventions and .NET 8 standards.
---

# NYBOSS .NET Developer

You are working on the NYBOSS .NET server repository.

Follow the project creation and modification rules defined in this skill and its reference files.

## Technology

- Use .NET 8 for newly created projects.
- Newly created projects must target `net8.0`.
- Use the .NET CLI for project creation and solution management.
- Do not use npm, TypeScript, tsx, Angular CLI, or other client-side tooling for server-side project creation.

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

Do not add the legacy `BNPP.NYBOSS` prefix to newly created projects.

Existing project names must not determine the naming convention for new projects.

## Project Creation

When the user requests a new Business project, determine the required project composition.

### Default Project

If the user asks for a:

- simple project
- basic project
- normal project
- standard project
- regular project
- project without specifying a structure

create the default structure:

- API
- Common
- Repository
- Services

### Explicit Components

If the user explicitly specifies project components, create the requested components.

Do not create additional components that were not requested.

### Similar Project

If the user asks to create a project similar to an existing project:

1. Inspect the existing project's structure.
2. Determine its project composition.
3. Determine the project types used by its components.
4. Recreate the required structure using the new project name.
5. Apply the new project naming convention.
6. Do not copy the existing project's naming convention.
7. Do not blindly copy project-specific dependencies or configuration.

## Project Contents

Creating a project includes its required structural contents.

Generate the folders and standard files applicable to each selected project component.

Use the repository's established structure and conventions.

Do not invent unnecessary files or application functionality.

## Project References

When multiple components belonging to the newly created project are generated, references between those components must point to the newly created project's components.

For example, the default dependency direction is:

`<ProjectName>.API`
→ `<ProjectName>.Services`

`<ProjectName>.Services`
→ `<ProjectName>.Common`

`<ProjectName>.Services`
→ `<ProjectName>.Repository`

`<ProjectName>.Repository`
→ `<ProjectName>.Common`

Never reference another application's equivalent project when creating a new application.

External NYBOSS project references must not be added automatically.

Only add external project references when explicitly requested.

## Solution

Use the existing solution:

`BNPP.NYBOSS.NextGen.Server.sln`

Add all newly created projects to the existing solution.

Do not create a separate solution.

## .NET CLI

Use the appropriate .NET CLI commands.

For API projects:

`dotnet new webapi --framework net8.0`

For class-library projects:

`dotnet new classlib --framework net8.0`

Add a project to the solution using:

`dotnet sln <solution> add <project>`

Add a project reference using:

`dotnet add <project> reference <referenced-project>`

Always explicitly target `net8.0`.

## Application Location

Business applications are normally created under:

`Apps/Business/`

Core applications are normally created under:

`Apps/Core/`

Use the location specified by the user when provided.

For a Business project without a specified location, use:

`Apps/Business/`

## Project Configuration

Generated `.csproj` files must follow the requirements of the selected project type and NYBOSS conventions.

Do not blindly copy an existing project's complete `.csproj`.

Do not automatically copy:

- existing project names
- legacy assembly names
- legacy root namespaces
- unrelated package references
- unrelated project references
- business-specific configuration

## Project Contents Boundary

Create the structural files required for the selected project.

Do not automatically invent business functionality.

Examples of functionality that must not be invented unless requested:

- Controllers
- Models
- Interfaces
- Service implementations
- Repository implementations
- Handlers
- Strategies
- API endpoints
- Business logic

## Validation

Before completing project creation:

1. Verify all requested projects were created.
2. Verify all generated projects target `net8.0`.
3. Verify project names follow `<ProjectName>.<Component>`.
4. Verify required folders and standard files were created.
5. Verify all generated projects are added to the existing solution.
6. Verify internal references point to the newly created project's components.
7. Verify no unintended external project references were added.
8. Verify no unnecessary project components were created.
