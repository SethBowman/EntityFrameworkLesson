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

1. **Install Entity Framework:**
   - Right-click on your project in Visual Studio.
   - Select "Manage NuGet Packages."
   - Search for "Microsoft.EntityFrameworkCore" and install the package.

```csharp
// Install Entity Framework
// Right-click on your project in Visual Studio.
// Select "Manage NuGet Packages."
// Search for "Microsoft.EntityFrameworkCore" and install the package.
using Microsoft.EntityFrameworkCore;
```

2. **Create a DbContext Class:**
   - In your project, create a class that inherits from `DbContext`. This class represents your database session.
   - Configure the database connection in the `OnConfiguring` method.

```csharp
// Create a DbContext Class
// This class represents a session with the database
public class MyDbContext : DbContext
{
    // DbSet properties represent database tables
    public DbSet<User> Users { get; set; }

    // This method is called to configure the database connection
    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        // Configure the database connection string here
        optionsBuilder.UseSqlServer("your_connection_string");
    }
}
```

### Performing CRUD Operations:

3. **Define Entity Classes:**
   - Create classes that represent your entities. These classes will be used to interact with the database.

```csharp
// Define Entity Classes
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

4. **Querying Data:**
   - Use LINQ to query data from the database.

```csharp
// Querying Data
// Create a new DbContext instance
using (var context = new MyDbContext())
{
    // Use LINQ to query data from the "Users" table
    var users = context.Users.Where(u => u.FirstName == "John").ToList();
}
```

5. **Adding and Updating Data:**
   - Add new entities or update existing ones and persist the changes to the database.

```csharp
// Adding and Updating Data
// Create a new DbContext instance
using (var context = new MyDbContext())
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

6. **Deleting Data:**
   - Remove entities from the database.

```csharp
// Deleting Data
// Create a new DbContext instance
using (var context = new MyDbContext())
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
