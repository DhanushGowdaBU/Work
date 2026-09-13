# Similar Project

If the user requests:

"Create <ProjectName> similar to <ExistingProject>"

the existing project is a structural reference only.

Inspect the existing project at the directory/tree level.

Determine:

- project names
- project types
- project directories
- folder names
- file names
- project-to-project relationships

Do not read existing source files to reproduce their implementation.

Do not copy file contents.

Do not copy business logic.

Do not copy configuration values.

Do not copy secrets or connection strings.

Do not copy application-specific settings.

---

# Similar Project Structure

For every applicable existing project:

1. Create the equivalent new project.
2. Apply the new naming convention:
   <ProjectName>.<Component>
3. Create the equivalent folders.
4. Create the equivalent file names.
5. Create the applicable .csproj.
6. Recreate applicable internal project references using the new project names.
7. Add all generated projects to the existing solution.

For example, if the structural reference contains:

<ExistingComponent>/
├── FolderA/
├── FolderB/
├── FolderC/
├── FileA.cs
└── FileB.cs

the generated project should contain the equivalent structure:

<ProjectName>.<Component>/
├── FolderA/
├── FolderB/
├── FolderC/
├── FileA.cs
└── FileB.cs

The file names and folder hierarchy are reproduced.

The file contents are NOT reproduced at this stage.

---

# File Content Boundary

Creating a file means creating the file itself.

It does not mean copying the contents of the corresponding file from an existing project.

For example:

If the reference project contains:

Controllers/SomeController.cs

create the corresponding controller file in the new project, but do not copy the controller implementation.

If the reference project contains:

appsettings.json

create the required file, but do not copy the reference application's configuration values.

If the reference project contains:

RegisterServices.cs

create the file structurally, but do not copy the reference implementation.

File implementation will be handled separately.
