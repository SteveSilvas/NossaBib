# Arquitetura do NossaBib

## Visão geral

NossaBib é uma aplicação web ASP.NET Core construída sobre o modelo de
Razor Pages. A solução é composta por um único projeto de aplicação que
hospeda a camada web, o pipeline HTTP e as páginas renderizadas no servidor.

## Estrutura da solução

```
NossaBib.sln                     Solução que agrega os projetos
NossaBib/
  NossaBib.csproj                Projeto da aplicação web (Microsoft.NET.Sdk.Web)
  Program.cs                     Ponto de entrada e construção do host
  Startup.cs                     Configuração de serviços e pipeline HTTP
  appsettings.json               Configuração base
  appsettings.Development.json   Sobrescritas do ambiente de desenvolvimento
  Properties/
    launchSettings.json          Perfis de execução local
  Pages/
    Index.cshtml(.cs)            Página inicial
    Privacy.cshtml(.cs)          Política de privacidade
    Error.cshtml(.cs)            Tratamento de erros
    _ViewImports.cshtml          Imports compartilhados das views
    _ViewStart.cshtml            Layout padrão das views
    Shared/
      _Layout.cshtml             Layout principal
      _ValidationScriptsPartial.cshtml
  wwwroot/
    css/                         Folhas de estilo da aplicação
    js/                          Scripts do cliente
    lib/                         Bibliotecas de terceiros (Bootstrap, jQuery)
```

## Camadas

### Host e inicialização

`Program.cs` usa o host genérico do .NET para criar e executar a aplicação.
O método `CreateHostBuilder` registra `Startup` como classe de configuração.

### Configuração da aplicação

`Startup.cs` centraliza duas responsabilidades:

- `ConfigureServices`: registra os serviços de Razor Pages.
- `Configure`: monta o pipeline HTTP com tratamento de exceções, HSTS,
  redirecionamento para HTTPS, arquivos estáticos, roteamento, autorização
  e mapeamento de endpoints.

### Apresentação

As páginas seguem o modelo code-behind do Razor Pages. Cada página possui um
arquivo `.cshtml` para a marcação e um `.cshtml.cs` com o `PageModel`
correspondente. O layout compartilhado em `Pages/Shared/_Layout.cshtml`
define a estrutura comum de navegação, conteúdo e rodapé.

## Dependências

As dependências são resolvidas pelo SDK do .NET, sem pacotes externos
declarados além do framework compartilhado do ASP.NET Core. As bibliotecas
de front-end são versionadas localmente em `wwwroot/lib`.

## Convenções de código

- Target framework: `net8.0` (LTS).
- `LangVersion` definido como `latest`.
- Tipagem explícita em declarações de variáveis e membros.
- Tipos de referência anuláveis habilitados (`Nullable`).
- Usings implícitos habilitados (`ImplicitUsings`).
- Namespaces no formato file-scoped.
- Avisos tratados como erros (`TreatWarningsAsErrors`).
- Ausência de comentários no código-fonte.

## Fluxo de requisição

1. A requisição entra pelo host configurado em `Program.cs`.
2. O pipeline definido em `Startup.Configure` aplica tratamento de erros,
   HTTPS, arquivos estáticos e roteamento.
3. O endpoint selecionado executa o `PageModel` da página Razor.
4. A página é renderizada com o layout compartilhado e devolvida ao cliente.

## Execução e build

```
dotnet restore
dotnet build -c Release
dotnet run --project NossaBib/NossaBib.csproj
```
