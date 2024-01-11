# Lesson: Introduction to C# Entity Framework

## What is Entity Framework?

Entity Framework (EF) is an Object-Relational Mapping (ORM) framework developed by Microsoft for .NET applications. It simplifies the interaction between your application and the database by providing a set of tools and libraries for database access. EF enables developers to work with databases using C# classes, making it easier to manage data and perform database operations.

### Key Concepts in Entity Framework:

- **Entities:** Represent the data model and are typically mapped to database tables.
- **DbContext:** Represents a session with the database and allows you to query and save data.
- **Mappings:** Define the relationship between entities and database tables.

## Prerequisites:

Before getting started with Entity Framework, make sure you have the following installed:

- **Microsoft SQL Server:**

  - Install the free Developer Edition from [Microsoft SQL Server Downloads](https://www.microsoft.com/en-us/sql-server/sql-server-downloads).

- **SQL Server Management Studio (SSMS):**
  - Download and install SSMS from [SQL Server Management Studio](https://learn.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-ver16).
  - Open SSMS and connect to the server by typing "localhost" as the server.

## Why Use Entity Framework?

**Abstraction of Database Logic:**

- Entity Framework abstracts away the complexity of database interactions, allowing developers to focus on business logic rather than database operations.
- It eliminates the need to write raw SQL queries, making the code more readable and maintainable.

**Rapid Development:**

- EF enables rapid development by providing a code-first approach where you define your data model using C# classes, and the database schema is generated automatically.

**Cross-Database Compatibility:**

- Entity Framework supports various database systems, making it easy to switch between databases without changing your application code.

**Change Tracking:**

- EF tracks changes to entities, and with a single call, you can persist those changes to the database. This reduces the amount of boilerplate code required for data manipulation.

## How to Use Entity Framework in a Console Application

**Setting Up Entity Framework:**

- **Install Entity Framework Core Tools:**
  - Right-click on your project in Visual Studio.
  - Select "Manage NuGet Packages."
  - Install the following packages:
    - `Microsoft.EntityFrameworkCore.Design`
    - `Microsoft.EntityFrameworkCore.Tools`
    - `Microsoft.EntityFrameworkCore.SqlServer`
    - `Microsoft.Extensions.Configuration.Json`

```csharp
// Install Entity Framework Core Tools
// Right-click on your project in Visual Studio.
// Select "Manage NuGet Packages."
// Install the following packages:
// - Microsoft.EntityFrameworkCore.Design
// - Microsoft.EntityFrameworkCore.Tools
// - Microsoft.EntityFrameworkCore.SqlServer
// - Microsoft.Extensions.Configuration.Json
```

**User Model:**

- **Create User Model:**
  - Define a class that represents the "Users" entity.

```csharp
// Create User Model
// This class represents an entity in the "Users" table
public class User
{
    // This property represents the primary key in the "Users" table
    public int Id { get; set; }

    // These properties represent columns in the "Users" table
    public string FirstName { get; set; }
    public string LastName { get; set; }
}
```

- **Create a DbContext Class:**
  - In your project, create a class that inherits from `DbContext`. This class represents your database session.
  - Configure the database connection in the `OnConfiguring` method.

```csharp
// Create a DbContext Class
// This class represents a session with the database
public class Testdb : DbContext
{
    // DbSet properties represent database tables
    public DbSet<User> Users { get; set; }

    // This method is called to configure the database connection
    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        // Load connection string from appsettings.json
        IConfigurationRoot configuration = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("appsettings.json")
            .Build();

        // Configure the database connection string here
        optionsBuilder.UseSqlServer(configuration.GetConnectionString("DefaultConnection"));
    }
}
```

**Appsettings.json File:**

- Create an `appsettings.json` file in your project with the following content:

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Data Source=localhost;Initial Catalog=Testdb;Integrated Security=True;Encrypt=False"
  }
}
```

**.gitignore File:**

- **Create .gitignore File:**
  - Create a `.gitignore` file in your project and exclude `appsettings.json` under "user specific files."

```plaintext
# Create .gitignore File
# Create a .gitignore file in your project and exclude appsettings.json under "user specific files."
# ...

# User-specific files
appsettings.json
```

**Creating the Database:**

- **Create Database from the IDE:**
  - Open the Package Manager Console. (Tools > NuGet Package Manager > Package Manager Console)
  - Run the following commands:
    ```bash
    Add-Migration InitialCreate
    Update-Database
    ```
    This will generate a migration file based on your model and apply the migration to create the database.

**Performing CRUD Operations:**

- **Adding and Updating Data:**
  - Add new entities or update existing ones and persist the changes to the database.

```csharp
// Adding and Updating Data
// Create a new DbContext instance
using (var context = new Testdb())
{
    // Add a new user to the "Users" table
    var newUser = new User { FirstName = "Jane", LastName = "Doe" };
    context.Users.Add(newUser);

    // Update an existing user in the "Users" table (if one existed)
    var existingUser = context.Users.Find(1); //Using the Id
    existingUser.LastName = "UpdatedLastName";

    // Save changes to the database
    context.SaveChanges();
}
```

- **Querying Data:**
  - Use LINQ to query data from the database.

```csharp
// Querying Data
// Create a new DbContext instance
using (var context = new Testdb())
{
    // Use LINQ to query data from the "Users" table
    var users = context.Users.Where(u => u.FirstName == "Jane").ToList();
}
```

- **Deleting Data:**
  - Remove entities from the database.

```csharp
// Deleting Data
// Create a new DbContext instance
using (var context = new Testdb())
{
    // Find the user with Id = 1 in the "Users" table
    var userToDelete = context.Users.Find(1);

    // Remove the user from the "Users" table
    context.Users.Remove(userToDelete);

    // Save changes to the database
    context.SaveChanges();
}
```

**Exercise Problems:**

- **Exercise 1:**

  - Create a new console application and set up a `DbContext` class representing a simple "Product" entity with properties like Id, Name, and Price. Perform the following actions:
    - Add three products to the database.
    - Query and display the products with a price higher than a specified value.

- \*\*Exercise

2:\*\*

- Extend the previous application to update the price of one of the products. After updating, query and display all products in the database.

- **Exercise 3:**
  - Implement a deletion operation in the application. Choose one of the products and delete it from the database. Query and display the remaining products.

Feel free to explore additional features of Entity Framework to enhance your understanding.
