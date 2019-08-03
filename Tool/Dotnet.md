# .NET Core

## Install from AUR
```
packer -S dotnet-cli
```

## Install from Docker
```
docker run -it  -v /work/dotnet:/root/dotnet microsoft/dotnet:latest
```

## Usage
```
dotnet --info
dotnet new – Initializes a sample console C# project.
dotnet add package package-name
dotnet restore – Restores the dependencies for a given application.
dotnet build – Builds a .NET Core application.
dotnet publish – Publishes a .NET portable or self-contained application.
dotnet run – Runs the application from source.
dotnet test – Runs tests using a test runner specified in the project.json.
dotnet pack – Creates a NuGet package of your code.
```

.NET Core projects are folder based, by default all the source files in the same folder or sub folders are included in the project.

## Books
- [Fundamentals of Computer Programming with C#](https://www.introprogramming.info/english-intro-csharp-book/read-online/chapter-preface/)
