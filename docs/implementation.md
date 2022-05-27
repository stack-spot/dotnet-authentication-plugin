### **Inputs**
Os inputs necessários para utilizar o plugin são:  

| **Campo** | **Valor** | **Descrição** |
| :--- | :--- | :--- |
| Url | https://myauthenticationserver/token | Url do servidor de autenticação/autorização |

#### Uso

Adicione ao seu `IServiceCollection`, via `services.AddTokenAuthentication()`. no `Startup` da aplicação ou `Program`. Tenha como parâmetro de entrada a `url` do seu provedor de Autenticação e Autorização. 

```csharp
services.AddTokenAuthentication(url);
```

### Configurações Adicionais

Siga os seguintes passos para fazer configurações adicionais no plugin:  

1. Defina o `[Authorize]` para suas `Controllers` e/ou `Endpoints`. 

```csharp
[Authorize]
public class MyController : ControllerBase
{
    ...
}
```

2. Em seu arquivo `Startup` ou `Program`, garanta as configurações básicas de **ApplicationBuilder** para `app.Authorization()` e `app.Authentication()`. 

```csharp
...
app.UseRouting();

app.UseAuthentication();
app.UseAuthorization();

app.UseEndpoints();
...
```

3. Configure a `IServiceCollection` para que as Controllers tenham o filtro de Autorização.

```csharp
...
services.AddControllers(options => 
{
    options.Filters.Add(new AuthorizeFilter());

});
...
```

4. Se utilizar o Swagger em sua aplicação, configure as definições de segurança para a autenticação. Exemplo:

```csharp
...
services.AddSwaggerGen(c => 
{
    c.SwaggerDoc("v1", new OpenApiInfo { Title = "MyApi", Version = "v1"});
    c.AddSecurityDefinition("Bearer", new OpenApiSecurityScheme()
    {
        In = ParameterLocation.Header,
        Description = "Authorization Token",
        Name = "Authorization",
        Type = SecuritySchemeType.OAuth2
    });
});
...
```