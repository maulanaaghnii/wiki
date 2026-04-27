# Modular Monolith

## Dotnet
Create solution
```bash
dotnet new sln -n MyApp
```

Create projects
```bash
dotnet new webapi -n MyApp.Api -f net8.0
```

Add projects to solution
```bash
dotnet sln add MyApp.Api/MyApp.Api.csproj
```

add module user
```
mkdir Modules/User
cd Modules/User
dotnet new classlib -n User.Domain -f net8.0
dotnet new classlib -n User.Application -f net8.0
dotnet new classlib -n User.Infrastructure -f net8.0
dotnet new classlib -n User.Persistence -f net8.0

cd ../..
dotnet sln add .\Modules\User\User.Application\User.Application.csproj
dotnet sln add .\Modules\User\User.Domain\User.Domain.csproj
dotnet sln add .\Modules\User\User.Infrastructure\User.Infrastructure.csproj
dotnet sln add .\Modules\User\User.Persistence\User.Persistence.csproj
```


add DependencyInjection.cs in application layer


add reference to application layer from api layer
```bash
dotnet add MyApp.Api reference .\Modules\Users\Users.Application\Users.Application.csproj
dotnet add Modules/Users/Users.Application reference .\Modules\Users\Users.Domain\Users.Domain.csproj
dotnet add Modules/Users/Users.Persistence reference .\Modules\Users\Users.Application\Users.Application.csproj
dotnet add Modules/Users/Users.Persistence reference .\Modules\Users\Users.Domain\Users.Domain.csproj
```

add package to application layer
```bash
dotnet add Modules/User/User.Application package Microsoft.Extensions.DependencyInjection.Abstractions --version 8.0.1
```

migration
```bash
dotnet ef migrations add Init --project Modules/Users/Users.Persistence --startup-project MyApp.Api
dotnet ef database update --project Modules/Users/Users.Persistence --startup-project MyApp.Api
```
