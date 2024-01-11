# Lesson: Introduction to C# Entity Framework

## What is Entity Framework?

Entity Framework (EF) is an Object-Relational Mapping (ORM) framework developed by Microsoft for .NET applications. It simplifies the interaction between your application and the database by providing a set of tools and libraries for database access. EF enables developers to work with databases using C# classes, making it easier to manage data and perform database operations.

### Key Concepts in Entity Framework:

1. **Entities:** Represent the data model and are typically mapped to database tables.
2. **DbContext:** Represents a session with the database and allows you to query and save data.
3. **Mappings:** Define the relationship between entities and database tables.

## Why Use Entity Framework?

### 1. Abstraction of Database Logic:

- Entity Framework abstracts away the complexity of database interactions, allowing developers to focus on business logic rather than database operations.
- It eliminates the need to write raw SQL queries, making the code more readable and maintainable.

### 2. Rapid Development:

- EF enables rapid development by providing a code-first approach where you define your data model using C# classes, and the database schema is generated automatically.

### 3. Cross-Database Compatibility:

- Entity Framework supports various database systems, making it easy to switch between databases without changing your application code.

### 4. Change Tracking:

- EF tracks changes to entities, and with a single call, you can persist those changes to the database. This reduces the amount of boilerplate code required for data manipulation.

## How to Use Entity Framework

### Setting Up Entity Framework:

1. **Install Entity Framework Core Tools:**
   - Right-click on your project in Visual Studio.
   - Select "Manage NuGet Packages."
   - Install the following packages:
     - `Microsoft.EntityFrameworkCore.Design`
     - `Microsoft.EntityFrameworkCore.Tools`
     - `Microsoft.EntityFrameworkCore.SqlServer`
     - `Microsoft.Extensions.Configuration.Json`

```csharp
// Step 1: Install Entity Framework Core Tools
// Right-click on your project in Visual Studio.
// Select "Manage NuGet Packages."
// Install the following packages:
// - Microsoft.EntityFrameworkCore.Design
// - Microsoft.EntityFrameworkCore.Tools
// - Microsoft.EntityFrameworkCore.SqlServer
// - Microsoft.Extensions.Configuration.Json
```

2. **Create a DbContext Class:**
   - In your project, create a class that inherits from `DbContext`. This class represents your database session.
   - Configure the database connection in the `OnConfiguring` method.

```csharp
// Step 2: Create a DbContext Class
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

### User Model:

3. **Create User Model:**
   - Define a class that represents the "Users" entity.

```csharp
// Step 3: Create User Model
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

### Appsettings.json File:

Create an `appsettings.json` file in your project with the following content:

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Data Source=localhost;Initial Catalog=Testdb;Integrated Security=True;Encrypt=False"
  }
}
```

### .gitignore File:

4. **Create .gitignore File:**
   - Create a `.gitignore` file in your project and exclude `appsettings.json` under "user specific files."

```plaintext
# .gitignore
# ...

# User-specific files
appsettings.json
```

### Creating the Database:

5. **Create Database from the IDE:**
   - Open the Package Manager Console.
   - Run the following commands:
     ```bash
     Add-Migration InitialCreate
     Update-Database
     ```
     This will generate a migration file based on your model and apply the migration to create the database.

### Performing CRUD Operations:

6. **Querying Data:**
   - Use LINQ to query data from the database.

```csharp
// Step 6: Querying Data
// Create a new DbContext instance
using (var context = new Testdb())
{
    // Use LINQ to query data from the "Users" table
    var users = context.Users.Where(u => u.FirstName == "John").ToList();
}
```

7. **Adding and Updating Data:**
   - Add new entities or update existing ones and persist the changes to the database.

```csharp
// Step 7: Adding and Updating Data
// Create a new DbContext instance
using (var context = new Testdb())
{
    // Add a new user to the "Users" table
    var newUser = new User { FirstName = "Jane", LastName = "Doe" };
    context.Users.Add(newUser);

    // Update an existing user in the "Users" table
    var existingUser = context.Users.Find(1);
    existingUser.LastName = "UpdatedLastName";

    // Save changes to the database
    context.SaveChanges();
}
```

8. **Deleting Data:**
   - Remove entities from the database.

```csharp
// Step 8: Deleting Data
// Create a new DbContext instance
using (var context = new Testdb())
{
    // Find the user with Id = 2 in the "Users" table
    var userToDelete = context.Users.Find(2);

    // Remove the user from the "Users" table
    context.Users.Remove(userToDelete);

    // Save changes to the database
    context.SaveChanges();
}
```

## Exercise Problems:

### Exercise 1:

Create a new console application and set up a `DbContext` class representing a simple "Product" entity with properties like Id, Name, and Price. Perform the following actions:

- Add three products to the database.
- Query and display the products with a price higher than a specified value.

### Exercise 2:

Extend the previous application to update the price of one of the products. After updating, query and display all products in the database.

### Exercise 3:

Implement a deletion operation in the application. Choose one of the products and delete it from the database. Query and display the remaining products.

Feel free to explore additional features of Entity Framework to enhance your understanding.
