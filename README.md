# ASP.NET
Comandos e configurações básicas para a criação de projetos em ASP.NET

### Projeto ASP.NET
1. Criar Diretório da solução
```powershell
mkdir MeuApp
```

2. Navegar para o diretório
```powershell
cd .\MeuApp\
```

3. Criar arquivo de solução
```powershell
dotnet new sln -n MeuApp
```

4. Criar diretório `src`
```powershell
mkdir src
```

5. Navegar ao diretório `src`
```powershell
cd .\src\
```

6. Criar projeto - neste caso escolhemos um app mvc
```powershell
dotnet new mvc -n MeuWebApp`
```

7. Navegar para o diretório do projeto
```powershell
cd .\MeuWebApp\
```

8. Vincular projeto à solução
```powershell
dotnet sln ..\..\MeuApp.sln add .\MeuWebApp.csproj
```

### Instalar EntityFramework para SQL Server
```powershell
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
dotnet tool install --global dotnet-ef
dotnet add package Microsoft.EntityFrameworkCore.Tools
dotnet add package Microsoft.EntityFrameworkCore.Design
```

### Migrations
```powershell
dotnet ef migrations add initial
dotnet ef database update
```

### Publicação
```powershell
dotnet publish -c Release
```

### Annotations
```csharp
DataAnnotations && DataAnnotations.Schema || IEntityTypeConfiguration
```

### Authentication & Authorization:
- Adicionando dependências
```powershell
dotnet add package Microsoft.AspNetCore.Authentication
dotnet add package Microsoft.AspNetCore.Authentication.JwtBearer
```

- Configuração

Criar Settings.cs (class static) com atributo static secret: "VALOR".

```csharp
//Configure Services (depois do AddControllers):
var key = Encoding.ASCII.GetBytes(Settings.Secret);
    services.AddAuthentication(x =>
            {
                x.DefaultAuthenticateScheme = JwtBearerDefaults.AuthenticationScheme;
                x.DefaultChallengeScheme = JwtBearerDefaults.AuthenticationScheme;
            })
            .AddJwtBearer(x =>
            {
                x.RequireHttpsMetadata = false;
                x.SaveToken = true;
                x.TokenValidationParameters = new TokenValidationParameters
                {
                    ValidateIssuerSigningKey = true,
                    IssuerSigningKey = new SymmetricSecurityKey(key),
                    ValidateIssuer = false,
                    ValidateAudience = false
                };
            });

//Configure pipeline:
	app.UseAuthentication();
	app.UseAuthorization();
```
- Para Autenticar: No Header do Request >> `Authorization: Bearer VALOR_TOKEN`



### Outros utilitários CLI .NET 
- Listar todas as versões instaladas do .NET
```powershell
dotnet --info
```

- Listar SDKs instalados
```powershell
dotnet --list-sdks
```

- Listar Runtimes instalados
```powershell
dotnet --list-runtimes
```

- Criar projeto com versão específica do .NET
```powershell
dotnet new web -o MyWebApp -f netX.X
```

- Criar arquivo `global.json` na raiz da pasta de projetos para rodar todos projetos com a versão uma única versão
```powershell
{
	"projects": ["src"],
	"sdk": {
		"version": "^6.0.201"
	}
}
```

- Migrations passando o contexto como parâmetro
```powershell
dotnet ef migrations add MYCOMMENT --context MYCONTEXT
dotnet ef database update --context MYCONTEXT
```

- Habiliar certificados de dev
```powershell
dotnet dev-certs https --check --trust
```
