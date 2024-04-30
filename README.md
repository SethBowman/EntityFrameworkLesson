# Lesson: Introduction to C# Entity Framework in Visual Studio Code

## What is Entity Framework?

Entity Framework (EF) is an Object-Relational Mapping (ORM) framework developed by Microsoft for .NET applications. It simplifies the interaction between your application and the database by providing a set of tools and libraries for database access. EF enables developers to work with databases using C# classes, making it easier to manage data and perform database operations.

### Key Concepts in Entity Framework:

- **Entities:** Represent the data model and are typically mapped to database tables.
- **DbContext:** Represents a session with the database and allows you to query and save data.
- **Mappings:** Define the relationship between entities and database tables.

## Why Use Entity Framework?

Entity Framework provides various benefits, including abstraction of database logic, rapid development, cross-database compatibility, and change tracking. These features allow you to focus on business logic without worrying about complex database operations.

## Prerequisites:

Before getting started with Entity Framework in Visual Studio Code, make sure you have the following installed:

- **.NET SDK:**

  - Install the .NET SDK from [dotnet.microsoft.com](https://dotnet.microsoft.com/download).

- **Visual Studio Code:**
  - Download and install Visual Studio Code from [code.visualstudio.com](https://code.visualstudio.com/).
- **Visual Studio Code Extensions:**

  - Open Visual Studio Code.
  - Go to the Extensions view by clicking on the square icon on the sidebar or pressing Ctrl+Shift+X.
  - Install the following extensions:
    - `.NET Install Tool for Visual Studio Code`: Provides .NET SDK installation capabilities.
    - `C# for Visual Studio Code (powered by OmniSharp)`: Adds C# support.
    - `C# Dev Kit`: Additional features for C# development.
    - `Visual NuGet`: Allows you to manage NuGet packages.

- **MySQL Server and Workbench:**
  - Install MySQL Server from [MySQL Downloads](https://dev.mysql.com/downloads/mysql/). Choose the appropriate version for your system.
  - Install MySQL Workbench from [MySQL Workbench Downloads](https://dev.mysql.com/downloads/workbench/).

## Setting Up Entity Framework:

1. **Install .NET EF Tool:**

   - Open Visual Studio Code.
   - Click on the Extensions view by clicking on the square icon on the sidebar or pressing Ctrl+Shift+X.
   - Search for ".NET Core SDK" in the Extensions view search box.
   - Look for ".NET Core SDK" by Microsoft and click Install.

2. **Create a .NET Console Application:**

   - Create a new directory for your project.
   - Open Visual Studio Code and navigate to the directory.
   - Press Ctrl+Shift+P to open the Command Palette.
   - Type "Create New Project" and select ".NET: Create New Project".
   - Choose "Console Application" and press Enter.
   - Enter a project name and select a location to save the project.

3. **Install Entity Framework Core Packages:**
   - Open Visual Studio Code.
   - Click on the Extensions view by clicking on the square icon on the sidebar or pressing Ctrl+Shift+X.
   - Search for "Visual NuGet" in the Extensions view search box and install it.
   - Use Visual NuGet to install the following NuGet packages:
     - `Microsoft.EntityFrameworkCore`: The main Entity Framework Core package.
     - `Pomelo.EntityFrameworkCore.MySql`: Allows Entity Framework to interact with MySQL.
     - `Microsoft.EntityFrameworkCore.Tools`: Provides additional tools for working with Entity Framework in .NET.

## Creating the DbContext Class:

- **Create DbContext Class (Testdb.cs):**

```csharp
using System.IO;
using Microsoft.EntityFrameworkCore;
using Microsoft.Extensions.Configuration;

// This class represents a session with the database
public class Testdb : DbContext
{
    // Represents a table in the database
    public DbSet<User> Users { get; set; }

    // Configures the database connection
    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        // Load connection string from appsettings.json
        IConfigurationRoot configuration =
            new ConfigurationBuilder()
                .SetBasePath(Directory.GetCurrentDirectory())
                .AddJsonFile("appsettings.json")
                .Build();

        // Configure the database connection for MySQL
        optionsBuilder.UseMySQL(configuration.GetConnectionString("DefaultConnection"));
    }
}
```

## User Model (User.cs):

```csharp
using System;

// This class represents an entity in the "Users" table
public class User
{
    // Represents the primary key in the "Users" table
    public int Id { get; set; }

    // Represents columns in the "Users" table
    public string FirstName { get; set; }
    public string Last Name { get; set;
}
```

## Appsettings.json File:

- Create an `appsettings.json` file in your project with the following content:

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=localhost;Database=Testdb;User=root;Password=your_password"
  }
}
```

**Ensure `appsettings.json` is copied to the output directory if it changes:**

- Open the `.csproj` file in your Visual Studio Code project.
- Add an `ItemGroup` section with the following content:

```xml
<ItemGroup>
  <None Update="appsettings.json">
    <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
  </None>
</ItemGroup>
```

This XML snippet in your project file ensures that `appsettings.json` is copied to the output directory only if it's newer than the existing copy.

## .gitignore File:

- **Create .gitignore File:**
  - Create a `.gitignore` file in your project and exclude `appsettings.json` under "user specific files."

```plaintext
# Create .gitignore File
# This file tells Git which files or folders to ignore when pushing to a repository
# This prevents sensitive files, like appsettings.json, from being uploaded to the repository

# Build and object files
bin/
obj/

# .NET user-specific files
*.user
*.userosscache
*.suo
*.sln.docstates

# User-specific files
appsettings.json
```

## Creating the Database:

- **Create Database from the Visual Studio Code Terminal:**
  - Open Visual Studio Code.
  - Press Ctrl+` to open the integrated terminal.
  - Navigate to the project directory.
  - Run the following commands:
    ```bash
    dotnet ef migrations add InitialCreate
    dotnet ef database update
    ```
    This will generate a migration file based on your model and apply the migration to create the database.

## Performing CRUD Operations:

- **Querying Data:**

  - Use LINQ to query data from the database.

- **Adding and Updating Data:**

  - Add new entities or update existing ones and persist the changes to the database.

- **Deleting Data:**
  - Remove entities from the database.

## Exercise Problems:

1. **Exercise 1:**

   - Create a new console application and set up a `DbContext` class representing a simple "Product" entity with properties like Id, Name, and Price. Perform the following actions:
     - Add three products to the database.
     - Query and display the products with a price higher than a specified value.

2. **Exercise 2:**

   - Extend the previous application to update the price of one of the products. After updating, query and display all products in the database.

3. **Exercise 3:**
   - Implement a deletion operation in the application. Choose one of the products and delete it from the database. Query and display the remaining products.
