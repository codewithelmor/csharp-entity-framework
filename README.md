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

## Code-First Approach

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

## Database-First Approach

Entity Framework Database First is an approach in Entity Framework where you start with an existing database, and then generate the entity classes and DbContext based on that database. This is in contrast to Code First, where you start by defining your entity classes in code, and the database schema is generated from those classes.

Here are the key steps involved in using Entity Framework Database First:

1. **`Create a New EDMX File`**:

* In Visual Studio, you can add a new ADO.NET Entity Data Model (EDMX) file to your project. This file will be used to define the conceptual model, which represents the entities in your database.

2. **`Generate from Database`**:

* In the EDMX designer, right-click and choose "Update Model from Database." This opens a wizard that allows you to connect to your existing database and select the tables/views/stored procedures you want to include in your model.

3. **`Configure Entities`**:

* Once the wizard completes, the EDMX file will contain entity classes that correspond to the tables in your database. You can now customize these entities by adding validation, additional properties, or navigation properties.

4. **`Generate DbContext`**:

* After configuring your entities, you can use the "Add Code Generation Item" feature to generate a DbContext class along with other necessary classes for your model.

5. **`Use the DbContext in Your Application`**:

* Instantiate your generated DbContext in your application and use it to query and manipulate data. You can use LINQ queries and other features of Entity Framework to interact with the database.

```csharp
using (var context = new YourGeneratedDbContext())
{
    var customers = context.Customers.ToList();
    // Perform other operations...
}
```

6. **`Updating Model from Database Changes`**:

* If your database schema changes over time, you can update your model to reflect these changes. Right-click on the EDMX file and select "Update Model from Database" to bring in the changes.

7. **`Database First and Code First Hybrid`**:

* In some scenarios, you might use a combination of Database First and Code First. For example, you can generate the initial model from the database and then use Code First migrations to manage changes over time.

Entity Framework Database First is useful when you have an existing database and want to quickly generate a model to work with in your application. It can be particularly beneficial in scenarios where the database schema is maintained separately from the application code or where you are working with a legacy database.

### Scaffolding

Entity Framework Database First Scaffolding is a feature that allows you to generate entity classes and a DbContext based on an existing database using the command-line interface (CLI) or Package Manager Console (PMC) in Visual Studio. This is a quick and efficient way to create a data model for an application based on an existing database schema.

Here are the steps to perform Database First Scaffolding:

**Using Package Manager Console (PMC)**:

1. **`Install Entity Framework Tools`**:

* Before using scaffolding, ensure that you have the Entity Framework Tools installed. You can install it using the following PMC command:

```bash
Install-Package Microsoft.EntityFrameworkCore.Tools
```

2. **`Scaffold DbContext and Entities`**:

* Use the **`Scaffold-DbContext`** command to generate a DbContext and entity classes based on your existing database.

```bash
Scaffold-DbContext "YourConnectionString" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

* Replace **`"YourConnectionString"`** with the connection string to your database and adjust the provider (**`Microsoft.EntityFrameworkCore.SqlServer`** in the example) based on your database provider.
* The **`-OutputDir`** parameter specifies the directory where the generated files will be placed.

**Using .NET CLI**:

1. **`Install Entity Framework Tools`**:

* Ensure that you have the Entity Framework Tools installed. You can install it using the following .NET CLI command:

```bash
dotnet tool install --global dotnet-ef
```

2. **`Scaffold DbContext and Entities`**:

* Use the **`dotnet ef dbcontext scaffold`** command to generate a DbContext and entity classes based on your existing database.

```bash
dotnet ef dbcontext scaffold "YourConnectionString" Microsoft.EntityFrameworkCore.SqlServer -o Models
```

* Replace **`"YourConnectionString"`** with the connection string to your database and adjust the provider (**`Microsoft.EntityFrameworkCore.SqlServer`** in the example) based on your database provider.
* The **`-o`** or **`--output`** option specifies the directory where the generated files will be placed.

After running the scaffolding command, Entity Framework will generate the necessary files, including the DbContext class and entity classes, in the specified directory. You can then use these generated classes in your application to interact with the database.

Remember to review and customize the generated code as needed, especially if you want to add data annotations, customize relationships, or make other adjustments based on your application's requirements.
