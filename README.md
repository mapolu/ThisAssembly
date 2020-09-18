# ThisAssembly

[![Version](https://img.shields.io/nuget/v/ThisAssembly.svg?color=royalblue)](https://www.nuget.org/packages/ThisAssembly)
[![Downloads](https://img.shields.io/nuget/dt/ThisAssembly.svg?color=green)](https://www.nuget.org/packages/ThisAssembly)
[![License](https://img.shields.io/github/license/kzu/ThisAssembly.svg?color=blue)](https://github.com//kzu/ThisAssembly/blob/main/LICENSE)
[![Build](https://github.com/kzu/ThisAssembly/workflows/build/badge.svg?branch=main)](https://github.com/kzu/ThisAssembly/actions)


Exposes project and assembly level information as constants in the ThisAssembly 
class using source generators powered by Roslyn.

The main generated entry point type is `ThisAssembly` in the global namespace, 
and is declared as partial so you can extend it too with manually created members.

Each package in turn extends this partial class too to add their own constants.

The [ThisAssembly](https://nuget.org/packages/ThisAssembly) meta-package includes 
all the other packages for convenience.

> NOTE: as of .NET 5.0 RC1, only C# is supported for source generators
> so even if the generators here can emit proper VB and F#, no code will be generated 
> (yet) for those languages. When support is introduced for those languages,
> things will Just Work without the need to update these packages.

> NOTE: if intellisense isn't working properly, try closing and reopening 
> Visual Studio. If that doesn't work, please set the *$(IncludeSourceGeneratorIntellisenseFix)* 
> property to *true* in your project. This is a temporary workaround for a 
> [Roslyn issue](https://github.com/dotnet/roslyn/issues/44093) that should be 
> solved by the final release of .NET5.

## ThisAssembly.AssemblyInfo

[![Version](https://img.shields.io/nuget/v/ThisAssembly.AssemblyInfo.svg?color=royalblue)](https://www.nuget.org/packages/ThisAssembly.AssemblyInfo)
[![Downloads](https://img.shields.io/nuget/dt/ThisAssembly.AssemblyInfo.svg?color=green)](https://www.nuget.org/packages/ThisAssembly.AssemblyInfo)

This package generates a static `ThisAssembly.Info` class with public 
constants exposing the following attribute values generated by default for SDK style projects:

* AssemblyConfigurationAttribute
* AssemblyCompanyAttribute
* AssemblyTitleAttribute
* AssemblyProductAttribute

* AssemblyVersionAttribute
* AssemblyInformationalVersionAttribute
* AssemblyFileVersionAttribute

If your project includes these attributes by other means, they will still be emitted properly 
on the `ThisAssembly.Info` class.

![](img/ThisAssembly.AssemblyInfo.png)

## ThisAssembly.Metadata

[![Version](https://img.shields.io/nuget/v/ThisAssembly.Metadata.svg?color=royalblue)](https://www.nuget.org/packages/ThisAssembly.Metadata)
[![Downloads](https://img.shields.io/nuget/dt/ThisAssembly.Metadata.svg?color=green)](https://www.nuget.org/packages/ThisAssembly.Metadata)

This package provides a static `ThisAssembly.Metadata` class with public 
constants exposing each `[System.Reflection.AssemblyMetadata(..)]` defined for 
the project.

![](img/ThisAssembly.Metadata.png)

For an attribute declared (i.e. in *AssemblyInfo.cs*) like:

```csharp
  [assembly: System.Reflection.AssemblyMetadataAttribute("Foo", "Bar")]
```

A corresponding `ThisAssembly.Metadata.Foo` constant with the value `Bar` is provided. 
The metadata attribute can alternatively be declared using MSBuild syntax in the project 
(for .NET 5.0+ projects that have built-in support for `@(AssemblyMetadata)` items):

```xml
    <ItemGroup>
      <AssemblyMetadata Include="Foo" Value="Bar" />
    </ItemGroup>
```

Generated code:

C#:

```csharp
  partial class ThisAssembly
  {
      public static partial class Metadata
      {
          public const string Foo = "Bar";
      }
  }
```

VB:
```vbnet
  Namespace Global
    Partial Class ThisAssembly
          Partial Class Metadata
              Public Const Foo = "Bar"
          End Class
      End Class
  End Namespace
```

F#:
```fsharp
  module internal ThisAssembly

  module public Metadata =
      [<Literal>]
      let public Foo = @"Bar"
```

## ThisAssembly.Project

[![Version](https://img.shields.io/nuget/v/ThisAssembly.Project.svg?color=royalblue)](https://www.nuget.org/packages/ThisAssembly.Project)
[![Downloads](https://img.shields.io/nuget/dt/ThisAssembly.Project.svg?color=green)](https://www.nuget.org/packages/ThisAssembly.Project)

This package generates a static `ThisAssembly.Project` class with public 
constants exposing project properties that have been opted into this mechanism by adding 
them as `ThisAssemblyProject` MSBuild items in project file, such as:

```xml
  <PropertyGroup>
    <Foo>Bar</Foo>
  </PropertyGroup>
  <ItemGroup>
    <ThisAssemblyProject Include="Foo" />
  </ItemGroup>
```

![](img/ThisAssembly.Project.png)

Generated code:

C#:

```csharp
  partial class ThisAssembly
  {
      public static partial class Project
      {
          public const string Foo = "Bar";
      }
  }
```

VB:
```vbnet
  Namespace Global
    Partial Class ThisAssembly
          Partial Class Project
              Public Const Foo = "Bar"
          End Class
      End Class
  End Namespace
```

F#:
```fsharp
  module internal ThisAssembly

  module public Project =
      [<Literal>]
      let public Foo = @"Bar"
```