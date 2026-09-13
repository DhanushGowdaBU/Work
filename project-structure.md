# NYBOSS Project Structure

The NYBOSS server repository contains multiple applications and project types.

Project composition can vary depending on the purpose of the application.

## Business Applications

Business applications are normally located under:

Apps/Business/

A simple Business project uses:

<ProjectName>.API
<ProjectName>.Common
<ProjectName>.Repository
<ProjectName>.Services

Result:

Apps/Business/<ProjectName>/
├── <ProjectName>.API/
├── <ProjectName>.Common/
├── <ProjectName>.Repository/
└── <ProjectName>.Services/

---

## Core Applications

Core applications are normally located under:

Apps/Core/

The project composition must be determined from the user's request or an explicitly selected existing project pattern.

---

## Project Naming

New projects use:

<ProjectName>.<Component>

Examples:

<ProjectName>.API
<ProjectName>.Common
<ProjectName>.Repository
<ProjectName>.Services

Do not apply legacy organizational prefixes to newly created projects.

---

## Project Types

Common project types include:

API
Common
Repository
Services

Other project types may exist in the repository.

They should only be created when:

- the user explicitly requests them, or
- the user requests a project based on an existing project structure containing those components.

---

## Target Framework

New projects target:

net8.0

unless the user explicitly requests another framework.

---

## Project References

References should normally be established between components belonging to the same newly created project.

For example:

<ProjectName>.API
    -> <ProjectName>.Services

<ProjectName>.Services
    -> <ProjectName>.Common
    -> <ProjectName>.Repository

<ProjectName>.Repository
    -> <ProjectName>.Common

External project references are not automatically added.

---

## Solution

The server repository uses:

BNPP.NYBOSS.NextGen.Server.sln

New projects must be added to this solution.

Do not create a separate solution for an application.

---

## Existing Project Patterns

When the user explicitly requests a project similar to an existing project, inspect that project's structure and use it as the pattern.

Reuse:

- project composition
- applicable project types
- applicable project-level configuration

Do not blindly copy:

- project names
- legacy naming
- unrelated references
- unrelated packages
- business-specific implementation

The requested new project name must be applied to the generated structure.
