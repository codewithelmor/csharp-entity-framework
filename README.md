# Entity Framework (EF)

**`Entity Framework (EF)`** is an object-relational mapping (ORM) framework developed by Microsoft for .NET applications. It enables developers to work with relational databases using .NET objects and eliminates the need for much of the data-access code that developers would normally need to write.

Here are some key features and concepts associated with Entity Framework:

1. **`Object-Relational Mapping (ORM)`**: Entity Framework allows developers to work with databases using .NET objects, which are mapped to database tables. This mapping is handled by EF, and developers can interact with the database using familiar object-oriented programming concepts.

2. **`Entities`**: In Entity Framework, entities are the .NET objects that represent the data stored in a database. Each entity typically corresponds to a table in the database, and properties of the entity correspond to columns in the table.

3. **`DbContext`**: The DbContext class is a crucial part of Entity Framework. It represents a session with the database and is used to query and save data. Developers create a **`DbContext`** class for their application, which includes DbSet properties representing the entities they want to query and manipulate.

4. **`LINQ (Language Integrated Query)`**: Entity Framework supports LINQ, a feature in C# and VB.NET that allows developers to query data using a syntax that is similar to SQL. LINQ queries are translated by EF into SQL queries and executed against the database.

5. **`Code-First and Database-First Approaches`**: Entity Framework supports both code-first and database-first development approaches. In the code-first approach, developers define the domain model in code, and EF generates the database schema. In the database-first approach, developers start with an existing database, and EF generates the corresponding .NET classes.

6. **`Migrations`**: Entity Framework Migrations enable developers to evolve the database schema over time. Migrations are used to propagate changes to the database schema based on changes to the code.

7. **`Lazy Loading and Eager Loading`**: Entity Framework supports lazy loading, which means that related entities are loaded from the database only when accessed. It also supports eager loading, where developers can specify in advance which related entities should be loaded with the main entity.

8. **`Database Providers`**: Entity Framework is designed to work with various database providers, including SQL Server, MySQL, PostgreSQL, and SQLite, among others. The choice of provider depends on the specific database system used by the application.

Overall, Entity Framework simplifies data access and helps developers focus more on the application's business logic rather than dealing with low-level database interactions.

## Code First

Entity Framework Code First is an approach in Entity Framework where you define your domain model using plain old C# objects (POCOs) and then generate the database from that model. It allows you to create a database without having to design it in a visual designer or write SQL scripts. The database schema is generated based on the classes you define in your code.

Here are the key steps involved in using Entity Framework Code First:

1. **`Define POCO Classes`**:

* Create C# classes to represent your domain model. These classes will typically include properties that correspond to the columns in your database tables.
* You can use data annotations or the Fluent API to configure aspects of your model, such as specifying primary keys, foreign keys, and relationships between entities.

```csharp
public class Author
{
    public int AuthorId { get; set; }
    public string Name { get; set; }

    public ICollection<Book> Books { get; set; }
}

public class Book
{
    public int BookId { get; set; }
    public string Title { get; set; }

    public int AuthorId { get; set; }
    public Author Author { get; set; }
}
```

2. **`Create a DbContext`**:

* Create a class that inherits from **`DbContext`**, which is a part of Entity Framework.
* In the **`DbContext`** class, define **`DbSet`** properties for each entity you want to include in the database.

```csharp
public class LibraryContext : DbContext
{
    public DbSet<Author> Authors { get; set; }
    public DbSet<Book> Books { get; set; }
}
```

3. **`Connection String`**:

* Specify a connection string in your application configuration that points to your database. Entity Framework will use this connection string to connect to the database.

```xml
<connectionStrings>
    <add name="LibraryContext" 
         connectionString="Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Library;Integrated Security=True;" 
         providerName="System.Data.SqlClient" />
</connectionStrings>
```

4. **`Database Initialization`**:

* You can specify how Entity Framework initializes the database. This can include strategies such as dropping and recreating the database, creating it only if it doesn't exist, or using migrations for more controlled updates.

```csharp
Database.SetInitializer(new DropCreateDatabaseIfModelChanges<LibraryContext>());
```

5. **`Use the DbContext in Your Application`**:

* Instantiate your **`DbContext`** in your application and use it to interact with the database.

```csharp
using (var context = new LibraryContext())
{
    var author = new Author { Name = "John Doe" };
    var book = new Book { Title = "Entity Framework Basics", Author = author };

    context.Authors.Add(author);
    context.Books.Add(book);

    context.SaveChanges();
}
```

6. **`Generate Database`**:

* When you run your application, Entity Framework will create the database based on your model and configuration. You don't have to write SQL scripts or manually create the database.

Entity Framework Code First is a powerful approach that allows developers to focus on their domain model and application logic, while Entity Framework takes care of the database creation and interaction. It's particularly useful in scenarios where the database schema is evolving along with the application development.

### Commands

Entity Framework Code First uses a set of commands to perform various tasks related to database initialization, migrations, and interactions. These commands can be executed in the Package Manager Console within Visual Studio or using the .NET CLI. Here are some common 

Entity Framework Code First commands:

1. **`Enable Migrations`**:

* This command enables migrations for your project, allowing you to create and apply database schema changes.

```bash
PM> Enable-Migrations
```

2. **`Add Migration`**:

* This command creates a new migration file that contains the changes to the database schema based on your model changes.

```bash
PM> Add-Migration [MigrationName]
```

3. **`Update Database`**:

* This command applies pending migrations to the database, updating the schema to match the current state of your model.

```bash
PM> Update-Database
```

* To update to a specific migration, you can use:

```bash
PM> Update-Database -TargetMigration [MigrationName]
```

4. **`View Migrations`**:

* This command shows a list of migrations that have been applied and those that are pending.

```bash
PM> Get-Migrations
```

5. **`Remove Migration`**:

* This command removes the last applied migration. Use with caution, as it can lead to data loss.

```bash
PM> Remove-Migration
```

6. **`Script Migrations`**:

* This command generates a SQL script for a migration without applying it to the database.

```bash
PM> Script-Migration -From [PreviousMigration] -To [TargetMigration]
```

7. **`Seed Database`**:

* This command is used to seed the database with initial or test data.

```bash
PM> Update-Database -TargetMigration: $InitialDatabase
```

8. **`Drop Database`**:

* This command drops the database, and it's useful when you want to start fresh.

```bash
PM> Update-Database -TargetMigration: 0 -Force
```

9. **`Recreate Database`**:

* This command drops and then recreates the database, applying all migrations.

```bash
PM> Update-Database -TargetMigration: 0 -Force -Script
```

10. **`Generate SQL Script for Migration`**:

* This command generates a SQL script for a specific migration without applying it.

```bash
PM> Script-Migration -From [PreviousMigration] -To [TargetMigration]
```

11. **`Generate SQL Script for Update-Database`**:

* This command generates a SQL script for applying pending migrations without actually updating the database.

```bash
PM> Update-Database -Script
```

These commands provide a way to manage the database schema in Entity Framework Code First, whether you are working with an initial database creation or evolving the schema over time through migrations.
