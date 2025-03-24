# SimpleDto Generator

SimpleDto Generator is a source generator for .NET that automatically generates Data Transfer Objects (DTOs) from your existing classes. This library simplifies the process of creating DTOs by using attributes to specify which classes and properties should be included or ignored.

## Features

- Automatically generates DTOs from your existing classes.
- Supports both class and record types.
- Allows you to specify which properties to include or ignore using attributes.
- Compatible with .NET 8 and .NET Standard 2.0.


## Usage

1. **Define your source class:**


```csharp
public class Entity
{
    public int Id { get; set; }
    public string Name { get; set; }
    public DateTime CreatedAt { get; set; }
}

```

2. **Apply the `DtoFrom` attribute to generate a DTO:**


```csharp
[DtoFrom(typeof(Entity))]
public class EntityDto
{
}

```

3. **Optionally, use the `DtoMemberIgnore` attribute to exclude properties:**


```csharp
[DtoFrom(typeof(Entity))]
[DtoMemberIgnore(nameof(Entity.Description))] // ignore the Description property
[DtoMemberIgnore(typeof(BaseEntity))] // ingore all properties that has type BaseEntity or inherited from it
public class EntityDto
{
    [DtoMemberIgnore]
    public string Name { get; set; }
}

```

## Example

Here is a complete example demonstrating how to use SimpleDto Generator:

### Source Class


```csharp
internal sealed class Entity : BaseEntity
{
    public List<string>? Strings { get; set; }
    public NestedEntity? Nested { get; set; }
    public string? Description { get; set; }
    public CustomEnum CustomEnum { get; set; }
    public CustomEnum? NullableCustomEnum { get; set; }
}

internal sealed class NestedEntity : BaseEntity
{
    public string? Description { get; set; }
}

```

### DTO Class


```csharp
[DtoFrom(typeof(Entity))]
[DtoMemberIgnore(typeof(IEnumerable<>))]
[DtoMemberIgnore(nameof(Entity.Description))]
[DtoMemberIgnore(typeof(BaseEntity))]
public sealed partial class EntityDto
{
}

```

### Generated DTO

The generator will produce the following DTO:


```csharp
public sealed partial class EntityDto
{
    public int Id {get; set;}
    public ConsoleApp.CustomEnum CustomEnum {get; set;}
    public ConsoleApp.CustomEnum? NullableCustomEnum {get; set;}
}

```
